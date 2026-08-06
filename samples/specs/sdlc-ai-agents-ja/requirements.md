# Requirements Document

## Introduction

**SDLC AI Agents** システムは、ソフトウェア開発ライフサイクル（Software Development Life Cycle）の各工程を専門的に担当する複数のAI Agentで構成されるプラットフォームです。各agentは独立して、または順次・並列に連携して動作し、ソフトウェア開発チームの速度向上、品質向上、およびリスク低減を支援します。

サポートされる工程：要件分析、システム設計、Task List生成、プログラミング、テスト、デプロイ、運用・監視。

専門agentに加え、システムには以下が含まれます：Orchestrator（pipeline調整）、Context_Store（Artifact共有）、Knowledge_Store（RAG用vector databaseナレッジベース）、FAQ_Store（よくある質問への直接回答）、Sentiment_Agent（感情分析とサポートへの転送）、多階層権限管理システム（Organization/Team/Project）、AIプロンプトログ記録、AI品質評価（benchmarking）、Admin Portal集中管理、および自動管理メカニズム（RADIO reporting、Change Request、Q&A、Deliverables/Limitations、Definition of Done、Approve Checklist、Self-Review Loop）。

---

## Glossary

- **SDLC_System**: ソフトウェア開発ライフサイクルにおける全AI Agentを管理・調整する統合システム。
- **Orchestrator**: 中央調整Agent。専門agentの起動、割り当て、追跡を担当。
- **Requirements_Agent**: ユーザー入力からソフトウェア要件を分析・統合する専門Agent。
- **Design_Agent**: システム設計ドキュメント（アーキテクチャ、データモデル、API）を作成する専門Agent。
- **Task_Agent**: 設計ドキュメントを具体的な実装taskリストに分解する専門Agent。優先順位とdependencyを含む。
- **UIUX_Agent**: 設計ドキュメントからHTML prototype（wireframe/mockup）を生成する専門Agent。stakeholderがブラウザ上で直感的にUIを確認・承認可能。
- **Coding_Agent**: 承認済み設計ドキュメントに基づいてソースコードを生成する専門Agent。
- **Testing_Agent**: テストケースの作成、テスト実行、結果報告を行う専門Agent。
- **Deployment_Agent**: CI/CDプロセスの実行とターゲット環境へのアプリケーションデプロイを担当する専門Agent。
- **Operations_Agent**: 運用中のアプリケーション監視、インシデント警告、解決策の提案を行う専門Agent。
- **Sentiment_Agent**: 各promptでユーザーの感情を分析するために並列実行されるAgent。不満・怒りを検出し、エキスパートサポートへの転送を起動。
- **ProjectHealth_Agent**: プロジェクトの健全性（進捗、品質、リスク、コスト、velocity）をシステム内の全データソースから自動集計・報告するAgent。
- **Artifact**: agentの出力成果物（要件ドキュメント、設計ドキュメント、task list、ソースコード、テストレポートなど）。
- **Pipeline**: 開発目標を達成するために順次または並列に実行されるagentのチェーン。
- **Human_Reviewer**: Artifactの確認と承認を行う開発チームメンバー。
- **Context_Store**: agent間でコンテキストとArtifactを共有する中央ストレージ。
- **Knowledge_Store**: 企業・プロジェクトの内部知識をembeddings形式で保存するvector database。semantic searchをサポートし、agentがコンテキストに基づいて情報を検索可能（RAG pattern）。
- **RAG (Retrieval-Augmented Generation)**: AIがoutputを生成する前にナレッジベースからコンテキストをpromptに補足する技術。特定のコンテキストでより正確な回答を実現。
- **FAQ_Store**: 事前承認済みの質問-回答ペアのストレージ。semantic matchingによりAIモデルを呼び出さずに直接回答を提供。
- **SKU (Software Knowledge Utilization)**: 外部エキスパートのナレッジ管理・サポートシステム。AI Agentがユーザーのニーズに対応できない場合にissueを受け入れる。
- **Template_Registry**: テスト済み標準ソースコード（baseline templates）のストレージ。project scaffolds、feature modules、architecture patternsを含む。
- **Doc_Template_Registry**: プロセスドキュメント（requirements、design、tasks）およびシステム設定（skills、hooks、steering）の標準テンプレートストレージ。
- **Eval_Store**: AI品質評価テストスイート（evaluation test suites）のストレージ。input/ground truth/scoring criteriaを含み、agentのoutput品質のbenchmarkと検収に使用。
- **Project_Brain**: プロジェクトメモリ（second brain）。作業中にプロジェクト固有の知識を自動蓄積 — artifacts、decision logs、relationships、terminologyを含む — Knowledge_Store（企業共通知識）とは分離。
- **RADIO**: AIが実行データから自動生成する5要素の進捗報告フレームワーク（Review、Action、Difficulty、Information、Outcome）。
- **DoD (Definition of Done)**: 各artifact/phaseの必須完了基準。AIが「Done」とマークする前に自動チェック。
- **CR (Change Request)**: client/stakeholderからの変更要求。ライフサイクル管理：Pending → Approved → In Progress → Review → Done。
- **5WHY (Five Whys)**: bug/issueの根本原因（root cause）を見つけるために「なぜ？」を連続5回質問する手法。

---

## Requirements

### Requirement 1: Agentの初期化と管理

**User Story:** ソフトウェアエンジニアとして、SDLCの各工程に対してAI Agentを初期化・設定したい。チームのプロセスに合わせてシステムをカスタマイズするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 初期定義済みの9つの役割（Requirements_Agent、Design_Agent、UIUX_Agent、Task_Agent、Coding_Agent、Testing_Agent、Deployment_Agent、Operations_Agent、ProjectHealth_Agent）のいずれかに属するagentの登録メカニズムを提供する — R32-C1に基づくplugin mechanismにより新しい役割の追加が可能 — 必須フィールド：名前（最大100文字）、役割、使用AIモデル；オプションフィールド：追加設定パラメータ。
2. WHEN agentが正常に登録された場合、THE SDLC_System SHALL システム全体で一意の識別子（agent_id）を割り当て、ステータスを「active」と確認する。
3. IF agentの設定に必須フィールド（名前、役割、AIモデル）のいずれかが不足している場合、THEN THE SDLC_System SHALL 不足している各フィールドを具体的に列挙したエラーメッセージを返し、agentを登録しない。
4. IF agentの設定が更新される際にtaskが実行中の場合、THEN THE SDLC_System SHALL 変更を記録し、そのagentの現在のtask完了後に新しい設定を適用する。
5. WHEN agentが無効化（deactivated）される場合、THE SDLC_System SHALL agentのステータスを「inactive」に変更し、新しいタスクの受付を停止し、実行中のtaskはdeactivate発効前に正常完了を許可する。
6. IF 新しいagentが「active」状態の既存agentと同じ名前・役割で登録される場合、THEN THE SDLC_System SHALL 登録を拒否し、競合エラーメッセージを返す。
7. THE SDLC_System SHALL 各agentにtemperatureパラメータ（0.0〜2.0の値）の設定を許可する；WHEN temperature = 0の場合、agent SHALL deterministic output（同じ入力で同じ出力）を生成する；WHEN temperature > 0の場合、agent SHALL temperature値に比例してより多様なoutputを許可する；IF 設定されない場合、THEN temperature SHALL デフォルト0.7とする。

---

### Requirement 2: 要件分析Agent (Requirements_Agent)

**User Story:** Business Analystとして、AIに生の記述から要件を自動分析・構造化してほしい。要件ドキュメント作成時間を短縮するため。

#### Acceptance Criteria

1. WHEN 生の要件記述（raw requirement description）が提供された場合、THE Requirements_Agent SHALL 分析を行い、必須セクション（タイトル、詳細説明、分類タイプ、少なくとも1つのacceptance criteria）を含む構造化された要件ドキュメントを60秒以内に作成する。
2. THE Requirements_Agent SHALL 各要件をfunctional、non-functional、またはconstraintのいずれかに分類する。
3. IF 要件が複数のタイプに該当する可能性がある場合、THEN THE Requirements_Agent SHALL 最も適切なタイプを選択し、分類理由を要件に添付する。
4. WHEN 入力要件に2つ以上の条件間で論理的矛盾が含まれる場合、THE Requirements_Agent SHALL 矛盾する要件にフラグを付ける：各競合要件の識別子と矛盾する条件の具体的な記述を含む。
5. THE Requirements_Agent SHALL 各要件を少なくとも1つの測定可能なacceptance criteriaに紐付ける。各基準にはactor、action、observable outcomeを含む必要がある。
6. IF 入力に少なくとも1つの完全な要件（タイトル＋説明＋少なくとも1つのacceptance criteria）を作成するのに十分な情報が含まれていない場合、THEN THE Requirements_Agent SHALL Human_Reviewerに対する明確化質問のリストを返す。
7. WHEN 要件ドキュメントが保存または更新される場合、THE Requirements_Agent SHALL Context_StoreにArtifactを前バージョンから1増加した整数バージョン番号で記録する。

---

### Requirement 3: システム設計Agent (Design_Agent)

**User Story:** Software Architectとして、AIに要件ドキュメントから設計ドキュメントを自動生成してほしい。初期アーキテクチャ設計時間を短縮するため。

#### Acceptance Criteria

1. WHEN 承認済み要件ドキュメントが提供された場合、THE Design_Agent SHALL アーキテクチャ図（architecture diagram）、データモデル（data model entities）、API仕様（API endpoints）を含む設計ドキュメントを120秒以内に作成する。
2. WHEN 設計ドキュメントが作成完了した場合、THE Design_Agent SHALL 要件ドキュメントで「functional」とラベル付けされたすべての要件が少なくとも1つの設計コンポーネント（architecture diagramのnamed element、data model entity、またはAPI endpoint）にマッピングされていることを保証する。
3. WHEN 要件ドキュメントのバージョンが変更された場合、THE Design_Agent SHALL 変更を検出し、120秒以内に対応する設計ドキュメントを更新する。影響を受ける各セクションに「CHANGED」アノテーションと変更されたコンポーネント名のリストを付与する。
4. IF 入力要件ドキュメントがHuman_Reviewerにより承認されていない場合（ステータスが「approved」以外）、THEN THE Design_Agent SHALL 処理を拒否し、承認を要求するメッセージを返す。
5. WHEN 設計ドキュメントが正常に作成された場合、THE Design_Agent SHALL 入力要件ドキュメントのバージョンに対応するバージョンでContext_StoreにArtifactとして保存する。

---

### Requirement 4: UI/UX設計Agent (UIUX_Agent)

**User Story:** Product Designerとして、AIに設計ドキュメントからHTML prototypeを自動生成してほしい。コーディング開始前にstakeholderがブラウザ上でUIを確認・承認できるようにするため。

#### Acceptance Criteria

1. WHEN ステータス「approved」の設計ドキュメントが提供された場合、THE UIUX_Agent SHALL HTML prototypeを生成する：各screen/pageに1つのHTMLファイル、Bootstrap（デフォルト）または設定済みCSS frameworkを使用、responsive layout（mobile/tablet/desktop）。
2. THE UIUX_Agent SHALL screen間のnavigation linksを作成し、ユーザーがbackendなしでブラウザ上で直接クリックしてuser flowをテストできるようにする。
3. THE UIUX_Agent SHALL 各screenのspec Markdownファイルを付随生成する。内容：interaction logic（click、hover、form submit behavior）、states（loading、empty、error、success）、edge cases（long text、no data）、accessibility notes（ARIA labels、keyboard navigation）。
4. THE UIUX_Agent SHALL prototypeがDesign_Agentのdata modelに準拠していることを保証する：設計ドキュメントに記述されたfields、data types、relationshipsを正確に表示。
5. THE UIUX_Agent SHALL prototypeをstatic HTML + CSS（plaintext、R16-C2に基づきGitでdiff可能）として生成する；複雑なJavaScriptやbuild toolsは使用しない — prototypeはサーバーなしでブラウザから直接開けること。
6. THE SDLC_System SHALL プロジェクトごとのデフォルトCSS frameworkの設定を許可する：Bootstrap（default）、TailwindCSS、Material UI、またはcustom CSS；IF 設定なしの場合、THEN Bootstrap latest stable versionを使用する。
7. WHEN Human_Reviewerがprototypeをレビューする場合、THE SDLC_System SHALL 各screen上で直接アノテーション（要素ごとまたはエリアごとのコメント）を許可し、UIUX_Agentが修正できるようにする；annotationsはartifactと共に保存される。
8. THE UIUX_Agent SHALL outputを対応するfeature specフォルダ内の標準ディレクトリ構造で整理する（R16-C7に基づく）：
   ```
   /docs/specs/{feature-name}/ui/
   ├── index.html          — landing/home + 全screensへのnavigation
   ├── screens/            — 各screen (*.html)
   ├── specs/              — screen毎のinteraction specs (*.md)
   ├── assets/             — images, icons等
   └── design-tokens.json  — colors, spacing, typography tokens
   ```
