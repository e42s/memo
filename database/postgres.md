#Postgresql関連のメモ
#### nullを除いて（後ろにして）ソートさせたい
```sql
SELECT hoge FROM hoge ORDER BY hoge NULLS LAST;
```
  
  
#### numberのカラムで0を除いて（後ろにして）ソートさせたい
```sql
SELECT (hoge = 0) AS is_zero, hoge FROM hoge ORDER BY is_ZERO ASC, hoge;
```
  
  
#### numberのカラムで0とnullを除いて（後ろにして）ソートさせたい
```sql
SELECT (hoge = 0) AS is_zero, hoge FROM hoge ORDER BY is_ZERO ASC NULLS LAST, hoge NULLS LAST;
```
  
  
#### textのカラムで空文字を除いて（後ろにして）ソートさせたい
```sql
SELECT (hoge = '') AS is_empty, hoge FROM hoge ORDER BY is_empty ASC, hoge;
```
  
  
#### textのカラムで空文字とnullを除いて（後ろにして）ソートさせたい
```sql
SELECT (hoge = '') AS is_empty, hoge FROM hoge ORDER BY is_empty ASC NULLS LAST, hoge NULLS LAST;
```
  
  
#### textのカラムで0と空文字とnullを除いて（後ろにして）ソートさせたい
```sql
SELECT (hoge = 0) AS is_zero, (hoge = '') AS is_empty, hoge FROM hoge ORDER BY is_zero ASC NULLS LAST, is_empty ASC NULLS LAST, hoge NULLS LAST;
```
  
#### テーブルに何も入ってなかったらインサートする
```sql
INSERT INTO hoge (id, name) SELECT 1, 'foo' WHERE NOT EXISTS (SELECT id FROM hoge);
```
