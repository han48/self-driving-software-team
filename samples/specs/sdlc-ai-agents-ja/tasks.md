# 実装計画: SDLC AI Agents Platform

## 概要

この計画は、SDLC AI Agents Platform（ソフトウェア開発ライフサイクル全体をカバーするマルチエージェントシステム）を実装するものです。実装はボトムアップの依存関係順序に従います：インフラストラクチャ → コアフレームワーク → 特化エージェントと管理システム → 統合/可観測性レイヤー。すべてのコードは TypeScript で、PostgreSQL、Redis、RabbitMQ、Vector DB (Qdrant)、Object Storage (S3) を使用します。

## Tasks

- [ ] 1. プロジェクトセットアップとコアインフラストラクチャ
  - [ ] 1.1 プロジェクト構造、monorepo 設定、共有 TypeScript 設定の初期化
    - 設計に合わせたディレクトリ構造を作成: `/src`, `/tests`, `/config`, `/docs`
    - TypeScript、ESLint、Prettier、Jest + fast-check を設定
    - monorepo パッケージをセットアップ: `core`, `agents`, `orchestrator`, `api`, `admin`, `services`
    - _Requirements: R16-C7, R37-C1_

  - [ ] 1.2 データベーススキーマとマイグレーションインフラの作成
    - 設計に基づく25以上のSQLテーブルを実装 (organizations, teams, projects, users, user_roles, agents, ai_models, pipelines, pipeline_steps, artifacts, review_gates, prompt_logs, sentiment_results, knowledge_documents, faq_entries, feedback, sku_tickets, quotas, audit_logs, change_requests, qa_threads, eval_test_suites, eval_results, templates, radio_reports, project_brain_entries)
    - マイグレーションツールをセットアップ (例: node-pg-migrate または Prisma)
    - _Requirements: R15, R16, R24, R33, R34, R35_

  - [ ] 1.3 メッセージブローカーインフラのセットアップ (RabbitMQ/Kafka)
    - エージェント間通信用の exchange/queue トポロジーを定義
    - イベントパブリッシャーとコンシューマーの基底クラスを実装
    - パイプラインイベント、エージェントイベント、レビューイベントのイベントスキーマを定義
    - _Requirements: R12-C1, R12-C2, R29-C3_

  - [ ] 1.4 Redis キャッシュとレート制限インフラのセットアップ
    - Redis コネクションプールを設定
    - TTL管理付きキャッシュサービスを実装
    - クォータ適用のための分散ロックプリミティブを実装
    - _Requirements: R28, R35-C4, R35-C5_

  - [ ] 1.5 大容量アーティファクト用 Object Storage (S3) のセットアップ
    - アーティファクトコンテンツ用のストレージサービスインターフェースを実装
    - バケットポリシーとライフサイクルルールを設定
    - _Requirements: R15-C1, R28-C2, R28-C3_

- [ ] 2. Checkpoint - すべてのインフラストラクチャテストが通ることを確認
  - すべてのテストが通ることを確認し、質問があればユーザーに確認する。

- [ ] 3. コアフレームワーク — Agent Base と Registry
  - [ ] 3.1 IAgent インターフェースと AgentBase 抽象クラスの実装
    - AgentRole、AgentStatus、AgentConfig、AgentTask、AgentOutput 型を定義
    - ライフサイクルメソッドを実装: initialize, execute, handleFeedback, shutdown, heartbeat
    - 設定可能な間隔でのヘルスチェックメカニズムを実装
    - _Requirements: R1-C1, R1-C2, R1-C4, R1-C5, R1-C7_

  - [ ] 3.2 IAgentRegistry サービスの実装
    - register, deactivate, updateConfig, getAgent, listAgents を実装
    - バリデーション適用: 名前 ≤ 100文字、role 必須、modelId 存在確認
    - アクティブエージェントの (name, role) ユニーク制約を適用
    - 非アクティブ化ロジックを実装（実行中タスクの完了を許可）
    - _Requirements: R1-C1, R1-C2, R1-C3, R1-C5, R1-C6_

  - [ ]* 3.3 Agent Registration のプロパティテストを記述 (Properties 1-4)
    - **Property 1: Agent Registration Validation (Round-trip Completeness)**
    - **Property 2: Agent ID Uniqueness**
    - **Property 3: Active Agent Name-Role Uniqueness Constraint**
    - **Property 4: Temperature Configuration Bounds**
    - **Validates: Requirements R1-C1, R1-C2, R1-C3, R1-C6, R1-C7**

  - [ ]* 3.4 Agent Deactivation のプロパティテストを記述 (Property 25)
    - **Property 25: Agent Deactivation Prevents New Task Assignment**
    - **Validates: Requirements R1-C5**

- [ ] 4. Context Store の実装
  - [ ] 4.1 IContextStore サービスの実装
    - write, read, listVersions, delete, query 操作を実装
    - artifact_id ユニーク性 + バージョン制約を適用
    - バージョンが100を超えた場合のFIFOエビクションを実装
    - (artifact_id, version) 重複書き込み時のバージョン競合検出を実装
    - read-after-write 一貫性を保証
    - コンテンツストレージに S3、メタデータに PostgreSQL を統合
    - _Requirements: R15-C1, R15-C2, R15-C3, R15-C4, R15-C5, R15-C6_

  - [ ]* 4.2 Context Store のプロパティテストを記述 (Properties 5-8)
    - **Property 5: Artifact Version Monotonic Increment**
    - **Property 6: Context Store Read-After-Write Consistency (Round-trip)**
    - **Property 7: Context Store Version Limit Invariant**
    - **Property 8: Context Store Version Conflict Detection**
    - **Validates: Requirements R15-C1, R15-C2, R15-C3, R15-C4**

  - [ ]* 4.3 Context Store のエッジケースユニットテストを記述
    - 存在しない artifact_id/version に対する not-found エラーをテスト
    - 認証適用をテスト（未認証アクセスの拒否）
    - 境界値テスト: ちょうど100バージョン時の動作
    - _Requirements: R15-C5, R15-C6_

- [ ] 5. Model Router と AI インフラストラクチャ
  - [ ] 5.1 IModelRouter サービスの実装
    - route, registerModel, configureRouting メソッドを実装
    - ルーティングロジックを実装: データ機密性 → self-hosted、タスク複雑度 → モデル選択、コストプロファイル → 最適化
    - デプロイモード間のフォールバックメカニズムを実装
    - 外部AIプロバイダー呼び出し用のサーキットブレーカーを実装
    - _Requirements: R39-C1, R39-C2, R39-C3, R39-C6, R39-C7, R40-C1, R40-C3, R40-C4, R40-C7_

  - [ ] 5.2 AIプロバイダーアダプターの実装 (Cloud + Self-Hosted)
    - 両デプロイモード用の統一推論インターフェースを実装
    - CloudConfig アダプターを実装 (OpenAI, Anthropic, AWS Bedrock)
    - SelfHostedConfig アダプターを実装 (Ollama, vLLM endpoints)
    - Self-hosted モデル用のヘルスチェックを実装
    - _Requirements: R40-C1, R40-C2, R40-C5_

  - [ ] 5.3 Embedding Service の実装
    - Knowledge Store と FAQ マッチング用のエンベディング生成を実装
    - 複数のエンベディングモデルバックエンドをサポート
    - _Requirements: R17-C1, R17-C2_

  - [ ]* 5.4 Model Routing のプロパティテストを記述 — Data Sensitivity (Property 18)
    - **Property 18: Model Routing — Data Sensitivity Constraint**
    - **Validates: Requirements R39-C3, R40-C3, R40-C6**

  - [ ]* 5.5 Model Router のユニットテストを記述
    - コスト-パフォーマンスプロファイルのルーティング判断をテスト
    - プライマリモデル不可用時のフォールバック動作をテスト
    - サーキットブレーカーの状態遷移をテスト
    - _Requirements: R39-C3, R39-C6, R40-C4_

