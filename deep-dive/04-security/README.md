# Module 4. 웹·API 보안 취약점 검증 (10~12주차)

OWASP Top 10을 포함한 주요 웹/API 취약점을 검증하고, 프로덕션 코드베이스의 보안 및 품질을 체계적으로 감사하는 모듈이다.

## 주차별 학습 주제
- **10주차**: [인증·인가 및 권한 결함 방어](week10_auth_and_access_control.md)
  - BOLA / IDOR 리소스 소유권 검증
  - Mass Assignment 및 JWT 탈취 대응(RTR)
- **11주차**: [인젝션 및 웹 애플리케이션 공격 방어](week11_injection_and_web_security.md)
  - Raw SQL Injection, DOM XSS, CSRF, SSRF 방어
  - 정적 코드 분석(Semgrep) 파이프라인 연동
- **12주차**: [종합 시스템 및 코드베이스 감사](week12_comprehensive_code_audit.md)
  - 결합도 진단 및 통합 코드 리뷰 프로토콜
  - 적대적 관점의 PR 감사 및 결함 개선안
