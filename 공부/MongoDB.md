# 🍟 Mongo DB KT DS 강의

### 고성능, 고가용성, 쉬운 확장성을 제공하는 크로스 플랫폼, 문서 중심의 데이터베이스

## 😎 RDBMS와 비교

| RDBMS                      | MongoDB             |
| :------------------------- | :------------------ |
| Table                      | Collection          |
| Tuple/Row                  | Document            |
| Column                     | Field               |
| Table Join                 | Embedded Documents  |
| Primary Key                | '\_id' default 제공 |
| Database Server and Client | -                   |
| mysqld/oracle              | mongod              |
| mysql/sqlplus              | mongo, mongosh      |

## Database

- Collection을 위한 물리적인 컨테이네
- 하나의 MongoDB 서버는 여러 개의 database를 가질 수 있음

## Collection

- Documents에 대한 그룹, RDBMS의 테이블에 해당 됨
- 1개의 Collection은 1개의 database 내에 존재 함
- 1개의 collectnio 내에 documents는 다른 필드를 가질 수 있음.

## Document

- Field와 value 쌍으로 구성된 데이터 구조
- Field와 값은 다른 documents, arrays 내포 가능 함

` Sample document`

- 저장 시 binary json으로 저장 됨

```java
{
    name: "sue",        // <- field: value
    age: 26,
    atatus: "A",
    groups: ["news", "sports"]
}
```

## 🍕 MongoDB 특징

- Schema less (상품 카탈로그 같은 곳에서 사용하면 좋은 효과를 볼 수 있음.)
- No complex joins
- Deep query-ability
  - document-based 쿼리 언어를 사용하여 documents에 대한 동적 쿼리를 지원
- Ease of scale-out

## Why should use MongoDB

- Document Oriented Storage : 데이터는 JSON의 데이터 형식이 확장된 BSON 형태로 저장됨.
- Index 사용 가능
- Replication & High Availability
- Auto-Sharding (키 설계가 중요 -> MongoDB가 Balance 프로그램을 통해 자동으로 Sharding 해줌)
- Rich Queries

## Where should use MongoDB

- Big Data
- Content Management and Delivery
- Mobile and Social Infrastructure
- Data Hub

# Database and Collectoins

```sql
-- myNewDB가 없으면 새로 생성을 해준다.
use myNewDB
-- 데이터베이스를 생성하더라도 Collection이 존재하지 않으면 데이터 베이스 목록조회 에서 보이지 않음(show dbs)
```

# Documents

## Filed명에 대한 제약 사항

- 문자(String) 이여야 함
- \_id는 기본 키(Primary key)로 사용하도록 예약되어 있으며, 그것의 값은 Collection 내에서 유일해야만 함.
- $ 문자로 시작할 수 없으며 Dot(.) 및 Null 문자를 포함할 수 없음.

## Dot Notation

- MongoDB는 array에 대한 elements 와 embedded document의 fields를 접근하기 위해 dot notation을 사용함.
- Arrays
  - <array>.<index> -> zero-based index

```java
{
    contribs: ["a1", "b2", "c3"] // c3 는 "contribs.2" 에 해당함 (다른 Key 값이 없으므로)
}

{
    name: {first: "Tom", last: "Job"} // name.last
}
```

## Document 제약사항

- Document Size Limit
  - **최대 BSON doccument 사이즈는 16MB.**
  - 최대 document 사이즈는 단일 document가 과도한 양의 RAM을 사용할 수 없도록 하거나 전송 중에 과도한 대역폭을 사용할 수 없게 함
  - MongoDB는 최대 크기보다 큰 문서를 저장하기 위해 GridFS API를 제공함.
- Document Filed Order
  - MongoDB는 다음과 같은 경우를 제외하고 **쓰기 연산에 따라 docuemnt filed의 순서를 유지함**
  - **\_id field는 항상 document의 첫 번째 순서임**
  - Filed명 변경이 포함된 업데이트로 인해 document의 filed의 순서가 변경될 수 있음(2.6 이전 버전)
- The \_id Field
  - MongoDB는 Collectino에 저장된 각 document에 기본 키 역할을 하는 고유한 \_id field가 필요함
  - 만약, **\_id field 를 생략하고 document 를 입력하면, MongoDB 드라이버가 자동적으로 \_id filed 에 대해서 ObjectId를 생성함**
  - \_id field 동작 및 제약 조건
    - 기본적으로, **collection을 생성하는 동안 \_id field에 대해 unique 인덱스를 생성함**
    - \_id field는 array가 아닌 모든 BSON 데이터 형식의 값을 포함할 수 있음 (주의 : 복제 기능을 확실히 하려면, \_id field에 BSON 정규 표현식 유형의 값을 저장하지 말 것)

## MongoDB 서버 실행 방법

```bash
mongod --auth --port 27017 -dbpath D:\data\db
```

## 인증 방법

```bash
mongosh --username "myUserAdmin" --password "abc123" --authenticationDatabase admin
```

## Shutdown

```sql
use admin

db.shutdownServer()

-- MongoNetworkError: connection 5 to 127.0.0.1:27017 closed
```

## ✨ MongoDB 계정 만들기

```bash
use admin

db.createUser(
  {
    user: "myUserAdmin",
    pwd: passwordPrompt(),
    roles: [
      {
        role : "userAdminAnyDatabase", db: "admin"
      }
    ]
  }
)

# 입력 후 paswordPrompt에서 비밀번호 입력
```

## Mongo DB 인증 방법

1. 접속 하기전 계정 정보를 넣어서 접속

```bash
mongosh --username "myUserAdmin" --authenticationDatabase admin

# password를 커멘드에서 바로 작성 시 로그가 남을 수 있으므로 입력 후 password 입력
```

2. 접속 후 인증 받기

```bash
db.auth("아이디", "비번")
```
