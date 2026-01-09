---
name: refactoring
description: Refactoring guidelines, code smells, safe refactoring procedures
---

# Refactoring Skill

## Overview

リファクタリングの判断基準、コード臭の識別、安全なリファクタリング手順のガイド。

## リファクタリングの原則

### やるべき時

- テストが十分にある状態で行う
- 機能追加の前に、変更しやすくするため
- バグ修正の過程で、根本原因を解消するため
- コードレビューで指摘された場合

### やるべきでない時

- テストがない状態
- リリース直前
- 「ついでに」感覚での改善
- 動いているコードを理由なく変更

## コード臭（Code Smell）

### 1. 重複コード

**症状**: 同じロジックが複数箇所に存在

**対処**:
```typescript
// Before: 重複
function validateUser(user: User) {
  if (!user.email.includes('@')) throw new Error('Invalid email')
}
function validateAdmin(admin: Admin) {
  if (!admin.email.includes('@')) throw new Error('Invalid email')
}

// After: 共通化
function validateEmail(email: string) {
  if (!email.includes('@')) throw new Error('Invalid email')
}
```

### 2. 長すぎる関数

**症状**: 1つの関数が50行以上、複数の責務を持つ

**対処**: 責務ごとに分割

```typescript
// Before: 長すぎる
async function processShift(data: ShiftData) {
  // バリデーション（20行）
  // DB保存（15行）
  // カレンダー登録（20行）
  // 通知送信（10行）
}

// After: 分割
async function processShift(data: ShiftData) {
  const validated = validateShiftData(data)
  const saved = await saveShift(validated)
  await registerToCalendar(saved)
  await sendNotification(saved)
}
```

### 3. 長すぎるパラメータリスト

**症状**: 関数の引数が4つ以上

**対処**: オブジェクトにまとめる

```typescript
// Before
function createEvent(title: string, start: Date, end: Date, color: string, reminder: boolean) {}

// After
interface EventInput {
  title: string
  start: Date
  end: Date
  color?: string
  reminder?: boolean
}
function createEvent(input: EventInput) {}
```

### 4. 条件分岐の複雑化

**症状**: ネストが3段以上、複雑な条件式

**対処**: 早期リターン、ガード節

```typescript
// Before
function processRequest(user: User | null, data: Data | null) {
  if (user) {
    if (data) {
      if (user.isActive) {
        // 処理
      }
    }
  }
}

// After
function processRequest(user: User | null, data: Data | null) {
  if (!user) return
  if (!data) return
  if (!user.isActive) return
  // 処理
}
```

### 5. 不適切な命名

**症状**: `data`, `info`, `temp`, `tmp`, `x` などの曖昧な名前

**対処**: 意図を表す名前に変更

```typescript
// Before
const d = new Date()
const x = users.filter(u => u.active)

// After
const today = new Date()
const activeUsers = users.filter(user => user.isActive)
```

## 安全なリファクタリング手順

### 1. テストの確認

```bash
# 既存のテストが通ることを確認
npm run test

# テストがない場合は先に追加
```

### 2. 小さな変更の積み重ね

1回のコミットで1つの変更のみ：
- 関数の抽出
- 変数名の変更
- ファイルの移動

### 3. 変更後のテスト

```bash
# 変更のたびにテスト
npm run test

# TypeScriptのエラーチェック
npm run build
```

### 4. コミット

```bash
# 小さな単位でコミット
git commit -m "refactor: 関数名をvalidateEmailに変更"
```

## リファクタリングカタログ

### Extract Function（関数の抽出）

```typescript
// Before
function printInvoice(invoice: Invoice) {
  console.log('Invoice')
  console.log(`Customer: ${invoice.customer}`)
  console.log(`Amount: ${invoice.amount}`)
  console.log(`Due: ${invoice.dueDate}`)
}

// After
function printInvoice(invoice: Invoice) {
  printHeader()
  printDetails(invoice)
}

function printHeader() {
  console.log('Invoice')
}

function printDetails(invoice: Invoice) {
  console.log(`Customer: ${invoice.customer}`)
  console.log(`Amount: ${invoice.amount}`)
  console.log(`Due: ${invoice.dueDate}`)
}
```

### Replace Magic Number with Named Constant

```typescript
// Before
if (monthStartDay > 28) throw new Error('Invalid')

// After
const MAX_MONTH_START_DAY = 28
if (monthStartDay > MAX_MONTH_START_DAY) throw new Error('Invalid')
```

### Introduce Parameter Object

```typescript
// Before
function searchShifts(userId: string, startDate: Date, endDate: Date, status: string) {}

// After
interface ShiftSearchParams {
  userId: string
  startDate: Date
  endDate: Date
  status?: string
}
function searchShifts(params: ShiftSearchParams) {}
```

## このプロジェクトでの注意点

### 禁止事項

- `any` 型の導入
- `@ts-ignore` の使用
- 過度な抽象化（3回以上使われるまで共通化しない）
- Pages Router への変更（App Router必須）

### 推奨事項

- Server Component の活用
- Drizzle ORM のクエリビルダー使用
- Zod でのバリデーション
- shadcn/ui コンポーネントの活用

## チェックリスト（コピペ用）

```markdown
## リファクタリングチェックリスト

### 事前確認
- [ ] テストが存在し、すべてパスしている
- [ ] リファクタリングの目的が明確

### 実施中
- [ ] 1つの変更につき1コミット
- [ ] 変更のたびにテスト実行
- [ ] TypeScriptエラーがない

### 完了後
- [ ] すべてのテストがパス
- [ ] ビルドが成功
- [ ] 機能が正常に動作
```
