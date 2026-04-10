# MoodFlow — Screen List

- Version: v1.0.0
- Date: 2026-04-10
- Status: All screens are **planned** (design-first wireframe spec)

## 화면 목록

| Screen                     | Role                                      | Features              |
|----------------------------|-------------------------------------------|-----------------------|
| OnboardingScreen           | 최초 앱 소개 및 감정 기록 튜토리얼        | —                     |
| LoginScreen                | 이메일/소셜 로그인                        | F-026                 |
| SignUpScreen               | 회원가입                                  | F-026                 |
| HomeScreen                 | 오늘의 무드 요약 + 빠른 기록 진입         | F-001, F-030          |
| RecordScreen               | 감정 선택 (이모지 그리드 + 강도 슬라이더) | F-001, F-002, F-003   |
| TriggerSelectScreen        | 트리거 태그 선택/생성                     | F-008, F-009          |
| JournalWriteScreen         | 저널 작성 (감정 연동)                     | F-005, F-006          |
| JournalListScreen          | 저널 목록 + 검색                          | F-007                 |
| JournalDetailScreen        | 저널 상세 보기/수정                       | F-005                 |
| CalendarScreen             | 월간 감정 히트맵 캘린더                   | F-011                 |
| StatsDashboardScreen       | 통계 대시보드 (파이/트렌드/요약)          | F-012, F-013, F-014   |
| InsightScreen              | AI 인사이트 리포트 뷰                     | F-015, F-016, F-017   |
| InsightDetailScreen        | AI 리포트 상세 (패턴/트리거 분석)         | F-015, F-016          |
| MeditationListScreen       | 명상 가이드 목록                          | F-021                 |
| MeditationPlayerScreen     | 명상 타이머 + 호흡 가이드 재생            | F-022, F-023          |
| StreakScreen                | 스트릭 현황 + 배지                        | F-030, F-031          |
| NotificationSettingsScreen | 알림 시간/유형 설정                       | F-018, F-019, F-032   |
| PaywallScreen              | 프리미엄 구독 안내                        | F-028, F-029          |
| ExportScreen               | 데이터 내보내기 (CSV/PDF)                 | F-024, F-025          |
| SettingsScreen             | 앱 설정 (테마/언어/계정)                  | F-033, F-034          |
| ProfileScreen              | 프로필 관리                               | F-026                 |
| DataManagementScreen       | 계정 삭제 / 데이터 초기화                 | F-036                 |
| WatchQuickMoodScreen       | WearOS/watchOS 빠른 감정 탭              | F-035                 |

## 화면 수: 23개 (전체 planned)

## 와이어프레임 텍스트

### HomeScreen
```
+---------------------------+
|  MoodFlow         [Profile]|
+---------------------------+
|  Good morning, User!      |
|                           |
|  +---------------------+ |
|  | How are you feeling?| |
|  | (emoji grid 8)      | |
|  | [Record Now]        | |
|  +---------------------+ |
|                           |
|  12-day streak!           |
|                           |
|  +- Recent Records ----+ |
|  | 4/10 Happy    ****  | |
|  | 4/9  Anxious  **    | |
|  | 4/8  Calm     ***   | |
|  +---------------------+ |
|                           |
|  [Stats] [Journal] [Zen] |
+---------------------------+
|  Home Cal  +  Stats  Set  |
+---------------------------+
```

### RecordScreen
```
+---------------------------+
|  <- Record Mood           |
+---------------------------+
|                           |
|  How are you feeling?     |
|                           |
|  (emoji grid: 8 options)  |
|                           |
|  Intensity: ***oo (3/5)   |
|  ------*-----------       |
|                           |
|  Trigger tags:            |
|  [Work] [Relationship]    |
|  [Health] [Weather]       |
|  [Exercise] [+Add]        |
|                           |
|  Memo (optional)          |
|  +---------------------+ |
|  |                     | |
|  |                     | |
|  +---------------------+ |
|                           |
|  [Link to Journal]        |
|                           |
|  [Save]                   |
+---------------------------+
```

### CalendarScreen
```
+---------------------------+
|  <- Mood Calendar  2026.4 |
+---------------------------+
|  Sun Mon Tue Wed Thu Fri  |
|          1   2   3   4    |
|         H   C   S   H    |
|  6   7   8   9  10       |
|  T   A   C   X  ..       |
|  ...                      |
+---------------------------+
|  This Month Summary       |
|  Happy 40% | Calm 25%     |
|  Anxious 15% | Other 20%  |
+---------------------------+
|  Top Triggers: Work, Gym  |
+---------------------------+
```

### StatsDashboardScreen
```
+---------------------------+
|  <- Statistics   Week|Mon |
+---------------------------+
|  Emotion Distribution     |
|  (pie chart area)         |
|                           |
|  Emotion Trend (line)     |
|  (line chart area)        |
|  Mon Tue Wed Thu Fri Sat  |
|                           |
|  Top Triggers             |
|  1. Exercise -> Happy 78% |
|  2. Work -> Anxious 62%   |
|  3. Sleep -> Tired 55%    |
|                           |
|  [View AI Insights ->]    |
+---------------------------+
```

### InsightScreen
```
+---------------------------+
|  <- AI Insights           |
+---------------------------+
|  Weekly Report (4/4~10)   |
|  +---------------------+ |
|  | This week pattern:  | |
|  | Weekday anxiety up  | |
|  | Weekend happiness up| |
|  |                     | |
|  | Key findings:       | |
|  | - Better mood after | |
|  |   exercise          | |
|  | - Monday anxiety    | |
|  |   pattern           | |
|  |                     | |
|  | Recommendation:     | |
|  | Try morning         | |
|  | meditation on Mon   | |
|  +---------------------+ |
|                           |
|  Monthly Pattern Analysis |
|  [Premium] [Unlock]       |
|                           |
|  Trigger Insights         |
|  [View ->]                |
+---------------------------+
```
