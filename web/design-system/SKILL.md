---
name: design-system
description: Pokitwork 디자인 시스템 규칙. shadcn-svelte 테마 변수, 그라디언트, 색상 하드코딩 금지, Tailwind v4 문법, 컴포넌트 배치 규칙. UI 스타일링이나 컴포넌트 작업 시 참고한다.
---

# 디자인 시스템

테마 정의 파일: `src/routes/layout.css`

## 색상 체계

- **시맨틱 색상**: `bg-primary`, `text-muted-foreground` 등 — shadcn 테마 변수 사용.
- **그라디언트**: `from-gradient-from via-gradient-via to-gradient-to` — 커스텀 테마 변수.
- **색상 하드코딩 금지**: `text-blue-500` 같은 Tailwind 기본 팔레트 직접 사용 금지. 반드시 테마 변수를 거칠 것.

## UI 프레임워크

- **shadcn-svelte**: `$lib/components/ui/` — 수동 수정 금지. CLI로만 관리.
- **Tailwind CSS v4**: important 문법은 `size-12!` (v3의 `!size-12` 아님).
- **토스트**: svelte-sonner (`<Toaster richColors />`)

## 컴포넌트 배치

- `$lib/components/ui/` — shadcn 전용. 수동 수정 금지.
- `$lib/components/` — 페이지 간 공통으로 쓰이는 커스텀 컴포넌트만.
- `src/routes/[page]/` — 해당 페이지 전용 컴포넌트는 라우트 폴더에 배치.
- 마크업은 `+page.svelte`에서 바로 작성한다. 미리 추상화하지 않는다.
- 페이지 내 반복 패턴 → 같은 라우트 폴더에 컴포넌트로 추출.
- 여러 페이지에서 동일 패턴 → `$lib/components/`로 승격.

## 참고 디자인 활용

- Figma 디자인은 **참고용**이지, 코드를 따라가는 것이 아니다.
- SvelteKit + shadcn-svelte에 맞게 깔끔하게 구현한다.
