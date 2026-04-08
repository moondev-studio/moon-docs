# Splitly — Server API Spec

- Version: v1.0.5
- Date: 2026-04-08
- Source: `moon-server/src/main/kotlin/com/moondeveloper/server/splitly/controller/SplitlyController.kt`
- Base path: `/api/splitly/v1`

| Method | Path                                | Description                     |
|--------|-------------------------------------|---------------------------------|
| GET    | `/health`                           | Splitly 서브시스템 헬스체크     |
| GET    | `/settlements`                      | 정산 목록 조회                  |
| POST   | `/settlements`                      | 정산 생성                       |
| GET    | `/settlements/{id}`                 | 정산 상세 조회                  |
| PATCH  | `/settlements/{id}/title`           | 정산 제목 수정                  |
| PATCH  | `/settlements/{id}/members`         | 정산 멤버 변경                  |
| PATCH  | `/settlements/{id}/items`           | 정산 품목 변경                  |
| POST   | `/settlements/{id}/complete`        | 정산 완료 처리                  |
| DELETE | `/settlements/{id}`                 | 정산 삭제                       |
| POST   | `/share`                            | 공유 링크 생성                  |
| GET    | `/share/{shareId}`                  | 공유 정산 조회 (공개)           |
| DELETE | `/share/{shareId}`                  | 공유 링크 해제                  |
| GET    | `/ledger/entries`                   | 가계부 엔트리 목록              |
| POST   | `/ledger/entries`                   | 가계부 엔트리 생성              |
| PATCH  | `/ledger/entries/{id}`              | 가계부 엔트리 수정              |
| DELETE | `/ledger/entries/{id}`              | 가계부 엔트리 삭제              |
| GET    | `/ledger/stats/monthly`             | 월별 합계 통계                  |
| GET    | `/ledger/stats/category`            | 카테고리별 통계                 |
| GET    | `/ledger/stats/trend`               | 추세 통계                       |

Domain references:
- `server/splitly/domain/Settlement.kt`
- `server/splitly/domain/LedgerEntry.kt`
- `server/splitly/domain/ShareLink.kt`

추가 상세: `moon-server/docs/api-spec/splitly-api-v1.md` 참조.
