# 📡 Kafka
## Kafka 특징

### 1. 빠른 데이터 수집이 가능한 높은 처리량
HTTP 기반으로 전달되는 이벤트일지라도 이벤트가 카프카로 처리되는 응답시간은 불과 한 자릿수의 ms단위로 처리됐습니다. 이를 통해 광범위한 데이터 흐름 도입 가능
### 2. 순서 보장
이벤트 처리 순서가 보장되면서, 엔티티 간의 유효성 검사나 동시 수정 같은 무수한 복잡성 들이 제거됨으로써 구조 또한 매우 간결하다.
### 3. 적어도 한번 전송 방식
분산 네트워크 환경에서 데이터 처리에서 중요한 모범 사례는 **멱등성**입니다. 멱등성이란 동일한 작업을 여러 번 수행하더라도 결과가 달라지지 않는 것을 의미합니다.  
따라서 프로듀서가 재전송을 하더라도 데이터 변화는 일어나지 않습니다. 차선책인 '적어도 한 번' 전송 방식을 사용하더라도 간혹 이벤트들이 중복 발생할 수는 있으나 누락 없는 재전송이 가능하므로 메시지 손실에 대한 걱정이 사라집니다. 또한 백엔드 시스템들이 중복 메시지 처리가 가능하도록 허용된다면 복잡한 트랜잭션 처리가 필요없게 됩니다. 이로인해 아키텍처는 더욱 단순해지고 처리량 또한 높아집니다.
### 4. 자연스러운 백프레셔 핸들링
카프카의 클라이언트는 pull 방식을 사용합니다.  
pull 방식의 장점은 클라이언트 자기 자신의 속도로 데이터를 처리할 수 있다.  
push 방식은 브로커가 보내는 속도에 의존해야 한다는 한계가 있습니다.  
성능과 편리함에 집중하고자 풀 방식을 채탣한 카프카 클라이언트는 복잡한 피드백이나 제한의 요구사항이 사라져 간단하고 편리하게 클라이언트를 구현할 수 있다.
### 5. 강력한 파티셔닝
카프카의 파티셔닝 기능을 이용하면, 논리적으로 토픽을 여러 개로 나눌 수 있습니다. 또한 파티션에 적절한 키를 할당하기 위한 여러 고려사항이 있지만, 각 파티션들을 다른 파티션들과 관계없이 처리할 수 있으므로 효과적인 수평 확장이 가능해졌습니다.
### 6. ETC
로그 컴팩션 기능을 통해 스냅샷 역할이 가능해졌고, 새로운 애플리케이션이 나중에 읽어가는 방식도 문제 없이 처리가능  
프로듀서와 컨슈머가 완벽하게 분리된 비동기 방식을 사용함에 따라 애플리케이션의 병목 현상을 정확하게 파악할 수 있었고, 모니터링을 통해 지연에 대한 문제를 빠르게 해결 가능
  
