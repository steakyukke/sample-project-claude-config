---
name: github-workflow
description: Commit messages, PR creation, issue management
---

# GitHub Workflow Skill

## Overview

コミットメッセージ、PR作成、Issue管理のガイド。

## Commit Message Format

### 基本形式

```
<type>: <subject>

<body>（任意）
```

### Type 一覧

| Type | 用途 | 例 |
|------|------|-----|
| `feat` | 新機能追加 | `feat: シフト入力画面を追加` |
| `fix` | バグ修正 | `fix: カレンダー登録時のエラーを修正` |
| `refactor` | リファクタリング | `refactor: APIのバリデーション処理を整理` |
| `style` | コードスタイル修正 | `style: Biomeのフォーマットに準拠` |
| `docs` | ドキュメント追加・修正 | `docs: API設計書を更新` |
| `test` | テスト追加・修正 | `test: ユニットテストを追加` |
| `chore` | ビルド・設定変更 | `chore: Drizzle ORM をセットアップ` |

### ルール

- **subject**: 50文字以内、日本語で簡潔に
- **body**: 詳細な説明が必要な場合のみ（「なぜ」を重視）
- **過去形は使わない**: ❌「追加した」→ ✅「追加」
- **署名は付与しない**: Claude Code署名やCo-Authored-Byは不要

## コミット前チェックリスト

- [ ] `npm run lint` 実行済み
- [ ] `npm run build` エラーなし
- [ ] `npm run test` 成功
- [ ] `console.log()` 削除済み
- [ ] 環境変数のハードコードなし

## PR Creation

```markdown
## 概要
<!-- 変更内容の簡潔な説明 -->

## 変更点
-
-

## テスト方法
<!-- 動作確認の手順 -->

## 関連Issue
<!-- closes #123 など -->
```

## Issue Creation（設計結果の出力用）

```markdown
## 背景
<!-- なぜこの機能/修正が必要か -->

## ゴール
<!-- 達成したいこと -->

## 実装方針
<!-- 技術的なアプローチ -->

## タスク分解
- [ ] タスク1
- [ ] タスク2
```

## プランニングセッションの分離

設計と実装を分離してコンテキストを節約：

1. **プランニング**: 設計を議論 → 結論をIssueに出力
2. **実装**: 新しい会話でIssueを参照して実装