9. WHEN prototypeが正常に作成された場合、THE UIUX_Agent SHALL 入力設計ドキュメントのバージョンに対応するバージョンでContext_StoreにArtifactとして保存する。
10. THE Task_Agent SHALL UI prototypeをtask list生成時の追加入力として使用する：prototype内の各screen/componentは少なくとも1つの具体的なfrontend taskにマッピングされる。

---

### Requirement 5: Task List生成Agent (Task_Agent)

**User Story:** Tech Leadとして、AIに設計ドキュメントを具体的な実装taskリストに自動分解してほしい。チームが手動breakdownに時間をかけずにすぐコーディングに着手できるようにするため。

#### Acceptance Criteria

1. WHEN ステータス「approved」の設計ドキュメントが提供された場合、THE Task_Agent SHALL 実装taskリストに分解する。各taskに含む内容：タイトル、詳細説明、acceptance criteria、estimated effort（S/M/L/XL）、priority（P0-P3）、dependencies（先に完了すべきtask）、suggested assignee role。
2. THE Task_Agent SHALL 設計ドキュメント内のすべてのコンポーネント（architecture components、data models、API endpoints）が少なくとも1つのtaskでカバーされていることを保証する；IF taskのないコンポーネントを検出した場合、THEN 補足taskを自動作成する。
3. THE Task_Agent SHALL dependency graphに基づいて論理的な実行順序でtasksを並べ替える：dependencyのないtasksは並列実行可能、dependencyのあるtasksは依存taskの後に実行する。
4. THE Task_Agent SHALL tasksをfeature/moduleごとにグループ化し、対応するlabelsを付与してフィルタリングと進捗追跡を容易にする。
5. IF 設計ドキュメントに単一taskでは大きすぎるコンポーネント（estimated effort > XLまたは > 5人日）が含まれる場合、THEN THE Task_Agent SHALL 内部dependencyを持つsub-tasksに自動分割する。
6. WHEN task listが正常に作成された場合、THE Task_Agent SHALL Context_StoreにArtifactとして保存する。メタデータ：対応する設計バージョン、総task数、effort分布、critical path（タイムラインを決定する最長taskチェーン）。
7. THE Task_Agent SHALL 設計ドキュメントのバージョン変更時にtask listの更新をサポートする：変更されたコンポーネントを検出し、対応するtasksの追加/修正/削除を行い、影響を受けるtasksに「UPDATED」アノテーションを付与する。

---

### Requirement 6: プログラミングAgent (Coding_Agent)

**User Story:** Developerとして、AIに設計ドキュメントからソースコードを生成してほしい。新機能の実装速度を向上させるため。

#### Acceptance Criteria

1. WHEN ステータス「approved」の設計ドキュメントが提供された場合、THE Coding_Agent SHALL 設定済みプログラミング言語で設計ドキュメントに記述された各コンポーネントのソースコードを生成する。
2. WHEN ソースコードが生成された場合、THE Coding_Agent SHALL 作成された各public function/methodのunit testを付随生成し、生成コードのcode coverage最低80%を保証する。
3. WHEN ソースコードが生成された場合、THE Coding_Agent SHALL 設定済みlintingルールセットに基づいてコードを検査する；IF コードがlinting検査に不合格の場合、THEN THE Coding_Agent SHALL lintingエラーを自動修正し、pass済みコードのみを出力する；IF auto-fixでlintingエラーを解決できない場合（例：complexity limit違反）、THEN 残存エラーを説明付きでフラグし、Human_Reviewerに通知する。
4. IF 設計ドキュメントにコード生成に十分な情報がないコンポーネントが含まれる場合（input/output types不足、business logic description不足、またはAPI contract不足）、THEN THE Coding_Agent SHALL そのコンポーネントにフラグを付け、不足情報リストと共にDesign_Agentに仕様補足を要求する。
5. WHEN ソースコードが保存される場合、THE Coding_Agent SHALL Context_Storeにメタデータ付きで記録する：プログラミング言語、対応する設計バージョン、作成ファイルリスト、timestamp。
6. FOR ALL 生成されたソースコード、THE Coding_Agent SHALL ハードコードされた認証情報（credentials、API keys、passwords）が含まれていないことをスキャン・保証する；IF 検出した場合、THEN 環境変数への参照に置換する。

---

### Requirement 7: テストAgent (Testing_Agent)

**User Story:** QA Engineerとして、AIにテストケースの自動作成・実行をしてほしい。デプロイ前のソフトウェア品質を保証するため。

#### Acceptance Criteria

1. WHEN ソースコードと要件ドキュメントが提供された場合、THE Testing_Agent SHALL unit test、integration test、acceptance test（各要件のacceptance criteriaに基づくテスト）を含むテストスイートを作成する。
2. WHEN テストスイートが作成完了した場合、THE Testing_Agent SHALL 全テストを実行し、結果レポートを作成する。内容：各test case名、テストタイプ（unit/integration/acceptance）、pass/failステータス、code coverage率、failしたケースの詳細エラー説明。
3. WHEN テスト結果に少なくとも1つのfailがある場合、THE Testing_Agent SHALL 各エラーを重大度で分類する：critical（クラッシュまたはデータ損失を引き起こす）、major（主要機能が正しく動作しない）、minor（UI/cosmeticエラーでロジックに影響なし）。
4. IF テストスイートのpass率が90%未満の場合、THEN THE Testing_Agent SHALL ArtifactのDeployment_Agentへの移行をブロックし、Human_Reviewerに通知する：実際のpass率、fail数、criticalエラーのリスト。
5. WHEN ソースコードにparser/serializerコンポーネントが含まれる場合、THE Testing_Agent SHALL round-trip特性を検査する：全ての有効なオブジェクトに対して、serializeしてdeserializeした結果が元のオブジェクトと全ての値を持つ属性でdeep-equalであること。
6. WHEN テスト実行プロセスが完了した場合、THE Testing_Agent SHALL Context_Storeにテストレポートを保存する。全体ステータス：「passed」（pass率 ≥ 90%の場合）または「failed」（pass率 < 90%の場合）。

---

### Requirement 8: デプロイAgent (Deployment_Agent)

**User Story:** DevOps Engineerとして、AIにCI/CDプロセスとアプリケーションデプロイを自動実行してほしい。リリースプロセスにおける手動エラーを削減するため。

#### Acceptance Criteria

1. WHEN ステータス「passed」のテストレポートが確認された場合、THE Deployment_Agent SHALL ターゲット環境（development、staging、production）の設定に基づいてCI/CD pipelineを起動する。
2. WHEN デプロイが完了した場合、THE Deployment_Agent SHALL 最大300秒間、10秒ごとにhealth checkリクエストを送信する；IF 少なくとも1つのhealth checkがHTTP 200を返した場合、THEN デプロイは成功とみなす；IF 300秒後にすべてのhealth checkが失敗した場合、THEN THE Deployment_Agent SHALL 前バージョンへのrollbackを実行する。
3. IF デプロイ環境が「production」の場合、THEN THE Deployment_Agent SHALL デプロイ実行前にHuman_Reviewerからの明示的な確認を要求する；IF Human_Reviewerが60分以内に応答しない場合、THEN THE Deployment_Agent SHALL デプロイをキャンセルしてtimeoutを通知する。
4. THE Deployment_Agent SHALL pipeline実行の全ステップをログに記録する：開始時刻（ISO 8601）、終了時刻、各ステップのステータス（success/failed/skipped）、エラーメッセージ（ある場合）。
5. WHEN デプロイが正常に完了した場合、THE Deployment_Agent SHALL Context_Storeを更新する：デプロイ済みバージョン、ターゲット環境、デプロイ時刻（ISO 8601）。
6. IF rollbackも失敗した場合（rollback後のhealth checkが300秒以内に成功しない）、THEN THE Deployment_Agent SHALL ステータスを「critical_failure」とマークし、Human_Reviewerに緊急アラートを送信し、原因の詳細をログに記録する。

---

### Requirement 9: 運用・監視Agent (Operations_Agent)

**User Story:** Site Reliability Engineerとして、AIにアプリケーションを継続的に監視しインシデントを警告してほしい。production環境の問題に迅速に対応するため。

#### Acceptance Criteria

1. WHILE アプリケーションが運用中の場合、THE Operations_Agent SHALL メトリクスを収集する：CPU utilization (%)、memory utilization (%)、response latency (ms)、error rate (%)。周期は60秒以内。
2. WHEN メトリクスが設定された「warning」レベルの警告閾値を超えた場合（インシデントレベルには達していない）、THE Operations_Agent SHALL 検出から30秒以内に設定された通知チャネルに「warning」レベルのアラートを送信する；IF 通知チャネルが利用不可の場合、THEN 10秒間隔で最大3回リトライし、アラート送信エラーをログに記録する。
3. WHEN インシデントが検出された場合（定義：error rateが「critical」レベルの閾値を超過、またはサービスが3連続周期でhealth checkに応答しない — C2のwarningアラートとは異なりインシデントはroot cause分析が必要）、THE Operations_Agent SHALL 直近60分のシステムログを分析し、少なくとも1つの修正策を具体的なアクションステップ（plain-language steps）として提案する。confidence score（0.0〜1.0）を付与。
4. IF 連続5分間のエラー率が5%を超えた場合、THEN THE Operations_Agent SHALL 「critical」レベルのアラートを自動起動し、60秒以内にHuman_Reviewerに通知する。
5. THE Operations_Agent SHALL メトリクスとインシデントの履歴をContext_Storeに最低30日間保存する。30日以上経過したデータは設定されたポリシーに基づいてarchiveまたは削除可能。

---

### Requirement 10: プロジェクト健全性追跡Agent (ProjectHealth_Agent)

**User Story:** Project Managerとして、AIにプロジェクトの健全性（進捗、品質、リスク、コスト）を自動集計・報告してほしい。複数ソースから手動でデータ収集することなく、迅速に全体像を把握し早期に問題を発見するため。

#### Acceptance Criteria

1. THE ProjectHealth_Agent SHALL システム内の全ソースからデータを自動収集・集計する：task status（Task_Agent — done/in_progress/blocked）、test results（Testing_Agent — pass rate、coverage）、pipeline status（Orchestrator — success/failed/running）、Git activity（commits/merges per day）、evaluation scores（Eval_Store）、user feedback（positive/negative ratio）、sentiment escalations、token usage/cost、review gate wait times。
2. THE ProjectHealth_Agent SHALL 3レベルの定期自動レポートを作成する：(a) **Daily Summary** — 今日の活動要約（tasks completed、commits、issues）、(b) **Weekly Report** — 週間進捗、velocity、blockers、quality trend、(c) **Sprint/Release Report** — sprint/release総括と完全なmetrics。レポートの頻度と送信タイミングはプロジェクトごとにconfigurable。
3. THE ProjectHealth_Agent SHALL プロジェクト健全性指標を計算・表示する：**Progress Score**（% tasks done vs planned）、**Quality Score**（加重平均：test pass rate、AI eval score、feedback rating）、**Risk Score**（根拠：blocked tasks、overdue reviews、sentiment escalations、quality regressions）、**Velocity**（tasks/story points completed per sprint）、**Cost Efficiency**（token cost per task completed）。
4. WHEN Risk Scoreが設定された警告閾値を超えた場合（デフォルト > 7/10）、THE ProjectHealth_Agent SHALL 「project_at_risk」アラートをProject ManagerとTech Leadに自動送信する：具体的なrisk factors、impact assessment、suggested actionsを含む。
5. THE ProjectHealth_Agent SHALL 早期リスク徴候をproactiveに検出・警告する：(a) velocityが前2 sprintと比べて > 20%低下、(b) test pass rateが7日間で > 10%低下、(c) blocked tasksが同時に3を超過、(d) average review wait timeが > 24時間、(e) budget usageが60%の時間経過時点で > 80%のquota使用。
6. THE ProjectHealth_Agent SHALL Project Health Dashboardを提供する：timeline進捗（planned vs actual）、burn-down/burn-up chart、quality trend over time、risk radar chart、cost burn rate、team productivity metrics — すべてプロジェクトごとにconfigurable。
7. THE ProjectHealth_Agent SHALL 自然言語クエリをサポートする：PMが質問可能（例：「このsprintの進捗は？」「どのtaskがblockされている？」「今週のcode品質は先週と比べてどう？」）で、実際のデータに基づいた要約回答を受け取る。
8. THE ProjectHealth_Agent SHALL 生成した全レポートをContext_Store（R16-C2に基づくMarkdown format）およびGit repositoryに保存し、レポート履歴の閲覧と期間間の比較を可能にする。
9. THE SDLC_System SHALL プロジェクトごとのレポート送信チャネル設定を許可する：email、Slack、Teams、またはdashboard上の表示のみ；各レポートタイプは異なる受信者グループに送信可能。
10. THE SDLC_System SHALL プロジェクトごとにProjectHealth_Agentのenable/disableを許可する；WHEN disabled時、データは引き続き収集される（他のdashboard用）が、自動レポート生成とalert送信は行わない。

