指定された Issue の完了確認を行ってください。

Issue: $ARGUMENTS

手順:
1. GitHub Issue または docs/issues/ から該当 Issue を確認
2. 「ゴール / 完了条件（Acceptance Criteria）」を確認
3. 各 AC の達成状況をコードベースから検証
4. 未達成項目があれば警告し、残作業を提示
5. すべて達成済みなら完了サマリを生成

出力形式:
```
## Issue #X 完了確認

### Acceptance Criteria 達成状況
- [x] AC1: 説明
- [x] AC2: 説明
- [ ] AC3: 説明（未達成の場合は理由を記載）

### 結論
- 完了 / 未完了（残作業あり）

### 残作業（未完了の場合）
- タスク1
- タスク2
```
