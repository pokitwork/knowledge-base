---
name: skill-management
description: Pokitwork 지식베이스 스킬 관리. 스킬 갱신, 에이전트별 설정, gitignore 규칙, 새 스킬 기여 방법. "스킬 업데이트", "지식베이스 갱신", "스킬 최신화" 등의 요청 시 사용한다.
---

# 스킬 관리

이 프로젝트의 상세 규칙은 `pokitwork-knowledge-skills` 리포에 Agent Skills로 관리된다.

- 리포: https://github.com/pokitwork/pokitwork-knowledge-skills

## AGENTS.md

모든 Pokitwork 프로젝트는 루트에 `AGENTS.md` 파일을 둔다. 이 파일이 모든 AI 코딩 에이전트가 읽는 **진실 원천(Source of Truth)** 이다.

- 프로젝트별 AGENTS.md 템플릿은 해당 프로젝트의 `project-setup` 스킬을 참고한다.
- 에이전트별 설정 파일(CLAUDE.md 등)은 `@AGENTS.md`로 참조만 한다. 내용을 중복하지 않는다.

## 스킬 갱신

스킬이 설치된 디렉토리에서 pull한다.

```bash
git -C <스킬 설치 경로>/pokitwork pull
```

에이전트별 설치 경로:

| 에이전트 | 설치 경로 |
|----------|-----------|
| Claude Code | `.claude/skills/pokitwork` |
| Antigravity | `.antigravity/skills/pokitwork` |
| Cursor | `.cursor/skills/pokitwork` |
| OpenAI Codex | `.agents/skills/pokitwork` |

스킬이 아직 설치되지 않았다면:

```bash
git clone https://github.com/pokitwork/pokitwork-knowledge-skills <스킬 디렉토리>/pokitwork
```

## 에이전트별 설정

### Claude Code

`.claude/CLAUDE.md` 파일을 생성/수정:

```markdown
@AGENTS.md
```

### Gemini CLI

`.gemini/settings.json`의 `context.fileName`에 `"AGENTS.md"` 추가.

### OpenAI Codex

`AGENTS.md`가 프로젝트 루트에 있으면 자동으로 읽힌다. 추가 설정 불필요.

## gitignore 규칙

에이전트 설정 디렉토리는 각자 로컬에서만 관리한다. 프로젝트 `.gitignore`에 추가할 것:

```
.claude/
.antigravity/
.cursor/
.agents/
```

## 새 스킬 기여

새로운 스킬을 추가하거나 기존 스킬을 수정하고 싶다면 `pokitwork-knowledge-skills` 리포에 Pull Request를 요청한다.

1. 리포를 fork한다.
2. `{프로젝트}/{skill-name}/SKILL.md` 파일을 생성/수정한다.
3. PR을 올린다.