---

### Requirement 11: 感情分析・サポート転送Agent (Sentiment_Agent)

**User Story:** Product Ownerとして、AI Agentとのインタラクション中にユーザーのネガティブ感情をシステムに自動検知してほしい。AIがニーズに対応できない場合にエキスパートサポートへ適時転送するため。

#### Acceptance Criteria

1. THE SDLC_System SHALL Sentiment_Agentを提供する。各ユーザーprompt入力に対して感情分析（sentiment analysis）を実行し、positive、neutral、frustrated、angryのいずれかに分類する。confidence score（0.0〜1.0）付き。
2. THE Sentiment_Agent SHALL メインagentと並列（non-blocking）で実行される；感情分析結果は各interactionのmetadataに付加され、応答latencyを100ms以上増加させない。
3. WHEN Sentiment_Agentが同一セッション内の同一ユーザーの連続2回のインタラクションで confidence ≥ 0.7の「frustrated」または「angry」を検出した場合、THE SDLC_System SHALL escalationメカニズムを起動する。
4. WHEN escalationメカニズムが起動された場合、THE SDLC_System SHALL：(a) ユーザーにリクエストをエキスパートへ転送中であることを通知する、(b) SKUシステムにissueを自動作成する：問題要約、インタラクション履歴、sentiment timeline、処理中のagent_id、sentiment severityに基づくpriorityを含む。
5. THE SDLC_System SHALL 設定可能なAPIを通じてSKUシステムと統合する；IF SKUが利用不可の場合、THEN issueをqueueに入れ、60秒間隔で最大3回リトライし、同時に設定されたアラートチャネルにfallback通知を送信する。
6. THE SDLC_System SHALL 各agentまたはシステム全体のescalation閾値設定を許可する：escalate前の連続frustrated/angry回数（デフォルト2）、最低confidence レベル（デフォルト0.7）、ユーザーによる手動escalationリクエストの許可（デフォルト：許可）。
7. THE SDLC_System SHALL ユーザーがいつでも主体的にエキスパートへの転送を要求することを許可する（manual escalation）；システムはC4と同様のSKU issueをpriority「user_requested」で作成する。
8. THE Sentiment_Agent SHALL 各interactionの感情分析結果をContext_Storeに保存する：user_id、session_id、timestamp、sentiment label、confidence score、escalation_triggered（true/false）；データは最低90日間保持。
9. THE SDLC_System SHALL sentiment分析ダッシュボードを提供する：agent/team/project/期間別集計。内容：sentiment分布、escalation率、escalateまでの平均時間、特定agent/modelとネガティブsentimentの相関。
10. THE SDLC_System SHALL 各agentまたはシステム全体でSentiment_Agentのenable/disableを許可する；WHEN disabled時、感情分析は行わないが手動escalation（C7）は引き続き許可する。

---

### Requirement 12: Pipeline調整 (Orchestrator)

**User Story:** Engineering Managerとして、SDLCプロセスに沿ってagentをシステムに自動調整してほしい。最小限の介入で開発pipelineをエンドツーエンドで実行できるようにするため。

#### Acceptance Criteria

1. THE Orchestrator SHALL 2つのモードでPipeline設定をサポートする：sequential（順次 — 各ステップは前のステップ完了後に実行）およびconditional parallel（条件付き並列 — ユーザーが設定したboolean条件式に基づいてブランチが起動される。例：`artifact.status == "passed" AND target.env != "production"`）。
2. WHEN Pipelineのステップが失敗した場合、THE Orchestrator SHALL 直接依存するステップを停止し、失敗原因を記録し、60秒以内にHuman_Reviewerに通知する；独立ブランチ（失敗ステップへのdependencyがない）は通常通り実行を継続する。
3. THE Orchestrator SHALL Pipeline内の各agentのステータスを5秒以内の遅延で追跡・更新する。表示ステータス：「pending」「running」「completed」「failed」「timeout」。
4. WHEN Pipelineの全ステップがステータス「completed」で完了した場合、THE Orchestrator SHALL Pipeline総括レポートを作成する：各ステップ名、実行agent、最終ステータス、各ステップの実行時間、pipeline合計時間。
5. IF agentがタスク割り当てから300秒以内にheartbeatまたはacknowledgementを送信しない場合、THEN THE Orchestrator SHALL そのagentを「timeout」とマークし、60秒後にリトライ（最大2回）、合計3回試行後も応答がない場合は「failed」ステータスに移行する。

---

### Requirement 13: インタラクションモード (Interaction Modes)

**User Story:** Developerとして、AI Agentでの作業開始時に適切なインタラクションモードを選択したい。目的に合わせてシステムの処理プロセスを調整するため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 新しいセッション開始時に3つのインタラクションモード（Interaction Mode）のいずれかを選択することをユーザーに要求する：**Spec Mode**（「Plan first, then build」— coding前にrequirementsとdesignを作成）、**Fix Mode**（「Investigate, then fix」— 修正前にroot causeを分析）、**Vibe Mode**（「Just code, no ceremony」— 要求から直接code生成）。
2. WHEN ユーザーがSpec Modeを選択した場合、THE SDLC_System SHALL 完全なpipelineを実行する：Requirements_Agent → (approval) → Design_Agent → (approval) → UIUX_Agent → (approval) → Task_Agent → Coding_Agent → Testing_Agent → Deployment_Agent；review gatesはR14に基づき自動起動される。
3. WHEN ユーザーがFix Modeを選択した場合、THE SDLC_System SHALL 短縮pipelineを実行する：エラー情報収集 → root cause分析 → 解決策提案 + impact analysis → （確認）→ Coding_Agentがpatch生成 → Testing_Agentがregression test実行；review gateはproduction deploy前のみ必須。
4. WHEN ユーザーがVibe Modeを選択した場合、THE SDLC_System SHALL requirements/design/taskステップをスキップし、Coding_Agentがpromptから直接code生成する；Testing_Agentはbasic validation（linting + unit test）を実行する；手動で有効にしない限りreview gatesはオフ。
5. THE SDLC_System SHALL 蓄積済みcontextを失うことなく途中でのmode切替を許可する；前のmodeで作成されたArtifactsは保持される。
6. THE SDLC_System SHALL Adminにデフォルトmodeとチームまたはプロジェクトごとの利用可能mode制限の設定を許可する（例：production-criticalプロジェクトではVibe Modeを無効化）。
7. THE SDLC_System SHALL 各セッションで選択されたmodeをメタデータ付きでログに記録する：user_id、mode、timestamp、mode changes、実際に実行されたpipeline steps。
8. THE SDLC_System SHALL セッション全体を通してインターフェースに現在のmodeを明確に表示し、適用中のプロセスの短い説明を添える。

---

### Requirement 14: 人間の承認 (Human-in-the-Loop)

**User Story:** Tech Leadとして、次の工程に進む前に重要なArtifactを確認・承認したい。品質確保とリスク管理のため。

#### Acceptance Criteria

1. THE SDLC_System SHALL Human_ReviewerがPipeline内の任意の工程間遷移に必須の検査ポイント（review gate）を設定することを許可する。
2. WHEN Artifactが検査ポイントに到達した場合、THE Orchestrator SHALL Pipelineを一時停止し、Human_Reviewerに通知する：Artifact内容、現在の工程名、Artifactを作成したagent_id、Artifact作成timestamp。
3. WHEN Human_ReviewerがArtifactを承認した場合、THE Orchestrator SHALL 確認受信後10秒以内に次のステップからPipelineを再開する。
4. WHEN Human_ReviewerがArtifactをフィードバック付きで却下した場合、THE Orchestrator SHALL Artifactとフィードバックを作成元agentに返し、再処理を要求する。
5. IF Artifactが24時間以上Human_Reviewerからの応答なしで承認待ちの場合、THEN THE SDLC_System SHALL リマインダーを送信する；IF 48時間後も応答がない場合、THEN 設定された監督者にescalate通知を送信する：artifact_idと待機時間を含む。
6. IF Artifactが同じreview gateで連続3回却下された場合、THEN THE Orchestrator SHALL そのステップでPipelineを停止し、ステータスを「blocked」とマークし、Human_Reviewerに手動介入が必要であることを通知する（無限reworkループ防止のため）。

---

### Requirement 15: Agent間コンテキスト共有 (Context_Store)

**User Story:** Developerとして、agent間で情報とArtifactを自動共有してほしい。工程間でデータを手動で受け渡す必要がないようにするため。

#### Acceptance Criteria

1. WHEN agentがArtifactを書き込む場合、THE Context_Store SHALL 一意の識別子（artifact_id、長さ1-128文字）とバージョン（version、正の整数）の組み合わせで保存する；WHEN agentがArtifactを読み取る場合、THE Context_Store SHALL 要求されたartifact_idとversionに基づいて内容を返す。
2. THE Context_Store SHALL read-after-write consistencyを保証する：同一書き込みセッション内で、書き込み成功直後の読み取り操作は書き込んだ内容を正確に返す。
3. WHEN Artifactが更新される（新version）場合、THE Context_Store SHALL 最大100の以前のバージョンを保持し、artifact_id + versionによる任意のバージョンへのアクセスを許可する；WHEN バージョン数が100を超えた場合、THE Context_Store SHALL 最も古いバージョンを自動削除（FIFO）して制限を維持する。
4. IF agentが既に存在するartifact_idとversionでArtifactを書き込もうとした場合、THEN THE Context_Store SHALL 書き込み操作を拒否し、バージョン競合エラー（version conflict）を現在のバージョン情報と共に返す。
5. THE Context_Store SHALL 未認証（unauthenticated）のArtifactアクセスを拒否しエラーを返すことを保証する；Artifactデータは有効な認証セッション外からアクセス不可。保存時（at rest）および転送時（in transit）の両方。
6. IF agentが存在しないartifact_idまたはversionの読み取りを要求した場合、THEN THE Context_Store SHALL 「not found」エラーを要求されたartifact_idとversion情報と共に返す。

---

### Requirement 16: バージョン管理とストレージ (Version Control & Git Integration)

**User Story:** Developerとして、システムの全成果物（codeとドキュメント）をGitで管理し、各完了マイルストーンで自動commitしてほしい。変更履歴を明確にし、review、rollback、collaborationを容易にするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL Gitをpipelineの全output（source codeとドキュメントを含む：requirements、design、task list、test reports、deployment logs）の唯一のバージョン管理システムとして使用する；全ArtifactはGit repository内のファイルとして保存される。
2. THE SDLC_System SHALL 全工程の出力ドキュメントをdiff可能なplaintext形式とすることを規定する：requirements（Markdown）、design documents（Markdown + Mermaid/PlantUML for diagrams）、task lists（MarkdownまたはYAML）、test reports（MarkdownまたはJSON）、deployment logs（JSON）、configuration files（YAML/JSON）；ワーキングドキュメントにbinary format（DOCX、PDF）は使用しない — 外部共有時のみbinary exportを許可。
3. WHEN agentがpromptの処理を完了した場合（agentが1つのprompt入力からoutputを生成するたび）、THE SDLC_System SHALL 変更ファイルのGit commitを自動作成する。messageフォーマット：`[agent_role] prompt summary`（例：`[coding] Add JWT token generation to AuthService`）。commit bodyにメタデータを含む：pipeline_id、task_id、agent_id、prompt_log_id（R24参照）；これによりtaskごとだけでなくpromptごとの正確なrollbackが可能。
4. WHEN sub-taskまたはtaskが完了した場合（複数promptsを含む）、THE SDLC_System SHALL 軽量Gitタグ（lightweight tag）でマイルストーンを作成する：フォーマット `task/{task_id}/done`。
5. WHEN pipeline stageが完了した場合（Requirements → Design → Tasks → Code → Test → Deploy）、THE SDLC_System SHALL Git annotated tagでマイルストーンを作成する：フォーマット `stage/{stage_name}/v{version}`（例：`stage/design/v2`、`stage/testing/v1`）。
6. THE SDLC_System SHALL session/task/sub-taskの階層に基づくbranching strategyを使用する：
   - **Pipeline branch**（session開始時に作成）：`{mode}/{feature-slug}` — 例：`spec/user-authentication`、`fix/payment-timeout`、`vibe/prototype-dashboard`。
   - **Stage branch**（各stage開始時に作成）：`{mode}/{feature-slug}/{stage}` — 例：`spec/user-authentication/design`。
   - **Task branch**（task開始時に作成）：`{mode}/{feature-slug}/{task-id}` — 例：`spec/user-authentication/task-001`。
   - **Sub-task branch**（sub-task開始時に作成）：`{mode}/{feature-slug}/{task-id}/{sub-task-id}` — 例：`spec/user-authentication/task-001/sub-001`。
   
   Merge flow: sub-task → task branch（sub-task完了時）→ pipeline branch（taskがテストpass時）→ main（pipelineがfinal review gate R14をpass時）。Stage branches（req、design）はHuman_Reviewer approved時にpipeline branchにmerge。