- [ ] 6. Pipeline Orchestrator
  - [ ] 6.1 IPipelineOrchestrator サービスの実装
    - createPipeline, startPipeline, pausePipeline, resumePipeline, cancelPipeline, getPipelineStatus を実装
    - パイプライン状態マシンを実装 (Created → Running → Paused → Completed/Failed/Cancelled)
    - シーケンシャルおよび条件付き並列実行モードを実装
    - _Requirements: R12-C1, R12-C3, R12-C4_

  - [ ] 6.2 依存関係グラフ実行付き Task Scheduler の実装
    - PipelineStep の依存関係を DAG にパース
    - 独立ステップの並列実行を実装
    - 障害伝播を実装（依存先を停止、独立先は継続）
    - _Requirements: R12-C1, R12-C2_

  - [ ] 6.3 エージェントタイムアウト検出とリトライロジックの実装
    - ハートビートレスポンスを監視（300秒タイムアウト）
    - 60秒間隔、最大3回のリトライを実装
    - リトライ回数を使い果たした後 "failed" に遷移
    - _Requirements: R12-C5_

  - [ ] 6.4 Interaction Mode Router (IModeRouter) の実装
    - selectMode, switchMode, getActiveMode を実装
    - モード別パイプラインステップを設定: spec, fix, vibe
    - セッション中のモード切替時にコンテキストを保持
    - _Requirements: R13-C1, R13-C2, R13-C3, R13-C4, R13-C5_

  - [ ]* 6.5 Pipeline のプロパティテストを記述 (Properties 10-11)
    - **Property 10: Pipeline Dependency Graph Failure Propagation**
    - **Property 11: Pipeline Agent Timeout State Machine**
    - **Validates: Requirements R12-C2, R12-C5**

  - [ ]* 6.6 Orchestrator のユニットテストを記述
    - パイプライン完了時のサマリーレポート生成をテスト
    - モード切替時の蓄積アーティファクト保持をテスト
    - 条件付き並列ステップ実行をテスト
    - _Requirements: R12-C4, R13-C5_

- [ ] 7. Checkpoint - コアフレームワークとオーケストレーターのテストが通ることを確認
  - すべてのテストが通ることを確認し、質問があればユーザーに確認する。

- [ ] 8. Review Gate Manager
  - [ ] 8.1 IReviewGateManager サービスの実装
    - createGate, submitForReview, approve, reject, getChecklist を実装
    - チェックリスト適用を実装（承認前にすべての必須項目がチェック済みであること）
    - リジェクトカウンターと3回連続リジェクト後の "blocked" ステータスを実装
    - リマインダー（24時間）とエスカレーション（48時間）タイマーを実装
    - _Requirements: R14-C1, R14-C2, R14-C3, R14-C4, R14-C5, R14-C6_

  - [ ] 8.2 Approve Checklist システムの実装
    - ゲートタイプ別のデフォルトチェックリストを定義 (requirements, design, tasks, PR)
    - 条件付きチェックリスト項目を実装
    - プロジェクト/チーム別のカスタムチェックリスト設定を実装
    - _Requirements: R47-C1, R47-C2, R47-C3, R47-C4, R47-C5_

  - [ ]* 8.3 Review Gate のプロパティテストを記述 (Properties 12, 20)
    - **Property 12: Review Gate Blocking After 3 Rejections**
    - **Property 20: Approve Checklist Enforcement**
    - **Validates: Requirements R14-C6, R47-C2**

  - [ ]* 8.4 Review Gate のユニットテストを記述
    - リマインダー/エスカレーションのタイミングをテスト
    - チェックリストメトリクスの追跡（初回パス率）をテスト
    - 条件付きチェックリスト項目の表示をテスト
    - _Requirements: R14-C5, R47-C5, R47-C6_

- [ ] 9. Output Validation と Self-Review Loop
  - [ ] 9.1 IValidationEngine サービスの実装
    - validate メソッドをルールタイプ付きで実装: schema, required_sections, regex, guardrail
    - リトライループ（最大3回）を実装し、失敗時 "validation_failed" マーキング
    - エンタープライズガードレールを実装: PII/認証情報検出、出力長制限、ブラックリスト
    - _Requirements: R22-C1, R22-C2, R22-C3, R22-C4, R22-C5_

  - [ ] 9.2 Self-Review Loop Engine の実装
    - selfReview を基準付きで実装: clarity, completeness, consistency, compliance, riskDetection
    - 反復的 self-fix ループを実装（最大3回）
    - メタデータを付与: iterations count, issues found/fixed, confidence score, remaining issues
    - 処理チェーンを接続: Self-Review → Validation → Human Review
    - _Requirements: R43-C1, R43-C2, R43-C3, R43-C4, R43-C5_

  - [ ]* 9.3 Validation と Self-Review のプロパティテストを記述 (Properties 14, 19)
    - **Property 14: Validation Retry Bounded Loop**
    - **Property 19: Self-Review Loop Termination**
    - **Validates: Requirements R22-C3, R22-C4, R43-C2, R43-C4**

  - [ ]* 9.4 Validation Engine のユニットテストを記述
    - ガードレール適用をテスト（PII検出、ブラックリストマッチング）
    - バリデーションルールの CRUD とエージェントタイプ別割り当てをテスト
    - Self-review ログの完全性をテスト
    - _Requirements: R22-C5, R22-C6, R22-C7, R43-C6_

- [ ] 10. Git Integration Service
  - [ ] 10.1 IGitService の実装
    - commit, createBranch, mergeBranch, createTag, revert, push を実装
    - コミットメッセージフォーマットを実装: `[agent_role] prompt summary` + メタデータ本文
    - ブランチ戦略を実装: pipeline → stage → task → sub-task ブランチ
    - マージフローを実装: sub-task → task → pipeline → main
    - _Requirements: R16-C1, R16-C3, R16-C4, R16-C5, R16-C6_

  - [ ] 10.2 Context Store ↔ Git 同期の実装
    - すべてのアーティファクトに git_commit_sha 参照を保証
    - バージョンからコミットへのマッピングを実装
    - タスク/ステージ完了時のタグ作成を実装
    - _Requirements: R16-C9, R16-C10, R16-C11_

  - [ ]* 10.3 Git Integration のユニットテストを記述
    - コミットメッセージフォーマットの準拠をテスト
    - ブランチ階層の作成とマージフローをテスト
    - git revert によるロールバックをテスト
    - _Requirements: R16-C3, R16-C6, R16-C10_

