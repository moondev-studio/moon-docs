# ADR-004: AdMob v2 Integration Strategy
- 버전: v1.0.1 ~ v1.0.3
- 날짜: 2026-03 (추정, commit 3d4759c / f98a525 기반)
- 상태: 결정됨 (소급 기록)

## 배경
Free tier 사용자에게 광고를 노출하면서도 보상형 광고로 임시 프리미엄 체험을 제공해야 했다.
초기에는 `AdMobRepository`로 구현했으나, v2에서 `AdService` 인터페이스 + `PlatformAdService` expect/actual 패턴으로 재설계.

## 결정
- **3가지 광고 형식 채택**: Banner, Rewarded, RewardedInterstitial
  - Banner: 비프리미엄 사용자 하단 노출 (`AdMobBanner` composable, `SafeAdContainer` 래핑)
  - Rewarded: 시청 완료 시 12시간 프리미엄 체험 (`WatchAdForRewardUseCase`)
  - RewardedInterstitial: 보상형 전면 광고 (PlatformAdService에서 로드)
- **expect/actual 분리**: `AdConfig` (광고 단위 ID), `PlatformAdService` (로드/표시 로직)
  - commonMain: `AdService` interface + `AdConfig` expect object
  - androidMain: Google Mobile Ads SDK (`RewardedAd`, `RewardedInterstitialAd`)
  - iosMain: `IOSAdBridge` + `IOSRewardedAdBridge` (iOS AdMob SDK 래핑)
- **테스트/프로덕션 ID 분리**: `BuildConfig.DEBUG`로 Google 테스트 광고 ID 자동 전환
- **deprecated 코드 보존**: 기존 `AdMobRepository` 유지, 신규 코드는 `AdService`/`PlatformAdService` 사용
- **RewardedAdManager**: Android 전용 유틸리티로 `RewardedAd` 라이프사이클 관리

## 영향 범위
- `composeApp/src/commonMain/.../domain/service/AdService.kt` — 인터페이스
- `composeApp/src/commonMain/.../util/AdConfig.kt` — expect 광고 ID
- `composeApp/src/commonMain/.../data/service/PlatformAdService.kt` — expect 클래스
- `composeApp/src/androidMain/.../util/AdConfig.android.kt` — 프로덕션/테스트 ID
- `composeApp/src/androidMain/.../util/RewardedAdManager.kt` — Android 보상형 광고
- `composeApp/src/androidMain/.../util/AdMobBanner.android.kt` — 배너 composable
- `composeApp/src/iosMain/.../util/IOSAdBridge.kt`, `IOSRewardedAdBridge.kt`

## 이전 니즈 참조
ADR-002 (IAP Subscription Model) — Free tier 광고 수익 모델과 직결
