# プロジェクトルール

> このドキュメントは、Claude Code がこのプロジェクトで作業する際に従うべきルールと設定をまとめたものです。

## プロジェクト概要

**[プロジェクト名]** - プロジェクトの説明をここに記載してください。

- **技術スタック**: Next.js 15+ (App Router), TypeScript, Tailwind CSS, Drizzle ORM, Supabase

---

## Skills 参照テーブル

タスクに応じて適切なSkillを参照してください。

| タスク | Skill | 内容 |
|--------|-------|------|
| API実装・データベース操作 | `api-development` | Drizzle ORM, Zod, データフェッチング |
| UI実装・スタイリング | `ui-development` | shadcn/ui, Tailwind, デザインシステム |
| テスト作成 | `testing` | Vitest, Playwright |
| コミット・PR作成 | `github-workflow` | コミットメッセージ規約, PR/Issue |
| コーディング規約 | `coding-style` | Biome, TypeScript, 設計原則 |
| コードレビュー | `review` | セキュリティ, パフォーマンス, アクセシビリティ |
| デプロイ準備 | `deployment` | デプロイ前チェック, 環境変数, ロールバック |
| リファクタリング | `refactoring` | コード臭, 安全なリファクタリング手順 |

### Skill の呼び出し方

```
skill: api-development を参照して実装して
```

---

## カスタムコマンド

| コマンド | 用途 |
|---------|------|
| `/commit` | 変更内容をコミット |
| `/test` | テストを作成 |
| `/api-impl` | API を実装 |
| `/ui-impl` | UI コンポーネントを実装 |
| `/review` | セルフレビューを実施 |
| `/close-issue` | Issue の完了確認 |
| `/pre-deploy` | デプロイ前チェック |
| `/debug` | エラーの原因調査 |
| `/sync-docs` | ドキュメント同期チェック |

---

## 共通ルール

### 回答言語

**基本的に日本語で回答してください。**

- コードコメントやドキュメント生成も日本語で記述
- 技術用語は適宜英語を使用（例: Server Component, useState など）

### セキュリティ（最優先）

- **認証**: OAuth 2.0（Supabase Auth経由）
- **RLS**: テーブルには必ずRow Level Securityを設定
- **バリデーション**: フロント・バック両方で実施（Zod使用）
- **禁止**: APIキーのハードコード、ユーザー入力の直接DB挿入

### テーマ

**ライトテーマ固定** - `dark:` プレフィックスは使用しない

---

## 開発コマンド

```bash
npm run dev       # 開発サーバー起動
npm run build     # ビルド
npm run lint      # Lint + Format チェック
npm run format    # コード整形
npm run test      # ユニット・統合テスト
npm run test:e2e  # E2Eテスト
```

---

## ディレクトリ構成

```
src/
├── app/                    # Next.js App Router
│   ├── (auth)/            # 認証関連ページ
│   ├── (main)/            # メインページ（認証後）
│   └── api/               # API Routes
├── features/              # 機能別コンポーネント + hooks
├── components/            # 共通コンポーネント
│   ├── ui/                # shadcn/ui
│   └── layouts/           # レイアウト
├── lib/
│   ├── db/                # Drizzle（スキーマ、クエリ）
│   ├── supabase/          # Supabase認証
│   ├── validations/       # Zodスキーマ
│   └── services/          # ビジネスロジック
└── types/                 # 型定義
```

---

## 参考ドキュメント

- `docs/requirements.md` - 要件定義書
- `docs/architecture.md` - アーキテクチャ設計書
- `docs/database.md` - データベース設計書
- `docs/api.md` - API設計書