- [ ] 11. Unified Failure Handling
  - [ ] 11.1 IFailureHandler サービスの実装
    - handleFailure を3段階エスカレーション付きで実装: self-fix → 別アプローチ → 根本原因分析
    - スコープ別ロールバック戦略を実装 (code, spec, feature, deployment)
    - 4回目リトライ時の Human override メカニズムを実装
    - 障害レポート、RADIO "D" エントリ、ISSUE-XXX 作成を生成
    - _Requirements: R50-C1, R50-C2, R50-C3, R50-C4, R50-C5_

  - [ ]* 11.2 Unified Retry Escalation のプロパティテストを記述 (Property 22)
    - **Property 22: Unified Retry Escalation Path**
    - **Validates: Requirements R50-C1, R50-C2**

  - [ ]* 11.3 Failure Handling のユニットテストを記述
    - ロールバックスコープの判定をテスト
    - 障害レポート生成の内容をテスト
    - エージェント別の設定可能なリトライ回数をテスト
    - _Requirements: R50-C4, R50-C5, R50-C6_

- [ ] 12. Checkpoint - Validation、Git、Failure Handling のテストが通ることを確認
  - すべてのテストが通ることを確認し、質問があればユーザーに確認する。

- [ ] 13. Knowledge Store と RAG
  - [ ] 13.1 IKnowledgeStore サービスの実装
    - ingest を実装（ドキュメントチャンク化、エンベディング作成、インデックス）
    - search を実装（セマンティック類似度 + top-K、設定可能なスコープ）
    - update と delete を実装し、300秒以内に自動エンベディング更新
    - ドキュメントフォーマットをサポート: plain text, Markdown, PDF, DOCX
    - スコーピングを設定: global, organization, project, agent
    - _Requirements: R17-C1, R17-C2, R17-C3, R17-C4, R17-C5, R17-C6_

  - [ ]* 13.2 Knowledge Store のユニットテストを記述
    - セマンティック検索が200ms以内に関連結果を返すことをテスト
    - ドキュメント変更時のエンベディング更新をテスト
    - プロジェクト間のスコープ分離をテスト
    - _Requirements: R17-C1, R17-C4, R17-C5, R17-C7_

- [ ] 14. FAQ Store と Best-Match
  - [ ] 14.1 IFAQStore サービスの実装
    - FAQ エントリの CRUD 操作を実装
    - 関連性スコア計算付きセマンティックマッチングを実装
    - 閾値ベースのルーティングを実装: スコア ≥ 閾値 → faq_direct、それ以外 → fallback_to_ai
    - マッチングレイテンシ < 200ms を保証
    - _Requirements: R18-C1, R18-C2, R18-C3, R18-C4, R18-C5, R18-C6, R18-C7_

  - [ ]* 14.2 FAQ Threshold Routing のプロパティテストを記述 (Property 13)
    - **Property 13: FAQ Threshold Routing Decision**
    - **Validates: Requirements R18-C3, R18-C4**

  - [ ]* 14.3 FAQ Store のユニットテストを記述
    - CRUD 操作とステータス遷移をテスト
    - エージェント別の設定可能な信頼度閾値をテスト
    - FAQ マッチイベントのログ記録をテスト
    - _Requirements: R18-C1, R18-C5, R18-C7, R18-C8_

- [ ] 15. Project Brain
  - [ ] 15.1 Project Brain サービスの実装
    - アーティファクト、決定ログ、会話サマリー、用語を自動収集・インデックス
    - ナレッジグラフを実装: requirement ↔ design ↔ task ↔ code ↔ test の関係
    - 自然言語クエリインターフェースを実装（< 500ms レイテンシ）
    - 変更から120秒以内にエンベディングを自動更新
    - レビューコメントからの決定ログ抽出を実装
    - _Requirements: R21-C1, R21-C2, R21-C3, R21-C4, R21-C5, R21-C8_

  - [ ] 15.2 Project Brain の用語と保持管理の実装
    - プロジェクト固有の用語検出とユーザー確認を実装
    - 設定可能な保持ポリシーを実装（永続 vs. アーカイブ可能）
    - エントリの表示、検索、手動追加用 UI を実装
    - メトリクスを実装: インデックス済み項目数、ストレージサイズ、ヒット率、ギャップ分析
    - _Requirements: R21-C6, R21-C7, R21-C9, R21-C10_

  - [ ]* 15.3 Project Brain のユニットテストを記述
    - ナレッジグラフの関係クエリをテスト
    - 決定ログの自動抽出をテスト
    - 保持ポリシーの適用（有効期限、アーカイブ）をテスト
    - _Requirements: R21-C3, R21-C5, R21-C9_

- [ ] 16. Template Registries
  - [ ] 16.1 Template_Registry（ソースコードテンプレート）の実装
    - バージョニング付きコードテンプレートの CRUD を実装
    - テンプレート提案用のセマンティックマッチングを実装（閾値 ≥ 0.8）
    - テンプレートタイプをサポート: project scaffold, feature module, architecture pattern
    - スコーピングを実装: global, organization, team-private
    - テンプレート品質を適用: linting、セキュリティスキャン、コアコンポーネント用ユニットテスト
    - _Requirements: R19-C1, R19-C2, R19-C3, R19-C5, R19-C6, R19-C8, R19-C9_

  - [ ] 16.2 Doc_Template_Registry（ドキュメントと設定テンプレート）の実装
    - ドキュメントテンプレートの CRUD を実装 (requirements, design, task, skills, hooks, steering)
    - テンプレート継承を実装（子が親を拡張、オーバーライド/追加）
    - skills/hooks/steering テンプレート用のスキーマバリデーションを実装
    - diff 機能付きバージョニングを実装
    - _Requirements: R20-C1, R20-C2, R20-C4, R20-C5, R20-C6, R20-C7_

  - [ ]* 16.3 Template Registries のユニットテストを記述
    - テンプレートのバージョニングと diff をテスト
    - 継承解決をテスト
    - コードテンプレートのセマンティックマッチングをテスト
    - _Requirements: R19-C3, R19-C6, R20-C6, R20-C7_

- [ ] 17. Checkpoint - Knowledge、FAQ、Brain、Template のテストが通ることを確認
  - すべてのテストが通ることを確認し、質問があればユーザーに確認する。

- [ ] 18. Authorization と RBAC
  - [ ] 18.1 IAuthService の実装
    - authenticate, authorize, assignRole, delegate メソッドを実装
    - 5つのロールを実装: Viewer, Developer, Tech Lead, Admin, Super Admin
    - 階層スコープを実装: Organization → Team → Project
    - チーム間のデータ分離を実装（クロスチームは Super Admin の付与が必要）
    - 最大30日の自動取り消し付き委任を実装
    - ロックアウトを実装: 5分以内に5回の認証失敗 → 15分ロック
    - _Requirements: R33-C1, R33-C2, R33-C5, R34-C1, R34-C2, R34-C3, R34-C5, R34-C6_

  - [ ] 18.2 ユーザーライフサイクル管理の実装
    - ユーザー無効化/削除時に60秒以内にすべてのセッション/資格情報/委任を取り消し
    - レビュアー無効化時の承認再割り当てを実装
    - 5つのデフォルトロール以外のカスタム権限セットを実装
    - _Requirements: R34-C4, R34-C7, R34-C8_

  - [ ]* 18.3 Auth のプロパティテストを記述 (Properties 15-16)
    - **Property 15: Authentication Lockout Rule**
    - **Property 16: Hierarchical RBAC Authorization**
    - **Validates: Requirements R33-C5, R34-C2, R34-C3, R34-C6**

  - [ ]* 18.4 Authorization のユニットテストを記述
    - ロール階層の権限をテスト
    - 委任の有効期限切れ自動取り消しをテスト
    - クロスチームデータ分離をテスト
    - _Requirements: R34-C2, R34-C5, R34-C6_

