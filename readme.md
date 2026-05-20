# LLM Wiki Ideation Workflow

이 폴더는 개인 지식 관리와 아이디어 확장을 위해 만든 LLM Wiki 작업 공간입니다. 기본 방향은 Karpathy의 LLM Wiki 패턴을 기반으로, gstack의 `office-hours`식 아이디어 압박 질문과 Superpowers의 `brainstorming`식 단계적 정리를 결합하는 것입니다.

## Skill Folder

Codex가 사용하는 개인 스킬은 아래 위치에 있습니다.

```text
C:\Users\com\.codex\skills\llm-wiki-ideation\
├── SKILL.md
├── agents\
│   └── openai.yaml
└── references\
    ├── wiki-schema.md
    ├── ideation-roles.md
    └── brainstorming.md
```

각 파일의 역할은 다음과 같습니다.

- `SKILL.md`: 스킬의 메인 사용 규칙입니다. 언제 이 스킬을 쓰고, 어떤 순서로 wiki를 읽고 업데이트할지 정의합니다.
- `agents/openai.yaml`: Codex UI에 표시될 스킬 이름, 설명, 기본 프롬프트를 담습니다.
- `references/wiki-schema.md`: `llm-wiki`를 처음 만들 때 쓰는 기본 구조와 문서 규칙입니다.
- `references/ideation-roles.md`: gstack `office-hours`에서 영감을 받은 아이디어 압박 질문과 역할별 검토 기준입니다.
- `references/brainstorming.md`: Superpowers `brainstorming`에서 영감을 받은 질문, 접근안 비교, 스펙화 흐름입니다.

## Wiki Folder

현재 작업 폴더 안의 실제 지식 저장소는 아래 구조를 사용합니다.

```text
llm-wiki\
├── AGENTS.md
├── raw\
├── wiki\
│   ├── index.md
│   ├── log.md
│   ├── sources\
│   ├── concepts\
│   ├── entities\
│   ├── ideas\
│   └── decisions\
└── outputs\
    ├── docs\
    └── slides\
```

Graphify 산출물은 현재 폴더의 아래 위치에 생성됩니다.

```text
graphify-out\
├── graph.html
├── graph.json
├── GRAPH_REPORT.md
└── manifest.json
```

각 폴더의 역할은 다음과 같습니다.

- `llm-wiki/AGENTS.md`: 이 wiki를 어떻게 유지할지 정의하는 운영 가이드입니다.
- `llm-wiki/raw/`: 원본 자료를 보관합니다. 원본은 수정하지 않습니다.
- `llm-wiki/wiki/index.md`: wiki 전체 목차입니다.
- `llm-wiki/wiki/log.md`: ingest, query, idea, lint, doc, slides 작업 기록입니다.
- `llm-wiki/wiki/sources/`: 원본 자료별 요약 페이지를 둡니다.
- `llm-wiki/wiki/concepts/`: 반복해서 쓰이는 개념, 프레임워크, mental model을 둡니다.
- `llm-wiki/wiki/entities/`: 사람, 조직, 제품, 프로젝트, 장소 등을 둡니다.
- `llm-wiki/wiki/ideas/`: 아직 발전 중인 아이디어를 둡니다.
- `llm-wiki/wiki/decisions/`: 결정된 방향, 이유, 대안, 남은 질문을 둡니다.
- `llm-wiki/outputs/docs/`: 문서 초안 Markdown을 둡니다.
- `llm-wiki/outputs/slides/`: 발표자료 초안 Markdown을 둡니다.

## Idea Brainstorming Workflow

아이디어를 제시하면 기본 프로세스는 다음 순서로 진행됩니다.

```text
아이디어 입력
→ LLM Wiki 맥락 확인
→ gstack office-hours식 리프레이밍
→ Superpowers brainstorming식 접근안 비교
→ 추천 방향 확인
→ 짧은 스펙 작성
→ wiki에 축적
→ Graphify 그래프 갱신
→ 필요 시 문서/발표 초안 생성
```

## Step 1. Wiki Context Check

먼저 아래 파일을 읽고, 기존 지식과 연결점을 찾습니다.

- `llm-wiki/AGENTS.md`
- `llm-wiki/wiki/index.md`
- `llm-wiki/wiki/log.md`
- 관련된 `concepts`, `ideas`, `decisions`, `sources` 페이지

관련 지식이 있으면 그 맥락 위에서 아이디어를 확장합니다. 관련 지식이 없으면 “현재 wiki에는 관련 맥락이 없다”고 밝힌 뒤, 사용자가 준 아이디어 자체에서 출발합니다.

