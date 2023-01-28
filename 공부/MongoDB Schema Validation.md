# 🧱 MongoDB Schema Validation

[1. Behavior](#Behavior)  
[2. Restrictions](#Restrictions)  
[3. Bypass Document Validation](#Bypass-Document-Validation)

## ⛄Behavior

- Validation은 Update 또는 Insert 동안에 발생함.
- Collection에 대해 validation을 추가할 때, 존재하는 documents는 수정될 때까지 유효성 검사를 겪지 않음.

### Existing Documents

- validationLevel 옵션을 사용하여 존재하는 documents에 대해 통제 가능
- **Default로 validationLevel은 strict이며 MongoDB는 전체 Inserts, updates에 대해 validation rules을 적용,**
-

## 🍔 Restrictions

- admin, local 그리고 config 데이터베이스 내에 collectinos에 대해서는 validator를 지정할 수 없음
- system.\* collectino에 대해서 validator를 지정할 수 없음

## Bypass Document Validation

- bypassDocumentValidation 옵션을 사용하여 document validation을 우회할 수 있음
- 접근 제어가 활성화된 배포 환경을 위해, document validation을 우회하기 위해 인증된 사용자는 bypassDocumentValidation action을 가져야만 함.
- 내장된 dbAdmin, retore 역할이 이러한 action을 제공해 줌
- \*\*MongoDB 3.6 부터는 JSON Scheman validation을 지원하며, schema validatino을 수행하기 위한 수단으로 JSON Schema가 추천 됨