- [ ] 19. Quota とレート制限
  - [ ] 19.1 IQuotaService の実装
    - checkQuota, consumeQuota, getUsage, configureQuota を実装
    - エージェント/チーム/プロジェクト別クォータを設定可能なサイクル（日次/週次/月次）で実装
    - 80% 警告アラートと 100% ハードブロック（実行中パイプラインは完了を許可）を実装
    - バースト許容量を実装（デフォルト20%、最大1時間）
    - モデル別レート制限（リクエスト/分）を実装
    - _Requirements: R35-C1, R35-C2, R35-C3, R35-C4, R35-C5_

  - [ ]* 19.2 Quota のプロパティテストを記述 (Property 17)
    - **Property 17: Quota Threshold Alerts and Blocking**
    - **Validates: Requirements R35-C2, R35-C3**

  - [ ]* 19.3 Quota Service のユニットテストを記述
    - バースト許容量のタイミングと制限をテスト
    - クォータ超過時の実行中パイプライン完了をテスト
    - リアルタイム使用量レポートの精度をテスト
    - _Requirements: R35-C3, R35-C5, R35-C6_

- [ ] 20. Notification と Communication Service
  - [ ] 20.1 INotificationService の実装
    - send, configureChannel, getMetrics, delegate メソッドを実装
    - チャネルをサポート: Slack, Teams, Email, in-app
    - リマインダーエスカレーション戦略を実装: 即時 → 24h リマインド → 48h エスカレート → 72h タスク一時停止
    - 通知受信者の委任を実装
    - _Requirements: R51-C1, R51-C2, R51-C3_

  - [ ]* 20.2 Communication Reminder Escalation のプロパティテストを記述 (Property 23)
    - **Property 23: Communication Reminder Escalation Timeline**
    - **Validates: Requirements R51-C3**

  - [ ]* 20.3 Notification Service のユニットテストを記述
    - チャネル設定とルーティングをテスト
    - 委任転送をテスト
    - SLAベースの優先度処理をテスト
    - _Requirements: R51-C1, R51-C2, R51-C3_

- [ ] 21. Sentiment Analysis Service とエージェント
  - [ ] 21.1 ISentimentService と Sentiment_Agent の実装
    - analyze メソッドを実装: 感情分類 (positive/neutral/frustrated/angry) + 信頼度
    - ノンブロッキング並列実行を保証（< 100ms 追加レイテンシ）
    - エスカレーショントリガーを実装: 2回連続ネガティブ + 信頼度 ≥ 0.7
    - エスカレーション時の SKU チケット作成を実装（SKU 不可用時のリトライキュー付き）
    - 手動エスカレーションをサポート
    - _Requirements: R11-C1, R11-C2, R11-C3, R11-C4, R11-C5, R11-C6, R11-C7_

  - [ ] 21.2 Sentiment ダッシュボードと設定の実装
    - 90日保持期間で感情結果を保存
    - エージェント/システム別の設定可能なエスカレーション閾値を実装
    - エージェント別の有効/無効を実装
    - ダッシュボードを実装: 分布、エスカレーション率、相関分析
    - _Requirements: R11-C8, R11-C9, R11-C10_

  - [ ]* 21.3 Sentiment Escalation のプロパティテストを記述 (Property 9)
    - **Property 9: Sentiment Escalation Trigger Logic**
    - **Validates: Requirements R11-C3**

  - [ ]* 21.4 Sentiment Service のユニットテストを記述
    - ノンブロッキング実行（レイテンシ制約）をテスト
    - 手動エスカレーション時の正しい SKU issue 作成をテスト
    - 有効/無効トグル動作をテスト
    - _Requirements: R11-C2, R11-C7, R11-C10_

- [ ] 22. Checkpoint - Auth、Quota、Notification、Sentiment のテストが通ることを確認
  - すべてのテストが通ることを確認し、質問があればユーザーに確認する。

- [ ] 23. 特化エージェント — Requirements、Design、UIUX
  - [ ] 23.1 Requirements_Agent の実装
    - 生の入力から構造化された要件を作成（< 60秒）
    - 分類を実装: functional, non-functional, constraint
    - 矛盾検出とフラグ付けを実装
    - actor/action/outcome 形式の受入基準生成を実装
    - 入力が不十分な場合の明確化質問生成を実装
    - アーティファクトバージョニングを実装（保存/更新時にインクリメント）
    - _Requirements: R2-C1, R2-C2, R2-C3, R2-C4, R2-C5, R2-C6, R2-C7_

  - [ ] 23.2 Design_Agent の実装
    - 承認済み要件から設計ドキュメントを生成（< 120秒）
    - カバレッジバリデーションを実装: すべての機能要件が設計要素にマッピング
    - 要件バージョン変更時の変更検出と "CHANGED" アノテーションを実装
    - 要件が未承認の場合は処理を拒否
    - _Requirements: R3-C1, R3-C2, R3-C3, R3-C4, R3-C5_

  - [ ] 23.3 UIUX_Agent の実装
    - 承認済み設計から HTML プロトタイプを生成
    - 設定可能な CSS フレームワーク（デフォルト Bootstrap）でレスポンシブレイアウトを生成
    - ユーザーフローテスト用の画面間ナビゲーションリンクを生成
    - 画面ごとのインタラクション仕様 Markdown を生成
    - 標準ディレクトリ構造で出力を整理
    - **GUI Reference:** `gui/` が UIUX_Agent の模範的な出力フォーマット — エージェントは同様の構造、ナビゲーションパターン、Bootstrap 使用の HTML プロトタイプを生成すべき
    - _Requirements: R4-C1, R4-C2, R4-C3, R4-C4, R4-C5, R4-C6, R4-C8, R4-C9_

  - [ ]* 23.4 Requirements、Design、UIUX Agent のユニットテストを記述
    - 要件分類の精度をテスト
    - 矛盾検出をテスト
    - 設計カバレッジバリデーションをテスト
    - プロトタイプ構造の準拠をテスト
    - _Requirements: R2-C2, R2-C4, R3-C2, R4-C8_

