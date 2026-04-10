# ADR-007: Crossfade Navigation Strategy
- 버전: v1.0.0
- 날짜: 2026-03 (초기 설계)
- 상태: 결정됨 (소급 기록)

## 배경
Compose Multiplatform에서 화면 전환 방식을 결정해야 했다.
`navigation-compose` 라이브러리는 KMP 지원이 불안정하고, 타입 안전성과 단순성이 부족했다.

## 결정
- **navigation-compose 미사용**: 별도 라이브러리 의존성 없이 직접 구현
- **sealed class Screen**: 모든 화면을 sealed class로 정의하여 타입 안전성 확보
- **SnapshotStateList 백스택**: `mutableStateListOf<Screen>(Screen.Home)`으로 상태 관리
  - `navigateTo`: `backStack.add(screen)`
  - `navigateBack`: `backStack.removeAt(backStack.lastIndex)` (removeLast() 대신 Android 14 호환)
  - `navigateToRoot`: `backStack.clear()` + `backStack.add(screen)`
- **Crossfade 트랜지션**: `Crossfade(targetState = currentScreen)` 으로 화면 전환 애니메이션
- **BottomTab 통합**: `onTabSelected`에서 backStack을 clear 후 탭 화면으로 교체
- **PlatformBackHandler**: expect/actual로 시스템 뒤로가기 처리

## 영향 범위
- `composeApp/src/commonMain/.../App.kt` — 전체 네비게이션 로직 (backStack, Crossfade, Screen sealed class)
- `composeApp/src/commonMain/.../presentation/component/navigation/` — BottomBar, Tab 정의
- 전체 화면 (30+ Screen cases)

## 이전 니즈 참조
ADR-001 (Initial Design) — UI 프레임워크 선택의 구체 구현
