# CI/CD 가이드

## 원칙

- **Android**: GitHub Actions (`ubuntu-latest`) → Play Console 내부 트랙
- **iOS**: Xcode Cloud (OOM 우회) — GitHub Actions iOS는 Native link OOM 발생
- **Web**: Firebase Hosting (GitHub Actions)
- **로컬 빌드 우선**: CI는 검증용, 배포는 로컬 + Fastlane

## GitHub Secrets (4개 앱 공통)

| Secret | 설명 |
|--------|------|
| `IOS_CERTIFICATE_P12_BASE64` | moondeveloper_distribution.p12 (만료 2027-04-04) |
| `IOS_CERTIFICATE_PASSWORD` | p12 비밀번호 |
| `IOS_PROVISION_PROFILE_BASE64` | 앱별 프로비저닝 프로파일 |
| `KEYCHAIN_PASSWORD` | 키체인 비밀번호 |
| `ASC_API_KEY_CONTENT` | ASC API Key 내용 |
| `ASC_API_KEY_ID` | `29JY53H377` |
| `ASC_API_ISSUER_ID` | `656c7d95-5d6b-4b46-a200-0b8a157f5f11` |
| `GOOGLE_PLAY_SERVICE_ACCOUNT_JSON` | Play Console 서비스 계정 |

## 주요 패턴

```yaml
# secrets는 if: 조건에서 사용 불가 → env 컨텍스트 사용
env:
  HAS_SECRET: ${{ secrets.MY_SECRET != '' }}
steps:
  - if: env.HAS_SECRET == 'true'
```

## 알려진 이슈

| 이슈 | 원인 | 해결 |
|------|------|------|
| iOS OOM | `linkReleaseFrameworkIosArm64` 8GB 힙 초과 | Xcode Cloud 전환 |
| Android CI PASS but Play Console 반영 안됨 | versionCode 미증가 | `versionCode` 확인 |
| `secrets` in `if:` | GitHub Actions 제약 | `env` 컨텍스트 사용 |
