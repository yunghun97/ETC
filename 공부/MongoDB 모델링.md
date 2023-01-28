# ⛄MongoDB 모델링

### 모델링 방법

1. References
2. Embedded Data

## 1. Reference

- References는 **데이터 사이의 관계를 저장**
- **정규화된 데이터 모델**
- 데이터의 품질, 유연성 좋음

## 2. Embedded Data

- **document내에 관계된 데이터를 저장**하면서 데이터 사이의 관계를 관리
- document의 **field 또는 array 내부에 document를 내포하는 것이 가능**
- document 원자성에서 장점, 성능 좋음

## 🐳 Atomicity of Write Operatoins

- MongoDB에서 쓰기 수행은 document 레벨의 atomic
- 한 번의 쓰기 수행이 자동적으로 하나 이상의 collectino 또는 하나 이상의 document에 영향을 미치지는 못함.
- Embedded data로 데이터 모델은 반 정규화한하는 것은 하나의 document내에 표현된 엔티티를 위한 관계된 데이터 모두를 결합함.
- 데이터를 정규화하는 것은 데이터를 ㅕ러 개의 collection으로 나누게 되고, 완전한 atomic이 아닌 여러 번의 쓰기 수행이 필요하게 됨.
- **유연성과 atomic에 대한 규현 있는 스키마 설계가 중요함**

## 👨‍💻 Data Use and Performance

- 데이터 모델은 설계할 때, 애플리케이션들이 데이터베이스를 어떻게 사용할 지를 고려해야 함.
- **애플리케이션이 최근에 입력된 documents를 사용한다면 Capped Collections 사용을 고려해라.**
  - Capped Collectinos
    - 사용자가 Document 삭제 불가능
    - 변경은 가능하지만 더 큰 사이즈로 변경할 수 없음
    - Sharding X
- **애플리케이션이 Collection에 대해 주로 읽기 수행이 필요하다면, 공통적인 쿼리를 지원하기 위해 인덱스를 추가**하는 것이 성능을 향상시킬 수 있음

# 🐔 Data Modeling Concepts

```
Documents의 구조에 대한 핵심 고려사항은 'embed' or references' 사용을 결정하는 것
```

## 🤠 Embedded Data Models

- '반정규화' 모델로 알려져 있으며 MongoDB의 rich documents의 이득을 얻을 수 있음
- 엔티티들 간의 관계를 포함
- **일반적으로, 읽기 수행에더 좋은 성능을 제공하며 한번의 atomic 쓰기 수행으로 관계된 데이터 수정이 가능함.**
- **Embedded data model을 사용하는 경우**
  - 엔티티들 사이의 관계를 포함하고 있으며, 내포된 documents가 **1:1 관계**인 경우
  - **엔티티들의 one-to-many 관계를 갖고 있는 경우.** 이러한 관계에서는 "many"또는 자식 documents는 항상 "one" 또는 여러 개의 부모 documents의 내용과 함께 보여짐

## 💀 Nomalized Data Models

- 정규화 데이터 모델은 **documents 사에에 references를 사용하여 관계를 기술**함.
- **References 는 embedding 보다 더 높은 유연성 제공**
- **정규화된 데이터 모델을 사용하는 경우**
  - 내포하는 것이 데이터 중복을 초래하지만, **중복의 구현 보다 큰 충분한 읽기 성능 이득을 제공하지 못할 경우**(embed에서 데이터 중복이 너무 많거나 어떠한 이유로 인해 성능상의 이점이 없는 경우)
  - 더욱 복잡한 **many-to-many 관계를 표현하기 위해**
  - **큰 계층적 데이터 집합을 모델링 하기 위해**

## 🦧 Atomicity(원자성)

-

## 🐷 Sharding

- 수평적 확장을 제공하기 위해 sharding을 사용함.
- Sharding은 데이터베이스 내에 collection의 documents를 많은 mongod 인스턴스 또는 shards를 통해 분산하기 휘새 collection을 파티션하는 것을 가능케 함.
- Sharded collection에 대한 데이터와 애플리케이션 트래픽을 **분산하기 위해 shard key를 사용**함.
  - **적절한 shard key 선택이 성능에 있어 중요하기 때문에, shard key로 사용되는 field를 주의깊게 고려 필요.**
- 4.2 이전버전에서는 Shard Key는 불변이였음
- 5.0 부터는 collection을 reshard 할 수 있음.

## 🐢 Indexes

- **공통 쿼리들을 위한 성능을 향상시키기 위해 인덱스를 사용**
- MongoDB는 **'\_id' field에 대해 자동적으로 unique index를 생성함.**
- 인덱스 생성 시 고려 사항
  - 각각의 인덱스는 최소한 8kb의 데이터 공간을 필요로 함.
  - 인덱스를 추가하는 것은 때떄로 쓰기 수행에 대한 성능에 부정적인 영향을 끼침.
  - 쓰기에 비해 읽기 비율이 높은 collectinos은 인덱스를 추가하면 종종 이득이 있음. 인덱스들은 인덱스가 없는 읽기 수행에 영향을 끼치지 않음
  - 활성화될 때, 각각의 인덱스틑 디스크 공간과 메모리를 소비함, 이러한 사용은 중요할 수 있고, 용량 계획, 특히 working set 사이즈를 넘는 지에 대한 고민 때문에 추적되어야만 함.

## Capped Collection

- index에 대한 오버헤드 없이 더 많은 입력에 대한 높은 처리량을 지원함.
