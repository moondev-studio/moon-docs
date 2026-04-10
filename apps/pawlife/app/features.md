# PawLife — Feature Spec (Design)

- Status: design-spec (planned, not yet implemented)
- Date: 2026-04-10
- Source: Design-first spec based on scaffold structure and product overview

## MVP Features

| ID    | Category       | Feature                                    | Status  | Module (L3)              |
|-------|----------------|--------------------------------------------|---------|--------------------------|
| F-001 | Pet Profile    | Pet profile CRUD (create/read/update/delete) | planned | pawlife-feature-pet      |
| F-002 | Pet Profile    | Breed database (dog/cat/other species)     | planned | pawlife-domain           |
| F-003 | Pet Profile    | Pet photo upload                           | planned | pawlife-feature-pet      |
| F-004 | Pet Profile    | Multi-pet support (Free: 1, Premium: unlimited) | planned | pawlife-feature-pet  |
| F-005 | Pet Profile    | Pet weight tracking                        | planned | pawlife-feature-health   |
| F-006 | Walk           | GPS walk start/stop                        | planned | pawlife-feature-walk     |
| F-007 | Walk           | Real-time route tracking on map            | planned | pawlife-feature-walk     |
| F-008 | Walk           | Distance/time/calories/speed calculation   | planned | pawlife-feature-walk     |
| F-009 | Walk           | Walk history list + detail                 | planned | pawlife-feature-walk     |
| F-010 | Walk           | Walk route replay on map                   | planned | pawlife-feature-walk     |
| F-011 | Health Record  | Vaccination record CRUD                    | planned | pawlife-feature-health   |
| F-012 | Health Record  | Vet visit record CRUD                      | planned | pawlife-feature-health   |
| F-013 | Health Record  | Medication record CRUD                     | planned | pawlife-feature-health   |
| F-014 | Health Record  | D-day countdown for next vaccination/visit | planned | pawlife-feature-health   |
| F-015 | Health Record  | Health cost tracking                       | planned | pawlife-feature-health   |
| F-016 | Schedule       | Feeding record (meal type/amount/time)     | planned | pawlife-feature-health   |
| F-017 | Schedule       | Daily feeding timeline                     | planned | pawlife-feature-health   |
| F-018 | Schedule       | Photo diary with timestamp                 | planned | pawlife-feature-pet      |
| F-019 | Notification   | Walk reminder                              | planned | pawlife-domain           |
| F-020 | Notification   | Feeding time reminder                      | planned | pawlife-domain           |
| F-021 | Notification   | Vaccination/vet visit D-day alert          | planned | pawlife-domain           |
| F-022 | Notification   | Pet birthday reminder                      | planned | pawlife-domain           |
| F-023 | Team           | Create team (family/couple)                | planned | pawlife-feature-team     |
| F-024 | Team           | Invite via code                            | planned | pawlife-feature-team     |
| F-025 | Team           | Shared pet management                      | planned | pawlife-feature-team     |
| F-026 | Team           | Activity feed (who did what)               | planned | pawlife-feature-team     |
| F-027 | Sync           | Firestore cloud sync                       | planned | pawlife-data             |
| F-028 | Sync           | Offline-first with local cache             | planned | pawlife-data             |
| F-029 | Auth           | Firebase Auth (Google/Apple sign-in)       | planned | composeApp               |
| F-030 | Ads            | AdMob banner + interstitial (Free tier)    | planned | composeApp               |
| F-031 | Paywall        | Premium subscription (monthly/annual)      | planned | composeApp               |

## Post-MVP Features

| ID    | Category       | Feature                                    | Status  | Target  |
|-------|----------------|--------------------------------------------|---------|---------|
| F-032 | Statistics     | Walk statistics (weekly/monthly trends)    | planned | v1.1    |
| F-033 | Statistics     | Feeding statistics                         | planned | v1.1    |
| F-034 | Statistics     | Health expense summary                     | planned | v1.1    |
| F-035 | Statistics     | Monthly pet care report                    | planned | v1.1    |
| F-036 | Platform       | iOS Dynamic Island (walk in progress)      | planned | v1.1    |
| F-037 | Platform       | Android Glance Widget                      | planned | v1.1    |
| F-038 | Wearable       | WearOS walk start/stop                     | planned | v1.2    |
| F-039 | Wearable       | watchOS walk start/stop                    | planned | v1.2    |
| F-040 | Export         | Pet data CSV/JSON export                   | planned | v1.2    |

## Domain Models (from scaffold)

```kotlin
// pawlife-domain: Models.kt
enum class PetType { DOG, CAT, OTHER }
enum class PetGender { MALE, FEMALE }
enum class HealthRecordType { VACCINATION, VET_VISIT, MEDICATION, WEIGHT, OTHER }
enum class MealType { MORNING, LUNCH, DINNER, SNACK }

data class Pet(id, name, type, breed, gender, birthDate, isNeutered, weightKg, photoUrl, teamId, ownerId)
data class WalkSession(id, petId, userId, startTime, endTime, distanceMeters, durationSeconds, routePointsJson, caloriesKcal, isCompleted)
data class HealthRecord(id, petId, type, title, note, date, nextDueDate, vetName, costAmount)
data class FeedingRecord(id, petId, userId, mealType, amountGrams, timestamp, note)
data class PawLifeTeam(id, name, memberIds, petIds, inviteCode, createdBy)
```

## Module Structure (L3)

| Module | Features |
|--------|----------|
| `pawlife-domain` | Models, Repository interfaces, NotificationScheduler |
| `pawlife-data` | FirestorePetRepository, LocalSource, DataModule |
| `pawlife-feature-pet` | F-001~F-005, F-018 |
| `pawlife-feature-walk` | F-006~F-010 (WalkUseCase exists) |
| `pawlife-feature-health` | F-011~F-017 |
| `pawlife-feature-team` | F-023~F-026 |
| `composeApp` | F-029~F-031, navigation, DI |
