# MoodFlow — Server API Spec

- Version: v1.0.0
- Date: 2026-04-10
- Status: All endpoints are **planned** (design-first)
- Base path: `/api/moodflow/v1`
- Auth: Bearer token (Firebase Auth)

## Endpoints

| Method | Path                                  | Description                          | Auth     |
|--------|---------------------------------------|--------------------------------------|----------|
| GET    | `/health`                             | MoodFlow 서브시스템 헬스체크         | none     |
| POST   | `/moods`                              | 무드 엔트리 생성                     | required |
| GET    | `/moods`                              | 무드 엔트리 목록 조회 (페이징)       | required |
| GET    | `/moods/{id}`                         | 무드 엔트리 상세 조회                | required |
| PATCH  | `/moods/{id}`                         | 무드 엔트리 수정                     | required |
| DELETE | `/moods/{id}`                         | 무드 엔트리 삭제                     | required |
| GET    | `/moods/calendar`                     | 월간 캘린더 히트맵 데이터            | required |
| POST   | `/journals`                           | 저널 엔트리 생성                     | required |
| GET    | `/journals`                           | 저널 목록 조회 (페이징/검색)         | required |
| GET    | `/journals/{id}`                      | 저널 상세 조회                       | required |
| PATCH  | `/journals/{id}`                      | 저널 수정                            | required |
| DELETE | `/journals/{id}`                      | 저널 삭제                            | required |
| GET    | `/triggers`                           | 사전 정의 트리거 태그 목록           | required |
| POST   | `/triggers/custom`                    | 커스텀 트리거 태그 생성              | required |
| DELETE | `/triggers/custom/{id}`               | 커스텀 트리거 태그 삭제              | required |
| GET    | `/stats/distribution`                 | 감정 분포 통계                       | required |
| GET    | `/stats/trend`                        | 감정 트렌드 데이터                   | required |
| GET    | `/stats/summary`                      | 주간/월간 감정 요약                  | required |
| GET    | `/stats/trigger-correlation`          | 트리거-감정 상관관계                 | required |
| POST   | `/insights/weekly`                    | AI 주간 인사이트 생성 요청           | required |
| POST   | `/insights/monthly`                   | AI 월간 인사이트 생성 요청           | required |
| GET    | `/insights`                           | 인사이트 리포트 목록 조회            | required |
| GET    | `/insights/{id}`                      | 인사이트 리포트 상세 조회            | required |
| GET    | `/meditations`                        | 명상 가이드 목록 조회                | required |
| GET    | `/meditations/recommend`              | 현재 기분 기반 명상 추천             | required |
| GET    | `/streak`                             | 스트릭 현황 조회                     | required |
| POST   | `/export/csv`                         | 감정 데이터 CSV 내보내기             | premium  |
| POST   | `/export/pdf`                         | 월간 리포트 PDF 내보내기             | premium  |

## Endpoint 수: 28개 (전체 planned)

## Request / Response 예시

### POST `/moods`
```json
// Request
{
  "emotions": [
    { "type": "HAPPY", "emoji": "😊" }
  ],
  "intensity": 4,
  "memo": "오늘 운동 후 기분이 좋다",
  "triggerIds": ["trigger_exercise", "trigger_weather"]
}

// Response 201
{
  "id": "mood_abc123",
  "userId": "user_001",
  "emotions": [{ "type": "HAPPY", "emoji": "😊" }],
  "intensity": 4,
  "memo": "오늘 운동 후 기분이 좋다",
  "triggers": [
    { "id": "trigger_exercise", "label": "운동", "isCustom": false },
    { "id": "trigger_weather", "label": "날씨", "isCustom": false }
  ],
  "createdAt": "2026-04-10T09:30:00Z"
}
```

### GET `/moods/calendar?year=2026&month=4`
```json
// Response 200
{
  "year": 2026,
  "month": 4,
  "entries": [
    { "day": 1, "dominantEmotion": "HAPPY", "intensity": 4 },
    { "day": 2, "dominantEmotion": "CALM", "intensity": 3 },
    { "day": 3, "dominantEmotion": "SAD", "intensity": 2 }
  ]
}
```

### POST `/insights/weekly`
```json
// Request
{
  "weekStart": "2026-04-04"
}

// Response 202
{
  "id": "insight_xyz789",
  "status": "generating",
  "estimatedCompletionSec": 30
}
```

### GET `/insights/{id}`
```json
// Response 200
{
  "id": "insight_xyz789",
  "type": "WEEKLY",
  "status": "completed",
  "summary": "이번 주 전반적으로 긍정적인 감정이 우세했습니다...",
  "patterns": [
    "운동 후 행복감이 78% 증가",
    "월요일 오전에 불안 패턴 반복",
    "수면 부족 시 피로감 상관관계 0.82"
  ],
  "recommendations": [
    "월요일 아침 5분 명상을 시도해보세요",
    "운동 습관을 유지하세요 — 기분 개선 효과가 뚜렷합니다"
  ],
  "generatedAt": "2026-04-10T10:00:00Z"
}
```

## Domain References (planned)
- `moon-server/src/main/kotlin/.../moodflow/domain/MoodEntry.kt`
- `moon-server/src/main/kotlin/.../moodflow/domain/JournalEntry.kt`
- `moon-server/src/main/kotlin/.../moodflow/domain/TriggerTag.kt`
- `moon-server/src/main/kotlin/.../moodflow/domain/InsightReport.kt`
- `moon-server/src/main/kotlin/.../moodflow/domain/MeditationGuide.kt`

## AI Integration
- Claude API via moon-server proxy
- Endpoint: `/insights/weekly`, `/insights/monthly`
- 토큰 사용량 서버 제어
- Free: 월 1회 / Premium: 무제한
