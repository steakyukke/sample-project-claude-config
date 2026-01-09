エラーの原因調査とデバッグを行ってください。

## 入力情報

```
$ARGUMENTS
```

**入力形式について:**
- コマンドとエラーログを一緒に貼り付けてください
- 例: `npm run build` の出力、TypeScriptエラー、ランタイムエラーなど

## デバッグ手順

1. **入力の解析**
   - 実行されたコマンドを特定
   - エラーメッセージ・スタックトレースを抽出
   - エラーの種類を判別（ビルドエラー、型エラー、ランタイムエラー、etc.）

2. **関連ファイルの調査**
   - エラーに記載されたファイルを Read で確認
   - 依存関係・インポート先を追跡

3. **原因の特定**
   - 最も可能性の高い原因を特定
   - 必要に応じて複数の仮説を立てる

4. **修正の実施**
   - 具体的なコード修正を実行
   - 修正後、同じコマンドを再実行して確認

## 出力形式

```markdown
## デバッグ結果

### エラー概要
- コマンド: `npm run build`
- エラー種類: TypeScript Error (TS2322)
- 発生箇所: src/lib/services/xxx.ts:42

### 原因
原因の説明

### 修正内容
- ファイル: src/lib/services/xxx.ts
- 変更内容: 説明

### 確認結果
修正後のコマンド実行結果
```

## よくあるエラーパターン

| エラー | 主な原因 | 対処法 |
|--------|----------|--------|
| `Cannot find module` | パッケージ未インストール | `npm install` |
| `Type 'X' is not assignable` | 型の不一致 | 型定義の修正 |
| `ReferenceError: X is not defined` | 変数の未定義 | インポート追加 |
| `NEXT_REDIRECT` | redirect()の使用 | try-catch不要、正常動作 |
| `Hydration failed` | サーバー/クライアント不一致 | useEffect で動的処理 |
| `relation "X" does not exist` | DBマイグレーション未実行 | `npm run db:push` |
