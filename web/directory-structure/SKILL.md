---
name: directory-structure
description: Pokitwork 프로젝트 디렉토리 구조 가이드. SvelteKit 파일 기반 라우팅, 컴포넌트 배치 규칙, 탭 페이지 패턴, 현재 라우트 맵. 새 파일 생성이나 프로젝트 구조 파악 시 참고한다.
---

# 디렉토리 구조

## SvelteKit 기본 구조

```
src/
  app.html              ← HTML 셸
  app.d.ts              ← 글로벌 타입 (App.Locals 등)
  hooks.server.ts       ← SvelteKit 서버 훅 (인증 세션 주입)
  routes/               ← 파일 기반 라우팅
    +page.svelte        ← 페이지 컴포넌트
    +layout.svelte      ← 레이아웃 (하위 라우트에 상속)
    layout.css          ← 글로벌 CSS (테마 변수)
    api/                ← REST 엔드포인트 (+server.ts)
  lib/                  ← $lib 별칭
    auth-client.ts      ← better-auth 클라이언트
    components/         ← 공유 컴포넌트
    entities/           ← 도메인 헬퍼 함수 + 타입 re-export
    server/             ← 서버 전용 코드 ($lib/server)
      auth.ts           ← better-auth 서버 인스턴스
      db/               ← Drizzle 인스턴스 + 스키마
      domain/           ← Active Record 도메인 클래스
      infra/            ← Query Service 클래스
    utils.ts
```

## 라우팅 규칙

- `+` 접두어 파일만 라우트로 인식: `+page.svelte`, `+layout.svelte`, `+server.ts`
- `+` 없는 `.svelte` 파일은 일반 컴포넌트로 취급
- 하위 폴더도 라우트에 영향 없음 (단, `+page.svelte`가 있으면 라우트 생성)

## 탭 페이지 패턴

```
members/
  +page.svelte             ← 탭 구조만 관리
  member-tab.svelte        ← 구성원 탭
  member-tab/              ← member-tab 전용 하위 컴포넌트
    add-member-dialog.svelte
    invite-dialog.svelte
  department-tab.svelte    ← 부서 탭
  position-tab.svelte      ← 직책 탭
```

## 현재 라우트 구조

```
routes/
  api/
    login/check-email/+server.ts       ← POST
    invite/[token]/+server.ts          ← GET (토큰 검증)
    invite/[token]/accept/+server.ts   ← POST (초대 수락)
    admin/
      members/+server.ts              ← POST (추가)
      members/[id]/+server.ts         ← PATCH|DELETE
      members/invite/+server.ts       ← POST (초대)
      departments/+server.ts          ← POST (생성)
      departments/[id]/+server.ts     ← PATCH|DELETE
      departments/[id]/members/+server.ts          ← POST (배정)
      departments/[id]/members/[memberId]/+server.ts ← DELETE (해제)
      departments/reorder/+server.ts  ← PUT
      leaves/adjust/+server.ts        ← POST
      leaves/usages/+server.ts        ← GET
      notices/+server.ts              ← POST
      notices/[id]/+server.ts         ← PATCH|DELETE
      notice-categories/+server.ts    ← POST
      notice-categories/[id]/+server.ts ← PATCH|DELETE
    user/
      leave-requests/+server.ts       ← POST
      approvals/[id]/decide/+server.ts ← POST
  login/+page.svelte                   ← Email OTP
  invite/+page.svelte                  ← 초대 온보딩
  admin/
    +layout.svelte                     ← 사이드바 레이아웃
    +page.svelte                       ← 대시보드
    members/+page.svelte               ← 구성원/부서/직책 탭
    leaves/+page.svelte                ← 연차 현황
    approvals/+page.svelte             ← 결재현황/결재선 탭
    notices/+page.svelte               ← 공지사항/카테고리 탭
  (app)/
    +layout.svelte                     ← 모바일 하단 네비게이션
    +page.svelte                       ← 홈
    inbox/+page.svelte                 ← Inbox
    inbox/[id]/+page.svelte            ← 결재 상세
    directory/+page.svelte             ← 조직도
    directory/[id]/+page.svelte        ← 부서 상세
    profile/+page.svelte               ← 프로필
    notices/+page.svelte               ← 공지사항
    leaves/request/+page.svelte        ← 연차 신청
    leaves/calendar/+page.svelte       ← 부서 연차 현황
```