## Step 2. Office-Hours Reframe

gstack의 `office-hours`처럼 아이디어를 그대로 받아들이지 않고, 먼저 문제의 본질을 다시 봅니다.

확인하는 질문은 다음과 같습니다.

- 사용자가 실제로 겪는 고통은 무엇인가?
- 지금 아이디어는 너무 좁거나 너무 넓게 잡혀 있지 않은가?
- 가장 작게 검증할 수 있는 wedge는 무엇인가?
- 이 아이디어가 10배 더 좋아진다면 어떤 모습인가?
- 지금 하지 말아야 할 것은 무엇인가?
- 어떤 구체적 상황에서 “이건 지금 꼭 필요하다”고 말하게 되는가?
- 어떤 증거가 있으면 이 아이디어를 더 밀거나 접을 수 있는가?

이 단계의 출력은 보통 아래 형식을 따릅니다.

```markdown
## Reframe
- Original framing:
- Deeper pain:
- Smallest wedge:
- Bigger vision:
- Immediate test:
```

## Step 3. Superpowers Brainstorming

Superpowers의 `brainstorming`처럼 한 번에 너무 많은 질문을 하지 않고, 필요한 경우 핵심 질문 하나만 던집니다.

그 다음 2-3개의 접근안을 제시합니다.

```markdown
## Approaches

### Approach A
- Shape:
- Pros:
- Cons:

### Approach B
- Shape:
- Pros:
- Cons:

### Approach C
- Shape:
- Pros:
- Cons:

## Recommendation
- Recommended approach:
- Why:
```

아이디어가 크거나 문서, 발표, 구현으로 이어질 가능성이 있으면 바로 산출물을 만들지 않고 먼저 방향이 맞는지 확인합니다.

## Step 4. Concise Spec

방향이 잡히면 짧은 스펙으로 정리합니다.

```markdown
# Idea: Title

## Problem

## Audience

## Proposed Shape

## Non-goals

## Risks

## First Next Step

## Wiki Pages Used
```

재사용할 가치가 있으면 이 스펙은 `llm-wiki/wiki/ideas/`에 저장합니다. 이미 방향이 결정된 내용이라면 `llm-wiki/wiki/decisions/`에 저장합니다.

## Step 5. Wiki Filing

아이디어 확장이 끝나면 wiki를 업데이트합니다.

- 새 아이디어는 `wiki/ideas/`에 저장합니다.
- 결정된 내용은 `wiki/decisions/`에 저장합니다.
- 관련 개념은 `wiki/concepts/`에 연결하거나 새로 만듭니다.
- `wiki/index.md`에 새 페이지를 추가합니다.
- `wiki/log.md`에 작업 기록을 남깁니다.

페이지 연결은 Obsidian 스타일 링크를 사용합니다.

```markdown
[[Page Name]]
```

## Optional Outputs

사용자가 원할 때만 문서나 발표자료 초안을 만듭니다.

문서 초안:

```text
llm-wiki/outputs/docs/
```

발표자료 초안:

```text
llm-wiki/outputs/slides/
```

초안에는 어떤 wiki 페이지를 근거로 만들었는지 짧게 남깁니다.

## Visual Graph With Graphify

Graphify가 설치되어 있으며, 아이디어와 wiki 구조를 시각적으로 볼 때 사용합니다.

- 설치 패키지: `graphifyy 0.8.1`
- 시각화 파일: `graphify-out/graph.html`
- 요약 리포트: `graphify-out/GRAPH_REPORT.md`
- 그래프 데이터: `graphify-out/graph.json`

그래프 갱신 명령:

```powershell
C:\Users\com\AppData\Roaming\uv\tools\graphifyy\Scripts\python.exe -m graphify update . --force
```

API 키 없이도 구조 그래프는 갱신됩니다. Markdown, PDF, 이미지까지 의미 기반으로 풍부하게 그래프화하려면 `GEMINI_API_KEY`, `GOOGLE_API_KEY`, `ANTHROPIC_API_KEY`, `OPENAI_API_KEY` 중 하나가 필요합니다.

## Example Prompts

```text
Use $llm-wiki-ideation to expand this idea using my wiki: ...
```

```text
Use $llm-wiki-ideation to run office-hours and brainstorming on this idea: ...
```

```text
Use $llm-wiki-ideation to turn this idea into a wiki idea note: ...
```

```text
Use $llm-wiki-ideation to draft a document outline from this idea.
```

```text
Use $llm-wiki-ideation to draft a slide outline from this idea.
```
