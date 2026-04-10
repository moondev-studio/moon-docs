# ADR-010: CI/CD Pipeline Evolution
- 버전: v1.0.0 ~ v1.0.5
- 날짜: 2026-03 ~ 2026-04 (100+ CI 커밋 기반)
- 상태: 결정됨 (소급 기록)

## 배경
KMP 앱의 3-플랫폼(Android, iOS, Web) CI/CD를 구축하면서 OOM, 코드 서명, 멀티 레포 의존성 등
수많은 장애를 겪었다. 약 100회의 반복 수정 끝에 안정적 파이프라인 확립.

## 결정

### Phase 1: Self-hosted Runner (v1.0.0)
- Mac Mini M4 자체 호스팅 러너 (`moon-m4`)
- Android + iOS 순차 빌드 (동시 빌드 시 OOM)
- **문제**: 14GB RAM에서 iOS K/N 링커 OOM (8g~12g 힙 시도), Android R8 OOM
- 12+ 커밋으로 OOM 대응: `SerialGC`, `12g heap`, `DevirtualizationAnalysis 비활성화`

### Phase 2: GitHub-hosted Runner 전환 (commit a44d882, vc=36)
- `ubuntu-latest` (Android/Web) + `macos-15` / `macos-15-xlarge` (iOS)
- **문제**: moon-app-libs/moon-kmp-libs 시블링 체크아웃 필요 (Private repo, `SYNC_PAT`)
- 19 loop 수정 (vc=48~66): iOS provisioning, ExportOptions, versionCode 충돌

### Phase 3: Unified Pipeline (commit 6dfb859)
- test -> android + ios + web 병렬 -> Slack notify
- Fastlane 기반: Android Play Console + iOS TestFlight + Firebase Hosting (Web)
- Android OOM: `Xmx5g`, `--no-parallel`, lint skip
- iOS 서명: Manual code sign style, provisioning profile UUID 매핑

### Phase 4: Xcode Cloud + GitHub Actions 분리 (commit b45e58a)
- iOS: Xcode Cloud (Apple 관리 인프라로 서명/빌드 위임)
- Android + Web: GitHub Actions 유지
- **문제**: Xcode Cloud 러너가 x86_64 → JDK arch 불일치 6 loop 수정
- 최종: `Xmx3g + max-workers=1` (Android), timeout-minutes 제한

### Phase 5: 현재 형태 (v1.0.5)
- `deploy.yml`: Android + Web only (GitHub Actions)
- iOS: Xcode Cloud 별도 관리
- `SYNC_PAT`으로 moon-app-libs Private repo 체크아웃
- Gradle: `Xmx5g`, `MaxMetaspaceSize=512m`

### 핵심 교훈
- KMP iOS 빌드는 메모리 집약적 (K/N 링커 10g+ 필요) → Apple 인프라 위임이 최선
- Private 멀티 레포 CI는 PAT 토큰 + 시블링 체크아웃 패턴 필수
- Self-hosted runner OOM은 해결 불가 → 클라우드 러너 전환

## 영향 범위
- `.github/workflows/deploy.yml` — Android + Web 배포
- Xcode Cloud 설정 (Apple Developer Console) — iOS 배포
- `build-logic/` — Gradle 메모리/병렬 설정
- Fastlane (`Fastfile`, `Matchfile`) — 배포 자동화

## 이전 니즈 참조
ADR-001 (Initial Design) — 3-플랫폼 동시 배포 요구사항
ADR-003 (iOS Review Rejections) — iOS 배포 파이프라인 안정화 필요성