7. THE SDLC_System SHALL codeとドキュメントを分離した標準ディレクトリ構造でrepositoryを整理する：
   ```
   /docs/requirements/    — requirements documents (*.md)
   /docs/design/          — design documents + diagrams (*.md)
   /docs/tasks/           — task lists (*.md or *.yaml)
   /docs/specs/           — feature specs (requirements + design + tasks + radio per feature)
   /docs/bugs/            — bugfix documents (bugfix.md, design.md, tasks.md per bug)
   /docs/change-request/  — CR backlog + per-CR specs
   /docs/qa/              — Q&A index + threads
   /docs/deliverables/    — deliverables + limitations per sprint
   /docs/decisions/       — decision logs (*.md)
   /src/                  — source code
   /tests/                — test code
   /config/               — configuration files
   /reports/              — test reports, deployment logs (*.md, *.json)
   ```
8. THE SDLC_System SHALL プロジェクトごとのGit remote（GitHub、GitLab、Bitbucket、self-hosted）設定を許可する；各commitまたは設定可能なbatch（デフォルト：各task完了後にpush）でchangesをpushする。
9. WHEN Human_Reviewerがreview gateでapprove/rejectした場合、THE SDLC_System SHALL Gitに決定を記録する：approval → merge commitにメッセージ `[review] Approved by {user} - {comment}`；rejection → branch上のcommitにメッセージ `[review] Rejected by {user} - {reason}`（履歴保持のため）。
10. THE SDLC_System SHALL rollbackをサポートする：IF 任意のArtifact（codeまたはドキュメント）の前バージョンに戻す必要がある場合、THE SDLC_System SHALL 独自メカニズムではなくGit revert/checkoutを使用し、single source of truthを保証する。
11. THE SDLC_System SHALL Context_Store（R15）とGit repositoryの同期を保証する：Context_Store内の全Artifactは対応するGit commit SHAへの参照を持つ；version numberによるArtifactクエリは特定のGit commitにマッピング可能。

---

### Requirement 17: 企業ナレッジベース (Knowledge Base & Vector Store)

**User Story:** Engineering Managerとして、agentに企業・プロジェクトの内部知識を検索する能力を持たせたい。agentの回答と成果物が組織の具体的なコンテキストに適合するようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL Knowledge_Store（vector database）コンポーネントを提供する。内部ドキュメントをvector embeddings形式で保存し、各クエリ200ms以内のsemantic similarity searchをサポートする。
2. THE SDLC_System SHALL ユーザーによる複数形式でのドキュメントingestを許可する：plain text、Markdown、PDF、DOCX；システムはドキュメントの自動chunk化、embeddings作成、indexing（インデックス作成）を実行する。
3. WHEN agentがtaskを実行する場合、THE SDLC_System SHALL agentが現在のコンテキストでKnowledge_Storeをクエリし、最も関連性の高いtop-Kドキュメント（K configurable、デフォルトK=5）を取得してoutput生成の追加コンテキストとして使用することを許可する（RAG pattern）。
4. THE SDLC_System SHALL Knowledge_Storeのスコープ設定を許可する：システム全体（global）、プロジェクト別（project-scoped）、agent別（agent-scoped）。
5. WHEN Knowledge_Store内のソースドキュメントが更新または削除された場合、THE SDLC_System SHALL 300秒以内に対応するembeddingsを自動更新する。
6. THE SDLC_System SHALL 各agentに対するKnowledge_Store使用のenable/disable設定を許可する；WHEN disabled時、agentは直接入力のみに基づいて動作する。
7. THE SDLC_System SHALL agentがKnowledge_Storeをクエリするたびにログを記録する：agent_id、query summary、返された結果数、平均relevance score。

---

### Requirement 18: FAQ Best-Matchシステム (FAQ Direct Response)

**User Story:** Engineering Managerとして、よくある質問（FAQ）はAI生成ではなく事前承認済みの回答ストアから直接回答してほしい。正確性、一貫性の確保とコスト節約のため。

#### Acceptance Criteria

1. THE SDLC_System SHALL FAQ_Storeコンポーネントを提供する。質問-回答ペアのCRUD管理を許可する。メタデータ：category、tags、作成日、更新日、ステータス（active/inactive）。
2. WHEN agentが入力質問を受け取った場合、THE SDLC_System SHALL FAQ_Store内の全質問とsemantic matching（意味的照合）を実行し、各FAQペアのrelevance scoreを計算する。各matching処理200ms以内。
3. IF FAQ best-matchのrelevance scoreが設定されたconfidence閾値を超えた場合（デフォルト ≥ 0.85）、THEN THE SDLC_System SHALL AIモデルを呼び出さずに事前承認済み回答を直接返す。
4. IF 全FAQのrelevance scoreがconfidence閾値未満の場合、THEN THE SDLC_System SHALL 質問を通常のAI agent処理に転送する（R17に基づくKnowledge_Store/RAGとの組み合わせ可能）。
5. THE SDLC_System SHALL confidence閾値（0.0〜1.0）の各agentまたはシステム全体での設定を許可する。
6. WHEN FAQ_Storeがbest-matchを返す場合、THE SDLC_System SHALL 以下を付加する：FAQ_id、relevance score、レスポンスのソースラベル「faq_direct」。
7. THE SDLC_System SHALL 各agentに対するFAQ best-matchメカニズムのenable/disableを許可する。
8. THE SDLC_System SHALL FAQ best-matchが起動されるたびにログを記録する：agent_id、元の質問、FAQ_id、relevance score、結果（direct_responseまたはfallback_to_ai）。

---

### Requirement 19: 標準ソースコードリポジトリ (Baseline Template Registry)

**User Story:** Developerとして、新プロジェクト作成時や一般的な機能追加時にシステムがテスト済み標準ソースコードを自動使用してほしい。時間節約と基盤品質の確保のため。

#### Acceptance Criteria

1. THE SDLC_System SHALL Template_Registryコンポーネントを提供する。標準ソースコードテンプレート（baseline）の保存と管理を許可する。各テンプレートに含む内容：名前、説明、tags、source code repository URLまたはembedded code、dependencies一覧、使用ガイド、ステータス（active/deprecated）。
2. THE SDLC_System SHALL 複数タイプのテンプレート登録を許可する：project scaffold、feature module、architecture pattern。
3. WHEN Coding_Agentがプロジェクト作成または機能追加の要求を受けた場合、THE SDLC_System SHALL Template_Registryをsemantic matchingで自動検索する；IF relevance score ≥ 0.8のテンプレートが見つかった場合、THEN テンプレート使用を提案する。
4. IF ユーザーがテンプレート使用を承認した場合、THEN THE Coding_Agent SHALL テンプレートからscaffoldし、AIはカスタマイズ部分（business logic、config、extension points）にのみ使用する。
5. THE SDLC_System SHALL 動作設定を許可する：「auto_use」「suggest」（デフォルト）、または「ignore」。
6. THE SDLC_System SHALL テンプレートのversioningをサポートする；WHEN テンプレートが更新された場合、旧バージョンは引き続き利用可能。
7. THE SDLC_System SHALL Admin PortalからのTemplate_Registry管理を許可する：CRUD、stats、security review後の「verified」badge。
8. THE SDLC_System SHALL 全activeテンプレートがpass済みであることを保証する：linting、security scan、coreコンポーネントのunit test。
9. THE SDLC_System SHALL template scopingをサポートする：global、organization、team-private（R34に基づく）。
10. THE SDLC_System SHALL テンプレート使用時にログを記録する：template_id、version、agent_id、project_id、コード維持率 vs AI生成率（%）。

---

### Requirement 20: ドキュメント・設定テンプレートリポジトリ (Document & Configuration Template Registry)

**User Story:** Tech Leadとして、プロセスドキュメントとシステム設定の標準テンプレートリポジトリが欲しい。全チームが正しいフォーマットで内部基準に準拠したドキュメントを作成できるようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL Doc_Template_Registryコンポーネントを提供する。6タイプのテンプレート保存・管理を許可する：requirements template、design template、task template、skills template、hooks template、steering template。
2. THE SDLC_System SHALL 各テンプレートに以下を含むことを許可する：名前、タイプ、説明、テンプレート内容（placeholders付き）、required sectionsリスト、記入済みサンプル、使用ガイド、tags、バージョン、ステータス（active/draft/deprecated）。
3. WHEN Requirements_Agent、Design_AgentまたはTask_Agentが新しいドキュメントを作成する場合、THE SDLC_System SHALL デフォルトのactiveテンプレート（またはproject configで指定されたテンプレート）を基礎構造として自動適用する。
4. THE SDLC_System SHALL ユーザーにスキーマ自動バリデーション付きの標準フォーマットでskills/hooks/steeringテンプレートの作成・管理を許可する。
5. THE SDLC_System SHALL 同一タイプに複数テンプレートをサポートする；ユーザーまたはAdminがproject/teamごとにデフォルトテンプレートを選択可能。
6. THE SDLC_System SHALL テンプレート継承をサポートする：子テンプレートが親テンプレートを継承し、独自部分をoverride/追加。
7. THE SDLC_System SHALL ドキュメントテンプレートのバージョン間diff機能付きversioningをサポートする。
8. THE Admin Portal SHALL Doc Template Managementページを提供する：CRUD、preview、stats、JSON/YAMLインポート/エクスポート。
9. THE SDLC_System SHALL template scopingをサポートする：global、organization、team-private。override hierarchy付き（R34に基づく）。
10. WHEN Output Validation（R22）がoutputを検査する場合、THE SDLC_System SHALL 適用中テンプレートのrequired sectionsとschemaを自動validation rulesとして使用する。

---

### Requirement 21: プロジェクトメモリ (Project Second Brain)

**User Story:** Developerとして、AIに作業中のプロジェクト固有知識を自動蓄積・整理してほしい。プロジェクトが大きくなっても、手動で指示することなくagentが関連情報を素早く見つけられるようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 各プロジェクトにProject_Brain（Knowledge_Storeとは分離）コンポーネントを提供する。作業中に生成される知識を自動収集・インデックス化する：Artifacts（requirements、designs、task lists、code、test reports）、decision logs（解決策Aを選んだ理由）、conversation summaries（重要な議論の要約）、project-specific terminology。
2. THE Project_Brain SHALL 新しいArtifactがContext_Storeに保存された場合またはrepositoryに変更があった場合に自動更新する：embeddings作成、entities/relationships抽出、全文検索インデックス（full-text index）作成を各変更後120秒以内に実行。
3. THE Project_Brain SHALL プロジェクトコンポーネント間の関連を示すknowledge graphを維持する：requirement ↔ design component ↔ task ↔ code file ↔ test case；agentが関係に基づいてクエリ可能（例：「requirement Xを実装するcodeは？」「module Yをカバーするtestは？」）。
4. WHEN agentがプロジェクト内でtaskを実行する場合、THE SDLC_System SHALL 関連コンテキストをProject_Brain（top-K relevant items、K configurable、デフォルトK=10）から自動クエリし、Knowledge_Store（R17）から一般コンテキストも取得する；重複結果がある場合、Project_BrainがKnowledge_Storeより優先される。
5. THE Project_Brain SHALL decision logsを自動記録する：WHEN Human_Reviewerがコメント付きでArtifactをapprove/rejectした場合、またはWHEN ユーザーが会話中に解決策選択の理由を説明した場合、THE SDLC_System SHALL 抽出してdecision entryとして保存する：context、alternatives considered、decision made、rationale、timestamp。
6. THE Project_Brain SHALL project-specific terminologyをサポートする：プロジェクト内で頻出するが一般辞書にない用語/フレーズを自動検出し、ユーザーによる意味の確認/定義を許可する；agentはoutput生成時にこのterminologyを使用する。
7. THE SDLC_System SHALL ユーザーにProject_Brainの閲覧、検索、手動追加を許可するインターフェースを提供する：decision logsの追加、relationships編集、outdated/deprecated情報のマーキング。
8. THE Project_Brain SHALL 自然言語クエリをサポートする：ユーザーまたはagentが質問可能（例：「なぜMongoDBではなくPostgreSQLを選んだ？」「AuthServiceに依存するモジュールは？」）で、蓄積データに基づいた回答を500ms以内に受け取る。
9. THE SDLC_System SHALL プロジェクトごとのProject_Brain retention policy設定を許可する：永久保持データ（decisions、terminology）、configurable期間後にarchive可能なデータ（conversation summaries — デフォルト180日）。
10. THE SDLC_System SHALL Project_Brainのメトリクスを提供する：インデックス済みアイテム総数、ストレージサイズ、クエリ頻度、hit rate（ユーザーフィードバックに基づく有用な結果があったクエリの%）、結果のないトップクエリ（gaps）（カバレッジ改善用）。

