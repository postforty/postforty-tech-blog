# Module 1. 성능 병목 및 DB 최적화 (1~3주차)

데이터베이스 내부 구조부터 실행 계획, 런타임 및 브라우저 병목까지 프로덕션 환경의 성능 문제를 근본적으로 분석하고 튜닝하는 모듈이다.

## 주차별 학습 주제
- **1주차**: [스토리지 엔진과 인덱스 내부 구조](week01_storage_engine_and_index.md)
  - B-Tree vs LSM-Tree 구조 비교
  - 클러스터드 / 세컨더리 / 커버링 인덱스 동작 원리
  - 다중 컬럼 인덱스 정렬 순서 및 손익분기점
  - 100만 건 더미 데이터 벤치마크
- **2주차**: [실행 계획 분석과 쿼리 튜닝](week02_execution_plan_and_query_tuning.md)
  - 옵티마이저 동작 및 `EXPLAIN ANALYZE` 지표 해석
  - ORM N+1 방지 및 불필요한 조인 제거
- **3주차**: [런타임 및 프런트엔드 병목 분석](week03_runtime_and_frontend_bottlenecks.md)
  - DB 커넥션 풀 고갈 원리
  - k6 부하 테스트 및 렌더링 파이프라인 프로파일링
