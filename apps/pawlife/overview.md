# PawLife — Product Overview
> Version: 1.1.0 | Updated: 2026-04-10 | Status: design-spec (planned)
> Work Plan: [67] / [92] | Ref: PawLife-Product-Spec (MCP)

## Product Concept
| Item | Detail |
|------|--------|
| Slogan | "온 가족이 함께 키우는 반려동물 다이어리" |
| Target | 전세계 반려동물 보호자 (가족/커플 단위 공동 관리) |
| Platform | Android, iOS, Web, WearOS, watchOS |
| Bundle ID | com.moondeveloper.pawlife |
| Revenue | AdMob (Free tier) + Subscription KRW 3,900/mo / KRW 29,900/yr |
| Differentiation | GPS walk tracking with live route, family team sharing, health D-day alerts |

## Target Users
1. **Primary**: 반려동물 1~3마리를 키우는 가족/커플 (공동 돌봄)
2. **Secondary**: 1인 반려인 (기록 + 알림 중심 사용)
3. **Tertiary**: 반려동물 카페/펫시터 (다두 관리, 팀 기능 활용)

## MVP Scope (v1.0)
1. Pet Profile CRUD (multi-pet, Free tier: 1 pet)
2. GPS Walk Tracking (route display, distance/time/calories/speed)
3. Health Record Cards (vaccination/checkup/medication, D-day alerts)
4. Daily Records (feeding, photo timeline, memo)
5. Team Sharing (family co-management) — CoupleSync module reuse
6. Reminders (walk/feeding/vaccination/birthday)

## Post-MVP Roadmap
- v1.1: Statistics dashboard (walk/feeding trends, monthly summary)
- v1.2: Wearable integration (WearOS/watchOS walk start/stop, heart rate)
- v1.3: Pet SNS feed (team-based photo sharing)
- v1.4: AI health insights (symptom checker, breed-specific advice)

## Revenue Model
| Tier | Price | Features |
|------|-------|----------|
| Free | KRW 0 | 1 pet, basic records, ads |
| Premium Monthly | KRW 3,900/mo | Unlimited pets, no ads, statistics, export |
| Premium Annual | KRW 29,900/yr | Same as monthly (2 months free) |

## Code Status (Scaffold)
- `composeApp/.../navigation/Screen.kt` — 7 screens defined (Home, Walk, WalkHistory, Health, Feeding, Team, Settings)
- `libs/pawlife-domain` — Models (Pet, WalkSession, HealthRecord, FeedingRecord, PawLifeTeam) + Repository interfaces
- `libs/pawlife-data` — Firestore + Local source implementations
- `libs/pawlife-feature-walk` — WalkUseCase (start/updateRoute/completeWalk) with GeoPoint integration
- `libs/pawlife-feature-health` — HealthViewModel, FeedingViewModel (scaffold)
- `libs/pawlife-feature-pet` — PetViewModel, PetListViewModel (scaffold)
- `libs/pawlife-feature-team` — TeamViewModel (scaffold)

## Technical Decisions
- GPS: `moon-maps-kmp` (new OSS, Google Maps SDK + MapKit common wrapper)
- Photos: Coil + Firebase Storage
- Walk Records: Real-time Firestore GeoPoint array
- Health Data: `moon-health-kmp` (HealthKit + Health Connect)
- Timer: FocusOn reuse (walk duration measurement)
- Architecture: 4-Tier KMP (L1 OSS / L2 app-libs / L3 pawlife-libs / L4 composeApp)
- State Mgmt: MVI
- DI: Koin
- Networking: Ktor 3.x
- Backend: moon-server (Spring Boot, Railway)

## Platform-Specific Features
- iOS Dynamic Island / Live Activity: real-time elapsed time + distance during walks
- Android Samsung Now Bar + Glance Widget
- WearOS/watchOS: walk start/stop, heart rate (HealthKit)

## New OSS Module Dependencies
| Module | Priority | Purpose |
|--------|----------|---------|
| moon-maps-kmp | P1 | GPS/Map wrapper |
| moon-liveactivity-kmp | P1 | Dynamic Island / Now Bar |
| moon-wearable-kmp | P2 | WearOS/watchOS |
| moon-health-kmp | P2 | HealthKit/Health Connect |

## Module Reuse
| Module | Reuse Rate |
|--------|-----------|
| HabitFlow streak | 70% |
| FocusOn timer | 80% |
| CoupleSync team sharing | 75% |
| moon-sync/auth/analytics/billing/ui-kmp | 100% |

## L3 Module Structure
| Module | Responsibility |
|--------|---------------|
| `pawlife-domain` | Domain models, repository interfaces, NotificationScheduler |
| `pawlife-data` | Firestore/local data source implementations |
| `pawlife-feature-pet` | Pet profile CRUD presentation |
| `pawlife-feature-walk` | Walk tracking presentation + WalkUseCase |
| `pawlife-feature-health` | Health records + feeding presentation |
| `pawlife-feature-team` | Team sharing presentation |
