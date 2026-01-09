---
name: deployment
description: Pre-deploy checklist, environment variables, rollback procedures
---

# Deployment Skill

## Overview

デプロイ前チェック、環境設定、ロールバック手順のガイド。

## デプロイ先

- **ホスティング**: Vercel
- **データベース**: Supabase (PostgreSQL)
- **ドメイン**: Cloudflare
- **Cron**: Vercel Cron

## デプロイ前チェックリスト

### 1. コード品質

| 項目 | コマンド | 確認内容 |
|------|---------|----------|
| Lint | `npm run lint` | エラーがないこと |
| Build | `npm run build` | ビルドが成功すること |
| Test | `npm run test` | 全テストがパスすること |
| E2E | `npm run test:e2e` | E2Eテストがパスすること |

### 2. セキュリティ

| 項目 | 確認内容 |
|------|----------|
| 依存関係 | `npm audit` で脆弱性がないこと |
| 環境変数 | 本番用の値が設定されていること |
| APIキー | ハードコードされていないこと |
| RLS | すべてのテーブルにRLSが設定されていること |

### 3. 環境変数

#### 必須環境変数

```bash
# Supabase
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

# 認証（必要に応じて追加）
# GOOGLE_CLIENT_ID=
# GOOGLE_CLIENT_SECRET=

# 外部API（プロジェクトに応じて追加）
# EXTERNAL_API_KEY=
```

#### Vercel での設定確認

1. Vercel Dashboard → Settings → Environment Variables
2. Production / Preview / Development それぞれに設定
3. 機密性の高い変数は「Sensitive」をオン

### 4. データベース

| 項目 | 確認内容 |
|------|----------|
| マイグレーション | 本番DBに適用済みか |
| RLS | `src/lib/db/rls.sql` が実行済みか |
| シード | 必要なマスターデータが投入済みか |
| バックアップ | デプロイ前にバックアップを取得 |

### 5. パフォーマンス

| 項目 | 確認内容 |
|------|----------|
| バンドルサイズ | `npm run build` で確認、大きすぎないか |
| 画像最適化 | `next/image` を使用しているか |
| キャッシュ | 適切なキャッシュヘッダーが設定されているか |

## デプロイ手順

### 通常デプロイ（Vercel）

```bash
# 1. mainブランチにマージ（自動デプロイ）
git checkout main
git merge feature/xxx
git push origin main

# または手動デプロイ
vercel --prod
```

### プレビューデプロイ

```bash
# PRを作成すると自動でプレビューURLが生成される
git push origin feature/xxx
# → Vercelが自動でプレビュー環境をデプロイ
```

## ロールバック手順

### Vercel でのロールバック

1. Vercel Dashboard → Deployments
2. 正常だったデプロイメントを選択
3. 「...」→「Promote to Production」

### Git でのロールバック

```bash
# 直前のコミットに戻す
git revert HEAD
git push origin main

# 特定のコミットに戻す
git revert <commit-hash>
git push origin main
```

### データベースのロールバック

```bash
# Supabase Dashboard から
# 1. Database → Backups
# 2. 復元したいポイントを選択
# 3. 「Restore」を実行
```

## 障害対応

### 障害発生時のフロー

1. **検知**: エラーログ、ユーザー報告
2. **影響範囲の特定**: どの機能が影響を受けているか
3. **一次対応**: 必要に応じてロールバック
4. **根本原因の調査**: ログ、コードを確認
5. **修正・再デプロイ**: 修正を適用
6. **振り返り**: 再発防止策の検討

### 確認すべきログ

| サービス | ログの場所 |
|---------|-----------|
| Vercel | Dashboard → Deployments → Functions |
| Supabase | Dashboard → Logs |
| ブラウザ | DevTools → Console |

## チェックリスト（コピペ用）

```markdown
## デプロイ前チェックリスト

### コード品質
- [ ] `npm run lint` 成功
- [ ] `npm run build` 成功
- [ ] `npm run test` 成功
- [ ] `npm run test:e2e` 成功

### セキュリティ
- [ ] `npm audit` で重大な脆弱性がない
- [ ] 環境変数が本番用に設定されている
- [ ] APIキーがハードコードされていない

### データベース
- [ ] マイグレーションが適用済み
- [ ] RLSが設定済み
- [ ] バックアップを取得済み

### 動作確認
- [ ] プレビュー環境で動作確認済み
- [ ] 主要フローが正常に動作する
```
