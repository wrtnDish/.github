<div align="center">

# WRTN Dish - AI 기반 음식 추천 시스템 <img src="https://github.com/wrtnDish/dish-frontend/blob/main/public/wrtn-dish.png" alt="WRTN Dish Logo" width="45" align="center">

> Agentica를 활용한 지능형 음식 추천 플랫폼

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Bun](https://img.shields.io/badge/Bun-000000?logo=bun&logoColor=white)](https://bun.sh)
[![NestJS](https://img.shields.io/badge/NestJS-E0234E?logo=nestjs&logoColor=white)](https://nestjs.com/)
[![React](https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black)](https://react.dev/)

</div>

## 프로젝트 소개

**WRTN Dish**는 날씨 데이터, 사용자 히스토리, 위치 기반 레스토랑 정보를 통합하여 실시간으로 최적의 음식을 추천하는 AI 에이전트 시스템입니다. **Agentica RPC** 프레임워크를 기반으로 구축되어 OpenAI GPT-4o-mini를 활용한 지능형 대화형 추천 서비스를 제공합니다.

###  주요 특징

- **AI 에이전트 기반 추천**: Agentica RPC를 활용한 멀티 에이전트 오케스트레이션
- **날씨 연동 추천**: 실시간 날씨 정보를 반영한 음식 카테고리 제안
- **위치 기반 검색**: Naver Local Search API를 통한 주변 맛집 정보 제공
- **실시간 채팅**: WebSocket 기반 스트리밍 응답으로 자연스러운 대화 경험
- **타입 안전성**: Typia 기반 런타임 타입 검증 및 Nestia SDK 자동 생성
- **개인화 추천**: 사용자의 식사 히스토리 및 선호도 분석

## 아키텍처

### 기술 스택

#### 백엔드 (Server)
- **프레임워크**: NestJS 10.x + Nestia (타입 안전 REST/WebSocket)
- **AI 오케스트레이션**: Agentica RPC 0.31.x
- **런타임 검증**: Typia 9.x
- **AI 모델**: OpenAI GPT-4o-mini
- **외부 API**: 기상청 API, Naver Local Search
- **통신**: TGrid (타입 안전 WebSocket RPC)

#### 프론트엔드 (Client)
- **프레임워크**: React 19.x + Vite 7.x
- **스타일링**: Tailwind CSS 4.x
- **마크다운 렌더링**: React Markdown + Syntax Highlighting (Prism, Highlight.js)
- **AI 통신**: Agentica RPC Client

#### 런타임 & 도구
- **런타임**: Bun 또는 Node.js
- **언어**: TypeScript 5.9.x
- **코드 품질**: ESLint, Prettier

### AI 에이전트 구조

```
ChatController (WebSocket 진입점)
    │
    └── OrchestratorAgentController (스마트 오케스트레이터)
        ├── 의도 분석 (Intent Analysis)
        ├── 대화 상태 관리 (Conversation State)
        │
        ├── WeatherAgentController (날씨 전문 에이전트)
        │   └── WeatherService (날씨 정보 수집)
        │
        ├── IntegratedFoodAgentController (통합 음식 추천)
        │   ├── WeatherService (날씨 정보 수집)
        │   ├── FoodService (음식 카테고리 관리)
        │   ├── UserHistoryService (사용자 선호도 분석)
        │   └── IntegratedScoringService (통합 점수 계산)
        │
        └── RestaurantController (맛집 검색)
```

**스마트 오케스트레이터 패턴**:
- **단일 진입점**: 21개 함수 → 1개 오케스트레이터로 단순화
- **의도 분석**: 사용자 메시지를 분석하여 적절한 서비스 자동 선택
- **컨텍스트 관리**: 대화 상태를 추적하여 중복 정보 제공 방지
- **메타 질문 처리**: "너 뭐할 수 있어?" 같은 시스템 질문 지원

**Function Calling Flow**:
1. 사용자 메시지 → WebSocket 수신
2. OrchestratorAgentController가 의도 분석
3. 적절한 에이전트로 라우팅 (날씨, 음식, 맛집)
4. 대화 컨텍스트 업데이트
5. 통합 결과 생성 → 스트리밍 응답

## 시작하기

### 사전 요구사항

- [Bun](https://bun.sh) 1.0 이상 (권장)
- Node.js 18 이상 (또는 Bun 대신 사용)
- 외부 API 키:
- [OpenRouter API Key](https://openrouter.ai/settings/keys)
- [기상청 API Key](https://apihub.kma.go.kr/) (기상청 API Hub 회원가입 필요)
- [Naver Open API](https://developers.naver.com/main/) (Client ID & Secret)

### 설치 및 실행

#### 1. 저장소 클론

```bash
git clone https://github.com/wrtn/wrtn-dish.git
cd wrtn-dish
```

#### 2. 백엔드 설정

**Bun 사용 (권장)**:
```bash
cd server

# 의존성 설치
bun install

# 환경변수 설정
cp .env.example .env  # 아래 환경변수 예시 참고

# TypeScript 트랜스포머 설치 (필수)
bun run prepare

# 프로젝트 빌드 (SDK + Main + Test)
bun run build

# 개발 서버 실행
bun run dev
```

**npm 사용**:
```bash
cd server

# 의존성 설치
npm install

# 환경변수 설정
cp .env.example .env  # 아래 환경변수 예시 참고

# TypeScript 트랜스포머 설치 (필수)
npm run prepare

# 프로젝트 빌드 (SDK + Main + Test)
npm run build

# 개발 서버 실행
npm run dev
```

**환경변수 (.env 예시)**:
```bash
# Server Configuration
API_PORT=37001

# OpenRouter API Configuration
# Get your API key from: https://openrouter.ai/settings/keys
OPENROUTER_API_KEY=sk-or-your_openrouter_api_key_here

# 기상청(KMA) API Configuration
# Get your API key from: https://apihub.kma.go.kr/ (회원가입 필요)
WEATHER_API_KEY=your_kma_api_key_here

# Naver Open API Configuration
# Get your credentials from: https://developers.naver.com/main/
NAVER_CLIENT_ID=your_naver_client_id_here
NAVER_CLIENT_SECRET=your_naver_client_secret_here
```

#### 3. 프론트엔드 설정

**Bun 사용 (권장)**:
```bash
cd client

# 의존성 설치
bun install

# 환경변수 설정 (선택사항)
# .env 파일에 아래 내용 추가
# VITE_AGENTICA_WS_URL=ws://localhost:37001/chat

# 개발 서버 실행
bun run dev
```

**npm 사용**:
```bash
cd client

# 의존성 설치
npm install

# 개발 서버 실행
npm run dev
```

#### 4. 접속

- **프론트엔드**: http://localhost:5173
- **백엔드 API**: http://localhost:37001
- **Swagger Editor**: http://localhost:37001/editor

## 프로젝트 구조

```
wrtn-dish/
├── server/                    # NestJS 백엔드
│   ├── src/
│   │   ├── controllers/       # WebSocket & REST 컨트롤러
│   │   │   ├── chat/          # AI 채팅 컨트롤러 (Agentica Agents)
│   │   │   │   ├── ChatController.ts              # WebSocket 진입점
│   │   │   │   ├── OrchestratorAgentController.ts # 스마트 오케스트레이터
│   │   │   │   ├── IntegratedFoodAgentController.ts # 통합 음식 추천
│   │   │   │   └── WeatherAgentController.ts      # 날씨 전문 에이전트
│   │   │   ├── food/          # 음식 추천 REST 엔드포인트
│   │   │   ├── weather/       # 날씨 REST 엔드포인트
│   │   │   └── restaurant/    # 레스토랑 검색 엔드포인트
│   │   ├── services/          # 비즈니스 로직 서비스
│   │   │   ├── IntegratedScoringService.ts   # 통합 점수 계산
│   │   │   ├── WeatherService.ts             # 날씨 정보 수집
│   │   │   ├── FoodService.ts                # 음식 카테고리 관리
│   │   │   ├── UserHistoryService.ts         # 사용자 선호도 분석
│   │   │   └── ...
│   │   ├── providers/         # 외부 API 프로바이더
│   │   │   └── restaurant/RestaurantProvider.ts
│   │   ├── api/               # Nestia 자동생성 API 구조체 & SDK
│   │   │   ├── structures/    # API 타입 정의
│   │   │   └── functional/    # 자동생성 SDK 클라이언트
│   │   ├── data/              # 정적 데이터 (음식 카테고리, 도시 좌표)
│   │   ├── utils/             # 유틸리티 함수 (좌표 변환, 에러 처리 등)
│   │   ├── modules/           # NestJS 모듈
│   │   └── executable/        # 실행 진입점 (server, swagger)
│   ├── test/                  # E2E 테스트
│   ├── packages/api/          # 배포 가능한 API SDK 패키지
│   └── lib/                   # 컴파일된 코드
│
├── client/                    # React 프론트엔드
│   ├── src/
│   │   ├── components/        # React 컴포넌트
│   │   ├── hooks/             # 커스텀 훅
│   │   ├── provider/          # Context 프로바이더
│   │   └── App.tsx            # 메인 앱
│   └── dist/                  # 빌드 결과물
│
└── README.md                  # 이 문서
```

## 테스트

### 백엔드 테스트

```bash
cd server

# 테스트 빌드 및 전체 테스트 실행
npm run build:test
npm test  # 또는 bun test

# 개별 테스트 파일 실행
node bin/test/features/api/<test-file>.js

# Bun 전용 테스트 스크립트
bun run bun:test           # 모든 유닛 테스트
bun run bun:test:unit      # 유닛 테스트만
bun run bun:test:integration  # 통합 테스트만
bun run bun:test:all       # 전체 테스트 (유닛 + 통합)

# Webpack 빌드 테스트
npm run test:webpack
```

## 개발 가이드

### SDK 재생성

컨트롤러 인터페이스 수정 후 반드시 SDK를 재생성해야 합니다:

```bash
cd server
npm run build:sdk  # 또는 bun run build:sdk
```

### 코드 품질 관리

```bash
cd server

# Linting
npm run eslint        # 또는 bun run eslint
npm run eslint:fix    # 또는 bun run eslint:fix

# Formatting
npm run prettier      # 또는 bun run prettier
```

### Swagger 문서 확인

서버 실행 후 http://localhost:37001/editor 에서 API 문서 확인 가능

## 라이선스

이 프로젝트는 **MIT License**로 배포됩니다.

```
Copyright (c) 2025 WiSoft

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```

자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.

### 오픈소스 고지

이 프로젝트는 다양한 오픈소스 라이브러리를 사용합니다:
- **server/NOTICE**: 백엔드 종속성 라이선스 목록
- **client/NOTICE**: 프론트엔드 종속성 라이선스 목록

종속성 라이선스 확인:
```bash
cd server && npx license-checker --summary  # 또는 bunx license-checker --summary
cd client && npx license-checker --summary  # 또는 bunx license-checker --summary
```

## 대회 정보

본 프로젝트는 **Agentica 프레임워크 활용 대회**를 위해 개발되었습니다.

### 핵심 기술 포인트

1. **Agentica RPC 활용**: 멀티 에이전트 오케스트레이션으로 복잡한 추천 로직 구현
2. **실시간 Function Calling**: OpenAI Function Calling과 에이전트 통합
3. **타입 안전성**: TypeScript + Typia + Nestia로 엔드투엔드 타입 안정성 보장
4. **확장 가능한 아키텍처**: 서비스 기반 설계로 새로운 추천 요소 추가 용이

## 팀 정보

**Team WiSoft**
- 팀장: [YuJaeyoung](https://github.com/Yu-Jaeyoung)
- 팀원: [MoonDongmin](https://github.com/MoonDongmin)

## 참고 링크

- [Agentica 공식 문서](https://wrtnlabs.io/agentica/docs/)
- [NestJS 공식 문서](https://docs.nestjs.com/)
- [Nestia 공식 문서](https://nestia.io/)
- [Bun 공식 사이트](https://bun.sh/)

## 문의

프로젝트 관련 문의사항은 GitHub Issues를 통해 남겨주세요.

---

**Built with ❤️ using Agentica**