---

### Requirement 22: AI出力検査 (Output Validation & Guardrails)

**User Story:** Tech Leadとして、正式Artifactとして保存する前にAIのoutputをシステムに自動検査してほしい。フォーマットエラー、内容不足、ルール違反を早期発見するため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 各agentタイプのoutputに対するvalidation rulesの設定を許可する：schema validation、required sections check、custom regex/pattern rules。
2. WHEN agentがoutputを生成した場合、THE SDLC_System SHALL outputをContext_StoreにArtifactとして保存する前に全validation rulesを自動実行する。
3. IF outputがvalidation ruleに違反した場合、THEN THE SDLC_System SHALL Artifact保存を拒否し、validation結果をagentに返し、agentに修正・再生成を要求する（最大3回retry）。
4. IF 3回retry後もoutputがvalidationをpassしない場合、THEN THE SDLC_System SHALL Artifactを「validation_failed」とマークし、ステータス「draft」にしてHuman_Reviewerに通知する。
5. THE SDLC_System SHALL 全agents対象の企業レベルguardrail rulesをサポートする：output内のPII/credentialsの禁止、output長さ制限、禁止語/フレーズのblacklist。
6. THE SDLC_System SHALL 各validation時にログを記録する：agent_id、artifact_id、applied rules、passed/failed rules、retry回数、最終結果。
7. THE SDLC_System SHALL AdminにAdmin PortalからのValidation rules管理を許可する：CRUD rules、agent typesへの割り当て、violation rateの統計表示。

---

### Requirement 23: AI品質評価とBenchmarking (AI Quality Evaluation & Benchmarking)

**User Story:** AI Engineerとして、AI Agentのoutput品質を客観的に評価するシステムが欲しい。agentのパフォーマンスの良し悪しを把握し、品質検収の根拠を得るため。

#### Acceptance Criteria

1. THE SDLC_System SHALL Eval_Storeを提供する。各agentのevaluation test suitesの作成・管理を許可する：入力セット、ground truth、scoring criteria、metadata。
2. THE SDLC_System SHALL 3つの評価方法をサポートする：Automated Metrics（semantic similarity、BLEU/ROUGE、factual accuracy、completeness、code metrics）、LLM-as-Judge（AI evaluatorがaccuracy/relevance/coherence/harmfulnessで1-5点採点）、Human Evaluation。
3. THE SDLC_System SHALL 以下のモードでevaluationの実行を許可する：manual、scheduled（daily/weekly）、on-change（agent config変更時）。
4. WHEN evaluationが完了した場合、THE SDLC_System SHALL レポートを作成する：aggregate score（0-100）、基準別スコア、下位10% test cases、前回との差分、pass/failステータス。
5. THE SDLC_System SHALL 検収閾値の設定を許可する（デフォルト70/100）；IF agentが閾値未達の場合、THEN 「below_quality_threshold」とマークしてAdminにalert。
6. WHEN 評価スコアがbaselineと比べて10%以上低下した場合、THE SDLC_System SHALL 「quality_regression」アラートを詳細付きで作成する。
7. THE SDLC_System SHALL Human Evaluationワークフローを提供する：outputsのauto-sampling → エキスパートレビュー → quality score更新 → 良好なoutputsを新しいground truthとしてマーク。
8. THE SDLC_System SHALL Quality Dashboardを提供する：経時的トレンド、agents比較、user feedback（R25/R26）との相関、カテゴリ別heat map。
9. THE SDLC_System SHALL A/B evaluationをサポートする：同一test suiteで旧config vs 新configを比較。statistical significance（p < 0.05）付き。
10. THE SDLC_System SHALL evaluation結果を最低365日間保存し、実行時点のagent config snapshotとリンクする。
11. THE SDLC_System SHALL evaluation scopingをサポートする：global、project-specific、agent-specific test suites。
12. THE Admin Portal SHALL Evaluation Managementページを提供する：test suitesのCRUD、CSV/JSONインポート、schedules、thresholds、Human Evaluation assignments。

---

### Requirement 24: Prompt Input/Outputログ記録 (AI Interaction Logging)

**User Story:** AI Engineerとして、AIモデルに送信された全promptとレスポンスを記録したい。debug、hallucination検出、fine-tuning、compliance auditに活用するため。

#### Acceptance Criteria

1. WHEN agentがAIモデルを呼び出した場合、THE SDLC_System SHALL ログを記録する：agent_id、pipeline_id、timestamp（ISO 8601）、full prompt input、full response output、AIモデル、inference パラメータ（temperature、max_tokens、top_p）。
2. THE SDLC_System SHALL 全てのAI呼び出しをログに記録する。retry attemptsおよびagent間の内部呼び出しを含む。
3. THE SDLC_System SHALL prompt/output logsを最低90日間保存する。
4. THE SDLC_System SHALL agent毎のverbosity設定を許可する：「full」「summary」（500文字に切り詰め）、または「metadata_only」；デフォルト「full」。
5. THE SDLC_System SHALL prompt/output logsがimmutable（append-only）であることを保証する。
6. THE SDLC_System SHALL 以下の条件でログをクエリするAPIを提供する：agent_id、pipeline_id、期間、AIモデル、キーワード。
7. IF promptに機密データ（PII、credentials、「sensitive」タグ）が含まれる場合、THEN 保存前に `[REDACTED]` に自動置換する。
8. THE SDLC_System SHALL メトリクスを計算・付加して記録する：token count（input + output）、latency（ms）、estimated cost。

---

### Requirement 25: Output品質フィードバック (Feedback Loop)

**User Story:** Developerとして、agent使用後にoutput品質を評価したい。システムが継続的に改善されるようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL ユーザーに各Artifact生成後のoutput評価を許可する（thumbs up/down + 最大1000文字のコメント）。
2. THE SDLC_System SHALL フィードバックをメタデータ付きで保存する：agent_id、pipeline_id、artifact_id、user_id、timestamp、rating、comment、prompt log参照（R24）。
3. THE SDLC_System SHALL agent/model/team/project別のフィードバックダッシュボードを提供する：positive/negative比率、トレンド、直近top 10 negative。
4. WHEN negative比率が7日間で30%を超えた場合（最低10フィードバック）、THE SDLC_System SHALL Adminにalert。
5. THE SDLC_System SHALL Adminにフィードバックを「actionable」または「dismissed」とマークすることを許可する；「actionable」はimprovement taskとリンクされる。
6. THE SDLC_System SHALL フィードバックを最低180日間保存する。

---

### Requirement 26: ユーザーフィードバック・評価システム (User Feedback & Rating System)

**User Story:** Product Ownerとして、ユーザーからの利用体験に関する包括的なフィードバック受付システムが欲しい。サービス品質を継続的に改善するため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 多面的評価を提供する：1-5星rating、基準別評価（accuracy、speed、relevance、completeness）、コメント（最大2000文字）。
2. THE SDLC_System SHALL サポートする：セッション後のprompt、feedback widget、定期アンケート（configurable、デフォルト2週間）。
3. THE SDLC_System SHALL フィードバックを自動分類する：bug_report、quality_issue、feature_request、praise、general；IF confidence < 0.7 THEN 「unclassified」。
4. WHEN フィードバックがrating ≤ 2星またはcategory「bug_report」の場合、THE SDLC_System SHALL SKU issueを作成する（1星 → critical、2星 → major）。
5. THE SDLC_System SHALL フィードバックをメタデータおよび処理ステータス付き（new/acknowledged/in_progress/resolved/dismissed）で保存する。
6. THE SDLC_System SHALL ダッシュボードを提供する：average rating、分布、top issues、satisfaction trend、NPS。
7. THE SDLC_System SHALL チームオーナーに以下の場合alertする：avg rating < 3.5（7日間）、新規bug_report、feature_request ≥ 5 upvotes。
8. THE SDLC_System SHALL 同一Team/Project内でのフィードバック/feature_requestのupvoteを許可する。
9. THE SDLC_System SHALL Adminにフィードバックへの返信を許可する → ユーザーに通知。
10. THE SDLC_System SHALL フィードバックを365日間保存 + CSV/JSONエクスポート。

---

### Requirement 27: SKU TicketとFeedback管理 (Ticket & Feedback Management Portal)

**User Story:** Support Managerとして、SKU ticketsとuser feedbackの集中管理画面が欲しい。処理進捗を追跡するため。

#### Acceptance Criteria

1. THE Admin Portal SHALL SKU Ticket Managementを提供する：ticketsリスト（ticket_id、タイトル、ソース、priority、ステータス、assignee、作成日、待機時間）。
2. THE Admin Portal SHALL tickets のフィルタ/検索を許可する：ステータス、priority、ソース、assignee、agent、team/project、期間。
3. THE Admin Portal SHALL 以下を許可する：ticket詳細表示、assignee割り当て、ステータス更新、internal notes追加、ユーザーへの返信 — 全てaudit logに記録。
4. THE Admin Portal SHALL User Feedback Managementを提供する：kanban board、category/rating/agent/teamフィルタ、bulk actions、feedback → SKU ticketリンク。
5. THE Admin Portal SHALL SKUダッシュボードを表示する：open/resolved trend、MTTR、SLA超過tickets（critical 48h、major 5日）。
6. THE Admin Portal SHALL Feedbackダッシュボードを表示する：rating trend、category distribution、top feature requests、top negative agents。
7. WHEN critical SKU ticketが30分以内にassignされない場合、SHALL Support Managerにalert + 「Requires Immediate Attention」を表示。
8. THE Admin Portal SHALL priority毎のSLA設定を許可する（first response + resolution time）；WHEN breach時はマーク + assignee + managerに通知。
9. THE Admin Portal SHALL R34の権限管理に準拠する：role/team/projectに基づくscoped visibility。

---

### Requirement 28: パフォーマンス (Performance)

**User Story:** Engineering Managerとして、システムの応答が速くpipeline処理が効率的であってほしい。ボトルネックを作らないため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 最低10のpipelineを同時実行し、単独実行と比べてresponse timeの低下を20%以内に抑える。
2. THE Context_Store SHALL Artifact読み取り（50MB未満）を500ms以内に完了する。
3. THE Context_Store SHALL Artifact書き込み（50MB未満）を2000ms以内に完了する。
4. THE Orchestrator SHALL 新しいPipelineを5秒以内に初期化する。
5. THE SDLC_System SHALL 管理APIのresponse timeを95th percentileリクエストで1000ms未満に維持する。

---

### Requirement 29: 拡張性 (Scalability)

**User Story:** Platform Engineerとして、プロジェクト数とagent数が増加しても拡張可能なシステムが欲しい。

#### Acceptance Criteria

1. THE SDLC_System SHALL パフォーマンスSLA R28に違反することなく最低50のagentの同時動作をサポートする。
2. THE Context_Store SHALL パフォーマンス低下なく最低10,000 Artifactをサポートする。
3. THE SDLC_System SHALL horizontal scalingをサポートする：instance追加時に60秒以内に負荷を自動分散。
4. WHEN pipelineがcapacityを超えた場合、THE SDLC_System SHALL 拒否するのではなくqueueに入れ、priorityに基づいて処理する。

---

### Requirement 30: 可用性 (Availability)

**User Story:** SREとして、システムが常にサービス提供可能であってほしい。

#### Acceptance Criteria

1. THE SDLC_System SHALL 月間uptime最低99.5%を維持する。
2. THE Context_Store SHALL 月間uptime最低99.9%を維持する。
3. IF あるコンポーネントに障害が発生した場合、THEN 他のコンポーネントは動作を継続する；影響を受けたpipelineは一時停止し、復旧時に自動再開する。
4. THE SDLC_System SHALL 最大120秒でのgraceful shutdownをサポートする。

---

### Requirement 31: 信頼性 (Reliability)