- [ ] 24. 特化エージェント — Task、Coding、Testing
  - [ ] 24.1 Task_Agent の実装
    - 設計からタスク分解を実装: title, description, AC, effort, priority, dependencies, assignee role
    - すべての設計コンポーネントが少なくとも1つのタスクでカバーされることを保証
    - 依存関係グラフの順序付けと並列タスクの識別を実装
    - 機能/モジュール別のグループ化とラベルを実装
    - XL タスクのサブタスクへの自動分割を実装
    - 設計バージョン変更時のタスク更新を実装
    - _Requirements: R5-C1, R5-C2, R5-C3, R5-C4, R5-C5, R5-C6, R5-C7_

  - [ ] 24.2 Coding_Agent の実装
    - 承認済み設計から設定言語でコード生成を実装
    - ユニットテスト同時生成を実装（≥ 80% カバレッジ目標）
    - linting チェック + 自動修正ループを実装
    - セキュリティスキャンを実装: ハードコードされた認証情報なし
    - Template_Registry 統合によるテンプレート使用を実装
    - _Requirements: R6-C1, R6-C2, R6-C3, R6-C4, R6-C5, R6-C6, R19-C3, R19-C4_

  - [ ] 24.3 Testing_Agent の実装
    - テストスイート生成を実装: unit, integration, acceptance テスト
    - テスト実行とレポート生成を実装
    - 重要度分類を実装: critical, major, minor
    - デプロイメントゲートを実装: パス率 < 90% の場合ブロック
    - パーサー/シリアライザーコンポーネント用のラウンドトリップテストを実装
    - _Requirements: R7-C1, R7-C2, R7-C3, R7-C4, R7-C5, R7-C6_

  - [ ]* 24.4 Task、Coding、Testing Agent のユニットテストを記述
    - タスク依存関係グラフ生成の正確性をテスト
    - コード認証情報スキャン検出をテスト
    - テスト実行レポートのフォーマットをテスト
    - _Requirements: R5-C3, R6-C6, R7-C2_

- [ ] 25. 特化エージェント — Deployment、Operations、ProjectHealth
  - [ ] 25.1 Deployment_Agent の実装
    - 環境別 CI/CD パイプライントリガーを実装 (dev/staging/production)
    - ヘルスチェックループを実装: 10秒ごと、最大300秒
    - ヘルスチェック失敗時のロールバックを実装
    - 本番デプロイメント承認ゲートを実装（60分タイムアウト）
    - デプロイメントログを実装: 開始、終了、ステップごとのステータス、エラーメッセージ
    - _Requirements: R8-C1, R8-C2, R8-C3, R8-C4, R8-C5, R8-C6_

  - [ ] 25.2 Operations_Agent の実装
    - メトリクス収集を実装: CPU、メモリ、レイテンシ、エラー率（≤ 60秒サイクル）
    - 閾値違反時の警告アラートを実装（30秒以内）
    - インシデント検出を実装（3回連続ヘルスチェック失敗またはエラー率 > 5% が5分間）
    - ログからの根本原因分析を実装（60分ウィンドウ）
    - 30日メトリクス保持 + アーカイブポリシーを実装
    - _Requirements: R9-C1, R9-C2, R9-C3, R9-C4, R9-C5_

  - [ ] 25.3 ProjectHealth_Agent の実装
    - すべてのシステムソースからのデータ収集を実装
    - 日次/週次/スプリントレポート生成を実装
    - ヘルススコアを実装: progress, quality, risk, velocity, cost efficiency
    - リスク閾値アラートを実装（> 7/10 → 自動アラート）
    - 早期警告検出を実装（ベロシティ低下、テストパス率低下、ブロックされたタスク）
    - 自然言語クエリインターフェースを実装
    - _Requirements: R10-C1, R10-C2, R10-C3, R10-C4, R10-C5, R10-C6, R10-C7, R10-C8_

  - [ ]* 25.4 Deployment、Operations、ProjectHealth Agent のユニットテストを記述
    - ヘルスチェックループとロールバックトリガーをテスト
    - 重大障害処理（ロールバックも失敗する場合）をテスト
    - リスクスコア計算の精度をテスト
    - 早期警告閾値検出をテスト
    - _Requirements: R8-C2, R8-C6, R10-C4, R10-C5_

- [ ] 26. Checkpoint - すべての特化エージェントテストが通ることを確認
  - すべてのテストが通ることを確認し、質問があればユーザーに確認する。

- [ ] 27. 管理システム — RADIO、Change Request、Q&A
  - [ ] 27.1 IRADIOGenerator サービスの実装
    - 5コンポーネント形式の generate メソッドを実装 (R, A, D, I, O)
    - トリガーベースの生成を実装: task_complete, test_fail, new_blocker, end_of_day, sprint_end, wave_complete
    - レポートを Git の `docs/specs/{feature-name}/radio.md` に保存
    - 重要度ベースのアラートを実装（D severity ≥ High → Tech Lead + PM に通知）
    - 履歴取得とタイムライン比較を実装
    - _Requirements: R42-C1, R42-C2, R42-C3, R42-C4, R42-C5, R42-C6_

  - [ ] 27.2 ICRManager (Change Request Management) の実装
    - ライフサイクルを実装: Pending → Approved → In Progress → Review → Done | Rejected
    - CR バックログを `docs/change-request/backlog.md` に実装
    - 承認時に requirements.md/design.md/tasks.md 付き CR ディレクトリを自動作成
    - 小規模 CR ショートカットを実装（工数 < 1日: 設計スキップ）
    - CR ごとのインパクト分析を実装
    - ダッシュボードを実装: open/in-progress/done トレンド、スループット、サイクルタイム
    - _Requirements: R44-C1, R44-C2, R44-C3, R44-C4, R44-C5, R44-C6, R44-C7, R44-C8_

  - [ ] 27.3 Q&A Management システムの実装
    - QA スレッドライフサイクルを実装: Open → In Discussion → Answered → Closed
    - エージェントが曖昧さを検出した際の QA スレッド自動作成を実装
    - フォローアップ付きマルチラウンドディスカッションをサポート
    - 結論 + アクションアイテムワークフローを実装
    - QA 結論承認時にスペック/設計を自動更新
    - オープン QA スレッドが存在する場合、スペック進行をブロック
    - 期限追跡と期限超過時の RADIO "D" フラグを実装
    - _Requirements: R45-C1, R45-C2, R45-C3, R45-C4, R45-C5, R45-C6, R45-C7, R45-C8_

  - [ ]* 27.4 RADIO、CR、Q&A のユニットテストを記述
    - 実データからの RADIO レポート生成をテスト
    - CR ライフサイクル遷移をテスト
    - Q&A スペックブロッキング適用をテスト
    - _Requirements: R42-C3, R44-C1, R45-C8_

