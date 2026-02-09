# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

LogWatch Admin - 실시간 로그 수집, 분석 및 모니터링을 위한 웹 기반 어드민 대시보드

**기술 스택:**
- Next.js 16 (App Router)
- TypeScript
- Tailwind CSS v4
- Turbopack

## 개발 명령어

```bash
# 의존성 설치
npm install

# 개발 서버 시작 (localhost:3010)
npm run dev

# 프로덕션 빌드
npm run build

# 프로덕션 서버 실행
npm run start

# 린트 검사
npm run lint
```

## 아키텍처 및 구조

### 라우팅 시스템

Next.js App Router를 사용하며 두 가지 라우트 그룹으로 구성:

1. **`(dashboard)` 그룹** - Sidebar + Header 레이아웃
   - `/` - 대시보드 메인
   - `/logs` - 로그 목록 및 검색
   - `/alerts` - 알림 관리
   - `/settings` - 시스템 설정

2. **`(auth)` 그룹** - 중앙 정렬 레이아웃
   - `/login` - 로그인
   - `/register` - 회원가입

### 디렉토리 구조 원칙

- `app/` - Next.js App Router 페이지 및 API 라우트
- `components/ui/` - 재사용 가능한 UI 프리미티브 (Button, Input, Card 등)
- `components/layout/` - 레이아웃 셸 컴포넌트 (Sidebar, Header)
- `components/features/` - 기능별 복합 컴포넌트
- `lib/` - 공유 유틸리티, 상수, 헬퍼 함수
- `hooks/` - 커스텀 React 훅
- `types/` - TypeScript 타입 정의
- `services/` - API 클라이언트 함수 및 외부 서비스 연동
- `styles/` - 글로벌 스타일

## 코딩 컨벤션

### Import 별칭
`@/*`는 `src/*`에 매핑됨:
```typescript
import { Sidebar } from "@/components/layout/Sidebar";
```

### 컴포넌트 Export
**named export 사용** (default export 사용하지 않음):
```typescript
export function LogTable() { ... }
```

### API 라우트 핸들러
`NextResponse.json()`으로 응답 반환:
```typescript
import { NextResponse } from "next/server";

export async function GET() {
  return NextResponse.json({ logs: [] });
}
```

## API 엔드포인트

| 메서드 | 경로 | 설명 |
|--------|------|------|
| `GET` | `/api/logs` | 로그 목록 조회 |
| `GET` | `/api/logs/:id` | 로그 상세 조회 |
| `GET` | `/api/alerts` | 알림 규칙 목록 |
| `POST` | `/api/alerts` | 알림 규칙 생성 |
| `GET` | `/api/stats` | 대시보드 통계 데이터 |
| `POST` | `/api/auth/login` | 로그인 |
| `POST` | `/api/auth/register` | 회원가입 |

## 주요 기능

- **실시간 로그 스트림** - 로그 레벨별 필터링 (ERROR, WARN, INFO, DEBUG)
- **로그 검색** - 키워드, 날짜/시간 범위, 소스별 필터
- **알림 시스템** - 임계값 기반 알림 규칙 및 채널 관리
- **시스템 관리** - 사용자 계정, 로그 수집 소스, 보존 정책
