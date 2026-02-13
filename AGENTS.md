# Pokitwork Knowledge Skills

Pokitwork 프로젝트의 공유 지식베이스. [Agent Skills (SKILL.md)](https://agentskills.io) 표준을 따르며, 모든 AI 코딩 에이전트에서 사용할 수 있다.

## 구조

```
web/                             ← pokitwork web (SvelteKit)
  project-setup/SKILL.md         ← 프로젝트 초기 설정 (AGENTS.md 생성 + 에이전트별 설정)
  sveltekit-conventions/SKILL.md ← SvelteKit 서버 레이어 아키텍처
  design-system/SKILL.md         ← 디자인 시스템 규칙
  directory-structure/SKILL.md   ← 프로젝트 디렉토리 구조 + 라우트 맵
  server-api/SKILL.md            ← Query Service 및 REST API 카탈로그
common/                          ← 모든 프로젝트 공통
  skill-management/SKILL.md      ← 스킬 갱신, 에이전트 설정, gitignore, 기여 방법
server/                          ← pokitwork-api (예정)
app/                             ← pokitwork-app (예정)
```

## SKILL.md 형식

각 스킬은 YAML frontmatter + Markdown 본문으로 구성된다.

```markdown
---
name: skill-name
description: 스킬 설명. 에이전트가 이 스킬을 언제 사용할지 판단하는 기준이 된다.
---

# 스킬 제목

본문 내용...
```

- `name`: 스킬 식별자. 폴더명과 일치시킨다.
- `description`: 에이전트가 스킬 선택 시 참고하는 설명. 사용 시나리오를 구체적으로 적는다.

## 기여 규칙

- 스킬 추가: `{프로젝트}/{skill-name}/SKILL.md` 파일을 생성한다.
- 스킬 수정: 해당 SKILL.md 파일을 직접 수정한다.
- 스킬 내용은 Pokitwork 프로젝트에 적용되는 실제 규칙과 패턴을 담는다.
- 코드 예시는 실제 프로젝트 코드와 일관성을 유지한다.