# ADR-002: MVP Scope
- Version: v1.0.0
- Date: 2026-04-10
- Status: decided

## Background
PawLife requires a clear MVP boundary to ship v1.0 within the MoonDeveloper multi-app development timeline.
The scaffold already defines domain models (Pet, WalkSession, HealthRecord, FeedingRecord, PawLifeTeam) and a working WalkUseCase,
but most features remain unimplemented. We need to decide which features ship in v1.0 and which are deferred.

## Decision

### MVP (v1.0) includes:
1. **Pet Profile CRUD** — multi-pet support with Free tier limit (1 pet)
2. **GPS Walk Tracking** — start/stop, real-time route, distance/time/calories (WalkUseCase exists)
3. **Health Records** — vaccination, vet visit, medication CRUD with D-day alerts
4. **Feeding Records** — daily meal logging with timeline view
5. **Team Sharing** — create/join team, shared pet management (CoupleSync reuse)
6. **Reminders** — push notifications for walk, feeding, vaccination, birthday
7. **Auth** — Firebase Auth (Google/Apple sign-in)
8. **Subscription** — AdMob (Free) + Premium paywall (KRW 3,900/mo, KRW 29,900/yr)
9. **Sync** — Firestore cloud sync with offline-first local cache

### MVP excludes (deferred to v1.1+):
- Statistics dashboard (walk/feeding trends, monthly reports)
- iOS Dynamic Island / Live Activity
- Android Glance Widget
- WearOS / watchOS support
- Photo diary (dedicated screen)
- Data export (CSV/JSON)
- AI health insights

## Rationale
- **Walk tracking is the core differentiator** — it must ship in v1.0 with full GPS route support
- **Team sharing reuses CoupleSync** (75% reuse rate) — low effort, high value for family users
- **Statistics are cosmetic** — they enhance retention but are not essential for initial adoption
- **Wearable/platform features** require new OSS modules (moon-wearable-kmp, moon-liveactivity-kmp) that are not yet built
- **Free tier limit (1 pet)** creates clear upgrade motivation without crippling the experience

## Options Considered
1. **Full-featured v1.0** (everything including statistics + wearable) — rejected: too much scope for timeline
2. **Minimal v1.0** (pet profile + walk only, no team/health) — rejected: insufficient differentiation
3. **Balanced v1.0** (core features + team, defer platform-specific) — **selected**

## Impact
- High: defines what the initial release contains
- Affects: feature development order, module priority, OSS dependency timeline
- Dependencies: moon-maps-kmp (P1, required for walk tracking in MVP)

## Previous ADR
- ADR-001: Initial Design (4-Tier KMP architecture, GPS strategy)
