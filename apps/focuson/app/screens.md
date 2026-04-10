# FocusOn — Screen List

- Version: v1.0.0
- Date: 2026-04-10
- Source: `FocusOn/composeApp/src/commonMain/kotlin/com/moondeveloper/focuson/presentation/**/*Screen.kt`

## Bottom Tabs

| Tab | Label | Root Screen |
|-----|-------|-------------|
| HOME | 홈 | HomeScreen |
| TIMER | 타이머 | TimerScreen |
| PLANNER | 플래너 | DdayScreen |
| STATISTICS | 통계 | StatisticsScreen |
| FRIENDS | 친구 | FriendScreen |
| STUDY_ROOM | 스터디룸 | StudyRoomScreen |
| EVENT | 이벤트 | EventScreen |
| PROFILE | 프로필 | ProfileScreen |

## Screen List

| Screen                  | Role                                        | Features                     |
|-------------------------|---------------------------------------------|------------------------------|
| AuthScreen              | 소셜 로그인 (Google/Apple/Kakao)            | F-015                        |
| OnboardingScreen        | 최초 온보딩 위저드 (일일 목표 설정)         | F-017                        |
| MainScreen              | 탭 네비게이션 컨테이너                      | --                           |
| HomeScreen              | 메인 대시보드                               | F-009, F-010, F-012          |
| TimerScreen             | 포모도로 타이머 / 스톱워치                  | F-001, F-002, F-003, F-004   |
| DdayScreen              | D-day 시험 플래너                           | F-025                        |
| AddExamBottomSheet      | 시험 추가 바텀시트                          | F-025                        |
| StatisticsScreen        | 일간/주간/월간 통계 + 업적                  | F-009, F-010, F-011, F-012, F-013 |
| FriendScreen            | 친구 목록 / 요청 관리                       | F-018, F-020                 |
| RankingScreen           | 친구/글로벌 랭킹                            | F-019                        |
| StudyRoomScreen         | 스터디룸 목록 (공개/내 방)                  | F-021, F-022                 |
| CreateRoomBottomSheet   | 스터디룸 생성 바텀시트                      | F-021                        |
| RoomDetailScreen        | 스터디룸 상세 (실시간 멤버 상태)            | F-023, F-024                 |
| EventScreen             | 라이브 이벤트 목록/참여/랭킹               | F-026, F-027                 |
| SoundMixerScreen        | 다중 사운드 레이어 믹서                     | F-005, F-006                 |
| PremiumSoundPackScreen  | 프리미엄 사운드 팩 구매                    | F-007                        |
| ShareCardScreen         | 공유 카드 생성/공유                         | F-028                        |
| AiInsightScreen         | AI 피크 타임 분석 / 스트릭 위험 예측        | F-029, F-030                 |
| AiCoachingScreen        | AI 코칭 챗봇                                | F-031                        |
| YearReviewScreen        | AI 연간 리뷰 생성/공유                      | F-032                        |
| PaywallScreen           | 구독 페이월                                 | F-034, F-035                 |
| ProfileScreen           | 프로필 / 설정 진입점                        | F-016                        |
| SettingsScreen          | 앱 설정                                     | --                           |
| B2BDashboardScreen      | B2B 조직 대시보드                           | F-038                        |
| SsoConfigScreen         | SSO 인증 설정                               | F-039                        |
