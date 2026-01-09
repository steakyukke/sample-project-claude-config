---
name: api-development
description: API implementation, Drizzle ORM, Zod validation, data fetching patterns
---

# API Development Skill

## Overview

API実装、Drizzle ORM、Zodバリデーション、データフェッチングパターンのガイド。

## File Structure

```
src/
├── app/api/             # API Routes
├── lib/db/
│   ├── schema.ts        # Drizzleスキーマ
│   ├── index.ts         # DB接続
│   └── queries/         # クエリ関数
├── lib/validations/     # Zodスキーマ
└── lib/services/        # ビジネスロジック
```

## Data Flow

```
Client → API Route → Query関数 → Drizzle ORM → Supabase(PostgreSQL)
```

## Drizzle ORM

### クエリ関数の配置

`src/lib/db/queries/` に集約する。

```typescript
// src/lib/db/queries/settings.ts
export async function getSettingsByUserId(userId: string) {
  return await db.query.userSettings.findFirst({
    where: eq(userSettings.userId, userId),
  })
}
```

### 型の推論

```typescript
import type { InferSelectModel, InferInsertModel } from 'drizzle-orm'
import { userSettings } from '@/lib/db/schema'

type UserSettings = InferSelectModel<typeof userSettings>
type NewUserSettings = InferInsertModel<typeof userSettings>
```

### トランザクション

複数操作は `db.transaction()` を使用。

```typescript
await db.transaction(async (tx) => {
  await tx.insert(users).values(newUser)
  await tx.insert(userSettings).values(defaultSettings)
})
```

### RLSポリシー

テーブル作成後、必ず `src/lib/db/rls.sql` を実行。

### マイグレーション運用

スキーマ変更時は以下の手順で適用:

```bash
# 1. schema.ts を編集後、マイグレーションSQL生成
npm run dev:db:generate

# 2. DBに適用
npm run dev:db:migrate

# 3. 必要に応じてRLSポリシーを更新
# src/lib/db/rls.sql を編集し、Supabase SQLエディタで実行
```

**注意:** `drizzle-kit push` は Supabase との互換性問題があるため使用しない。

## Zod Validation

### スキーマ定義

`src/lib/validations/` に集約。フロント・バック共通で使用。

```typescript
// src/lib/validations/settings.ts
import { z } from 'zod'

export const settingsSchema = z.object({
  monthStartDay: z.number().min(1).max(28),
  calendarId: z.string(),
  defaultPatternId: z.string().uuid(),
})

export type SettingsInput = z.infer<typeof settingsSchema>
```

### API Routeでの使用

```typescript
export async function POST(request: Request) {
  const body = await request.json()
  const result = settingsSchema.safeParse(body)

  if (!result.success) {
    return NextResponse.json(
      { error: result.error.flatten() },
      { status: 400 }
    )
  }

  // result.data は型安全
  const settings = result.data
  // ...
}
```

### Client Componentでの使用

```typescript
import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'

const form = useForm<z.infer<typeof settingsSchema>>({
  resolver: zodResolver(settingsSchema),
})
```

## Data Fetching Patterns

### Server Component（推奨）

```typescript
// 直接データ取得（推奨）
async function SettingsPage() {
  const settings = await getSettings()
  return <SettingsForm settings={settings} />
}
```

### Client Component（避ける）

```typescript
// 避ける: useEffect + fetch
'use client'
const [data, setData] = useState(null)
useEffect(() => {
  fetch('/api/settings').then(/* ... */)
}, [])
```

## Security

- RLSポリシー必須
- ユーザー入力は必ずZodでバリデーション
- Drizzleのパラメータ化クエリを使用（SQLインジェクション対策）
- 環境変数は `.env.local` に保存
