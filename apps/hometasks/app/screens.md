# HomeTasks — Screen List

- Version: v1.0.0
- Date: 2026-04-10
- Status: All screens are **planned** (design-first)

## Screens

| Screen                       | Role                                         | Features              | Status  |
|------------------------------|----------------------------------------------|-----------------------|---------|
| OnboardingScreen             | First-launch walkthrough                     | --                    | planned |
| LoginScreen                  | Firebase Auth sign-in                        | F-024                 | planned |
| SignUpScreen                 | New account registration                     | F-024                 | planned |
| HomeScreen                   | Dashboard hub (today's tasks, quick stats)   | F-001, F-016          | planned |
| TaskListScreen               | Full task list with filters                  | F-001, F-002, F-006   | planned |
| AddTaskScreen                | Create new task form                         | F-001, F-002, F-003, F-005 | planned |
| TaskDetailScreen             | View/edit task details                       | F-001, F-004, F-005, F-006 | planned |
| TaskCompletionScreen         | Mark done + optional photo proof             | F-013, F-014          | planned |
| TeamSetupScreen              | Initial team creation                        | F-007, F-008          | planned |
| TeamDashboardScreen          | Member list, roles, management               | F-008, F-009          | planned |
| InviteScreen                 | Generate/share invite link or code           | F-007                 | planned |
| JoinTeamScreen               | Accept invite and join team                  | F-007                 | planned |
| RotationScheduleScreen       | View and configure rotation schedule         | F-010, F-011          | planned |
| SwapRequestScreen            | Request/approve task swaps                   | F-012                 | planned |
| StatsDashboardScreen         | Completion rates, workload distribution      | F-016, F-017          | planned |
| WeeklyReportScreen           | Weekly summary with charts                   | F-018                 | planned |
| MonthlyTrendScreen           | Monthly trend analysis                       | F-019                 | planned |
| RewardDashboardScreen        | Points balance, badges, streak               | F-020, F-021, F-022   | planned |
| RewardHistoryScreen          | Detailed reward/point history                | F-023                 | planned |
| LeaderboardScreen            | Member ranking by points/completions         | F-020, F-021          | planned |
| PaywallScreen                | Premium subscription purchase                | F-027                 | planned |
| NotificationSettingsScreen   | Push notification preferences                | F-028                 | planned |
| LanguageSelectionScreen      | App language selection                        | F-029                 | planned |
| ProfileScreen                | User profile view/edit                       | F-024                 | planned |
| FeedbackScreen               | Submit user feedback                         | F-030                 | planned |

## Screen Count: 25

## Navigation Graph (planned)

```
Onboarding -> Login/SignUp -> HomeScreen
                              |-- TaskListScreen -> AddTaskScreen / TaskDetailScreen -> TaskCompletionScreen
                              |-- TeamDashboardScreen -> InviteScreen / JoinTeamScreen
                              |-- RotationScheduleScreen -> SwapRequestScreen
                              |-- StatsDashboardScreen -> WeeklyReportScreen / MonthlyTrendScreen
                              |-- RewardDashboardScreen -> RewardHistoryScreen / LeaderboardScreen
                              +-- ProfileScreen -> NotificationSettingsScreen / LanguageSelectionScreen / PaywallScreen / FeedbackScreen
```
