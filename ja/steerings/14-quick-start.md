---
inclusion: manual
---

# Steering: クイックスタートガイド

## 適用タイミング
チームがAI-First、Spec-Drivenプロセスを新たに適用する時。

> 📖 詳細: [`quick-start.md`](../quick-start.md)を参照

## フロー概要

```
粗い要件 → [AI: Spec] → ✅ → [AI: Design] → ✅ → [AI: Tasks] → ✅ → [AI: Code+Test] → [AI: RADIO] → ✅ Done
```

## 6ステップ

1. **準備:** `openspec/`フォルダを作成
2. **要件:** AIに粗い説明を提供
3. **Spec:** AIが生成 → Human承認
4. **Design:** AIが生成 + HTMLプロトタイプ → Human承認
5. **Tasks:** AIが生成 → Human承認 → AIが実行
6. **レビュー:** AIがRADIO生成 → HumanがPR承認

## 準備チェックリスト

- [ ] `openspec/`フォルダ作成済み
- [ ] AI Agentセットアップ済み (Kiro/Claude)
- [ ] チームが理解: AIが実行、Humanが承認
- [ ] 少なくとも1人が承認チェックリストを把握
- [ ] CI/CDパイプラインで自動テスト実行可能
