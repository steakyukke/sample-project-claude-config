コードベースの変更に基づいて、ドキュメントの更新が必要な箇所を特定してください。

対象: $ARGUMENTS

手順:
1. 最近の変更を確認
   - git diff または指定されたファイル/機能を確認

2. 関連ドキュメントの特定
   - docs/requirements.md - 要件定義
   - docs/architecture.md - アーキテクチャ
   - docs/database.md - データベース設計
   - docs/api.md - API設計

3. 差分の検出
   - コードと仕様書の差異を特定
   - 更新が必要な箇所をリストアップ

4. 更新案の提示
   - 具体的な修正内容を提案

出力形式:
```
## ドキュメント同期チェック

### 変更された機能
- 機能1
- 機能2

### 更新が必要なドキュメント

#### docs/04_api.md
- 箇所: `/api/shifts` エンドポイント
- 変更内容: 新しいクエリパラメータ `status` を追加
- 提案:
  ```markdown
  修正内容
  ```

#### docs/03_database.md
- 箇所: shifts テーブル
- 変更内容: 新しいカラム `status` を追加
- 提案:
  ```markdown
  修正内容
  ```

### 更新不要
- docs/01_requirements.md
- docs/05_sitemap.md
```
