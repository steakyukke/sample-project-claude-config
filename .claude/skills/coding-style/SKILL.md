---
name: coding-style
description: Biome linting, TypeScript rules, code conventions
---

# Coding Style Skill

## Overview

Biome、TypeScript、コーディング規約のガイド。

## Biome

**Biome に完全準拠。**

- インデント: **スペース2個**
- セミコロン: 必要な場合のみ
- クォート: シングルクォート優先
- インポート自動整理: 有効

### コマンド

```bash
npm run format  # コード整形
npm run lint    # Lint + Format チェック
```

## TypeScript

### 厳格な型定義

- ❌ `any` の使用は原則禁止
- ❌ `@ts-ignore` の使用禁止
- ❌ 型アサーション（`as`）の乱用

**❌ 悪い例**

```typescript
const data: any = await fetch('/api/settings')
const settings = data as UserSettings
```

**✅ 良い例**

```typescript
const response = await fetch('/api/settings')
const data: ApiResponse<UserSettings> = await response.json()
if (data.data) {
  const settings = data.data
}
```

### 型の推論を活用

```typescript
// Zodスキーマから型を推論
type SettingsInput = z.infer<typeof settingsSchema>

// Drizzleから型を推論
type UserSettings = InferSelectModel<typeof userSettings>
```

### 共通型定義

`src/types/index.ts` に集約。

## Comments

- **過度なコメントは避ける**: コードが自明な場合はコメント不要
- **複雑なロジック**: 「なぜ」を説明するコメントを追加
- **コメント言語**: 日本語

**❌ 不要**

```typescript
// ユーザーIDを取得
const userId = user.id
```

**✅ 必要**

```typescript
// NOTE: 終日予定はendに翌日を指定する必要がある（Google Calendar API仕様）
const end = { date: addDays(date, 1).toISOString().split('T')[0] }
```

## Design Principles

- **DRY原則**: 3回以上繰り返す場合のみ共通化
- **YAGNI原則**: 将来必要になるかもしれない機能は実装しない
- **早すぎる最適化は避ける**: まずは動くコードを書く

## Prohibited

- ❌ `console.log()` をコミットに含める
- ❌ `any` 型の使用
- ❌ `@ts-ignore` の使用
- ❌ 環境変数のハードコード
- ❌ APIキー・トークンのコミット
- ❌ 過度な抽象化・汎用化
- ❌ Pages Routerの使用（App Router必須）
