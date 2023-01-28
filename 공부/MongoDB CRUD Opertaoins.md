# 🧙‍♀️ MongoDB CRUD Operataions

## 🚗 Create Opertaions

- Create 또는 Insert 수행은 collection에 documents를 추가함
  - 만약 collection이 존재하지 않으면, insert 수행이 collection을 생성함
- MongoDB는 collection에 documents를 **입력하기 위해 다음 메소드를 제공**함. (version 3.2 부터)
  - db.collection.insertOne()
  - db.collection.insertMany()
- **Insert 수행은 하나의 collection을 대상으로 하며**, 모든 쓰기 수행은 하나의 document 레벨에 대해 원자성.

```bash
db.users.insertOne(  #   <- collection
    {      #  <- document
        name: "use", #  <- field: value
        age: 27, #  <- field: value
        status: "playing" # <- field: value
    }
)

db.users.insertMany(
  [
    {
      name: "lee",
      age: 45,
      status: "pending",
      mobile: "01059595959"
    },
    {
      name: "son",
      age: 31,
      status: "active",
      mobile: 01077772323 # 동일한 필드에 자료형이 달라도 상관없이 들어감
    }
  ]
)
```

## 🚓 Read Operations

- Read 수행은 collection 으로부터 documents를 추출함
  - documents를 위해 collection을 쿼리함
- MongoDB는 collectino으로부터 documents를 읽기 위해 다음 메소드를 제공함,
  - db.collection.find()
- 반환할 documents 를 식별하기 위해 쿼리 필터 또는 기준을 지정할 수 있음.

```sql
db.users.find(  -- <- collection
    {
        { age: {$gt: 18} } -- <- query criteria RDB Where 절
        { name: 1, address: 1} -- <- projections // RDB select 절   name : 0 <- name은 select 하지 않음
    }
).limit(5)   -- <- cursor modifier


SELECT name, address
FROM users
WHERE age > 18
AND rownum <= 5
```

### 정렬

```sql
db.customer.find(
  {age : {$gt: 30}},
  {name : 1, age : 1, status: 1}).sort({"age": 1})  -- : 1 내림차순 정렬 : -1 오름차순 정렬
```

## 🍹 Query Documents

- Select All Documents in a Collection

```sql
db.inventory.find({})
SELECT * FROM inventory
```

- Specify Equality Condition

```sql
db.inventory.find( {status: "D"})
SELECT * FROM inventory i where i.status = "D";
```

### 비교 연산자

```bash
$eq(=), $gt(>), $gte(>=), $lt(<), $lte(<=), $ne(<>), $in(in), $nin(not in)

```

### AND 연산자

- 암묵적으로 ,(콤마)는 논리적 AND 연산을 수행함.

```bash
db.customer.find(
    {status: "active", age : { $gt: 20}}
)
```

### OR 연산자

- $or 연산자를 사용하여 논리적 OR 연산을 수행함

```sql
db.customer.find(
  {$or: [
    {status: "active"},
    {age: {$gt: 30}}
      ]
    }
  )
```

### Like 검색

- SQL 문의 LIKE 검색처럼 MongoDB에서도 // 기호를 활용하여 LIKE 검색 가능
- **활용 예시**
  - db.store.find({addr: /강남구/}) - '강남구'가 포함된 모든 문자열 검색
  - db.store.find({addr: /^서울/}) - 서울로 **시작하는** 모든 문자열 검색
  - db.store.find({addr: /1동$/}) - '1동'으로 **끝나는** 모든 문자열

## 🦉 Update Documents

- MongoDB는 collection에 documents를 수정하기 위해 다음 메소드를 제공함
  - db.collection.updateOne(<filter>, <update>,<options>)
  - db.collection.updateMany(<filter>, <update>,<options>)
  - db.collection.replaceOne(<filter>, <replacement>,<options>)
- Update Documents in a Collection
  - 필드 값을 수정하기 위해 $set과 같은 update 연산자를 제공
  - $set과 같은 update 연산자는 필드가 존재하지 않으면 필드를 생성함.

### Replace a Document

- '\_id' 필드를 제외하고 하나의 document의 전체 내용을 대체하기 위해 db.collection.replaceOne() 메소드의 두 번째 인수값인 새로운 document로 대체함.
- document 를 대체할 때 필드/값 쌍으로 구성되어야만 함. (update 연산자를 포함하지 말 것!)
- 대체할 document는 원래 document와 다른 필드들을 갖는 것이 가능함
- '\_id' 필드는 불변하기 때문에 생략 가능, 그러나, 만약 '\_id' 필드를 포함한다면 현재 값과 같은 값을 가져야만 함.

## Delete Documents

- MongoDB에 collection에 documents를 삭제하기 위해 다음 메소드를 제공함. (version 3.2 부터)
  - db.collection.deleteMany()
  - db.collection.deleteOne()
- Delete All Documents

  - db.collection.deleteMany({})

- Delete All Documents that Match a Comdition
  - 삭제할 documents를 식별하기 위해 기준 또는 필터를 지정할 수 있음
  - 필터는 읽기(find) 수행과 같은 구문을 사용
- **Delete 수행은 collectino의 모든 documents를 삭제하더라고 인덱스를 삭제하지 않음**

### Retryable Writes

- MongoDB 3.6 버전 이상부터 지원 (4.2부터는 Default)
- 네트워크 및 기타 오류 발생 시 애플리케이션에서 따로 구현할 필요없이 MongoDB Driver단에서 사용
