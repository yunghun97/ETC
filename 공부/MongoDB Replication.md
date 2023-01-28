# 🧱 MongoDB Replication

- MongoDB에서 복제 셋은 동일한 데이터 셋을 유지하는 mongod 프로세스들의 하나의 그룹임
- 복제 셋은 데이터의 중복성과 고 가용성을 제공
- **Replication은 중복성과 데이터 가용성을 증가시켜주며, 다른 데이터베이스 서버들에 여러 개의 데이터 복사본을 가짐으로 인해 하나의 데이터베이스 서버의 장애 상황에 대해 fault tolerance를 제공함**
- Replicatino은 또한 다른 서버들에 읽기를 수행하여 읽기 성능을 향상시킬 수 있음
- 다른 데이터 센터들 내에 데이터의 복사본들을 유지하는 것이 분산된 애플리케이션들을 위한 데이터의 지역성과 가용성을 증가시킬 수 있음
- DR(Diaster recovery), Reporting 또는 백업과 같은 목적으로 추가적인 데이터 복사본을 유지할 수 있음.

## MongoDB 에서의 복제

- 동일한 복제 셋 내에 Primary 노드는 1개 이며, 다른 노드들은 Secondary 노드임
- Primary 노드는 모든 쓰기 수행을 받음
- Primary 노드 장애 시에 선출 과정을 거쳐 Secondary 중에 하나의 노드가 새로운 Primary로 전환 됨.
- Primary 로부터의 수행들을 비동기적으로 Secondary에 적용함(Asynchronous Replication)

## Primary 구분

```bash
rs.status();

status: 1 # primary
status: 2 # secondary
```
