# Deep Dive: 웹 풀스택 핵심 엔지니어링 12주 코스

단순한 프레임워크 문법이나 라이브러리 사용법을 넘어, 프로덕션 레벨에서 요구되는 **성능(Performance), 안정성(Reliability), 아키텍처(Architecture), 보안(Security)**의 4대 핵심 축을 깊이 있게 파고들고 검증하는 엔지니어링 시리즈이다.

---

## 전체 로드맵 및 목차

### Module 1. 성능 병목 및 DB 최적화 (1~3주차)
- [01-performance/](01-performance/)
  - **1주차: 스토리지 엔진과 인덱스 내부 구조**
    - B-Tree vs LSM-Tree 구조 비교
    - 클러스터드 / 세컨더리 / 커버링 인덱스 동작 원리
    - 다중 컬럼 인덱스 정렬 순서 및 손익분기점
    - *실전*: 100만 건 더미 데이터 적재 후 인덱스 전후 벤치마크
  - **2주차: 실행 계획 분석과 쿼리 튜닝**
    - MySQL 옵티마이저 동작 메커니즘
    - `EXPLAIN ANALYZE` 지표(`type`, `key`, `rows`, `extra`) 분석
    - ORM N+1 문제 원인 및 불필요한 조인 제거
    - *실전*: 슬로우 쿼리 Full Table Scan을 Index Scan으로 튜닝
  - **3주차: 런타임 및 프런트엔드 병목 분석**
    - DB 커넥션 풀(Connection Pool) 고갈 및 스레드 경합 원리
    - 비동기 직렬 처리 병렬화 및 논블로킹 I/O 최적화
    - 브라우저 렌더링 파이프라인과 메모리 누수 프로파일링
    - *실전*: k6 동시 부하 테스트를 통한 병목 진단 및 응답 시간 단축

---

### Module 2. 엣지 케이스 및 안정성 설계 (4~6주차)
- [02-reliability/](02-reliability/)
  - **4주차: 동시성 제어와 락(Lock) 메커니즘**
    - 트랜잭션 격리 수준(Isolation Level)과 MVCC 원리
    - 비관적 락(Pessimistic) vs 낙관적 락(Optimistic) vs 원자적 연산
    - 레코드 락, 갭 락(Gap Lock), 넥스트 키 락, 데드락 발생 원인 분석
    - *실전*: 중복 클릭(따닥) 재고 차감 및 출금 동시성 공격 방어
  - **5주차: 시스템 복원력(Resilience)과 장애 격리**
    - 네트워크 지연 대응: 타임아웃, 지수 백오프, 서킷 브레이커
    - 멱등성 키(Idempotency Key) 설계와 중복 결제 방지
    - 분산 트랜잭션 한계 극복을 위한 트랜잭션 아웃박스(Transactional Outbox) 패턴
    - *실전*: 외부 API 장애 시나리오 모킹 및 정합성 보장 구현
  - **6주차: 가치 중심 테스트 설계와 엣지 케이스 방어**
    - 경계값 분석(Boundary Value Analysis) 기법
    - 고전파(Classic) vs 런던파(London) 단위 테스트 설계
    - 비결정적 요소 격리를 위한 험블 객체(Humble Object) 패턴
    - *실전*: 실패 엣지 케이스 테스트 선작성 및 코드 견고성 검증

---

### Module 3. 소프트웨어 아키텍처 및 시스템 설계 (7~9주차)
- [03-architecture/](03-architecture/)
  - **7주차: 도메인 주도 설계(DDD)와 계층 분리**
    - 유비쿼터스 언어와 바운디드 컨텍스트(Bounded Context)
    - 엔티티, 밸류 객체(VO), 애그리거트 루트의 책임과 경계
    - 절차지향 서비스 로직을 순수 도메인 모델로 분리
    - *실전*: 비대한 Service/Controller 계층의 도메인 모델링 리팩토링
  - **8주차: 의존성 규칙과 프런트엔드 구조 설계**
    - 클린 / 헥사고날 아키텍처(Ports & Adapters)와 의존성 역전 원칙(DIP)
    - 프런트엔드 상태 성격 분리 (Server State / UI State / URL State)
    - 비즈니스 로직과 UI 컴포넌트의 결합을 끊는 Headless 패턴
    - *실전*: 레이어 간 의존성 규칙 명문화 및 아키텍처 가이드라인 정립
  - **9주차: 대규모 트래픽 시스템 디자인**
    - 캐시 전략 (Look-aside, Write-through) 및 캐시 스탬피드(Stampede) 방어
    - 메시지 브로커(Kafka, RabbitMQ)를 활용한 비동기 디커플링
    - CQRS(명령/조회 책임 분리) 기초 및 DB 샤딩 전략
    - *실전*: 대규모 선착순 트래픽 시스템 아키텍처 설계 및 명세서 작성

---

### Module 4. 웹·API 보안 취약점 검증 (10~12주차)
- [04-security/](04-security/)
  - **10주차: 인증·인가 및 권한 결함 방어**
    - OWASP API 1위 BOLA / IDOR (인가 검증 누락) 원리와 공격 패턴
    - Mass Assignment 취약점 및 DTO 오버라이딩 방어
    - JWT 탈취 대응: Refresh Token Rotation(RTR) 및 무효화 전략
    - *실전*: 리소스 소유권 검증 가드 구현 및 취약 코드 점검
  - **11주차: 인젝션 및 웹 애플리케이션 공격 방어**
    - 동적 검색 쿼리 Raw SQL Injection 방어
    - DOM XSS 및 CSRF 방어 메커니즘
    - SSRF(Server-Side Request Forgery) 공격 차단 기법
    - *실전*: PortSwigger 취약점 랩 실습 및 정적 코드 분석(Semgrep) 파이프라인 연동
  - **12주차: 종합 시스템 및 코드베이스 감사(Audit)**
    - 결합도 및 응집도 관점의 아키텍처 품질 평가
    - 보안·성능·안정성 통합 코드 리뷰 프로토콜
    - 적대적 관점(Adversarial)의 PR 결함 검출
    - *실전*: 취약점 및 병목이 심어진 풀스택 PR 감사 및 최종 개선안 도출
