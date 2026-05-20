# Brainstorming House

LLM Wiki, Graphify, gstack, Superpowers를 함께 사용하는 개인 지식 기획 작업공간입니다.

이 저장소는 AI 에이전트/기업 리서치 자료를 `sources/`에 모으고, LLM이 `llm-wiki/` 아래에 요약/개념/엔티티/아이디어 노트를 축적한 뒤, Graphify로 `graphify-out/` 지식 그래프를 생성하는 흐름을 기준으로 합니다.

## 현재 저장소 정책

원본 자료와 생성된 지식 파일은 개인 작업물이라 GitHub에 올리지 않습니다.

- `sources/`: 원본 Markdown, PDF, 웹/SNS 캡처 자료
- `llm-wiki/`: LLM이 정리한 위키 노트와 산출물
- `graphify-out/`: Graphify 리포트, JSON, HTML, 캐시
- `llm-wiki-graphify-gstack-superpowers-install-guide.txt`: 로컬 설치 가이드

위 경로는 `.gitignore`로 내용 파일을 제외하고, clone 사용자가 폴더 구조를 볼 수 있도록 `.gitkeep`만 추적합니다.

## 폴더 구조

```text
Brainstorming-House/
├── AGENTS.md
├── readme.md
├── scripts/
│   ├── build-wiki-graph.mjs
│   └── localize-wiki-ko.mjs
├── sources/
│   ├── pdf/
│   ├── sns/
│   └── web/
├── llm-wiki/
│   ├── raw/
│   ├── wiki/
│   │   ├── sources/
│   │   ├── concepts/
│   │   ├── entities/
│   │   ├── ideas/
│   │   └── decisions/
│   └── outputs/
│       ├── apps-script/
│       ├── docs/
│       └── slides/
└── graphify-out/
    └── cache/
```

## 필수 의존성

| 도구 | 용도 | 설치 URL |
| --- | --- | --- |
| Git | 저장소 clone, commit, push | https://git-scm.com/downloads |
| GitHub CLI | GitHub 인증, repo 생성/확인 | https://cli.github.com/ |
| Node.js / npm / npx | Superpowers 설치, 로컬 graph HTML 생성 스크립트 | https://nodejs.org/ |
| Python 3.10+ | Graphify 실행 기반 | https://www.python.org/downloads/ |
| uv | Python CLI 도구 설치 권장 방식 | https://github.com/astral-sh/uv |
| Codex | AI coding/agent 작업 환경 | https://github.com/openai/codex |
| Obsidian | 로컬 Markdown wiki 탐색 | https://obsidian.md/download |

Windows 예시:

```powershell
winget install --id Git.Git -e
winget install --id GitHub.cli -e
winget install --id OpenJS.NodeJS.LTS -e
winget install --id Python.Python.3.12 -e
winget install --id astral-sh.uv -e
npm i -g @openai/codex
```

## 설치된/사용 중인 스킬

### LLM Wiki

LLM Wiki는 원본 자료를 반복해서 다시 읽는 대신, LLM이 원본을 Markdown 지식베이스로 컴파일하고 cross-link를 쌓아가는 방식입니다.