- [ ] 28. 管理システム — Deliverables、DoD、Spec Versioning、Bugfix
  - [ ] 28.1 Deliverables & Limitations Management の実装
    - スプリント/フェーズ終了時に実行データから成果物を自動集約
    - 分類を実装: feature (fully done), partial, documentation
    - 未完了タスク、失敗テスト、既知の問題から制限事項を自動検出
    - 各制限事項に対する対応計画を実装
    - 双方向リンクを実装 (limitation ↔ spec/CR/task)
    - _Requirements: R46-C1, R46-C2, R46-C3, R46-C4, R46-C5, R46-C6, R46-C7_

  - [ ] 28.2 IDoDEnforcer サービスの実装
    - アーティファクトタイプ別の check メソッドを実装: spec, design, tasks, feature, bugfix
    - 失敗基準時の詳細な失敗リスト付きブロッキングを実装
    - オーバーライドメカニズムを実装（Tech Lead のみ、監査ログ付き）
    - プロジェクト別の設定可能な DoD 基準を実装
    - _Requirements: R48-C1, R48-C2, R48-C3, R48-C4, R48-C5, R48-C6_

  - [ ] 28.3 ISpecVersioning サービスの実装
    - detectChangeType を実装: Major（スコープ/ロジック）、Minor（詳細追加）、Patch（タイポ/フォーマット）
    - diff からの自動検出付き bumpVersion を実装
    - requiresApproval ロジックを実装（Major/Minor → 要承認、Patch → 自動承認）
    - 影響を受けるアーティファクト識別用の flagDownstream を実装
    - _Requirements: R52-C1, R52-C2, R52-C3_

  - [ ] 28.4 Bugfix Workflow の実装
    - バグ修正ドキュメントフォーマットを実装: Current Behavior, Expected Behavior, Unchanged Behavior
    - 5WHY 分析を実装（根本原因まで3-5回の反復）
    - 予防計画の要件を実装
    - "Unchanged Behavior" をカバーするリグレッションテスト生成を実装
    - Bugfix DoD の適用を実装
    - _Requirements: R49-C1, R49-C2, R49-C3, R49-C4, R49-C5, R49-C6_

  - [ ]* 28.5 DoD と Spec Versioning のプロパティテストを記述 (Properties 21, 24)
    - **Property 21: Definition of Done Enforcement**
    - **Property 24: Spec Versioning — Change Type Determines Approval Requirement**
    - **Validates: Requirements R48-C2, R48-C3, R52-C1, R52-C2, R52-C3**

  - [ ]* 28.6 Deliverables、DoD、Spec Versioning、Bugfix のユニットテストを記述
    - DoD オーバーライドの監査証跡をテスト
    - 成果物自動集約の精度をテスト
    - スペックバージョンバンプの自動検出をテスト
    - 5WHY 分析の深度適用をテスト
    - _Requirements: R46-C1, R48-C6, R52-C1, R49-C2_

- [ ] 29. Checkpoint - すべての管理システムテストが通ることを確認
  - すべてのテストが通ることを確認し、質問があればユーザーに確認する。

- [ ] 30. 可観測性スタック
  - [ ] 30.1 IPromptLogger (AI インタラクションログ) の実装
    - 追記専用の不変ログを実装
    - ログ内容: agent_id, pipeline_id, timestamp, prompt, response, model_id, deployment_mode, params, metrics
    - 保存前の PII/認証情報自動マスキングを実装
    - 設定可能な詳細度を実装: full, summary, metadata_only
    - 最低90日の保持期間を実装
    - クエリ API を実装: agent_id, pipeline_id, 時間範囲, model, keyword で検索
    - _Requirements: R24-C1, R24-C2, R24-C3, R24-C4, R24-C5, R24-C6, R24-C7, R24-C8_

  - [ ] 30.2 構造化ログサービスの実装
    - JSON 構造化ログを実装: timestamp, agent_id, action, pipeline_id, status, execution time
    - リクエストごとのユニーク trace_id による分散トレーシングを実装
    - エラー時のログ + トレース + メトリクスを連結する相関レポートを実装
    - _Requirements: R36-C1, R36-C2, R36-C5_

  - [ ] 30.3 メトリクスとモニタリングの実装
    - Prometheus 互換メトリクスエンドポイントを公開
    - リアルタイムダッシュボードを実装（≤ 10秒遅延）
    - 追跡対象: パイプラインスループット、エージェント実行時間、エラー率、トークン使用量、コスト
    - _Requirements: R36-C3, R36-C4_

  - [ ] 30.4 監査ログサービスの実装
    - 追記専用の不変監査ログを実装
    - 記録内容: actor, action, target, timestamp, result, details
    - 最低90日の保持期間を実装
    - コンプライアンスレビュー用クエリインターフェースを実装
    - _Requirements: R33-C3, R33-C4_

  - [ ]* 30.5 可観測性スタックのユニットテストを記述
    - プロンプトログの不変性をテスト（更新/削除操作なし）
    - PII 自動マスキングの精度をテスト
    - サービス間の trace_id 伝播をテスト
    - 状態変更操作に対する監査ログの完全性をテスト
    - _Requirements: R24-C5, R24-C7, R36-C2, R33-C3_

- [ ] 31. Feedback、Evaluation、SKU Tickets
  - [ ] 31.1 Feedback & Rating Service の実装
    - アーティファクトごとの thumbs up/down + コメント（最大1000文字）を実装
    - 多次元評価を実装（1-5 stars、基準別: accuracy, speed, relevance, completeness）
    - 自動分類を実装: bug_report, quality_issue, feature_request, praise, general
    - 30% ネガティブ率時のアラートを実装（7日間、最低10フィードバック）
    - ≤ 2 star / bug_report 時の SKU チケット作成を実装
    - _Requirements: R25-C1, R25-C2, R25-C3, R25-C4, R26-C1, R26-C2, R26-C3, R26-C4_

  - [ ] 31.2 Eval_Store と AI 品質評価の実装
    - 評価テストスイートの CRUD を実装（入力、正解データ、スコアリング基準）
    - 3つの方法をサポート: Automated Metrics, LLM-as-Judge, Human Evaluation
    - スケジュール評価を実装（手動、日次/週次、変更時）
    - 品質回帰検出を実装（低下 > 10% → アラート）
    - 統計的有意性付き A/B 評価を実装（p < 0.05）
    - Human Evaluation ワークフローを実装: 自動サンプリング → 専門家レビュー → スコア更新
    - _Requirements: R23-C1, R23-C2, R23-C3, R23-C4, R23-C5, R23-C6, R23-C7, R23-C9_

  - [ ] 31.3 SKU Ticket Management の実装
    - チケットライフサイクル管理を実装
    - 優先度別 SLA 設定を実装（初回応答 + 解決）
    - 重要チケット未アサイン > 30分時の自動アラートを実装
    - _Requirements: R27-C1, R27-C2, R27-C3, R27-C7, R27-C8_

  - [ ]* 31.4 Feedback、Eval、SKU のユニットテストを記述
    - フィードバック自動分類の精度をテスト
    - 評価スコアリングと回帰検出をテスト
    - SLA 違反検出とアラートをテスト
    - _Requirements: R26-C3, R23-C6, R27-C8_

