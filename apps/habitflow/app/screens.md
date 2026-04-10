# HabitFlow — Screen List

- Version: v1.0.0
- Date: 2026-04-10
- Source: `HabitFlow/composeApp/src/commonMain/kotlin/.../presentation/**/*Screen.kt` and `ui/screen/*Screen.kt`

| Screen                  | Role                                            | Features              |
|-------------------------|-------------------------------------------------|-----------------------|
| SplashScreen            | App launch splash with auth check               | --                    |
| OnboardingScreen        | First-time user onboarding flow                 | --                    |
| LoginScreen             | Email/Google/Apple sign-in and sign-up           | F-012, F-013, F-014   |
| HomeDashboardScreen     | Main dashboard: today's habits, check-in, streak| F-001, F-003, F-005, F-006, F-011 |
| HabitCreateScreen       | Create new habit (manual or from template)       | F-001, F-002          |
| TemplateGalleryScreen   | Browse 30 pre-built habit templates by category  | F-002                 |
| HabitDetailScreen       | Single habit detail: streak, records, edit       | F-001, F-003, F-004, F-006, F-010 |
| StatisticsScreen        | Weekly chart, monthly heatmap, per-habit stats   | F-008, F-009, F-010   |
| SettingsScreen          | Profile edit, theme, notifications, sign-out     | F-022, F-023, F-024, F-015 |
| PaywallScreen           | PRO subscription purchase / restore              | F-017, F-018, F-019   |

### Navigation Graph

Defined in `HabitFlow/composeApp/.../navigation/Screen.kt` as a sealed class:

```
Screen.Splash -> Screen.Onboarding (first launch) | Screen.Login (no auth) | Screen.Home (authenticated)
Screen.Login -> Screen.Home
Screen.Home -> Screen.HabitCreate | Screen.HabitDetail(habitId) | Screen.Statistics | Screen.Settings | Screen.Paywall
Screen.HabitCreate -> Screen.TemplateGallery
Screen.Settings -> Screen.Paywall
```

### ViewModel Mapping

| Screen                  | ViewModel                |
|-------------------------|--------------------------|
| LoginScreen             | AuthViewModel            |
| HomeDashboardScreen     | HomeDashboardViewModel   |
| HabitCreateScreen       | HabitCreateViewModel     |
| HabitDetailScreen       | HabitDetailViewModel     |
| StatisticsScreen        | StatisticsViewModel      |
| SettingsScreen          | SettingsViewModel        |
| PaywallScreen           | BillingViewModel         |

### Notes

- There are two `StatisticsScreen` files: one at `presentation/statistics/` (active, uses `StatisticsViewModel`) and a legacy one at `ui/screen/` (uses `ui/stats/StatisticsViewModel`). The `presentation/` version is the current implementation.
- `Screen.Profile` is defined in the navigation sealed class but no corresponding `ProfileScreen.kt` composable exists yet -- likely planned.
