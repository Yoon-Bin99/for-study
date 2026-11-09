# 미들웨어 스터디 자료

백엔드 서비스를 받쳐 주는 **캐시·메시징·검색·배치** 기술을 정리한 자료입니다.

- 기준: Redis 7~8 / Valkey, **Kafka 4.x (KRaft)**, Elasticsearch 8~9 / OpenSearch, RabbitMQ 4.x, Spring Boot 3.x / 4.x, Spring Batch 5 / 6
- 버전마다 다른 부분(Jackson 3 직렬화기, Spring Retry 제거 등)은 본문에 표시
- 예제는 다른 스터디 자료와 같은 주문·상품·쿠폰 도메인을 사용

## 문서 구성

학습 목표 → 개념·구조(그림) → 설정·예제 코드 → 실무 주의점 → 면접 질문 → 핵심 정리

## 목차

### 01.Redis
> 자료구조, 캐시 전략, 분산 락·선착순·레이트 리밋, 영속성·고가용성, Spring Data Redis

- [01. Redis 개요와 자료구조](./01.Redis/01.Redis개요와자료구조.md)
- [02. 캐시 전략](./01.Redis/02.캐시전략.md)
- [03. Redis 활용 패턴](./01.Redis/03.Redis활용패턴.md)
- [04. 영속성과 고가용성](./01.Redis/04.영속성과고가용성.md)
- [05. Spring Data Redis](./01.Redis/05.SpringDataRedis.md)

### 02.Kafka
> 파티션·컨슈머 그룹·복제, Spring Kafka, 전달 보장·멱등 컨슈머·Outbox, 운영과 모니터링

- [01. Kafka 심화 구조](./02.Kafka/01.Kafka심화구조.md)
- [02. Spring Kafka](./02.Kafka/02.SpringKafka.md)
- [03. 전달 보장과 순서](./02.Kafka/03.전달보장과순서.md)
- [04. Kafka 운영과 모니터링](./02.Kafka/04.운영과모니터링.md)

### 03.Elasticsearch
> 역색인과 분석, Query DSL과 한글(Nori) 검색, Spring 연동과 DB 동기화

- [01. Elasticsearch 개요와 역색인](./03.Elasticsearch/01.개요와역색인.md)
- [02. 검색 쿼리와 한글 분석](./03.Elasticsearch/02.검색쿼리와한글분석.md)
- [03. Spring 연동과 데이터 동기화](./03.Elasticsearch/03.Spring연동과데이터동기화.md)

### 04.메시지큐
> RabbitMQ(Exchange·ack·DLX), AWS SQS·SNS 팬아웃

- [01. RabbitMQ](./04.메시지큐/01.RabbitMQ.md)
- [02. AWS SQS와 SNS](./04.메시지큐/02.SQS와SNS.md)

### 05.배치와스케줄링
> Spring Batch(5/6), @Scheduled·ShedLock·Quartz·K8s CronJob

- [01. Spring Batch](./05.배치와스케줄링/01.SpringBatch.md)
- [02. 스케줄링과 분산 환경](./05.배치와스케줄링/02.스케줄링과분산환경.md)

## 추천 학습 순서

| 단계 | 문서 | 이유 |
|---|---|---|
| 1 | 01.Redis 01~03 | 캐시·분산 락·선착순은 백엔드 면접 단골 |
| 2 | 02.Kafka 01, 03 | 파티션·컨슈머 그룹, 멱등 컨슈머·Outbox |
| 3 | 04.메시지큐 01 | Kafka와 RabbitMQ 비교 |
| 4 | 01.Redis 04~05, 02.Kafka 02 | 고가용성 구성과 스프링 연동 |
| 5 | 05.배치와스케줄링 | 정산·통계 배치, 다중 서버 스케줄링 |
| 6 | 03.Elasticsearch | 검색 기능이 있는 프로젝트를 할 때 |
| 7 | 02.Kafka 04, 04.메시지큐 02 | 운영 지표, AWS 관리형 큐 |

## 활용 팁

- Docker Compose 또는 Testcontainers로 Redis·Kafka·Elasticsearch를 띄우고 문서의 명령과 코드를 직접 실행해 보세요.
- 「선착순 쿠폰」(Redis + Kafka + DB 유니크), 「주문 이벤트」(Outbox + 멱등 컨슈머), 「상품 검색」(Nori + 동기화)처럼 문서를 엮어 작은 프로젝트로 만들면 포트폴리오와 면접 답변에 바로 쓸 수 있습니다.

## 함께 보면 좋은 자료

- 「Spring JPA 스터디 자료」: 트랜잭션, 동시성과 락, 성능 최적화
- 「테스트코드 스터디 자료」: Testcontainers, 동시성·비동기 테스트
- 「인프라 스터디 자료」: 쿠버네티스(CronJob), AWS, 모니터링

총 5개 대분류, 16개 문서