- [ ] 32. Default Hooks、Steering、Skills
  - [ ] 32.1 デフォルト hooks システムの実装
    - 7つのデフォルト hooks を実装: lint-on-code-save, validate-output-before-save, run-tests-after-task, security-scan-on-code, notify-on-pipeline-complete, sentiment-check, template-match-on-new-project
    - チーム/プロジェクト別の hook 有効/無効/カスタマイズを実装
    - デフォルトへのリセット機能を実装
    - _Requirements: R41-C1, R41-C4, R41-C5_

  - [ ] 32.2 デフォルト steering files と skills の実装
    - 8つのデフォルト steering files を作成: coding-standards, security-guidelines, architecture-principles, documentation-standards, git-workflow, testing-strategy, api-design-guidelines, deployment-checklist
    - 10のデフォルト skills を登録: database-migration, api-documentation, changelog-generator, dependency-audit, code-review, performance-analysis, incident-postmortem, task-estimation, translation, diagram-generator
    - システム更新時の新規デフォルトのオプトインメカニズムを実装
    - _Requirements: R41-C2, R41-C3, R41-C6_

  - [ ]* 32.3 Hooks/Steering/Skills のユニットテストを記述
    - Hook イベントトリガーをテスト
    - チーム別オーバーライドの分離をテスト
    - リセット機能がカスタム項目を保持することをテスト
    - _Requirements: R41-C4, R41-C5_

- [ ] 33. Plugin Agent Architecture
  - [ ] 33.1 カスタムエージェント用 plugin メカニズムの実装
    - カスタムロールサポート付き plugin 登録を実装
    - Plugin ライフサイクルを実装 (install, configure, activate, deactivate)
    - Plugin が既存のパイプラインとオーケストレーターに統合されることを保証
    - コアを修正せずに新しいエージェントロールを追加可能であることを保証
    - _Requirements: R1-C1 (plugin extension), R32_

  - [ ]* 33.2 Plugin Architecture のユニットテストを記述
    - Plugin の登録とディスカバリーをテスト
    - Plugin のコアからの分離をテスト
    - _Requirements: R32_

- [ ] 34. Checkpoint - Feedback、Eval、Hooks、Plugin のテストが通ることを確認
  - すべてのテストが通ることを確認し、質問があればユーザーに確認する。

- [ ] 35. API Gateway と Admin Portal
  - [ ] 35.1 REST/gRPC API Gateway の実装
    - すべてのサービス用の OpenAPI/Protobuf ドキュメント付きエンドポイントを実装
    - 認証ミドルウェア（トークンベース）を実装
    - レート制限ミドルウェアを実装
    - パイプラインイベント通知用 webhook サービスを実装
    - 95パーセンタイルレスポンスタイム < 1000ms を保証
    - _Requirements: R37-C1, R37-C3, R28-C5_

  - [ ] 35.2 Admin Portal — エージェントとパイプライン管理の実装
    - エージェント管理 UI を実装: CRUD、ステータス監視、設定編集
    - パイプライン管理 UI を実装: 作成、監視、一時停止、再開、キャンセル
    - システム設定を実装: デフォルト temperature、FAQ confidence、Knowledge top-K、retention
    - **GUI Reference:** `gui/screens/agents.html`, `gui/screens/pipeline.html`, `gui/screens/admin.html`
    - _Requirements: R38-C1, R38-C2_

  - [ ] 35.3 Admin Portal — AI Model と Evaluation 管理の実装
    - AI Model Management を実装: モデル登録/削除、レート制限、使用統計
    - Evaluation Management を実装: テストスイート CRUD、スケジュール、閾値、Human Eval アサイン
    - Quality Dashboard を実装: トレンド、エージェント比較、ヒートマップ
    - **GUI Reference:** `gui/screens/admin.html` (AI Model Usage セクション、Quota セクション)
    - _Requirements: R38-C3, R23-C8, R23-C12_

  - [ ] 35.4 Admin Portal — Templates、Users、Health、Logs の実装
    - Template Management (Code + Doc) を実装: CRUD、プレビュー、統計、インポート/エクスポート
    - User & Permissions Management を実装: ロール、スコープ、委任
    - System Health ビューを実装: コンポーネントステータス、稼働時間、リソース使用量
    - Logs & Audit ビューアーを実装: プロンプトログ、監査ログ、相関レポートのクエリ
    - Backup & Recovery を実装: 履歴表示、手動バックアップ、ポイントインタイムリストア
    - **GUI Reference:** `gui/screens/users.html`, `gui/screens/admin.html` (System Health, Settings)
    - _Requirements: R19-C7, R20-C8, R34-C8, R38-C4, R38-C5, R38-C7, R38-C8_

  - [ ] 35.5 Admin Portal — SKU Tickets と User Feedback の実装
    - SKU Ticket Management を実装: 一覧、フィルター、アサイン、ステータス、ノート、レスポンス
    - User Feedback Management を実装: カンバン、フィルター、一括操作、チケットへのリンク
    - SKU Dashboard を実装: open/resolved トレンド、MTTR、SLA 違反
    - Feedback Dashboard を実装: 評価トレンド、カテゴリ、トップ課題、機能リクエスト
    - **GUI Reference:** `gui/screens/feedback.html`
    - _Requirements: R27-C1, R27-C2, R27-C3, R27-C4, R27-C5, R27-C6, R27-C9_

  - [ ] 35.6 Admin Portal — Dashboard と SDLC Workflow 画面の実装
    - メインダッシュボードを実装: 統計、アクティブパイプラインビュー、モードセレクター、エージェントステータス
    - Requirements 画面を実装: 生入力、AI分析、構造化出力、承認/却下
    - Design 画面を実装: アーキテクチャビュー、データモデル、API spec、トレーサビリティ
    - Task ボードを実装: カンバンビュー、依存関係の可視化、工数/優先度ラベル
    - Testing 画面を実装: テストレポート、カバレッジリング、品質ゲート、重要度バッジ
    - Deployment 画面を実装: 環境カード、CI/CD パイプラインステップ、モニタリング、承認ゲート
    - Knowledge 画面を実装: セマンティック検索、ドキュメント一覧、統計、Project Brain タブ
    - Project Health 画面を実装: スコア、RADIO レポート、リスクアラート、メトリクス
    - Sentiment 画面を実装: 分布チャート、エスカレーション一覧、設定、手動エスカレーション
    - **GUI Reference:** `gui/` のすべてのファイルが実装の決定的なビジュアルリファレンス
    - _Requirements: R4-C1, R4-C2, R10-C6, R11-C9, R38-C1_

  - [ ]* 35.7 API Gateway と Admin Portal のユニットテストを記述
    - 認証ミドルウェアの適用をテスト
    - レート制限の動作をテスト
    - RBAC スコープ別のポータル可視性をテスト
    - _Requirements: R37-C1, R28-C5, R38-C7_

- [ ] 36. パフォーマンス、スケーラビリティ、可用性
  - [ ] 36.1 水平スケーリングサポートの実装
    - インスタンス追加後60秒以内の自動負荷分散を実装
    - パイプラインが容量を超えた場合の優先度ベースキューを実装
    - グレースフルシャットダウンを実装（最大120秒）
    - コンポーネント分離を設定（1つの障害が他をクラッシュさせない）
    - _Requirements: R29-C3, R29-C4, R30-C3, R30-C4_

  - [ ] 36.2 バックアップ、リカバリー、耐久性の実装
    - 自動バックアップを実装（RPO ≤ 1時間）
    - ポイントインタイムリカバリーを実装（RTO ≤ 15分）
    - コミット済みアーティファクトのデータ損失ゼロを保証
    - ディザスタリカバリー手順を実装
    - _Requirements: R31-C1, R31-C2, R31-C3, R31-C5_

  - [ ]* 36.3 パフォーマンステストを記述
    - 10個の同時パイプラインで ≤ 20% レイテンシ劣化をテスト
    - Context Store の read < 500ms、write < 2000ms（< 50MB）をテスト
    - パイプライン初期化 < 5秒をテスト
    - Knowledge Store の検索 < 200ms をテスト
    - _Requirements: R28-C1, R28-C2, R28-C3, R28-C4, R17-C1_

