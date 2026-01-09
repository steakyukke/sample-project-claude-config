---
name: testing
description: Vitest unit/integration tests, Playwright E2E tests
---

# Testing Skill

## Overview

Vitest（ユニット・統合テスト）とPlaywright（E2Eテスト）のガイド。

## Test Types

| 種類 | ツール | 対象 |
|------|--------|------|
| ユニットテスト | Vitest | `lib/services/`, `lib/validations/` |
| 統合テスト | Vitest | API Routes |
| E2Eテスト | Playwright | 画面フロー |

## File Structure

```
tests/
├── unit/
│   ├── services/
│   └── validations/
├── integration/
│   └── api/
└── e2e/
    ├── auth.spec.ts
    └── shift-registration.spec.ts
```

## Commands

```bash
# ユニット・統合テスト
npm run test

# E2Eテスト
npm run test:e2e
```

## Vitest

### ユニットテスト例

```typescript
// tests/unit/validations/settings.test.ts
import { describe, it, expect } from 'vitest'
import { settingsSchema } from '@/lib/validations/settings'

describe('settingsSchema', () => {
  it('有効なデータを検証できる', () => {
    const validData = {
      monthStartDay: 1,
      calendarId: 'primary',
    }
    const result = settingsSchema.safeParse(validData)
    expect(result.success).toBe(true)
  })

  it('無効なmonthStartDayを拒否する', () => {
    const invalidData = {
      monthStartDay: 30,
      calendarId: 'primary',
    }
    const result = settingsSchema.safeParse(invalidData)
    expect(result.success).toBe(false)
  })
})
```

### 統合テスト例

```typescript
// tests/integration/api/settings.test.ts
import { describe, it, expect } from 'vitest'

describe('GET /api/settings', () => {
  it('認証済みユーザーの設定を取得できる', async () => {
    const response = await fetch('/api/settings', {
      headers: { Authorization: `Bearer ${testToken}` },
    })
    expect(response.status).toBe(200)
  })
})
```

## Playwright (E2E)

### テスト例

```typescript
// tests/e2e/auth.spec.ts
import { test, expect } from '@playwright/test'

test('Googleログインボタンが表示される', async ({ page }) => {
  await page.goto('/login')
  await expect(page.getByRole('button', { name: /Google/ })).toBeVisible()
})
```

## Best Practices

- テストは独立して実行可能に
- モックは最小限に
- 重要なユーザーフローをE2Eでカバー
- バリデーションロジックはユニットテストで網羅
