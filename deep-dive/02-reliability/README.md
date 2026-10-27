# Module 2. 엣지 케이스 및 안정성 설계 (4~6주차)

동시 요청에 따른 데이터 정합성 깨짐, 외부 API 의존성 장애, 예외적인 엣지 케이스를 격리하고 방어하는 모듈이다.

## 주차별 학습 주제
- **4주차**: [동시성 제어와 락(Lock) 메커니즘](week04_concurrency_and_locks.md)
  - 트랜잭션 격리 수준과 MVCC
  - 비관적 락 vs 낙관적 락 vs 원자적 연산
  - 중복 요청(따닥) 재고 차감 및 출금 방어
- **5주차**: [시스템 복원력과 장애 격리](week05_system_resilience_and_circuit_breaker.md)
  - 멱등성 키(Idempotency Key) 설계
  - 서킷 브레이커, 지수 백오프, 트랜잭션 아웃박스 패턴
- **6주차**: [가치 중심 테스트 설계와 엣지 케이스 방어](week06_value_driven_testing.md)
  - 경계값 분석과 험블 객체 패턴
  - 고전파 vs 런던파 단위 테스트 전략