- [ ] 37. セキュリティ強化
  - [ ] 37.1 セキュリティ対策の実装
    - すべての通信に TLS 1.2+ を適用
    - すべてのエージェント出力で認証情報スキャンを実装
    - エージェント別認証資格情報を実装
    - 保存時および転送時のデータ暗号化を保証
    - _Requirements: R33-C1, R33-C6, R6-C6, R15-C5_

  - [ ]* 37.2 セキュリティテストを記述
    - TLS 適用をテスト
    - 未認証アクセスの拒否をテスト
    - コード出力における認証情報検出をテスト
    - RBAC 境界の適用をテスト
    - _Requirements: R33-C1, R33-C6, R15-C5, R34-C3_

- [ ] 38. 統合配線と E2E テスト
  - [ ] 38.1 すべてのコンポーネントの接続 — フルパイプラインフロー
    - 接続: API Gateway → Orchestrator → Agents → Context Store → Git
    - 接続: Agents → Model Router → AI Providers
    - 接続: Orchestrator → Review Gate → Notification
    - 接続: Agents → Knowledge Store / FAQ Store / Project Brain
    - 接続: Orchestrator → RADIO Generator
    - 接続: すべてのサービス → Observability Stack (logging, tracing, metrics)
    - _Requirements: R12, R13, R14, R15, R16, R17, R18, R21, R24, R36_

  - [ ]* 38.2 統合テストを記述 — Agent ↔ Context Store ↔ Git
    - アーティファクト書き込み → git commit → アーティファクト読み取りのラウンドトリップをテスト
    - バージョンから commit-sha へのマッピングをテスト
    - _Requirements: R15, R16-C11_

  - [ ]* 38.3 統合テストを記述 — Pipeline フロー
    - フル Spec Mode パイプラインをテスト: requirements → design → UIUX → tasks → code → test → deploy
    - Fix Mode パイプラインをテスト: bug report → 5WHY → fix → regression test
    - Vibe Mode パイプラインをテスト: prompt → code → basic validation
    - セッション中のモード切替時のコンテキスト保持をテスト
    - _Requirements: R13-C2, R13-C3, R13-C4, R13-C5_

  - [ ]* 38.4 統合テストを記述 — Review gate とエスカレーションフロー
    - レビュー承認 → パイプライン再開をテスト
    - レビュー却下 → エージェント再作業をテスト
    - 3回却下 → パイプラインブロックをテスト
    - Sentiment エスカレーション → SKU チケット作成をテスト
    - クォータ超過 → 新規リクエストブロック、実行中パイプライン完了をテスト
    - _Requirements: R14, R11-C3, R11-C4, R35-C3_

  - [ ]* 38.5 E2E テストを記述 — マルチエージェント同時パイプライン
    - 10個の同時パイプラインをテスト（パフォーマンス SLA）
    - 共有 Context Store を用いたマルチエージェント連携をテスト
    - すべてのチャネルでの通知配信をテスト
    - _Requirements: R28-C1, R29-C1, R51_

- [ ] 39. Final Checkpoint - すべてのテストが通ることを確認
  - すべてのテストが通ることを確認し、質問があればユーザーに確認する。

## Notes

- `*` マークのタスクはオプションであり、MVP の高速化のためにスキップ可能
- 各タスクはトレーサビリティのため特定の requirements を参照
- **GUI Reference**: `gui/` の HTML プロトタイプがすべての UI 関連タスク（主にセクション35）のビジュアルリファレンス。実装はプロトタイプに示されたレイアウト、ナビゲーション、データ表示パターンに合わせるべき。
- Checkpoint により段階的な検証を確保
- プロパティテストはユニバーサルな正確性プロパティを検証（設計からの25プロパティ）
- ユニットテストは特定の例とエッジケースを検証
- 統合テストはコンポーネント間の相互作用を検証
- すべてのコードは TypeScript; PBT は `fast-check` ライブラリを使用
- データベース: PostgreSQL; キャッシュ/キュー: Redis + RabbitMQ; Vector DB: Qdrant; ストレージ: S3
- Design decisions D1-D10 が全体を通じて技術選択に影響

## Task Dependency Graph

```json
{
  "waves": [
    { "id": 0, "tasks": ["1.1"] },
    { "id": 1, "tasks": ["1.2", "1.3", "1.4", "1.5"] },
    { "id": 2, "tasks": ["3.1", "5.3"] },
    { "id": 3, "tasks": ["3.2", "4.1", "5.1", "5.2"] },
    { "id": 4, "tasks": ["3.3", "3.4", "4.2", "4.3", "5.4", "5.5"] },
    { "id": 5, "tasks": ["6.1", "6.4", "10.1", "18.1"] },
    { "id": 6, "tasks": ["6.2", "6.3", "10.2", "18.2", "19.1"] },
    { "id": 7, "tasks": ["6.5", "6.6", "10.3", "18.3", "18.4", "19.2", "19.3"] },
    { "id": 8, "tasks": ["8.1", "9.1", "11.1", "20.1", "30.4"] },
    { "id": 9, "tasks": ["8.2", "9.2", "11.2", "11.3", "20.2", "20.3"] },
    { "id": 10, "tasks": ["8.3", "8.4", "9.3", "9.4", "13.1", "14.1"] },
    { "id": 11, "tasks": ["13.2", "14.2", "14.3", "15.1", "16.1", "16.2"] },
    { "id": 12, "tasks": ["15.2", "15.3", "16.3", "21.1"] },
    { "id": 13, "tasks": ["21.2", "21.3", "21.4"] },
    { "id": 14, "tasks": ["23.1", "23.2", "23.3"] },
    { "id": 15, "tasks": ["23.4", "24.1", "24.2", "24.3"] },
    { "id": 16, "tasks": ["24.4", "25.1", "25.2", "25.3"] },
    { "id": 17, "tasks": ["25.4", "27.1", "27.2", "27.3"] },
    { "id": 18, "tasks": ["27.4", "28.1", "28.2", "28.3", "28.4"] },
    { "id": 19, "tasks": ["28.5", "28.6", "30.1", "30.2", "30.3"] },
    { "id": 20, "tasks": ["30.5", "31.1", "31.2", "31.3"] },
    { "id": 21, "tasks": ["31.4", "32.1", "32.2", "33.1"] },
    { "id": 22, "tasks": ["32.3", "33.2", "35.1", "35.2"] },
    { "id": 23, "tasks": ["35.3", "35.4", "35.5", "35.6"] },
    { "id": 24, "tasks": ["35.7", "36.1", "36.2", "37.1"] },
    { "id": 25, "tasks": ["36.3", "37.2", "38.1"] },
    { "id": 26, "tasks": ["38.2", "38.3", "38.4", "38.5"] }
  ]
}
```
