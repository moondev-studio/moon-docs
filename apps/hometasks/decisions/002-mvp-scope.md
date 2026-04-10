# ADR-002: MVP Scope Definition

- Version: v1.0.0
- Date: 2026-04-10
- Status: proposed

## Context

HomeTasks is a new app targeting cohabitants (couples, roommates, families) who need
a fair and transparent system for distributing household chores. Before implementation
begins, we need to define a clear MVP scope that balances user value with development
speed.

Key constraints:
- Solo developer (moondev-studio) managing multiple apps simultaneously
- Must maximize code reuse from existing apps (CoupleSync, HabitFlow)
- KMP (Kotlin Multiplatform) architecture must be maintained across all layers

## Decision

### MVP Features (6 areas, 30 features)

1. **Task Registration** (F-001 ~ F-006) — Full CRUD with categories, estimated time,
   assignee, recurrence, and priority
2. **Member Invite** (F-007 ~ F-009) — Team creation, invite link/code, member management
3. **Rotation Schedule** (F-010 ~ F-012) — Auto-rotation (round-robin), custom rules, swap requests
4. **Completion Check** (F-013 ~ F-015) — Mark done with timestamp, optional photo proof, undo
5. **Stats Dashboard** (F-016 ~ F-019) — Completion rates, workload distribution, weekly/monthly reports
6. **Reward Points** (F-020 ~ F-023) — Points per task, weekly MVP badge, streak, reward history (Premium-only features)

### CoupleSync Pairing Pattern Reuse

The CoupleSync app has a proven real-time pairing/team system that includes:
- Invite link generation with expiry
- Deep link handling for team join flow
- Real-time Firestore sync for team state
- Member presence and role management

**HomeTasks will reuse approximately 80% of this pattern**, adapting it from a 2-person
couple model to an N-person household model. Specific reuse points:

| CoupleSync Component | HomeTasks Adaptation |
|----------------------|----------------------|
| `CreatePairLinkUseCase` | `CreateInviteLinkUseCase` (same flow, different naming) |
| `JoinPairUseCase` | `JoinTeamUseCase` (extended for N members) |
| `PairSyncRepository` | `TeamSyncRepository` (Firestore collection structure adapted) |
| `PairStatusScreen` | `TeamDashboardScreen` (UI redesigned for N members) |
| Deep link scheme | Same `moondeveloper://` scheme, different path |

This reuse is expected to save 2-3 weeks of development time on the team management
feature area.

### HabitFlow Streak/Badge Reuse

The HabitFlow app's streak engine and badge system will be reused (~75%):
- Streak calculation logic (consecutive day tracking)
- Badge award and rendering components
- Adapted for task-completion context instead of habit context

## Alternatives Considered

### Option A: Minimal MVP (Tasks + Completion only)
- Pros: Fastest to ship
- Cons: No differentiation from basic to-do apps; no viral growth mechanism (no invite)
- Rejected: Insufficient user value

### Option B: Full feature set from day one
- Pros: Complete experience
- Cons: 3-4 month timeline; too risky for unvalidated market
- Rejected: Too large for initial validation

### Option C (Selected): 6-area MVP with module reuse
- Pros: Differentiated product, leverages existing code, shippable in 4-6 weeks
- Cons: Reward system adds complexity
- Selected: Best balance of differentiation and development speed

## Consequences

- Team management code will be extracted into a shared module in moon-app-libs (L2) after
  HomeTasks validates the N-person adaptation of the CoupleSync pattern
- Free tier limited to 2 members (matching CoupleSync), Premium unlocks unlimited
- Reward features gated behind Premium to drive subscription conversion
- Stats features provide value to free users, encouraging upgrade for rewards

## Related Decisions

- ADR-001: Initial Design (4-Tier KMP architecture, shared tech stack)
- CoupleSync: Pairing system design (reference implementation)
- HabitFlow: Streak/badge engine (reference implementation)
