# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**ZARIMAX (자리맥스)** — AI-powered commercial real estate matching platform connecting building owners (건물주), entrepreneurs (상가점주), and manufacturers (제조사). Core value proposition: "AI analyzes success probability in 10 seconds" using the formula `Success = f(Location, Item, Human)`.

The project is in **pre-development planning stage**. Planning documents and a landing page prototype exist, but no application code has been written yet.

## Deployment & Accounts

- **GitHub**: `toktoklabs/byeoljari` (https://github.com/toktoklabs/byeoljari)
  - Local git credential: 이 리포지토리는 `git config credential.helper ""`로 설정되어 toktoklabs 계정 자격 증명을 별도 사용 (시스템 기본 계정 `KYUNAM-carrot`과 분리)
- **Vercel**: `eunseos-projects-9975a347` 스코프, 프로젝트명 `byeoljari`
  - Production URL: https://byeoljari.vercel.app
  - GitHub `main` 브랜치 푸시 시 자동 배포
  - 정적 사이트 배포 (`public/` 디렉토리, 빌드 명령 없음)

## Planned Tech Stack

- **Frontend**: Flutter 3.x (Web + iOS + Android single codebase), Riverpod 2.x state management, Kakao Map SDK, fl_chart
- **Backend**: Python FastAPI, PostgreSQL 16 + PostGIS (via Supabase), Redis cache
- **AI**: OpenAI GPT-4o-mini / Claude API for success prediction engine
- **External APIs**: 소상공인진흥공단 상권정보 API, 국민건강보험공단 API, Kakao Map API, Toss Payments (Phase 2)
- **Database**: Supabase (managed PostgreSQL) with Supavisor connection pooling in Transaction Mode (port 6543)

## Architecture Decisions

- **Database**: Supabase chosen over self-hosted PostgreSQL, MongoDB, Firebase — PostGIS spatial queries are essential for map-based features (commercial area analysis, radius search, district matching)
- **Connection pooling**: Use `NullPool` in SQLAlchemy and let Supavisor handle pooling. Disable statement/prepared statement caches (`statement_cache_size=0, prepared_statement_cache_size=0`)
- **Map SDK**: Primary choice is `flutter_kakao_maps`; fallback to `kakao_map_plugin` (WebView-based) if unstable. A map abstraction layer should be implemented for provider switching
- **Scaling path**: Supabase Pro Micro ($25/mo) → Medium ($85/mo) → self-hosted PostgreSQL + PgBouncer at 100K+ DAU

## Planned Directory Structure

```
zarimax/
├── lib/                          # Flutter frontend
│   ├── main.dart
│   ├── core/services/            # api_service, auth_service, map_service
│   ├── data/
│   │   ├── models/
│   │   ├── repositories/
│   │   └── providers/
│   ├── features/
│   │   ├── auth/
│   │   ├── property/
│   │   ├── item/
│   │   ├── mbti/
│   │   ├── ai_prediction/
│   │   ├── map/
│   │   ├── search/
│   │   └── dashboard/
│   └── shared/widgets/
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── routers/
│   │   ├── models/
│   │   ├── services/
│   │   └── utils/
│   └── requirements.txt
└── pubspec.yaml
```

## Development Phases

**Phase 1 — MVP (16 weeks, 29 features)**: Auth, building/vacancy CRUD, product management, MBTI analysis, AI success prediction engine, Kakao Map integration, basic search/dashboard. Deliverable: Flutter Web beta.

**Phase 2 — Full Features (18 weeks, +38 features)**: Simulations, contract management, Toss Payments, notifications, advanced analytics, commercial area map overlays, community features. Deliverable: iOS/Android app store launch.

## Planning Documents

All planning materials are in `기획자료/` directory:
- `자리맥스_2단계_개발계획_PRD_260206수정본.md` — Full PRD v2.1 with feature specs, sprint plan, and technical details
- `자리맥스_DB_비교분석.md` — Database comparison analysis and Supabase architecture decisions
- `자리맥스_최종_요건정의서_v3.docx` — Final requirements definition
- `gateway_swagger_guide.pdf` — API gateway guide
- `소상공인시장진흥공단_상가(상권)정보_OpenApi 활용가이드/` — Government commercial area API guide and industry classification data
- `소상공인시장진흥공단_상가(상권)정보_20251030/` — CSV datasets for all 17 Korean provinces (Oct 2025)

## Existing Assets

- `byeoljari_landing.html` — Landing page 원본 (vanilla HTML/CSS/JS, constellation animation, dark theme with gold #d4a853 accent)
- `public/index.html` — Vercel 배포용 랜딩 페이지 (byeoljari_landing.html 복사본)

## Language

All planning documents and user-facing content are in Korean (한국어).
