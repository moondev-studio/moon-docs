# MoodFlow — Feature Spec

- Version: v1.0.0
- Date: 2026-04-10
- Status: All features are **planned** (design-first, pre-implementation)

## 핵심 기능 (Features)

| ID    | 영역           | 기능                          | Status  |
|-------|----------------|-------------------------------|---------|
| F-001 | Mood           | 일일 무드 기록 (이모지 선택)  | planned |
| F-002 | Mood           | 감정 강도 슬라이더 (1~5)      | planned |
| F-003 | Mood           | 무드 메모 입력 (500자)        | planned |
| F-004 | Mood           | 다중 감정 선택 (혼합 무드)    | planned |
| F-005 | Journal        | 자유 형식 저널 작성           | planned |
| F-006 | Journal        | 감정 연동 저널 (무드 → 일기)  | planned |
| F-007 | Journal        | 저널 검색 (키워드/날짜)       | planned |
| F-008 | Trigger        | 트리거 태그 선택 (사전 정의)  | planned |
| F-009 | Trigger        | 커스텀 트리거 태그 생성       | planned |
| F-010 | Trigger        | 트리거-감정 상관관계 분석     | planned |
| F-011 | Statistics     | 감정 히트맵 (월간 캘린더)     | planned |
| F-012 | Statistics     | 감정 분포 파이 차트           | planned |
| F-013 | Statistics     | 감정 트렌드 라인 차트         | planned |
| F-014 | Statistics     | 주간/월간 감정 요약           | planned |
| F-015 | Insight        | AI 주간 감정 리포트           | planned |
| F-016 | Insight        | AI 월간 패턴 분석             | planned |
| F-017 | Insight        | AI 트리거 인사이트            | planned |
| F-018 | Notification   | 일일 기록 리마인더            | planned |
| F-019 | Notification   | 스트릭 유지 알림              | planned |
| F-020 | Notification   | 주간 리포트 알림              | planned |
| F-021 | Meditation     | 기분 기반 명상 추천           | planned |
| F-022 | Meditation     | 호흡 운동 가이드              | planned |
| F-023 | Meditation     | 명상 타이머                   | planned |
| F-024 | Export         | 감정 데이터 CSV 내보내기      | planned |
| F-025 | Export         | 월간 리포트 PDF 내보내기      | planned |
| F-026 | Auth           | 로그인 / 로그아웃             | planned |
| F-027 | Auth           | 로컬-클라우드 동기화          | planned |
| F-028 | Paywall        | 프리미엄 구독 / 한도 체크     | planned |
| F-029 | Paywall        | Free 기록 제한 (1회/일)       | planned |
| F-030 | Streak         | 연속 기록 스트릭 카운터       | planned |
| F-031 | Streak         | 스트릭 배지/보상              | planned |
| F-032 | Settings       | 알림 시간 설정                | planned |
| F-033 | Settings       | 테마 (다크/라이트) 전환       | planned |
| F-034 | Settings       | 언어 선택                     | planned |
| F-035 | WearOS         | 빠른 감정 탭 (워치)           | planned |
| F-036 | Data Mgmt      | 계정 삭제 / 데이터 초기화     | planned |

## 기능 수: 36개 (전체 planned)

## 화면 목록

See `screens.md`.

## 데이터 모델 (예정)

```kotlin
// 위치: moodflow/libs/moodflow-core/src/commonMain/kotlin/.../domain/model/

data class MoodEntry(
    val id: String,
    val userId: String,
    val emotions: List<Emotion>,
    val intensity: Int,           // 1~5
    val memo: String?,            // max 500 chars
    val triggers: List<TriggerTag>,
    val createdAt: Long
)

data class Emotion(
    val type: EmotionType,
    val emoji: String
)

enum class EmotionType {
    HAPPY, SAD, ANGRY, ANXIOUS, CALM, EXCITED, TIRED, NEUTRAL
}

data class TriggerTag(
    val id: String,
    val label: String,
    val isCustom: Boolean
)

data class JournalEntry(
    val id: String,
    val userId: String,
    val moodEntryId: String?,     // linked mood
    val content: String,
    val createdAt: Long,
    val updatedAt: Long
)

data class InsightReport(
    val id: String,
    val userId: String,
    val type: InsightType,        // WEEKLY, MONTHLY
    val summary: String,
    val patterns: List<String>,
    val generatedAt: Long
)

enum class InsightType { WEEKLY, MONTHLY }

data class MeditationGuide(
    val id: String,
    val title: String,
    val durationSec: Int,
    val targetEmotions: List<EmotionType>
)

enum class PremiumTier { FREE, PRO_MONTHLY, PRO_ANNUAL }
```
