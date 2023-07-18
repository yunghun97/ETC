# 🐋 MongodDB 쿼리문 예시

1. Index 생성

```sql
-- db.collection.createIndex({column: sortType});
--  sortType
--  1 : Ascending 오름차순
-- -1 : Descending 내림차순

db.collection.createIndex({name: 1});
```
