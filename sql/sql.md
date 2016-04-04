# SQLのメモ
## 削除系
### データ削除
```sql
DELETE FROM table;
```
```sql
TRUNCATE table;
```
### テーブル削除
```sql
DROP table;
```
  
## 追加
```sql
INSERT INTO table (column1, column2...) VALUES (value1, value2...);
```
  
## 更新
```sql
UPDATE table SET column1 = value1, column2 = value2 [WHERE ...];
```

## シーケンス
### シーケンス一覧
```sql
SELECT sequence_name FROM information_schema.sequences
```
### 値の設定
```sql
SELECT setval('hoge_seq', value);
```