## 🐬 이벤트 버스 vs 카프카
이벤트 버스 : 개발자가 지정한 개의 컴포넌트 간에 데이터를 주고받을 수 있는 방법 & 이벤트 버스 사용 시 상위-하위 컴포넌트 구조를 유지하지 않아도 다른 컴포넌트로 데이터 전달이 가능하다.
### 이벤트 버스
- 이벤트 버스는 서빙 레이어와 스토리지 레이어가 분리되어 있어 추가적인 홉이 필요하다.
- 이벤트 버스는 fsync()로 블로킹
- 더 많은 하드웨어 자원 필요
### 카프카
- 카프카는 하나의 프로세스에서 스토리지와 요청을 모두 처리
- OS에 의존에 백드라운드로 fsync()를 처리 제로카피를 사용
- 비교적 적은 하드웨어 자원
> [제로카피](https://soft.plusblog.co.kr/) : CPU의 개입을 받지 않고 한 메모리의 영역에서 다른 메모리의 영역으로 데이터를 카피하는 작업을 말한다.  
이는 CPU 사이클을 절약하여서 네트워크나 SSD와 같이 빠른 데이터가 필요한 시스템에서 유용하게 이용된다.
### 카프카 Producer, Consumer, Broker
![image](https://user-images.githubusercontent.com/71022555/165877810-1156dd1b-913f-410b-9538-eb1cd1503c47.png)
---
## 카프카 주요 특징
### 1. 높은 처리량과 낮은 지연시간
컨플루언트 블로그에 메시징 시스템의 성능 비교자료에서는 카프카, 펄사, 래빗 총 세 가지 메시징 시스템 비교 시 처리량은 카프카가 가장 높고 응답속도는 래빗 MQ가 가장 빠르다 하지만 처리량과 응답속도를 함께 비교하면 카프카가 압도적이다.
### 2. 높은 확장성
kafka는 손쉬운 확장이 가능하도록 잘 설계 되었습니다. 
### 3. 고가용성
kafka 초기 버전에서는 메시지를 빠르게 처리하는 것이 목표였는데 시간이 지나면서 고가용성 측면도 중요하게 여기게 됐습니다. 2013년 클러스터 내 리플리케이션(replication) 추가했고 이를 통해 고가용성을 확보했다.
### 4. 내구성
프로듀서는 kafka로 메시지를 전송할 때 프로슈서의 acks라는 옵션을 조정해서 메시지의 내구성을 강화할 수 있다. 메시지 내구성을 원한다면 옵션을 acks=all로 사용할 수 있습니다. 이렇게 프로듀서에 의해 kafka로 전송되는 모든 메시지는 안전한 저장소인 kafka의 로컬 디스크에 저장됩니다. 버그나 장애 발생 시 과거의 메시지를 불러와 재처리할 수 있습니다. 여러 브로커에 메시지가 저장되므로 브로커 한 대가 망가져도 복구가 가능하다.
### 5. 개발 편의성
kafka는 메시지를 전송하는 역할을 하는 **프로듀서(producer)**와 메시지를 가져오는 역할을 하는 **컨슈머(consusmer)**가 완벽하게 분리되어 동작하고 서로 영향을 주지도 받지도 않습니다. 이러한 구성에 따라 각자의 역할을 개발하여 사용하면 됩니다.  
또한 개발 편의성을위해 kafka는 kafka connect와 스키마 레지스트리(Schema Registry)를 제공하는데. Schema Registry는 kafka를 사용하는 많은 개발자가 데이터 활용 보다는 데이터를 파싱하는 데 많은 시간을 소모하는 것을 보완하고자 스키마를 정의해서 사용할 수 있도록 개발된 애플리케이션입니다. kafka connect는 producer와 consumer를 따로 개발하지 않고도 kafka와 연동해 손쉽게 소스와 싱크로 데이터를 보내고 받을 수 있는 별도의 애플리케이션입니다. kafka connect는 엘라스틱서치(Elasticsearch), HDFS등 다양한 소스와 싱크를 제공하므로 개발 편의성을 높일 수 있습니다.
### 6. 운영 및 관리 편의성
kafka는 운영 및 관리 편의성, 안정적인 운영을 위한 모니터링 등은 관리자라면 반드신 알고 있어야 하는 사항입니다. 모니터링에 필요한 모든 내용과 운영 노하우가 담긴 Grafana 대시보드를 제공
  
---
## Kafka의 성장
### 리플리케이션(replication) 기능 추가 v0.8
### 스키마 레지스트리(Schema Registry) v0.8.2
### 카프카 커넥트(kafka connect) 공개 v0.9
### 카프카 스트림즈(kafka streams) 공개 v0.10
### KSQL 공개
### 주피커 의존성에서 해방(v3.0)
  
---
## Kafka 구성 주요 요소
> **주키퍼(ZooKeeper)** : 아파치 프로젝트 애플리케이션 이름입니다. 카프카의 메타데이터 관리 및 브로커의 정상상태 점검 담당  

> **카프카(Kafka) OR 카프카 클러스터(Kafka Cluster)** 아파치 프로젝트 애플리케이션 이름입니다. 여러 대의 브로커로 구성한 클러스터를 의미합니다.  

> **브로커(Broker)** : 카프카 애플리케이션이 설치된 서버 또는 노드  

> **프로듀서(Producer)** : 카프카로 메시지를 보내는 역할을 하는 클라이언트를 총칭  

> **컨슈머(Comsumer)** : 카프카로 메시지를 꺼내가는 역할을 하는 클라이언트를 총칭합니다.  

> **토픽(Topic)** : 카프카는 메시지 피드들을 토피긍로 구분하고, 각 토픽의 이름은 카프카 내에서 고유합니다.  

> **파티션(Partition)** 하나의 토픽이 한 번에 처리할 수 있는 한계를 높이기 위해 토픽 하나를 여러 개로 나눠 병렬처리가 가능하게 만들 것을 파티션이라고 합니다.  
적절한 파티션 개수 참고 사이트 : [컨플루언트](https://eventsizer.io)  

> **세그먼트** 프로듀서 Producer로 전송한 메시지는 각각의 파티션에 저장되어 있다. 각 메시지들은 세그먼트 라는 로그 파일의 형태로 브로커의 로컬디스크에 저장됩니다.  
개념 : Partition[세그먼트0, 세그먼트1, 세그먼트2]
# 프로듀서
프로듀서는 카프카의 토픽으로 메시지를 전송하는 역할을 담당합니다.
### 프로듀서 디자인
![image](https://user-images.githubusercontent.com/71022555/165867616-e7fba351-180d-44ff-abdf-41f079557afc.png)  
##### (https://dzone.com/articles/take-a-deep-dive-into-kafka-producer-api)  
- ProducerRecord라고 표시된 부분은 kafka로 전송하기 위한 실제 데이터이며, 레코드는 Topic, Partition, Key, Value로 구성됩니다.
    - Topic, Value(메시지 내용)은 필수 값
    - 레코드의 Partition과 특정 Partition에 레코드를 정렬하기 위한 레코드의 Key는 선택사항 이다.
- 각 레코드들은 Producer의 send() 메소드를 통해 Serializer, Partitioner를 거치게 됩니다.
    - 레코드 선택사항인 Partition 지정 시 Partitioner는 아무 동작도 하지 않고 지정된 Partition으로 레코드를 전달합니다.
    - Partition을 지정하지 않은 경우 key를 가지고 Partition을 선택해 레코드를 전달
        - 기본적으로 라운드 로빈(round robin(순서대로 시간단위로 CPU를 할당))
- 이렇게 Producer 내부에서는 send() 메소드 동작 이후 레코드들을 Patition별로 잠시 모아두게 됩니다.
    - 레코드를 모아두는 이유는 Producer가 Kafka로 전송하기 전, 배치 전송을 하기 위함입니다.
    - 전송을 실패하면 재시도를 하게되며 지정된 횟수 초과시 최종 실패 전달, 전송 성공시 metaData를 리턴한다.
### 프로듀서의 주요 옵션
|프로듀서 옵션|설명|
|:---|:---|        
|bootstrap.server|Kafka 클러스터는 클러스터 마스터라를 개념이 없으므로, 클러스터 내 모든 서버가 클라이언트의 요청을 받을 수 있습니다. 클라이언트가 카프카 클러스터에 처음 연결하기 위한 host와 포트 정보를 나타냅니다.|
|client.dns.lookup|하나의 호스트에 여러 IP를 매핑해 사용하는 일부 환경에서 클라이언트가 하나의 IP와 연결하지 못할 경우에 다른 IP로 시도하는 설정입니다. use_all_dns_ips가 기본값으로, DNS에 할당된 호스트의 모든 IP를 쿼리하고 저장합니다. 첫 번째로 IP 접근이 실패하면, 종료하지 않고 다음 IP로 접근을 시도합니다. resolve_canonical_bootstrap_servers_only 옵션은 커버로스(kerberos)환경에서 FQDN을 얻기 위한 용도로 사용됩니다.|
|acks|Producer가 Kafka Topic의 리더 측에 메시지를 전송한 후 요청을 완료하기를 결정하는 옵션입니다. 0,1, all(-1)로 표현하며 0 : 빠른 전송을 의미하지만, 일부 메시지 손실 가능성이 있습니다. 1 : 리더가 메시지를 받았는지 확인하지만, 모든 팔로워를 전부 확인하지는 않습니다. 대부분 기본값으로 1을 사용합니다. all : 팔로워가 메시지를 받았는지 여부를 확인합니다. 다소 느릴 수는 있지만, 하나의 팔로워가 있는 한 메시지는 손실되지 않습니다.|
|buffer.memory|Producer가 Kafka 서버로 데이터를 보내기 위해 잠시 대기(배치 전송이나 딜레이 등)할 수 있는 전체 메모리 바이트(byte)입니다.|
|compression.type|Producer가 메시지 전송 시 선택할 수 있는 압축 타입입니다. none, gzip, snappy, liz4, zstd 중 원하는 타입 선택 가능|
|enable.idempotence|설정을 true로 하는 경우 중복 없는 전송이 가능하며, 이와 동시에 max.in.flight.requests.per.connection은 5이하 retries는 0 이상, acks는 all로 설정해야 합니다.|
|max.in.flight.requests.per.connection|하나의 커넥션에서 Producer가 최대한 ACK 없이 전송할 수 있는 요청 수 입니다. 메시지의 순서가 중요하다면 1로 설정할 것을 권장하시만, 성능은 다소 떨어집니다.|
|retries|일시적인 오류로 인해 전송에 실패한 데이터를 다시 보내는 횟수 입니다.|
|batch.size|Producer는 동일한 파티션으로 보내는 여러 데이터를 함께 배치로 보내려고 시도합니다. 적절한 배치 크기 설정은 성능에 도움을 줍니다.|
|linger.ms|배치 형태의 메시지를 보내기 전에 추가적인 메시지를 위해 기다리는 시간을 조정하고, 배치 크기에 도달하지 못한 상황에서 linger.ms 제한 시간에 도달했을 때 메시지를 전송합니다.|
|transactional.id|'정확히 한 번 전송'을 위해 사용하는 옵션이며, 동일한 TransactionalId에 한해 정확히 한 번을 보장합니다. 옵션을 사용하기 전 enable.idempotence를 true로 설정해야 합니다.|

### Producer 메시지 보내고 확인하지 않기 예제
```java        
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;

import java.util.Arrays;
import java.util.Properties;

public class ConsumerAuto {
    public static void main(String[] args) {
        Properties props = new Properties(); //Properties 오브젝트를 시작합니다.
        props.put("bootstrap.servers", "도메인 OR Public IP:9091,도메인 OR Public IP:9092,도메인 OR Public IP:9093"); //브로커 리스트를 정의합니다.
        props.put("group.id", "peter-consumer01"); //컨슈머 그룹 아이디 정의합니다.
        props.put("enable.auto.commit", "true"); //자동 커밋을 사용합니다.
        props.put("auto.offset.reset", "latest"); //컨슈머 오프셋을 찾지 못하는 경우 latest로 초기화 합니다. 가장 최근부터 메시지를 가져오게 됩니다.
        props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer"); //문자열을 사용했으므로 StringDeserializer 지정합니다.
        props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props); //Properties 오브젝트를 전달하여 새 컨슈머를 생성합니다.
        consumer.subscribe(Arrays.asList("peter-basic01","chat-test")); //구독할 토픽을 지정합니다.

        try {
            while (true) { //무한 루프 시작입니다. 메시지를 가져오기 위해 카프카에 지속적으로 poll()을 하게 됩니다.
                ConsumerRecords<String, String> records = consumer.poll(1000); //컨슈머는 폴링하는 것을 계속 유지하며, 타임 아웃 주기를 설정합니다.해당 시간만큼 블럭합니다.
                for (ConsumerRecord<String, String> record : records) { //poll()은 레코드 전체를 리턴하고, 하나의 메시지만 가져오는 것이 아니므로, 반복문 처리합니다.
                    System.out.printf("Topic: %s, Partition: %s, Offset: %d, Key: %s, Value: %s\n",
                            record.topic(), record.partition(), record.offset(), record.key(), record.value());
                }
            }
        } catch (Exception e){
            e.printStackTrace();
        } finally {
            consumer.close(); //컨슈머를 종료합니다.
        }
    }
}

/*
dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka-clients</artifactId>
    <version>2.7.0</version>
</dependency>
*/
```

### 동작 과정
1. Properties 객체 생성
2. 브로커 리스트 정의
3. 메시지 Key, Value는 문자열 타입이므로 Kafka의 기본 StringSerializer를 지정
4. Properties 객체를 전달해 새 프로듀서 생성
5. ProducerRecord 객체 생성
6. send() 메소드를 사용해 메시지를 전송한 후 get() 메소드를 통해 Kafka의 응답을 기다립니다. -> 자바 Future 객체로 RecordMetadata를 리턴받지만, 리턴 값을 무시하므로 전송 성공 여부는 알 수 없다.
7. Kafka 브로커에게 메시지를 전송한 후의 에러는 무시자히만, 전송 전에 에러가 발생하면 예외를 처리할 수 있음
8. Producer 종료

# 컨슈머(Consumer)
### Kafka의 토픽에 저장되어 있는 메시지를 가져오는 역할을 담당합니다. 
### 단순하게 메시지를 가져오는 것 같지만 내부적으로 Consumer 그룹, 리밴런싱 등 여러 동작을 수행합니다.

## 컨슈머의 기본 동작
1. Producer가 Consumer Topic으로 메시지를 전송하면 해당 메시지들을 브로커들의 로컬 디스크에 저장됩니다. 그리고 우리는 컨슈머를 이용해 Topic에 저장된 메시지를 가져올 수 있습니다.
2. Consumer 그룹은 하나 이상의 Consumer들이 모여있는 그룹을 의미하고 Consumer는 반드시 컨슈머 그롭에 속하게 됩니다.
3. 그리고 이 Consumer 그룹은 각 Partition의 리더에게 Kafka Topic에 저장된 메시지를 가져오기 위한 요청을 보냅니다.
4. 이때 Partition의 수와 Consumer 수(하나의 컨슈메 그룹 안에 있는 컨슈머 슈)는 1:1로 매핑되는 것이 이상적
    - Consumer 수가 Partition 수보다 더 많다고 해서 더 빠르게 Topic 메시지를 빠르게 가져오거나 처리량이 높아지는 것이 아니라 더 많은 Consumer가 대기 상태로 존재
    - 간혹 액티브/스탠바이 개념으로 추가 Consumer가 더 있으면 좋을 것이라 생각이 가능하지만 Consumer 그룹내에세 리밸런싱 동작을 통해 장애가 발생한 Consumer의 역할을 동일한 그룹에 있는 다른 Consumer가 그 역할을 대신 수행하므로 장애 대비를 위한 추가 Consumer 리소스를 할당하지 않아도 됩니다.
## 컨슈머의 주요 옵션
다양한 옵션을 제공한다 Consumer를 어떻게 처리하고 다루느냐에 따라 Consumer 동작에서 메시지의 중복, 유실 등 여러 가지 상황이 발생할 수 있습니다. Consumer의 사용 목적이 최대한 안정적이며 지연이 없도록 Kafka로부터 메시지를 가져오는 것인데 이를 위한 옵션을 잘 이해하고 사용해야 원하는 형태로 동작할 것입니다. Consumer는 옵션에 따라 오토 커밋, 배치 등을 설정할 있다.
|컨슈머 옵션|설명|
|:---|:---|
|bootstrap.servers|Producer와 동일하게 브로커의 정보를 입력합니다.|
|fetch.min.bytes|한 번에 가져올 수 있는 최소 데이터 크기입니다. 만약 지정한 크기보다 작은 경우, 요청에 응답하지 않고 데이터가 누적될 때까지 대기합니다.|
|group.id|Consumer가 속한 Consumer 그룹을 식별하는 식별자 입니다. 동일한 그룹내의 Consumer 정보는 모두 공유됩니다.|
|heartbeat.interval.ms|하트비트가 있다는 것은 Consumer의 상태가 active임을 의미합니다. session.timeout.ms와 밀접한 관계가 있으며, session.timeout.ms보다 낮은 값으로 설장해야 합니다. 일반적으로 session.timeout.ms의 1/3으로 설정합니다.|
|max.partition.fetch.bytes|Partition당 가져올 수 있는 최대 크기를 의미합니다.|
|session.time.out.ms|이 시간을 이용해, Consumer가 종료된 것인지를 판단합니다. Consumer는 주기적으로 하트비트를 보내야 하고, 만약 이 시간 전까지 하트비트를 보내지 않았다면 해당 Consumer는 종료된 것으로 간주하고 Consumer 그룹에서 제외하고 리밸런싱을 시작합니다.|
|enable.auto.commit|백그라운드로 주기적으로 오프셋을 커밋합니다.|
|auto.offset.reset|Kafka에서 초기 오프셋이 없거나 현재 오프셋이 더 이상 존재하지 않는 경우 다음 옵션으로  reset합니다. earliest : 가장 초기의 오프셋 값으로 설정 latest : 가장 마지막의 오프셋 값으로 설정 none: 이전 오프셋값을 찾지 못하면 에러를 나타냅니다.|
|fetch.max.bytes|한 번의 가죠오기 요청으로 가져올 수 있는 최대 크기입니다.|
|group.instance.id|Consumer의 고유한 식별자입니다. 만약 설정한다면 static멤버로 간주되어, 불필요한 리밸런싱을 하지 않습니다.|
|isolation.level|트랜잭션 Consumer에서 사용되는 옵션으로, read_uncommitted는 기본값으로 모든 메시지를 읽고, read_committed는 틀내잭션이 완료된 메시지만 읽습니다.|
|max.poll.records|한 번의 poll() 요청으로 가져오는 최대 메시지 수|
|partition.assignment.strategy|Partition 할당 전략이며, 기본값을 range입니다.|
|fetch.max.wait.ms|fetch.min.bytes에 의해 설정된 데이터보다 적은 경우 요청에 대한 응답을 기다리는 최대 시간입니다.|

### 오토커밋 예제
```java
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;

import java.util.Arrays;
import java.util.Properties;

public class ConsumerAuto {
    public static void main(String[] args) {
        Properties props = new Properties(); //Properties 오브젝트를 시작합니다.
//        props.put("bootstrap.servers", "peter-kafka01.foo.bar:9092,peter-kafka02.foo.bar:9092,peter-kafka03.foo.bar:9092"); //브로커 리스트를 정의합니다.
        props.put("bootstrap.servers", "k6b102.p.ssafy.io:9091,k6b102.p.ssafy.io:9092,k6b102.p.ssafy.io:9093"); //브로커 리스트를 정의합니다.
        props.put("group.id", "peter-consumer01"); //컨슈머 그룹 아이디 정의합니다.
        props.put("enable.auto.commit", "true"); //자동 커밋을 사용합니다.
        props.put("auto.offset.reset", "latest"); //컨슈머 오프셋을 찾지 못하는 경우 latest로 초기화 합니다. 가장 최근부터 메시지를 가져오게 됩니다.
        props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer"); //문자열을 사용했으므로 StringDeserializer 지정합니다.
        props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props); //Properties 오브젝트를 전달하여 새 컨슈머를 생성합니다.
        consumer.subscribe(Arrays.asList("peter-basic01","chat-test")); //구독할 토픽을 지정합니다.

        try {
            while (true) { //무한 루프 시작입니다. 메시지를 가져오기 위해 카프카에 지속적으로 poll()을 하게 됩니다.
                ConsumerRecords<String, String> records = consumer.poll(1000); //컨슈머는 폴링하는 것을 계속 유지하며, 타임 아웃 주기를 설정합니다.해당 시간만큼 블럭합니다.
                for (ConsumerRecord<String, String> record : records) { //poll()은 레코드 전체를 리턴하고, 하나의 메시지만 가져오는 것이 아니므로, 반복문 처리합니다.
                    System.out.printf("Topic: %s, Partition: %s, Offset: %d, Key: %s, Value: %s\n",
                            record.topic(), record.partition(), record.offset(), record.key(), record.value());
                }
            }
        } catch (Exception e){
            e.printStackTrace();
        } finally {
            consumer.close(); //컨슈머를 종료합니다.
        }
    }
}
```
> 오프셋(offset) consumer에서 메시지를 어디까지 읽었는지 저장하는 값


## Kafka 리플리케이션(Replication)
Kafka는 무수히 많은 데이터 파이프라인의 정중앙에 위치하는 메인 허브 역할을 합니다. 이렇게 중앙에서 메인 허브 역할을 하는 Kafka Cluster가 만약 하드웨어 문제나 점검 등으로 인해 정상적으로 동작하지 못하거나, Kafka와 연결된 전체 데이터 파이프라인에 영향을 미친다면 심각한 이슈가 된다. 이를 Kafka는 초기 설계 단계부터 안정적인 서비스가 운영 될 수 있도록 구상되었다. 이때 안정성을 확보하기 위해 Kafka 내부에서는 **리플리케이션(Replication)**이라는 동작을 하게 됩니다.

### Replication 동작 개요
topic 생성 시 replication factor라는 옵션을 설정해야 합니다.
- zookeeper에 연걸
```bash
kafka-topics --zookeeper zk1:2181 --create --topic chat-test --partitions 3 --replication-factor 3
#  kafka-topics --zookeeper {주키퍼host이름 }:{주키퍼포트번호} --create --topic {토픽이름}--partitions 3 --replication-factor 3
```
- kafka broker에 생성
```bash
kafka-topics --bootstrap-server kafka1:9091 --topic --create --topic chat-test --partitions 3 --replication-factor 3
#  kafka-topics --zookeeper {카프카host이름 }:{카프카포트번호} --create --topic {토픽이름}--partitions 3 --replication-factor 3
```
- Topic 상세 보기
```bash
kafka-topics --bootstrap-server kafka1:9091 --topic chat-test --describe
# kafka-topics --bootstrap-server {카프카host이름 }:{카프카포트번호} --topic {토픽이름} --describe
```

- 로그 보기
```bash
# /var/lib/kafka/data/ OR /data 위치
# cd {topic이름}{0,1,2 파티션번호} ex) cd chat-test-0
kafka-dump-log --print-data-log --files 00000000000000000000.log # 0 20개
```

### 결과
kafka2, 3 브로커에도 동일한 메시지를 갖고 있는걸 확인 할 수 있다. 이렇게 replication-factor 옵션을 통해 N개의 리플리케이션이 있는 경우 N - 1 까지 브로커 장애가 발생해도 메시지 ㅅ노실 없이 안정적으로 메시지를 주고 받을 수 있습니다.

## 리더와 팔로워
#### 토픽 상세 보기 명령어 실행시 출력 내용 중 Partition의 리더(Leader)라는 부분이 있습니다. 모두 동일한 리플리케이션이라 하더라도 리더만의 역할이 따로 있기 때문에 Kafka는 리더를 특별히 강조해 표시합니다. 카프카는 내부적으로 모두 동일한 리플리케이션들을 리더와 팔로워로 구분하고 각자의 역할을 분담시킨다. 리더는 리플리케이션 중 하나가 선정되며, 모든 읽기와 쓰기는 그 리더를 통해서만 가능합니다. 즉 Producer는 모든 Replication에 메시지를 보내는 것이 아니라 리더에게만 메시지를 전송하며 Consumer또한 Reader로 부터 메시지를 가져옵니다.  
#### 팔로워들 역시 대기만 하는 것이 아니라 리더가 문제가 발생하거나 이슈가 있을 경우 언제든지 새로운 리더가 될 준비를 해야합니다. 따라서 Consumer가 Topic의 메시지를 꺼내 가는 것과 비슷한 동작으로 지속적으로 Partition의 리더 가 새로운 메시지를 받았는지 확인하고 새로운 메시지가 있다면 해당 메시지를 리더로부터 복제합니다.


--- 
참고도서 
> 실전 카프카 개발부터 운영까지 (고영만 지음)