**User Story:** Developerとして、データを失わず障害から復旧可能なシステムが欲しい。

#### Acceptance Criteria

1. THE Context_Store SHALL durabilityを保証する：single node failure時にArtifactが失われないこと。
2. THE SDLC_System SHALL Context_Storeを24時間ごとに別の物理的な場所にbackupする。
3. WHEN pipelineがシステム障害により中断された場合、THE Orchestrator SHALL checkpointを保存し、復旧時にcheckpointから再開を許可する。
4. THE SDLC_System SHALL RTO ≤ 15分を保証する。
5. THE SDLC_System SHALL RPO ≤ 1時間を保証する。

---

### Requirement 32: 保守性 (Maintainability)

**User Story:** Platform Engineerとして、拡張と保守が容易なシステムが欲しい。システム全体に影響を与えずに新しいagentの追加やAIモデルの変更ができるようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL coreソースコードの変更なしにplugin/extension mechanismを通じて新しいタイプのagent追加をサポートする。
2. THE SDLC_System SHALL 再デプロイなしに設定によるAIモデル変更を許可する；変更は次のtaskから有効。
3. THE SDLC_System SHALL loose couplingに準拠する：各agentは標準化されたinterfaceを通じて通信する；agentの内部変更は他のagentに影響しない。
4. THE SDLC_System SHALL API versioningを提供する：旧バージョンはdeprecated前に最低6ヶ月間サポート。
5. THE SDLC_System SHALL 全APIとagent interfaceの自動更新technical documentationを提供する。

---

### Requirement 33: セキュリティ (Security)

**User Story:** Security Engineerとして、データ保護と厳格なアクセス制御を備えたシステムが欲しい。

#### Acceptance Criteria

1. THE SDLC_System SHALL 全てのインタラクションにauthenticationを要求する；各agentは個別のcredentialを持つ。
2. THE SDLC_System SHALL RBACを実施する：agentは役割の範囲内でのみArtifactの読み書きが可能。明示的な権限付与がない限り。
3. THE SDLC_System SHALL 全ての状態変更操作のaudit logを記録する：actor、action、target、timestamp（ISO 8601）、結果。
4. THE SDLC_System SHALL audit logを最低90日間、immutableに保存する。
5. IF 5分以内に5回連続認証失敗があった場合、THEN 15分間ロックする + adminにalert。
6. THE SDLC_System SHALL TLS 1.2+で通信を暗号化する。

---

### Requirement 34: ユーザー権限管理 (User Authorization & Access Control)

**User Story:** System Administratorとして、システム内で誰が何をできるかを詳細に制御したい。

#### Acceptance Criteria

1. THE SDLC_System SHALL 5つの役割をサポートする：Viewer、Developer、Tech Lead、Admin、Super Admin（権限は段階的に増加）。
2. THE SDLC_System SHALL Organization → Team → Projectの範囲で権限を管理する；アクセス権は割り当てられた範囲に制限される。
3. IF 操作が権限を超える場合、THEN 拒否 +「access denied」エラーを返す + audit log。
4. THE SDLC_System SHALL デフォルト5役割以外のcustom permission setsを許可する。
5. THE SDLC_System SHALL 承認のdelegation（最大30日間、期限切れ時に自動取り消し）をサポートする。
6. THE SDLC_System SHALL Teams/Projects間のデータを分離する；cross-teamアクセスにはSuper Adminの明示的権限付与が必要。
7. WHEN userが削除/無効化された場合、SHALL 60秒以内に全session/credential/delegationを取り消す；待機中のapprovalはR14-C5に基づき代替reviewerに移行。
8. THE SDLC_System SHALL Super Admin向けのusers/roles/permissions管理インターフェースを提供する。

---

### Requirement 35: リソース制限とQuota (Rate Limiting & Quota Management)

**User Story:** Engineering Managerとして、team/projectごとのAIリソース使用量を制御してコストを管理したい。

#### Acceptance Criteria

1. THE SDLC_System SHALL quota設定を許可する：token limit、AI呼び出し回数、同時pipeline数 — agent/team/project毎の周期ごと。
2. WHEN usageがquotaの80%に達した場合、SHALL チームオーナーとAdminに「approaching limit」alert。
3. WHEN usageが100%に達した場合、SHALL 新しいリクエストをブロック +「quota exceeded」エラーを返す；実行中のpipelineは完了を許可。
4. THE SDLC_System SHALL AIモデルごとのrate limiting（requests/分）をサポートする。
5. THE SDLC_System SHALL burst allowance（デフォルト20%、最大1時間）をhard-block前に許可する。
6. THE SDLC_System SHALL team/project/agent/model別のリアルタイムおよび履歴usage reportを提供する。

---

### Requirement 36: 可観測性 (Observability)

**User Story:** DevOps Engineerとして、システムの監視とdebugを容易にしたい。

#### Acceptance Criteria

1. THE SDLC_System SHALL 全操作にstructured log（JSON）を作成する：timestamp、agent_id、action、pipeline_id、ステータス、実行時間。
2. THE SDLC_System SHALL リクエストごとに一意のtrace_idを持つdistributed tracingをサポートする。
3. THE SDLC_System SHALL Prometheus-compatible metrics endpointを公開する。
4. THE SDLC_System SHALL リアルタイムダッシュボード（≤ 10秒遅延）を提供する。
5. WHEN エラーが発生した場合、THE SDLC_System SHALL logs + traces + metricsをリンクしたcorrelation reportを作成する。

---

### Requirement 37: 互換性と統合 (Compatibility & Integration)

**User Story:** Developerとして、既存の開発ツールとシステムが統合できてほしい。

#### Acceptance Criteria

1. THE SDLC_System SHALL OpenAPI/Protobufドキュメント付きのREST APIおよび/またはgRPC APIを提供する。
2. THE Deployment_Agent SHALL 少なくとも3つのCI/CDプラットフォーム（GitHub Actions、GitLab CI、Jenkins）と統合する。
3. THE SDLC_System SHALL pipelineイベント用のwebhook notificationsをサポートする。
4. THE Coding_Agent SHALL VCS（Git: GitHub、GitLab、Bitbucket）と統合する。
5. THE SDLC_System SHALL 通知システムと統合する：Slack、Microsoft Teams、Email。

---

### Requirement 38: システム管理 (System Administration Portal)

**User Story:** System Administratorとして、システム全体の設定、リソース、ステータスを管理する集中管理インターフェースが欲しい。

#### Acceptance Criteria

1. THE SDLC_System SHALL 全管理機能を集約したAdmin Portal（web-based）を提供する：Agent Management、Pipeline Management、User & Permissions、Knowledge_Store、FAQ_Store、Template_Registry、Doc_Template_Registry、AI Models、AI Evaluation、System Settings、Logs & Audit、System Health、SKU Tickets、User Feedback。
2. THE Admin Portal SHALL System Settingsを提供する：default temperature、FAQ confidence閾値、Knowledge_Store top-K、retention policies、警告閾値。
3. THE Admin Portal SHALL AI Model Managementを提供する：モデルの登録/削除、rate limits、model/agent/team別usage statistics。
4. THE Admin Portal SHALL System Healthを提供する：全コンポーネントのリアルタイムステータス（healthy/degraded/down）、uptime、resource usage。
5. THE Admin Portal SHALL Backup & Recoveryを提供する：履歴表示、manual backup、RPO（R31-C5）範囲内のpoint-in-time restore。
6. THE Admin Portal SHALL Usage & Billingを提供する：token consumption、cost tracking、trend charts、budget alerts。
7. THE Admin Portal SHALL R34の権限管理に準拠する：各roleは適切な部分のみ表示。
8. THE Admin Portal SHALL portal経由の全設定変更にaudit logを記録する（old value → new value）R33-C3に基づく。

---

### Requirement 39: 適切なAIタイプの選択 (AI Model Selection Strategy)

**User Story:** AI Engineerとして、システムに複数のAIモデルタイプ（LLMだけでなく）をサポートし、各taskに最適なモデルタイプを設定できるようにしたい。全てにLLMをデフォルト使用するのではなく、コスト、速度、精度を最適化するため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 複数のAIモデルタイプの登録と使用をサポートする（以下を含むが限定されない）：LLM（Large Language Models — テキスト生成、code、複雑な分析用）、ML Classification models（sentiment分類、categorization、spam detection用）、Embedding models（semantic search、similarity matching用）、Code-specific models（code completion、linting、vulnerability scan用）、Rule-based engines（validation、pattern matching、workflow logic用）。
2. THE SDLC_System SHALL agentレベルだけでなく各タスクごとのmodel type設定を許可する：例えばSentiment_AgentはML Classification model（軽量、高速）をsentiment detectionに使用しつつ、escalation時のsummary作成にはLLMを使用；Coding_AgentはlintingにCode-specific modelを使用しつつ、code generationにはLLMを使用。
3. THE SDLC_System SHALL model routingメカニズムを提供する：task type、complexity、設定に基づき、システムが適切なモデルを自動選択する；IF taskが単純（classification、pattern matching）の場合、THEN 軽量/専門モデルを優先する；IF taskが複雑（generation、reasoning、multi-step）の場合、THEN LLMを使用する。
4. THE SDLC_System SHALL AdminにAdmin PortalからのModel routing rules設定を許可する：task_type → model_typeのmapping、メインモデル不可時のfallback model、切り替え条件（例：先にML modelを使用、confidence < thresholdの場合LLMにfallback）。
5. THE SDLC_System SHALL task typeごとのモデル使用効率を追跡・報告する：同一task typeに対する各モデルタイプ間のlatency、cost、accuracy比較。Adminが最適なmodel routingを判断するのに役立つ。
6. THE SDLC_System SHALL 各agentの「cost-performance profile」設定を許可する：「cost_optimized」（可能な場合は安価/軽量モデルを優先）、「performance_optimized」（最強モデルを優先）、または「balanced」（デフォルト — コストと品質のバランス）。
7. THE SDLC_System SHALL pluggability を保証する：新しいモデルタイプ（例：multimodal model、audio model）の追加はcoreシステムや既存agent logicの変更なしに新しいadapter登録のみで可能。
8. THE SDLC_System SHALL 各taskのmodel selection決定をログに記録する：task_type、considered models、selected model、applied routing rule、理由（例：「ML classifier selected — task is sentiment classification, cost_optimized profile」）。

---

### Requirement 40: AIモデルデプロイモード (AI Model Deployment Modes)

**User Story:** Platform Engineerとして、ローカル実行（self-hosted）とcloud API呼び出しの両方をシステムにサポートしてほしい。各組織のデータセキュリティ要件、コスト、パフォーマンスに基づいて柔軟に選択できるようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 3つのAIモデルデプロイモードをサポートする：(a) **Cloud API** — 外部プロバイダーのAPIを通じてモデルを呼び出す（OpenAI、Anthropic、Google、AWS Bedrock...）、(b) **Self-hosted** — 組織の内部インフラでモデルを実行（Ollama、vLLM、TGI、またはcustom inference server経由）、(c) **Hybrid** — 両方を組み合わせ、rulesに基づいてrouting。
2. THE SDLC_System SHALL モデルごとのdeployment mode設定を許可する：システムに登録された各モデル（R38-C3）にdeployment_mode（cloud/self-hosted）フィールドと対応するconnection configを含む — Cloud: API endpoint + API key + region；Self-hosted: internal endpoint + health check URL + GPU resource info。
3. THE SDLC_System SHALL data sensitivityに基づくrouting rulesの設定を許可する：IF 入力データに「confidential」「pii」タグが含まれる場合、またはpolicy「data_local_only」のprojectに属する場合、THEN self-hosted modelにrouteする；IF constraintがない場合、THEN cost-performance profile（R39-C6）に基づいてrouteする。
4. THE SDLC_System SHALL deployment mode間のfallbackをサポートする：IF self-hosted modelが利用不可（health check failまたはqueue過負荷）の場合、THEN cloud APIにfallback（data policyが許可する場合）；IF cloud APIがrate-limitedまたはunavailableの場合、THEN self-hostedにfallback（利用可能な場合）。
5. THE SDLC_System SHALL 各deployment mode専用のmonitoringを提供する：Cloud — API latency、rate limit remaining、cost per requestの追跡；Self-hosted — GPU utilization、queue depth、inference latency、model load status。
6. THE SDLC_System SHALL team/projectごとのdata residency policy設定をAdminに許可する：「cloud_allowed」（デフォルト — cloud API使用可能）、「local_preferred」（self-hosted優先、必要時cloudにfallback）、または「local_only」（cloud APIへのデータ送信厳禁 — 違反はブロックされalertを記録）。
7. THE SDLC_System SHALL deployment modeに関係なくagentとmodel inference間のインターフェースが統一されていることを保証する：agentはモデルがlocalかcloudかを知る必要がない；routing layerがconnection、retry、failoverを透過的に処理する。
8. THE SDLC_System SHALL 各モデル呼び出し時にdeployment modeをログに記録する（R24に追加）：model_id、deployment_mode（cloud/self-hosted）、endpoint used、data_policy applied、fallback triggered（true/false + 理由がある場合）。

