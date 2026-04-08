# FreelancePay — Product Overview
> Version: 1.0.0 | Updated: 2026-04-08 | Status: 개발 진행중
> Work Plan: [69] / [79] | Ref: FreelancePay-Product-Spec (MCP)

## 제품 개요
| 항목 | 내용 |
|------|------|
| 슬로건 | "프리랜서를 위한 가장 심플한 정산 도구" |
| 타겟 | 전세계 프리랜서, 1~5인 소규모 팀 |
| 플랫폼 | Android, iOS, Web (메인), 위젯 |
| 번들 ID | com.moondeveloper.freelancepay |
| 수익 | 구독 ₩4,900/월 / ₩39,900/년 (Free: 3 프로젝트) |

## 현재 구현 완료 (Work Plan #79)
- 프로젝트 CRUD
- 클라이언트 관리
- 다중 통화 설정 (moon-i18n-kmp)
- FocusOn 타이머 코어 재사용 준비
- Navigation: `composeApp/.../navigation/Screen.kt`

### 코드 상태 (실측)
- `composeApp/src/commonMain/kotlin/com/moondeveloper/freelancepay/navigation/Screen.kt` 존재
- Feature Screen/UseCase 모듈은 추가 구현 필요 (Domain/Data 레이어 정립 대기)

## 핵심 기능
1. 프로젝트 관리 (시급/고정가, 다중 통화 USD/KRW/JPY/EUR/GBP)
2. 시간 기록 (FocusOn 타이머 재사용, 수동 입력)
3. 인보이스 생성 (PDF via moon-server, SMTP 이메일 발송)
4. 정산 통계 (월별 수입 그래프, 클라이언트별 비교)

## 핵심 기술 결정
- 타이머: FocusOn 코드 재사용 (포모도로 모드 제거)
- 인보이스 PDF: `moon-server` 백엔드 (JasperReports/iText)
- 다중 통화: `moon-i18n-kmp`
- Web 메인: Firebase Hosting + SPA

## 다음 단계
- FeatureProjects / FeatureInvoices 모듈화
- UseCase 레이어 (`FreelanceDomain`)
- 서버 `/api/freelancepay/v1/*` 엔드포인트 구현
- PDF 생성 템플릿

## 모듈 재사용
| 모듈 | 재사용률 |
|------|---------|
| FocusOn 타이머 | 85% |
| Splitly 정산 UI | 60% |
| moon-billing/i18n/auth-kmp | 100% |

---

## ADR-001: 타이머 재사용 전략
- **Context**: 프리랜서 시간 기록이 핵심. FocusOn에 검증된 타이머 코어 존재.
- **Options**: (a) FocusOn 코어 100% 재사용 (포모도로 제거), (b) 신규 타이머 작성, (c) 서드파티 라이브러리
- **Decision**: FocusOn 타이머 코어 100% 재사용, 포모도로/세션 분할 제거, 순수 시간 측정만
- **Rationale**: 검증된 백그라운드 동작, KMP 호환, 개발 시간 절약. 정산 계산은 Unit Test 100% 커버리지 필수(돈 관련).
- **Impact**: medium
