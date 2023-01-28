# 👨‍💻 MongoDB Sharding

[Sharding Overview](#Sharding-Overview)
[Sharded Cluster](#Sharded-Cluster)  
[Shard Keys](#Shard-Keys)

~

## 🦧 Sharding Overview

- **Sharding 이란 여러 장비에 데이터를 분산하는 방법**
- MongoDB는 매우 큰 데이터 셋과 높은 데이터 처리량 수행을 지원하기 위해 sharding을 사용
- 일반적으로 시스템의 확장을 해결하기 위해 수직적 확장(Vertical scaling)과 수평적 확장(horizontal scaling)이 있으며, **MongoDB는 Sharding을 통해 수평적 확장(horizontal scaling)을 지원함.**

### Sharded Cluster 핵심 요소

- MongoDB sharded 클러스터는 다음과 같은 요소들로 구성됨
  1. shard: 각각의 shard는 sharded data 의 서브 셋을 포함. shard는 복제 셋으로 구성될 수 있음.
  2. mongos: mongos는 클라이언트 어플리케이션과 sharded 클러스터 사이의 인터페이스를 제공하는 **쿼리 라우터 역할**을 수행
  3. config servers: **클러스터를 위한 메타데이터와 환경설정 정보를 저장함.**
- MongoDB의 shards data는 coolection 레벨로 클러스터 내에 shards를 통해 collection 데이터를 분산함.

## ❗ Shard Keys

- Collection 내에 documents 를 분산하기 위해, MongoDB는 shard key를 사용하여 collection을 분할
- Shard key는 대상 collection의 모든 documents에 존재하는 불변의필드(또는 필드를)로 구성
- Collection 을 sharding 할 때 shard key를 선택하며, sharding 후에는 shard key 변경 불가(5.0버전 부터 변경 가능)
- Sharded collection은 단지 하나의 shard key 만을 가질 수 있음
- **Shard key의 선택은 sharded 클러스터의 성능, 효율성, 확정성에 영향을 미침**
  - 성능이 좋은 하드웨어와 인프라스트럭처를 가진 클러스터도 shard key의 선택에 의해 병목이 발생할 수 있음.
- Chunk는 sharded 데이터의 분할 단위로 default chunk size는 64 MB이며 늘리거나 줄일 수 있음.

## 🐔 Advatage of Sharding

- Reads / Writes
  - MongoDB는 sharded 클러스터 내에 shards를 통해 읽기와 쓰기 부하를 분산함.
  - 읽기와 쓰기 부하는 더욱 더 많은 shards를 추가함으로써 수평적으로 확장이 가능함.
  - Shard key를 포함한 쿼리에 대해, mongos는 특정 shard (또는 shards의 집합)에 질의할 수 있음
  - 이러한 수행이 클러스터 내에 모든 shard에게 broadcating하는 것보다 일반적으로 훨씬 효율적임.
- Storage Capacity
  - 데이터 셋이 커질수록 **추가적인 shards는 클러스터의 스토리지 용량을 증가시킴**
- High Availability
  - 만약, 하나 또는 여러 개의 shards가 불가능한 상태일지라도 sharded cluster는 부분적인 읽기/쓰기 작업을 계속적으로 수행할 수 있음.
  - 사용할 수 없는 shard의 데이터 하위 집합은 다운타임 동안 액세스할 수 없지만, 사용 가능한 shard에 대한 읽기 또는 쓰기는 계속 성공할 수 있음
  - 상용 환경에서는, 각각의 shards는 복제 셋으로 배포해야 중복성과 가용성을 증가시킬 수 있음.

## 🐢 Sharding 전 고려사항

- Sharded 클러스터 인프라 요구사항 및 복잡성에는 신중한 계획, 실행 및 유지 관리가 필요함.
- 클러스터 성능 및 효율성을 보장하려면 shard key를 선택할 때 주의 깊게 고려해야 함.
- Sharding 후에는 shard key를 변경할 수 없으며(5.0버전 부터 변경 가능), sharded collection을 unshard 하는 것도 불가능 함.
- 만약 쿼리가 shard key 또는 복합 shard key의 prefix를 포함하지 않는다면, mongos는 broadcast를 수행하며, sharded 클러스터 내에 모든 shards에게 질의 함.

## 🐇 Sharding Strategy

#### sharded 클러스터를 통해 데이터를 분산하는 데 두 가지의 sharding 전략을 지원함.

1. Hashed Sharding
2. Ranged Sharding

### Hashed Sharding

- Hashed Sharding은 각 chunk가 해시된 shard key 값에 따라 범위가 지정됨.
- Shard key의 범위가 가까울수록, 해시 된 값은 동일한 chunk에 있지 않을 것.
- 해시 값에 기반한 데이터 분포는 특히 shard key 가 단조롭게 변경되는 데이터 셋에서 보다 균일한 데이터 분포를 용이하게 함. **그러나 해시 분포는 shard key에 대한 범위 기반 쿼리가 단일 shard를 대상으로 할 가능성이 적어 클러스터 전체에서 broadcast 작업이 더 많이짐.**

### Sharding Strategy

- shard key 값에 따라 범위로 데이터를 나누는 것을 포함
- 각 chunk 는 shard key 값에 따라 범위가 지정됨
- 값들이 가까울수록 shard key의 범위는 동일한 chunk에 있을 확률이 높음
- **Ranaged sharding의 효율성은 shard key 선택에 달려 있음. 잘못 고려된 shard key는 데이터가 고르지 않게 분산되어 sharding 의 일부 이점을 무효화하거나 성능 병목 현상을 일으킬 수 있음.**
