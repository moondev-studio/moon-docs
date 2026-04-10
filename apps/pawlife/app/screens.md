# PawLife — Screen Spec (Design)

- Status: design-spec (planned, not yet implemented)
- Date: 2026-04-10
- Source: Design-first spec based on `Screen.kt` navigation + product requirements

## Screen List

| ID   | Screen Name         | Route              | Status  | Features          |
|------|---------------------|--------------------|---------|-------------------|
| S-01 | Home                | `home`             | planned | F-001, F-016~F-017 |
| S-02 | Pet Add/Edit        | `pet/edit/{id?}`   | planned | F-001~F-004       |
| S-03 | Pet Detail          | `pet/{petId}`      | planned | F-001, F-005      |
| S-04 | Walk                | `walk`             | planned | F-006~F-008       |
| S-05 | Walk History        | `walk_history`     | planned | F-009~F-010       |
| S-06 | Health              | `health/{petId}`   | planned | F-011~F-015       |
| S-07 | Health Record Add   | `health/add/{petId}` | planned | F-011~F-013     |
| S-08 | Feeding             | `feeding/{petId}`  | planned | F-016~F-017       |
| S-09 | Team                | `team`             | planned | F-023~F-026       |
| S-10 | Team Invite         | `team/invite`      | planned | F-024             |
| S-11 | Settings            | `settings`         | planned | -                 |
| S-12 | Login               | `login`            | planned | F-029             |
| S-13 | Paywall             | `paywall`          | planned | F-031             |
| S-14 | Statistics          | `stats`            | planned | F-032~F-035       |
| S-15 | Photo Diary         | `diary/{petId}`    | planned | F-018             |

## Screen Descriptions

### S-01: Home
- Primary dashboard showing all registered pets as horizontal cards
- Today's feeding timeline (who fed, when, how much)
- Upcoming D-day alerts (vaccination, vet visit)
- Quick action buttons: Start Walk, Add Feeding, Add Health Record
- Bottom navigation: Home / Walk / Health / Team / Settings

### S-02: Pet Add/Edit
- Form: name, type (dog/cat/other), breed (searchable dropdown), gender, birth date, neutered toggle, weight, photo
- Breed database search with auto-complete
- Photo picker (camera + gallery)
- Free tier limit check (1 pet) with paywall trigger

### S-03: Pet Detail
- Pet profile card with photo, basic info
- Weight history chart
- Recent walk summary
- Recent health records
- Navigation to Walk History, Health, Feeding sub-screens

### S-04: Walk
- Full-screen map with real-time GPS route drawing
- Start/pause/stop controls
- Live stats overlay: elapsed time, distance (km), speed (km/h), estimated calories
- Pet selector (which pet is walking)
- Background location tracking when app is minimized
- iOS: Dynamic Island / Live Activity integration point

### S-05: Walk History
- List of past walks with date, distance, duration, route thumbnail
- Tap to view walk detail: full route on map, stats breakdown
- Route replay animation on map
- Filter by pet, date range

### S-06: Health
- Per-pet health record list grouped by type (vaccination, vet visit, medication)
- D-day badges for upcoming events
- Total health cost summary
- FAB to add new record

### S-07: Health Record Add
- Form: type (vaccination/vet_visit/medication/weight/other), title, date, next due date, vet name, cost, notes
- Type-specific fields (e.g., vaccine name for vaccination)
- Auto-suggest next due date based on record type

### S-08: Feeding
- Per-pet daily feeding timeline
- Quick-add buttons for meal types (morning/lunch/dinner/snack)
- Form: meal type, amount (grams), time, note
- Today's feeding summary with total grams

### S-09: Team
- Team member list with avatars
- Recent activity feed (who walked, who fed, etc.)
- Team invite code display + share button
- Leave team / manage members (owner only)

### S-10: Team Invite
- Enter invite code to join existing team
- OR create new team with name input
- Deep link support for invite URLs

### S-11: Settings
- Account management (profile, sign out, delete account)
- Notification preferences (walk/feeding/health reminders)
- Theme (light/dark/system)
- Language selection
- Premium status / manage subscription
- Data export
- App version, privacy policy, terms

### S-12: Login
- Firebase Auth sign-in (Google, Apple)
- Onboarding flow for first-time users (add first pet)

### S-13: Paywall
- Premium feature comparison (Free vs Premium)
- Monthly / Annual subscription options
- Restore purchases
- Triggered from: pet limit, ad removal, statistics, export

### S-14: Statistics (Post-MVP)
- Walk trends: weekly/monthly distance, duration charts
- Feeding patterns: meal regularity chart
- Health expense breakdown: monthly cost chart
- Pet care score / streak

### S-15: Photo Diary
- Per-pet photo timeline with date grouping
- Photo upload with caption
- Grid / timeline view toggle