---

### Requirement 41: デフォルト設定 (Default Hooks, Steering & Skills)

**User Story:** Platform Engineerとして、初期化時に基本的なhooks、steering、skillsセットをシステムに準備してほしい。チームがゼロから設定することなくすぐに使い始められるようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL システム初期化時にデフォルトhooksセットを提供する。最低限以下を含む：
   - **lint-on-code-save**: codeファイル変更時に自動linting実行（fileEdited、patterns: *.ts, *.py, *.java, *.go）。
   - **validate-output-before-save**: Artifact書き込み前にtemplate/schemaに基づくoutput検査（preToolUse、toolTypes: write）。
   - **run-tests-after-task**: 各task完了後にtest suite実行（postTaskExecution）。
   - **security-scan-on-code**: 新規codeファイル作成時にcredentialsとvulnerabilitiesスキャン（fileCreated、patterns: *.ts, *.py, *.java, *.go）。
   - **notify-on-pipeline-complete**: pipeline完了時にsummary作成と通知送信（agentStop）。
   - **sentiment-check**: ユーザーがprompt送信するたびに感情分析（promptSubmit）。
   - **template-match-on-new-project**: 新規プロジェクト作成要求検出時に適切なテンプレートを検索・提案（promptSubmit）。

2. THE SDLC_System SHALL システム初期化時にデフォルトsteering filesセットを提供する。最低限以下を含む：
   - **coding-standards.md**（inclusion: auto）: coding conventions、naming、folder structure、design patternsの標準規則。
   - **security-guidelines.md**（inclusion: auto）: セキュリティ規則 — credentials hardcode禁止、input validation、OWASP top 10 awareness。
   - **architecture-principles.md**（inclusion: auto）: アーキテクチャ原則 — clean architecture、separation of concerns、dependency injection。
   - **documentation-standards.md**（inclusion: auto）: ドキュメント作成規則 — format、required sections、language style。
   - **git-workflow.md**（inclusion: auto）: gitプロセス — branching strategy、commit message format、PR conventions。
   - **testing-strategy.md**（inclusion: fileMatch、pattern: *.test.*）: テスト戦略 — coverage targets、naming conventions、test types。
   - **api-design-guidelines.md**（inclusion: fileMatch、pattern: *controller*, *route*）: API設計規則 — REST conventions、versioning、error response format。
   - **deployment-checklist.md**（inclusion: manual）: デプロイ前チェックリスト — review points、rollback plan、monitoring setup。

3. THE SDLC_System SHALL システム初期化時にデフォルトskillsセットを提供する。最低限以下を含む：
   - **database-migration**: data model changesからmigration files生成（Coding_Agent）。
   - **api-documentation**: codeからOpenAPI/Swagger docs生成（Coding_Agent、Design_Agent）。
   - **changelog-generator**: commits/PRsからchangelogを自動生成（Deployment_Agent）。
   - **dependency-audit**: outdated/vulnerableなdependenciesチェック（Testing_Agent、Coding_Agent）。
   - **code-review**: coding standardsに基づくcodeレビュー + 改善提案（Coding_Agent）。
   - **performance-analysis**: metrics/logsからのbottleneck分析（Operations_Agent）。
   - **incident-postmortem**: alert historyからincident report生成（Operations_Agent）。
   - **task-estimation**: historical dataに基づくeffort見積もり（Task_Agent）。
   - **translation**: ドキュメント/UIテキストの他言語翻訳（Requirements_Agent）。
   - **diagram-generator**: テキスト記述からdiagrams生成（Mermaid/PlantUML）（Design_Agent）。

4. THE SDLC_System SHALL Adminに任意のデフォルトhook/steering/skillのcustomize、disable、削除を許可する；変更は設定されたteam/projectにのみ影響し、global defaultsには影響しない。

5. THE SDLC_System SHALL Adminにteam/projectのデフォルトへのリセットを許可する；リセットは独自に作成したcustom itemsに影響を与えず、全hooks/steering/skillsをデフォルト状態に復元する。

6. WHEN システムが新しいデフォルトhooks/steering/skillsを含むバージョンに更新された場合、THE SDLC_System SHALL Adminに新しいitemsを通知し、稼働中のteams/projectsに自動適用せずopt-in有効化を許可する。

---

### Requirement 42: 自動RADIOレポートシステム (Automated RADIO Reporting)

**User Story:** Project Managerとして、実行データからRADIOレポート（Review、Action、Difficulty、Information、Outcome）をシステムに自動生成してほしい。developerの主観的報告に依存せず正確な進捗を把握するため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 5つの必須要素のフォーマットでRADIO reportを自動生成する：**R**（Review — specに対する現在の状態）、**A**（Action — 実行中の作業）、**D**（Difficulty — escalation必要なblockers、risks）、**I**（Information — 進捗に影響する追加情報）、**O**（Outcome — 達成された結果、具体的metrics）。
2. THE SDLC_System SHALL 以下のいずれかのtrigger発生時にRADIO reportの自動生成を起動する：(a) task完了（task statusがdoneに変更）、(b) テスト失敗（CI/CDがfail報告）、(c) 新しいblocker/risk検出、(d) 営業日の終わり（progressがある場合）、(e) sprint終了、(f) task dependency graphにおけるwave完了；最低頻度：active時は1日1回以上。
3. THE SDLC_System SHALL RADIO reportを実際のデータ（commits、test results、spec compliance、pipeline status、task status）から生成する — 主観的self-reportには基づかない。
4. THE SDLC_System SHALL RADIO reportを標準パス `docs/specs/{feature-name}/radio.md` でGit repositoryに保存する。完全なversion history付き。
5. WHEN RADIO reportの「D」（Difficulty）にseverity ≥ Highの項目が含まれる場合、THE SDLC_System SHALL Tech LeadとPMにblockerの具体的内容を含むalertを自動送信する。
6. THE SDLC_System SHALL Human_Reviewerにtimelineに沿ったRADIO reports履歴の閲覧と期間間の比較を許可し、トレンドを検出できるようにする。

---

### Requirement 43: AI自己レビューループ (AI Self-Review Loop)

**User Story:** Tech Leadとして、レビュー提出前にAIにoutputを自己チェックしてほしい。reject回数を減らし、初回からのartifact品質を向上させるため。

#### Acceptance Criteria

1. WHEN agentがoutput（spec、design、tasks、code）を生成した場合、THE SDLC_System SHALL Output Validation（R22）に移行する前にself-reviewループを起動する：agentが以下の基準でoutputを自己評価する：clarity（曖昧さなし）、completeness（完全性）、consistency（内部矛盾なし）、compliance（spec/design入力への準拠）、risk detection（潜在リスクの検出）。処理順序：Self-Review (R43) → Output Validation (R22) → Human Review (R14)。
2. IF self-reviewで問題が検出された場合、THEN agentは自己修正しself-reviewを繰り返す；Human提出前に最大3回ループ。
3. WHEN self-reviewが完了した場合（passまたは3回終了）、THE SDLC_System SHALL メタデータを付加記録する：self-review loop回数、検出・修正済みissuesリスト、最終confidence score（0.0–1.0）、残存issues（ある場合）。
4. IF 3回のself-review後も未解決issuesが残る場合、THEN agentはoutputと共に残存issuesリストを提出し、Humanがレビュー時に注意すべき点を知らせる。
5. THE SDLC_System SHALL agentごとおよびartifact-typeごとのself-review基準設定を許可する；Adminは検査基準の追加/削除が可能。
6. THE SDLC_System SHALL 全self-review iterationsをログに記録する：input、found issues、applied fixes、final decision（pass/escalate）。

---

### Requirement 44: Change Request管理 (Change Request Management)

**User Story:** Project Managerとして、明確なライフサイクルでChange Requestをシステムに管理してほしい。CRの見落としがなく、受付から完了まで全変更を追跡するため。

#### Acceptance Criteria

1. THE SDLC_System SHALL Change Request管理システムを提供する。ライフサイクル：Pending → Approved → In Progress → Review → Done；またはPending → Rejected。各CRに一意の識別子（CR-XXX）。
2. THE SDLC_System SHALL CRバックログを `docs/change-request/backlog.md`（plaintext、diff可能）に集中保存する。内容：CR_id、タイトル、説明、ソース（client/internal）、priority、status、作成日、estimated effort、assigned sprint。
3. WHEN CRがapproveされた場合、THE SDLC_System SHALL 自動的にディレクトリ `docs/change-request/CR-XXX/` を作成する。内容：requirements.md、design.md（effort > 1日の場合）、tasks.md — 新機能と同じspec-drivenプロセスに準拠。
4. THE SDLC_System SHALL AIがCRの実行を開始する前にHuman_Reviewer（PMまたはTech Lead）の承認を要求する。
5. WHEN CR effort < 1日（small CR）の場合、THE SDLC_System SHALL designステップのスキップを許可し、requirements + tasksのみを要求する。
6. WHEN CRが完了した場合、THE SDLC_System SHALL backlog.mdを自動更新（status → Done）し、CR用RADIO reportを生成し、regression testを実行する。
7. THE SDLC_System SHALL AIに各CRのimpact analysisを許可する：影響を受けるcomponents、estimated effort、risks、他のfeatures/CRsとのdependencies。
8. THE SDLC_System SHALL CRダッシュボードを提供する：open/in-progress/doneトレンド、sprint毎のthroughput、average cycle time、overdue CRs。

---

### Requirement 45: Q&A管理 (Question & Answer Management)

**User Story:** Business Analystとして、spec内の曖昧な点をシステムに自動検出し、明確化質問をthread管理してほしい。ambiguityの見落としがなく、全結論がspecに反映されるようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL Q&A Managementシステムを提供する。構造：各質問は独立したthread（QA-XXX）でライフサイクルを持つ：Open → In Discussion → Answered → Closed。
2. THE SDLC_System SHALL Q&Aインデックスを `docs/qa/index.md` に、各threadを `docs/qa/QA-XXX/thread.md` に保存する。内容：QA_id、質問、context（関連spec/design）、priority（High/Medium/Low）、target（誰が回答）、status、deadline。
3. WHEN agent（特にRequirements_AgentまたはDesign_Agent）が入力のambiguityを検出した場合、THE SDLC_System SHALL ステータスOpenの新しいQA threadを自動作成し、曖昧なspec/design部分への参照を付与する。
4. THE SDLC_System SHALL multi-round discussionをサポートする：各threadでAI、Human_Reviewer、external stakeholder間の複数回のやり取り（follow-up）を許可する。
5. WHEN QA threadが十分に回答された場合、THE SDLC_System SHALL Conclusion（結論要約）とAction Items（次のアクション：spec更新、CR作成等）の記録を要求する。
6. WHEN QAのconclusionがapprovedされた場合、THE SDLC_System SHALL 関連spec/designを自動更新し、逆参照を記録する（`Updated from QA-XXX`）。
7. THE SDLC_System SHALL deadlineを追跡する：High priority QAは48時間以内に回答必須、Mediumは5日以内；WHEN overdue時、RADIO「D」（R42）にフラグし、escalation matrix（R51）に基づいてescalate。
8. THE SDLC_System SHALL 関連する未解決のOpen QAが残っている場合、specを次の段階に進めることを禁止する。

---

### Requirement 46: DeliverablesとLimitations管理 (Deliverables & Limitations Management)

**User Story:** Delivery Managerとして、完了した成果物リストと未解決の制限事項をシステムに自動集約してほしい。手動収集なしでdelivery状態を明確に把握するため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 実行データからdeliverablesを自動集約する：tasks completed、test results、deployment status、spec compliance — 各sprint/phase終了時にファイル `docs/deliverables/sprint-XX/deliverables.md` を作成。
2. THE SDLC_System SHALL deliverablesを分類する：feature（完全実装 + テスト済み）、partial（実装済みだが不完全）、documentation（作成済みドキュメント）。
3. THE SDLC_System SHALL limitationsを自動検出・記録する：未完了tasks、failしているtest cases、known issues、未実装requirements — 各limitationに一意のID（LIM-XXX）。
4. THE SDLC_System SHALL limitationsをタイプで分類する：Not Implemented（要件は存在するが未着手）、Partial（途中）、Technical Constraint（克服不可能な技術的制限）、Known Issue（既知のバグで未修正）。
5. FOR EACH limitation、THE SDLC_System SHALL handling planの記録を要求する：「Next Sprint」「Phase 2」「Won't Fix」（理由付き）；stakeholderへ送信前にHuman_Reviewerのplan確認が必要。
6. THE SDLC_System SHALL `docs/deliverables/baseline.md` ファイルを維持し、最初のsprintからのbaseline metricsを記録して経時的な改善を比較可能にする。
7. THE SDLC_System SHALL 双方向リンクを提供する：limitationはspec/CR/taskを参照し、spec/CR/taskはlimitationを参照する。

