# Splitly — Product Overview
> Version: 1.0.5 | Updated: 2026-04-08 | Status: ASC 심사 대기

## 제품 개요
| 항목 | 내용 |
|------|------|
| 앱명 | Splitly |
| 슬로건 | 더 쉬운 정산, 더 행복한 관계 |
| 타겟 | 20~40대, 공동 지출 상황 (여행/회식/동거) |
| 플랫폼 | Android, iOS, Web (splitlyapp.net) |
| 번들 ID | com.moondeveloper.splitly |

## 핵심 기능
- 정산 계산 (영수증 기반, 커스텀 비율 지원)
- 그룹 관리 (멤버 초대, 지출 히스토리)
- 정산 완료 처리 (유료: 자동 저장)
- OCR 영수증 스캔 (moon-ocr-kmp)
- 웹 버전 연동 (Firebase Hosting, splitlyapp.net)

## 수익 모델
| Tier | 가격 | 기능 |
|------|------|------|
| Free | 무료 | 기본 정산, 광고 있음, 저장 버튼 수동 |
| Pro Monthly | ₩3,900/월 | 자동 저장, 광고 없음, 고급 통계 |
| Pro Annual | ₩29,900/년 | Pro + 2개월 무료 |

## 현재 버전 히스토리
| 버전 | 주요 변경 | 날짜 |
|------|---------|------|
| v1.0.0 | 초기 출시 (Android) | 2026-03 |
| v1.0.2 | IAP 추가, OSS 전환 | 2026-03 |
| v1.0.3 | AdMob PROD, versionCode 13 | 2026-04 |
| v1.0.5 | iOS 심사 거절 수정 (PassKit/ATT/삭제/IAP) | 2026-04 |

## 아키텍처
- L1: moon-kmp-libs OSS (io.github.moondev-studio)
- L2: moon-app-libs (includeBuild)
- L3: splitly/libs (splitly-expense, splitly-group 등)
- L4: splitly/composeApp
- Backend: moon-server /api/splitly/v1/*
- Web: Next.js + Firebase Hosting (splitlyapp.net)

## 서드파티
- Firebase Auth + Firestore
- AdMob (Android + iOS)
- Google Play + App Store
- AdSense (웹)

## 관련 ADR
- ADR-001: 초기 정산 알고리즘 설계
- ADR-002: IAP 모델 결정 (소모성 → 구독)
