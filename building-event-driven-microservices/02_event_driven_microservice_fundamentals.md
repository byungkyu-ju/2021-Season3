# 2장. 이벤트 기반 마이크로서비스 기초

## 2.1. 이벤트 콘텐츠
- 이벤트는 발생한 사건을 정확하게 나타내기 위해 필요한 모든 정보를 담아야 한다.

## 2.2. 이벤트 구조
- 포멧

|Key|Value|
|------|---|
|Unique ID|Description|

- 키 없는 이벤트

|Key|Value|
|------|---|
|N/A|ISBN:372719, Timestamp:1538913600|

- 엔티티 이벤트

|Key|Value|
|------|---|
|ISBN:372719|Author: Adam Bellemare|

- 키 있는 이벤트

|Key|Value|
|------|---|
|ISBN:372719|UserId:A537FE|
|ISBN:372719|UserId:BB0123|

## 2.3. 엔티티 이벤트에서 상태를 구체화
- 각 엔티티 이벤트는 key/value 테이블에 upsert되므로 키별로 가장 최근에 읽은 데이터를 알 수 있다.
- 반대로 update를 이벤트 스트림에 발행하여 테이블을 엔티티 이벤트의 스트림으로 바꿀 수도 있다.
  - 이를 테이블-스트림 이원성이라고 하며, 이벤트 기반 마이크로서비스에서 상태를 생성하는 기본 원리이다.

![https://www.confluent.io/blog/kafka-streams-tables-part-1-event-streaming/](https://cdn.confluent.io/wp-content/uploads/event-stream-1.gif)
![https://cdn.confluent.io/wp-content/uploads/changelog.gif](https://cdn.confluent.io/wp-content/uploads/changelog.gif)

## 2.4. 마이크로서비스를 이벤트 브로커로 강화
- 이벤트 브로커는 이벤트를 받아 큐 또는 파티션된 이벤트 스트림에 저장하고 이렇게 저장된 이벤트를 다른 프로세스가 소비할 수 있도록 제공한다.
- 이벤트 브로커 사용시 장점
  - 확장성
    - 스토리지
  - 보존성
    - 노드간에 이벤트 복제
  - 고가용성
    - 클러스터링을 통해 풀 가동상태 유지
  - 고성능
- 브로커의 최소 요건
  - 파티셔닝
  - 순서보장
  - 불변성
  - 인덱싱
  - 무기한 보존
  - 재연성

## 2.5. 이벤트 브로커 vs 메시지 브로커
- 이벤트 브로커
  - 순서대로 쌓은 사실 로그를 제공할 목적으로 설계됨
  - 인덱스를 통해 개별 액세스를 관리하기 때문에 독립적인 여러 컨슈머가 각자 필요한 이벤트를 마음대로 꺼내갈 수 있다.
  - 이벤트 보존 가능
- 메시지 브로커
  - 메시지큐만 제공하는 메시지 브로커는 큐 단위로 처리되기 때문에, 하위집합만 얻을 수 있다. 또한 모든 이벤트의 전체 사본을 얻을 수 없기 때문에 이벤트만으로는 상태를 정확하게 통신할 수 없다.
  - ack 이후 이벤트를 바로 삭제함

## 2.6. 대규모 마이크로서비스 관리
- Container
- VM
- Container + VM
  - CMS
    - k8s, docker Engine, Mesos Marathon, AWS ECS, Nomad