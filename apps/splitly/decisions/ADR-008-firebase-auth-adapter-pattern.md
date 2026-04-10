# ADR-008: Firebase Auth Adapter Pattern
- 버전: v1.0.0
- 날짜: 2026-03 (commit 97734a1 기반)
- 상태: 결정됨 (소급 기록)

## 배경
Firebase Auth를 KMP commonMain에서 사용하되, 플랫폼별 SDK 차이를 추상화해야 했다.
Android는 Google Play Services Firebase, iOS는 Firebase Swift SDK를 사용하므로 다층 추상화 필요.

## 결정
- **3-Layer 추상화**:
  1. **L1 (moon-auth-kmp)**: `AuthProvider` 인터페이스 (OSS, Firebase 무관)
  2. **L2 (moon-firebase-auth-kmp)**: `FirebaseAuthProvider` 구현체 (Firebase 의존, Private)
  3. **L3 (splitly-auth)**: `SplitlyAuthModule` — 플랫폼별 DI 바인딩
     - Android/iOS: `FirebaseAuthProvider` 주입
     - Desktop: `NoOpAuthProvider` 주입
  4. **L4 (composeApp)**: `AuthProviderAdapter` — `AuthProvider`를 `AuthRepository`로 래핑
- **AuthRepository 인터페이스**: Email/Google/Apple 로그인, 로그아웃, 계정 삭제, 비밀번호 변경
- **SocialAuthManager**: expect/actual 패턴으로 Google/Apple 소셜 로그인 분리
  - commonMain: `expect class SocialAuthManager` + `expect fun createSocialAuthManager()`
  - androidMain: `FirebaseAuth` + `OAuthProvider` 사용
  - iosMain: iOS 네이티브 인증 위임
- **테스트 전략**: `FakeAuthRepository`로 commonTest에서 ViewModel 테스트 (MockK 대신 Fake)

## 영향 범위
- `moon-auth-kmp/` — `AuthProvider` 인터페이스 (L1)
- `moon-firebase-auth-kmp/` — `FirebaseAuthProvider` 구현 (L2)
- `libs/splitly-auth/` — 플랫폼별 DI 모듈 (L3)
- `composeApp/.../data/repository/AuthProviderAdapter.kt` — Adapter (L4)
- `composeApp/.../auth/SocialAuthManager.kt` — expect/actual 소셜 인증
- `composeApp/.../domain/repository/AuthRepository.kt` — 도메인 인터페이스
- `composeApp/src/commonTest/.../test/fake/FakeAuthRepository.kt` — 테스트 Fake

## 이전 니즈 참조
ADR-001 (Initial Design) — KMP expect/actual 원칙의 인증 적용
ADR-006 (4-Tier Module Separation) — L1~L4 계층별 역할 배분