---

### Requirement 47: 承認チェックリスト強制 (Approve Checklist Enforcement)

**User Story:** Tech Leadとして、各review gateで必須チェックリストをシステムに強制してほしい。Human_Reviewerがapprove時に重要な基準を見落とさないようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 各review gateタイプの必須approve checklistを提供する：Approve Requirements、Approve Design、Approve Tasks、Approve PR（Code）；各checklistにチェックすべきitemsリストを含む。
2. THE SDLC_System SHALL approve許可前に全checklist itemsのチェックをHuman_Reviewerに要求する；IF いずれかのitemが未チェックの場合、THEN approveをブロックし未チェックitemsリストを表示する。
3. THE SDLC_System SHALL gate typeごとのdefault checklistを提供し、Adminにproject/teamごとのitems追加/削除カスタマイズを許可する。Default checklistの最低内容：(a) Approve Requirements — complete、clear、EARS format、testable、no contradictions、edge cases、glossary、no assumptions、risks flagged、Q&A resolved；(b) Approve Design — covers requirements、architecture sound、security、performance、DB design、API contract、GUI prototype reviewable、error handling、risks flagged；(c) Approve Tasks — covers design、granularity < 4h、clear dependencies、requirements reference、testable outcome、test tasks included；(d) Approve PR — spec compliance、design compliance、tests pass、coverage ≥ 80%、security、performance、code quality。
4. THE SDLC_System SHALL checklist結果をapprove決定と共にGitに記録する：reviewer、timestamp、チェック済みitemsリスト、comments（ある場合）。
5. THE SDLC_System SHALL conditional checklist itemsをサポートする：条件を満たした場合にのみ表示されるitems（例：「Security review」はartifactにauth/payment logicが含まれる場合のみ表示）。
6. THE SDLC_System SHALL メトリクスを提供する：first-time-pass率（rejectなしでapprove）、gate毎のaverage review time、最も見落とされやすいitems。

---

### Requirement 48: Definition of Done強制 (Definition of Done Enforcement)

**User Story:** Delivery Managerとして、task/feature完了を報告する前にAIにDefinition of Done（DoD）を自動チェックしてほしい。基準を完全に満たしていないartifactが「Done」とマークされないことを保証するため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 各artifact/phaseタイプのDoDを定義する：Spec（approved + versioned + no open QA + correct EARS format）、Design（approved + GUI reviewable + covers all requirements + no open RISK）、Tasks（approved + each task < 4h + clear dependencies + full traceability）、Feature（all tasks done + all tests pass + spec compliance ≥ 95% + PR merged + clean RADIO + deliverables updated）、Bugfix（root cause confirmed + fix merged + regression pass + no unchanged behavior broken + prevention applied）。
2. WHEN AIまたはHumanがartifact/phaseを「Done」とマークした場合、THE SDLC_System SHALL DoD checkを自動実行する：対応するDoDの各基準を検証する。
3. IF いずれかのDoD基準が未達の場合、THEN THE SDLC_System SHALL 「Done」ステータスへの移行をブロックし、未達基準リストを詳細付き（例：「2 tests failing」「QA-003 still Open」）で返す。
4. THE SDLC_System SHALL Adminにproject毎のDoD customize：各artifact typeの基準追加/削除/変更を許可する。
5. THE SDLC_System SHALL 各DoD check時にログを記録する：artifact_id、checked基準、基準毎のpass/fail、全体結果。
6. THE SDLC_System SHALL overrideメカニズムを提供する：Tech Leadが理由付きでforce-complete可能（例：「accepted technical debt」）— 決定はaudit logとRADIO「I」に記録される。

---

### Requirement 49: バグ修正専用ワークフロー (Bugfix-Specific Workflow)

**User Story:** QA Engineerとして、root cause分析（5WHY）、unchanged behavior特定（regression prevention）、予防措置適用を含む明確なbugfixプロセスをシステムに持たせたい。バグが再発せずregressionを引き起こさないようにするため。

#### Acceptance Criteria

1. WHEN Fix Modeが起動された場合、THE SDLC_System SHALL bugfixドキュメント（`docs/bugs/BUG-XXX/bugfix.md`）の作成を要求する。必須フォーマット3部構成：(a) Current Behavior (Defect) — 現在のエラー動作の記述、(b) Expected Behavior (Correct) — specに基づく正しい動作の記述、(c) Unchanged Behavior (Regression Prevention) — 変更してはいけない動作のリスト。
2. THE SDLC_System SHALL AIに各bugの5WHY（Five Whys）分析を要求する：root causeを見つけるために最低3回、最大5回「Why?」を連続で質問する；root causeは修正可能な原因でなければならない（「human error」ではない）。
3. THE SDLC_System SHALL bugfixドキュメントにPrevention Plan（バグ再発防止策）を含むことを要求する（例：linter rule追加、validation追加、test case追加）。
4. WHEN bugfix codeが生成された場合、THE Testing_Agent SHALL リストされた全「Unchanged Behavior」をカバーするregression test casesを作成する — 修正が既存動作を破壊しないことを保証。
5. THE SDLC_System SHALL bugfixのDoD checkを実施する：root cause confirmed + fix merged + regression test pass + unchanged behavior verified + prevention applied；いずれかの基準が欠けている場合はbugのcloseを禁止。
6. THE SDLC_System SHALL bugfixドキュメントをGit（`docs/bugs/BUG-XXX/`）に保存する。design.md（5WHY + solution）とtasks.mdを含み、バージョン管理プロセスR16に準拠。

---

### Requirement 50: 統一エラー処理・Retryポリシー (Unified Failure Handling & Retry Policy)

**User Story:** Tech Leadとして、全agentに統一されたretryとエラー処理ポリシーをシステムに持たせたい。エラー発生時の動作が一貫性があり、predictableで、明確なescalation pathがあるようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 全agentsにoutput生成失敗時の統一retryポリシーを適用する：Attempt 1 — agentがerror message/feedbackに基づいて自己修正；Attempt 2 — agentが別のapproachを試行（different algorithm、different structure）；Attempt 3 — agentがroot cause analysisを集約 + escalationレポートを作成。
2. IF 3回retry後もagentが失敗する場合、THEN THE SDLC_System SHALL：(a) そのtaskでagentを停止する、(b) failure report生成：3 attempts summary、root cause hypothesis、Human向けsuggested approach、(c) taskを「blocked」statusに変更、(d) RADIO「D」に記録、(e) severity ≥ MediumならISSUE-XXXを作成。
3. THE SDLC_System SHALL Human overrideをサポートする：agentが3回失敗した後、Human（Developer role）が直接介入するか、agentに具体的なガイダンスを提供して4回目のretryを許可。
4. THE SDLC_System SHALL output誤り時のrollback strategyを適用する：wrong code → `git revert`、wrong spec/design → revert version、feature全体が方向誤り → revert branch + spec phaseに戻る。
5. THE SDLC_System SHALL agent毎の最大retry回数（デフォルト3）およびattempt毎のtimeout（デフォルトはR12-C5に基づく）の設定を許可する。
6. THE SDLC_System SHALL 全retry attemptsをログに記録する：agent_id、task_id、attempt number、used approach、encountered error、outcome（success/fail/escalate）。

---

### Requirement 51: コミュニケーションプロトコル (Communication Protocol)

**User Story:** Project Managerとして、SLA response timeと自動リマインダーを含む明確なコミュニケーションプロトコルをシステムに持たせたい。approve/QA/escalationが忘れられないようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL コミュニケーションを5タイプに分類し、異なるSLAを設定する：Approve Request（4h High、24h Medium）、Clarification/QA（24h High、48h Medium）、Escalation（1h Critical、4h High）、Information/FYI（response不要）、CR notification（24h acknowledge）。
2. THE SDLC_System SHALL タイプごとのコミュニケーションチャネル設定をサポートする：Slack、Microsoft Teams、Email、In-app notification；各コミュニケーションタイプは異なるチャネルで送信可能。
3. WHEN AIがHumanのresponseを必要とする場合（approve、QA回答、escalation確認）、THE SDLC_System SHALL 自動reminderストラテジーを実行する：Attempt 1（即時）— notification送信；Attempt 2（24時間後）— remind + RADIO「D」にフラグ；Attempt 3（48時間後）— PM/監督者にescalate；Attempt 4（72時間後）— 関連taskをpauseし、別taskに切り替え。
4. THE SDLC_System SHALL role毎およびpriority毎のSLA response time設定を許可する；WHEN SLA違反時、ログ記録 + manager警告。
5. THE SDLC_System SHALL コミュニケーションメトリクスを提供する：role毎のaverage response time、SLA compliance rate、bottleneck roles（最も応答が遅い）、communication volume trend。
6. THE SDLC_System SHALL delegation をサポートする：Humanが不在時（休暇、多忙）、configurable期限でapprove/responseを代替者に委任可能（R34-C5に基づく）。

---

### Requirement 52: Specバージョニング戦略 (Spec Versioning Strategy)

**User Story:** Tech Leadとして、再承認が必要なタイミングの明確なルール付きでspecドキュメントにsemantic versioningを適用したい。小さな修正にoverheadを作らずに変更を厳密に管理するため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 全specドキュメント（requirements、design、tasks）にsemantic versioningを適用する：**Major**（X.0 — scope/logic変更）、**Minor**（X.Y — 詳細補足、edge case追加）、**Patch**（X.Y.Z — typo修正、format）。
2. WHEN specにMajorまたはMinor変更がある場合、THE SDLC_System SHALL 新バージョン発効前にHuman_Reviewerからの再承認を必須とする。
3. WHEN specにPatch変更（typo、formatのみ）がある場合、THE SDLC_System SHALL Humanレビューなしでauto-approveを許可する。
4. THE SDLC_System SHALL 各specドキュメントに以下の明記を要求する：version number、last updated date、updated by、approved by、change log table（version、date、changes、approved by）。
5. THE SDLC_System SHALL diff analysisに基づいて変更タイプ（Major/Minor/Patch）を自動検出する：requirement追加/削除 → Major；AC追加、clarify → Minor；typo/format → Patch；Humanは分類をoverride可能。
6. THE SDLC_System SHALL downstream artifacts（design、tasks、code）がupstream specの正しいversionを参照していることを保証する；WHEN upstream specがMajor/Minor変更された場合、更新が必要なdownstream artifactsをフラグする。

---

### Requirement 53: プロセス例外ルール (Process Exception Rules)

**User Story:** Tech Leadとして、full spec flowが不要な場合の明確なルールをシステムに持たせたい。不要なceremonyに遅延されることなく、チームがhotfix、config change、typoを柔軟に処理できるようにするため。

#### Acceptance Criteria

1. THE SDLC_System SHALL 以下のexception scopesをサポートする — spec flowの一部または全部のスキップを許可する：(a) **Production Hotfix**（critical、< 30分）— 直接fix → PR → approve → merge；fix後にbugfix.mdを作成、(b) **Config Change**（env variable、feature flag）— 直接PR、specなし、(c) **Typo/Copy Change** — 直接PR、specなし、(d) **Prototype/POC** — 簡潔なrequirements.mdのみ、design + tasksスキップ、(e) **Dependency Update**（security patch）— PR + tests pass → approve、specなし、(f) **Small UI Tweak**（< 1h effort）— requirements + tasks、designスキップ。
2. THE SDLC_System SHALL project毎のexception rules設定を許可する：Adminがexception typesとtrigger条件を追加/削除/変更可能。
3. WHEN ユーザーがexception scopeを選択した場合、THE SDLC_System SHALL exception理由を記録し、対応するpipeline stepsを削減する — ただしPR approvalとtest pass（最低限）は引き続き要求する。
4. THE SDLC_System SHALL デフォルトルールを適用する：full specとexceptionのどちらが必要か不明な場合 → デフォルトはfull spec（より安全）。
5. THE SDLC_System SHALL 全exception使用のaudit logを記録する：user_id、exception type、理由、関連artifact_id — exceptionの乱用を追跡するため。
6. THE SDLC_System SHALL メトリクスを提供する：sprint毎のexception vs full-spec比率、最も多いexception types、exception使用とdefect rateの相関。
