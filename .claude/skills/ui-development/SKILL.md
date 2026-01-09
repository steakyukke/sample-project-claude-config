---
name: ui-development
description: UI components, shadcn/ui, Tailwind styling, design system
---

# UI Development Skill

## Overview

UIコンポーネント、shadcn/ui、Tailwindスタイリング、デザインシステムのガイド。

## File Structure

```
src/
├── components/
│   ├── ui/              # shadcn/ui
│   └── layouts/         # レイアウト
└── features/            # 機能別コンポーネント
    ├── auth/
    ├── settings/
    └── shift/
```

## shadcn/ui

### インストール

```bash
npx shadcn-ui@latest add <component>
```

### カスタマイズ

- `src/components/ui/` で編集可能
- `cn()` でスタイル追加: `cn("既存クラス", "追加クラス")`
- ダークモード関連のスタイル（`dark:`）は削除

### Tailwind v4 対応（追加後に修正が必要）

shadcn/ui のコンポーネント追加後、Tailwind v4 構文に合わせて以下を修正：

| 変更前 | 変更後 |
|--------|--------|
| `data-[placeholder]:` | `data-placeholder:` |
| `data-[disabled]:` | `data-disabled:` |
| `max-h-[var(--css-var)]` | `max-h-(--css-var)` |
| `min-w-[var(--css-var)]` | `min-w-(--css-var)` |

**Select コンポーネント固有の修正**:
- `max-h-(--radix-select-content-available-height)` にフォールバック値 `300px` を追加
- Viewport から `h-[var(--radix-select-trigger-height)]` を削除（スクロール問題の原因）

## Theme

**ライトテーマ固定**

- ❌ `dark:` プレフィックスは使用しない
- ❌ ダークモードのメディアクエリは使用しない
- ✅ ライトテーマ用のスタイルのみ記述

## Color Palette

### 基本カラー（zinc系）

| 用途 | クラス |
|------|--------|
| ページ背景 | `bg-zinc-50` |
| カード/パネル背景 | `bg-white` |
| テキスト（メイン） | `text-zinc-900` |
| テキスト（サブ） | `text-zinc-600` |
| テキスト（補足） | `text-zinc-500` |
| ボーダー（薄） | `border-zinc-200` |
| ボーダー（濃） | `border-zinc-300` |

### アクセントカラー

| 用途 | クラス |
|------|--------|
| プライマリ | `ring-blue-500`, `bg-blue-600` |
| 成功 | `bg-green-50`, `border-green-200`, `text-green-700` |
| エラー | `bg-red-50`, `border-red-200`, `text-red-700` |
| 警告 | `bg-yellow-50`, `border-yellow-200`, `text-yellow-700` |

## Component Styles

### カード/パネル

```tsx
// 標準カード
<div className="rounded-xl border border-zinc-200 bg-white p-8 shadow-sm">

// コンパクトカード
<div className="rounded-lg border border-zinc-200 bg-white p-4 shadow-sm">
```

### ボタン

```tsx
// プライマリボタン
<button className="rounded-lg bg-blue-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50">

// セカンダリボタン
<button className="rounded-lg border border-zinc-300 bg-white px-4 py-2 text-sm font-medium text-zinc-900 shadow-sm hover:bg-zinc-50 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50">
```

### アラート/メッセージ

```tsx
// エラー
<div className="rounded-lg border border-red-200 bg-red-50 p-4 text-sm text-red-700">

// 成功
<div className="rounded-lg border border-green-200 bg-green-50 p-4 text-sm text-green-700">
```

## Spacing

| 用途 | 値 |
|------|-----|
| カード内パディング | `p-8`（標準）, `p-4`（コンパクト） |
| セクション間マージン | `space-y-8`, `space-y-6` |
| 要素間マージン | `space-y-4`, `mt-4`, `mb-4` |
| インラインギャップ | `gap-3`, `gap-2` |

## Typography

| 用途 | クラス |
|------|--------|
| ページタイトル | `text-3xl font-bold tracking-tight` |
| セクションタイトル | `text-xl font-semibold` |
| カードタイトル | `text-lg font-medium` |
| 本文 | `text-sm` または `text-base` |
| 補足テキスト | `text-xs` |

## Layout Patterns

### 中央配置（ログインなど）

```tsx
<div className="flex min-h-screen items-center justify-center bg-zinc-50 px-4">
  <div className="w-full max-w-sm space-y-8">
    {/* コンテンツ */}
  </div>
</div>
```

### メインレイアウト（認証後）

```tsx
<div className="min-h-screen bg-zinc-50">
  <header className="border-b border-zinc-200 bg-white">
    {/* ヘッダー */}
  </header>
  <main className="mx-auto max-w-4xl px-4 py-8">
    {/* メインコンテンツ */}
  </main>
</div>
```

## Responsive Design

### ブレークポイント

Tailwindはモバイルファースト。プレフィックスは**その幅以上**に適用。

| プレフィックス | 最小幅 | 用途 |
|--------------|-------|------|
| なし | 0px | スマホ（デフォルト） |
| `sm:` | 640px | タブレット縦 |
| `md:` | 768px | タブレット横 |
| `lg:` | 1024px | デスクトップ |

### レスポンシブカード（設定画面パターン）

スマホ: 上部配置、カード非表示、余白なし
タブレット以上: 中央配置、カード表示

```tsx
// ページレイアウト
<div className="min-h-screen bg-zinc-50 sm:flex sm:items-center sm:justify-center sm:p-8">
  <Card />
</div>

// カードコンポーネント
<Card className="w-full rounded-none border-0 bg-transparent shadow-none sm:max-w-md sm:rounded-lg sm:bg-card sm:shadow-lg">
  <CardHeader className="px-4 pb-4 sm:px-6">
    {/* ... */}
  </CardHeader>
  <CardContent className="px-4 sm:px-6">
    {/* ... */}
  </CardContent>
</Card>
```

### レスポンシブのポイント

- **モバイルファースト**: 基本スタイルはスマホ用、`sm:`以上で上書き
- **スマホではカード不要**: `bg-transparent shadow-none rounded-none`
- **スマホでは余白最小**: ページのパディングなし、コンテンツは`px-4`のみ

## Server vs Client Components

- **デフォルト**: Server Component
- **`'use client'`**: クライアント処理が必要な場合のみ

## Accessibility

- セマンティックHTML: `<button>`, `<nav>`, `<main>` など
- キーボード操作: すべての操作をキーボードで実行可能に
- ARIA属性: 必要に応じて追加
