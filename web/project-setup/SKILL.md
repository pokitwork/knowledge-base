---
name: project-setup
description: Pokitwork 프로젝트 지식베이스 초기 설정. AGENTS.md 생성, 에이전트별 설정 파일 구성, 프로젝트 컨벤션 부트스트랩. "프로젝트 셋업", "지식베이스 설정", "AGENTS.md 생성" 등의 요청 시 사용한다.
---

# Pokitwork 프로젝트 셋업

이 스킬은 새 프로젝트 또는 기존 프로젝트에 Pokitwork 표준 설정을 적용한다.

## 실행 절차

### 1. AGENTS.md 생성

프로젝트 루트에 `AGENTS.md` 파일을 아래 템플릿으로 생성한다.
이 파일은 모든 AI 코딩 에이전트가 자동으로 읽는 소스 오브 트루스이다.

```markdown
# Pokitwork

스마트 인사/연차 관리 시스템.

## 기술 스택

- **프레임워크**: SvelteKit 2 + Svelte 5
- **UI**: shadcn-svelte + Tailwind CSS v4
- **DB**: PostgreSQL + Drizzle ORM
- **인증**: better-auth (Email OTP)
- **패키지 매니저**: deno
- **검증**: Zod v4

## 코드 스타일

- 최소한의 깔끔한 주석. 의도가 코드만으로 명확하지 않을 때만 작성.
- 불필요한 주석, 당연한 내용의 주석은 작성하지 않는다.

## 컴포넌트 규칙

- `$lib/components/ui/` — shadcn 전용. 수동 수정 금지.
- `$lib/components/` — 페이지 간 공통 커스텀 컴포넌트만.
- `src/routes/[page]/` — 해당 페이지 전용 컴포넌트.
- 마크업은 `+page.svelte`에서 바로 작성. 미리 추상화하지 않는다.

## MCP 도구

Svelte MCP 서버가 설정되어 있으면, 아래 도구를 사용할 수 있다.

1. **list-sections**: Svelte/SvelteKit 문서 섹션 목록 조회. 관련 작업 시 가장 먼저 호출.
2. **get-documentation**: 특정 섹션의 전체 문서. 관련 모든 섹션을 한 번에 가져올 것.
3. **svelte-autofixer**: Svelte 코드 이슈 분석. 코드 작성 시 반드시 사용.
4. **playground-link**: Svelte Playground 링크 생성. 프로젝트 파일에 직접 작성한 경우 호출 금지.

```

### 2. 에이전트 설정 및 스킬 설치

`common/skill-management` 스킬을 참고한다.
