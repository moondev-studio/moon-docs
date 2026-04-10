# HomeTasks — Product Overview
> Version: 1.1.0 | Updated: 2026-04-10 | Status: planned (design-first)
> Ref: ADR-001-initial-design, ADR-002-mvp-scope

## Product Concept
| Item | Detail |
|------|--------|
| Slogan | "Together we live, together we clean" |
| Target | Couples, roommates, families (2+ cohabitants) |
| Platform | Android, iOS (KMP), Widget, Dynamic Island / Now Bar |
| Bundle ID | com.moondeveloper.hometasks |
| Revenue | AdMob (Free) + Subscription KRW 2,900/mo / KRW 22,900/yr |

## Problem Statement
Household chore distribution is a common source of friction among cohabitants.
Existing apps lack rotation schedules, gamification, and multi-platform real-time sync.

## MVP Scope (ADR-002)
1. **Task Registration** — CRUD for household tasks with category, estimated time, and assignee
2. **Member Invite** — Invite housemates via link/code (reusing CoupleSync pairing pattern)
3. **Rotation Schedule** — Auto-rotate task assignments on configurable cadence
4. **Completion Check** — Mark tasks done with timestamp; optional photo proof
5. **Stats Dashboard** — Completion rate comparison, workload distribution, weekly report
6. **Reward Points** — Points per task, weekly MVP badge, streak tracking (Premium)

## Revenue Model
| Tier | Price | Features |
|------|-------|----------|
| Free | KRW 0 | 2 members, basic tasks, completion check, ads |
| Premium | KRW 2,900/mo or KRW 22,900/yr | Unlimited members, reward points, badges, streak, no ads, advanced stats |

## Module Reuse
| Module | Reuse % | Notes |
|--------|---------|-------|
| CoupleSync team sharing | 80% | Invite link, real-time sync, member management |
| HabitFlow streak/badges | 75% | Streak engine, badge rendering |
| moon-sync/auth/analytics/billing/ui-kmp | 100% | Shared L1/L2 infrastructure |

## Platform-Specific Features (planned)
- iOS Dynamic Island: Chore-in-progress timer
- Android Samsung Now Bar + Glance Widget
- WearOS / watchOS: Quick completion tap

## Architecture
- L1: moon-kmp-libs (OSS)
- L2: moon-app-libs (Private, includeBuild)
- L3: HomeTasks/libs (hometasks-feature-tasks, hometasks-feature-team, hometasks-domain)
- L4: HomeTasks/composeApp (UI + navigation)
- State: MVI | DI: Koin | Network: Ktor 3.x | Backend: moon-server (Spring Boot, Railway)

## Next Steps
- [ ] Scaffold L3 modules with domain models
- [ ] Implement Firebase Auth + Firestore integration
- [ ] Build TaskListScreen, AddTaskScreen, TaskDetailScreen
- [ ] Build TeamSetupScreen with CoupleSync pairing reuse
- [ ] Design reward point system domain logic