- 참고 repo: https://github.com/lewislulu/llm-wiki-skill
- OpenAI Skills Catalog: https://github.com/openai/skills
- 현재 로컬 skill 경로 예시: `C:\Users\com\.codex\skills\llm-wiki-ideation\`

Codex 설치 예시:

```powershell
git clone https://github.com/lewislulu/llm-wiki-skill.git C:\tmp\llm-wiki-skill
New-Item -ItemType Directory -Force $env:USERPROFILE\.codex\skills
Copy-Item -Recurse -Force C:\tmp\llm-wiki-skill\llm-wiki $env:USERPROFILE\.codex\skills\llm-wiki
```

이 프로젝트에서는 `llm-wiki-ideation` 형태로 확장해 사용합니다. 기본 작업 순서는 다음과 같습니다.

1. `sources/`에 원본 자료 저장
2. LLM Wiki skill로 `llm-wiki/wiki/sources`, `concepts`, `entities`, `ideas`, `decisions` 작성
3. `llm-wiki/wiki/index.md`와 `llm-wiki/wiki/log.md` 갱신
4. Graphify로 지식 그래프 갱신

### Graphify

Graphify는 코드/문서/PDF/Markdown을 분석해 지식 그래프를 만들고, `graphify-out/GRAPH_REPORT.md`, `graph.json`, `graph.html` 등을 생성합니다.

- 공식 사이트: https://graphify.net/
- GitHub: https://github.com/safishamsi/graphify
- PyPI 패키지: https://pypi.org/project/graphifyy/

설치:

```powershell
uv tool install graphifyy
graphify install --platform codex
```

이 프로젝트의 Windows 권장 실행:

```powershell
C:\Users\com\AppData\Roaming\uv\tools\graphifyy\Scripts\python.exe -m graphify update . --force
```

로컬 HTML 위키 그래프 재생성:

```powershell
node scripts/build-wiki-graph.mjs
```

### gstack

gstack은 Garry Tan의 AI 작업용 스킬 묶음입니다. 이 저장소에서는 특히 `office-hours` 흐름으로 아이디어를 압박 질문으로 검증할 때 사용합니다.

- 공식 사이트: https://gstack.lol/
- GitHub: https://github.com/garrytan/gstack
- office-hours skill: https://github.com/garrytan/gstack/blob/main/office-hours/SKILL.md

Codex 설치 예시:

```bash
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/gstack
cd ~/gstack
./setup --host codex
```

Windows에서는 Git Bash 또는 WSL에서 위 명령을 실행하는 것을 권장합니다.

사용 예시:

```text
Use gstack office-hours to pressure-test this idea:
AI 에이전트 기업 도입 방안
```

### Superpowers

Superpowers는 agent가 바로 구현으로 뛰어들지 않고, brainstorming, planning, TDD, debugging, verification 같은 절차를 지키도록 돕는 스킬 묶음입니다.

- GitHub: https://github.com/obra/superpowers
- Skills 페이지: https://skills.sh/obra/superpowers

설치:

```powershell
npx skills add obra/superpowers
```

개별 스킬 설치 예시:

```powershell
npx skills add https://github.com/obra/superpowers --skill brainstorming
npx skills add https://github.com/obra/superpowers --skill verification-before-completion
```

이 프로젝트에서 자주 쓰는 스킬:

- `using-superpowers`: 관련 스킬을 먼저 확인
- `brainstorming`: 아이디어를 2-3개 접근으로 비교
- `writing-plans`: 구현 전 계획 작성
- `systematic-debugging`: 문제 재현과 원인 확인
- `verification-before-completion`: 완료 주장 전 검증

## 권장 작업 흐름

```text
자료 수집
→ sources/에 저장
→ LLM Wiki ingest
→ llm-wiki/wiki/에 지식 노트 작성
→ gstack office-hours로 아이디어 검증
→ Superpowers brainstorming으로 접근 비교
→ 문서/슬라이드/Apps Script 산출물 작성
→ Graphify update
→ graphify-out/GRAPH_REPORT.md 확인
```

## Codex 프롬프트 예시

자료 ingest:

```text
llm-wiki-ideation을 사용해서 sources/ 안의 자료를 ingest해줘.
원자료는 수정하지 말고, llm-wiki/wiki/sources, concepts, entities, ideas를 갱신해줘.
작업 후 index.md와 log.md도 갱신해줘.
```

아이디어 구체화:

```text
llm-wiki-ideation을 사용해서 아래 아이디어를 기존 wiki 맥락과 연결해 구체화해줘.
gstack office-hours 방식으로 압박 검증하고,
Superpowers brainstorming 방식으로 2-3개 접근을 비교한 뒤,
실행 가능한 문서 초안을 llm-wiki/outputs/docs/에 작성해줘.

아이디어:
AI 에이전트 기업 도입 방안
```

Graphify 갱신:

```text
Graphify를 갱신해줘.
Windows 경로 문제가 있으면 아래 명령을 사용해줘.
C:\Users\com\AppData\Roaming\uv\tools\graphifyy\Scripts\python.exe -m graphify update . --force
그 다음 graphify-out/GRAPH_REPORT.md의 핵심 노드를 요약해줘.
```

## GitHub에 올리는 것과 올리지 않는 것

GitHub에는 작업공간 구조, 운영 규칙, 스크립트만 올립니다. 개인 자료와 생성 산출물은 로컬에만 둡니다.

올리는 것:

- `AGENTS.md`
- `readme.md`
- `scripts/`
- 폴더 구조 유지를 위한 `.gitkeep`
- `.gitignore`

올리지 않는 것:

- `sources/**` 실제 자료
- `llm-wiki/**` 실제 위키 노트와 산출물
- `graphify-out/**` 실제 리포트/그래프/캐시
- 로컬 설치 가이드 txt

## 참고 링크

- LLM Wiki Skill: https://github.com/lewislulu/llm-wiki-skill
- OpenAI Skills Catalog: https://github.com/openai/skills
- Graphify: https://github.com/safishamsi/graphify
- Graphify PyPI: https://pypi.org/project/graphifyy/
- gstack: https://github.com/garrytan/gstack
- gstack office-hours: https://github.com/garrytan/gstack/blob/main/office-hours/SKILL.md
- Superpowers: https://github.com/obra/superpowers
- Superpowers Skills: https://skills.sh/obra/superpowers
