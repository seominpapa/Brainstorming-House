# Brainstorming House

LLM Wiki, Graphify, gstack, Superpowers를 함께 사용하는 개인 지식 기획 작업공간입니다.

이 저장소는 AI 에이전트/기업 리서치 자료를 `sources/`에 모으고, LLM이 `llm-wiki/` 아래에 요약/개념/엔티티/아이디어 노트를 축적한 뒤, Graphify로 `graphify-out/` 지식 그래프를 생성하는 흐름을 기준으로 합니다.

## 현재 저장소 정책

원본 자료와 생성된 지식 파일은 개인 작업물이라 GitHub에 올리지 않습니다.

- `sources/`: 원본 Markdown, PDF, 웹/SNS 캡처 자료
- `llm-wiki/`: LLM이 정리한 위키 노트와 산출물
- `graphify-out/`: Graphify 리포트, JSON, HTML, 캐시

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

폴더 역할:

| 폴더 | 역할 | GitHub 추적 방식 |
| --- | --- | --- |
| `scripts/` | 지식 그래프 생성, 위키 초기화/복구, 문서 생성처럼 재사용할 로컬 자동화 코드를 둡니다. | 스크립트 파일을 추적합니다. |
| `sources/` | 웹, SNS, PDF 등 원자료를 보관하는 입력 폴더입니다. | 실제 자료는 제외하고 `.gitkeep`만 추적합니다. |
| `llm-wiki/` | LLM이 정리한 source, concept, entity, idea, decision 노트와 문서/슬라이드/Apps Script 산출물을 둡니다. | 실제 노트와 산출물은 제외하고 `.gitkeep`만 추적합니다. |
| `graphify-out/` | Graphify 리포트, JSON, HTML 그래프, 캐시를 생성하는 출력 폴더입니다. | 실제 결과물은 제외하고 `.gitkeep`만 추적합니다. |

로컬 실행 중에는 `.codex/`, `.obsidian/`, `.superpowers/`, `.uv-cache/`, `.uv-tools/` 같은 숨김 폴더도 생길 수 있습니다. 이들은 Codex, Obsidian, Superpowers, uv/Graphify의 로컬 설정과 캐시이므로 GitHub에 올리지 않습니다.

## 1. Setting

가장 권장하는 설치 방식은 **Codex Desktop에서 아래 URL들을 붙여 넣고 설치를 맡기는 것**입니다. 필요한 의존성인 `Git`, `Node.js`, `uv`, `Python` 등이 없으면 Codex가 설치 명령을 실행합니다. 사용자는 승인 요청이 뜰 때 승인만 해주면 됩니다.

아래 단계로 실행합니다.

```text

1. Codex Desktop 설치/확인
https://openai.com/codex/

2. Obsidian Desktop 설치 및 프로젝트 폴더 생성
https://obsidian.md/download

3. Obsidian Web Clipper 크롬 확장자 설치
https://chromewebstore.google.com/detail/obsidian-web-clipper/cnjifjpddelmedmihgijeibhnjfabmlf

4. Codex Desktop 실행 후 좌측 사이드바 Project 메뉴에서 앞에서 설정한 Obsidian 폴더 선택

5. "아래 저장소를 현재 폴더에 바로 클론해줘. git 백업은 필요 없어"라고 요청
https://github.com/seominpapa/Brainstorming-House

6. LLM Wiki 스킬 추가(global 권장)
https://github.com/lewislulu/llm-wiki-skill

7. Graphify 설치(local 권장)
https://graphify.net/

8. gstack 스킬 추가(global 권장)
https://github.com/garrytan/gstack

9. Superpowers 스킬 추가(global 권장)
https://github.com/obra/superpowers
https://skills.sh/obra/superpowers

```

### 설치 대상 요약

| 대상 | 용도 | 설치 URL |
| --- | --- | --- |
| Codex Desktop | 프로젝트를 열고 설치/실행을 맡길 AI 작업 환경 | https://openai.com/codex/ |
| Obsidian Desktop | 로컬 Markdown wiki 탐색 | https://obsidian.md/download |
| Obsidian Web Clipper | 웹/SNS 자료를 `sources/`에 저장 | https://chromewebstore.google.com/detail/obsidian-web-clipper/cnjifjpddelmedmihgijeibhnjfabmlf |
| LLM Wiki | 원본 자료를 지속적인 Markdown 지식베이스로 정리 | https://github.com/lewislulu/llm-wiki-skill |
| Graphify | 지식 그래프와 HTML/JSON 리포트 생성 | https://graphify.net/ |
| gstack | `office-hours` 방식의 아이디어 압박 검증 | https://github.com/garrytan/gstack |
| Superpowers | brainstorming, planning, debugging, verification 스킬 묶음 | https://github.com/obra/superpowers |

### 의존성

Codex가 설치 여부를 확인하고 필요 시 설치할 수 있는 의존성입니다.

| 도구 | 왜 필요한가 | 설치 URL |
| --- | --- | --- |
| Git | 저장소 clone, 스킬 repo 다운로드, commit/push | https://git-scm.com/downloads |
| GitHub CLI | GitHub 인증, repo 생성/확인 | https://cli.github.com/ |
| Node.js / npm / npx | Superpowers 설치, `scripts/build-wiki-graph.mjs` 실행 | https://nodejs.org/ |
| Python 3.10+ | Graphify 실행 기반 | https://www.python.org/downloads/ |
| uv | `graphifyy` 같은 Python CLI 도구 설치 | https://github.com/astral-sh/uv |

## 2. 실행

1. Obsidian Web Clipper를 활용하여 관심 있는 웹/SNS 자료를 `sources/sns/`, `sources/web/` 폴더에 크래핑합니다.
2. PDF 파일은 `sources/pdf/` 폴더에 업로드합니다.
3. Codex에서 LLM Wiki 스킬로 `sources/` 자료 ingest를 수행합니다.
4. `llm-wiki/wiki/`에 source, concept, entity, idea, decision 노트를 축적합니다.
5. LLM Wiki 기반으로 Graphify 지식 graph를 생성합니다.
6. 기획 아이디어를 제시합니다. 예: “AI 에이전트 기업 내 도입방안”
7. LLM Wiki, gstack `office-hours`, Superpowers `brainstorming`을 활용해 아이디어를 구체화합니다.
8. 필요한 경우 `llm-wiki/outputs/docs/`, `llm-wiki/outputs/slides/`, `llm-wiki/outputs/apps-script/`에 산출물을 작성합니다.

Codex에 요청할 때는 뒤의 `Codex 프롬프트 예시`를 참고합니다.

## gstack과 Superpowers 사용 방식

gstack은 아이디어, 제품 방향, 구현 계획, 리뷰, QA, 릴리즈까지 이어지는 의사결정 보조 스킬 묶음입니다. 대표적으로 `office-hours`, `plan-ceo-review`, `plan-eng-review`, `plan-design-review`, `review`, `investigate`, `qa`, `qa-only`, `ship`, `document-release`, `document-generate`, `context-save`, `context-restore` 같은 스킬이 있습니다.

Superpowers는 Codex가 작업을 진행할 때 쓰는 실행 방법론 스킬 묶음입니다. 대표적으로 `brainstorming`, `writing-plans`, `test-driven-development`, `systematic-debugging`, `verification-before-completion`, `requesting-code-review`, `receiving-code-review`, `using-git-worktrees`, `finishing-a-development-branch` 같은 스킬이 있습니다.

이 프로젝트의 기본 조합은 gstack `office-hours`와 Superpowers `brainstorming`입니다. 아이디어, 전략, 보고서, PoC 방향을 잡을 때는 이 둘로 문제, 가정, 반박 질문, 2-3개 접근안을 먼저 정리합니다.

다만 `office-hours`와 `brainstorming`에만 고정하지 않습니다. 구현 계획이 필요하면 gstack `plan-eng-review`나 Superpowers `writing-plans`, 코드 작성이나 자동화 구현이 필요하면 Superpowers `test-driven-development`, 오류 원인 분석은 gstack `investigate`나 Superpowers `systematic-debugging`, 코드 리뷰는 gstack `review`나 Superpowers 코드 리뷰 스킬, QA와 릴리즈는 gstack `qa`, `qa-only`, `ship`, 완료 전 확인은 Superpowers `verification-before-completion`을 사용할 수 있습니다.

## 아이디어를 낼 때 참조 순서

사용자가 특정 아이디어를 제시하면 Codex는 먼저 전체 지식 지도를 보고, 그다음 wiki 색인과 관련 노트로 내려갑니다.

1. `graphify-out/GRAPH_REPORT.md`
   전체 지식 지도입니다. 많이 연결된 파일, 핵심 노드, 기존 아이디어와의 연결 가능성을 먼저 확인합니다.
2. `llm-wiki/wiki/index.md`
   wiki 목차입니다. 관련 concept, entity, idea, decision 문서를 찾는 출발점입니다.
3. 관련 wiki 문서
   예를 들어 AI 에이전트 아이디어라면 `기업 AI 에이전트 도입`, `AI 에이전트 선택 체크리스트`, `AI 에이전트 시장 지도` 같은 문서를 먼저 봅니다.
4. 기존 idea/decision 문서
   비슷한 아이디어나 이미 내린 결정이 있으면 새 아이디어를 그 문서와 연결합니다.
5. 원천 자료
   wiki만으로 근거가 부족하면 `llm-wiki/wiki/sources/` 또는 `sources/`의 원자료를 확인합니다.

관련 내용이 없을 때는 이렇게 처리합니다.

- 기존 그래프와 wiki에서 직접 관련된 문서를 찾지 못했다고 먼저 밝힙니다.
- 아이디어 자체를 1차 메모로 삼아 문제, 대상 사용자, 핵심 가정, 검증 질문, 첫 실험을 정리합니다.
- 다시 볼 가치가 있는 아이디어는 `llm-wiki/wiki/ideas/`에 새 노트로 저장합니다.
- 새 개념으로 분리할 가치가 있으면 `llm-wiki/wiki/concepts/`에 별도 노트를 만듭니다.
- 근거 자료가 부족하면 사용자가 `sources/`에 원자료를 추가하거나, 최신 확인이 필요한 경우 웹 검색으로 보강합니다.
- 새 문서가 생기면 `llm-wiki/wiki/index.md`, `llm-wiki/wiki/log.md`, Graphify 그래프를 갱신합니다.

아이디어가 구체화되면 산출물은 성격에 따라 저장합니다.

| 성격 | 저장 위치 |
| --- | --- |
| 발전 중인 아이디어 | `llm-wiki/wiki/ideas/` |
| 결정과 근거 | `llm-wiki/wiki/decisions/` |
| 실행계획/기획서 | `llm-wiki/outputs/docs/` |
| 발표 초안 | `llm-wiki/outputs/slides/` |
| Google Apps Script 코드 | `llm-wiki/outputs/apps-script/` |

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

## 참고: 터미널 수동 설치 명령어

Codex가 자동으로 처리하지 못하거나 직접 설치하고 싶을 때만 사용합니다.

Windows 의존성 설치 예시:

```powershell
winget install --id Git.Git -e
winget install --id GitHub.cli -e
winget install --id OpenJS.NodeJS.LTS -e
winget install --id Python.Python.3.12 -e
winget install --id astral-sh.uv -e
npm i -g @openai/codex
```

LLM Wiki 수동 설치 예시:

```powershell
git clone https://github.com/lewislulu/llm-wiki-skill.git C:\tmp\llm-wiki-skill
New-Item -ItemType Directory -Force $env:USERPROFILE\.codex\skills
Copy-Item -Recurse -Force C:\tmp\llm-wiki-skill\llm-wiki $env:USERPROFILE\.codex\skills\llm-wiki
```

Graphify 수동 설치 예시:

```powershell
uv tool install graphifyy
graphify install --platform codex
```

이 프로젝트의 Windows 권장 Graphify 실행:

```powershell
C:\Users\com\AppData\Roaming\uv\tools\graphifyy\Scripts\python.exe -m graphify update . --force
```

로컬 HTML 위키 그래프 재생성:

```powershell
node scripts/build-wiki-graph.mjs
```

gstack 수동 설치 예시:

```bash
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/gstack
cd ~/gstack
./setup --host codex
```

Windows에서는 Git Bash 또는 WSL에서 위 명령을 실행하는 것을 권장합니다.

Superpowers 수동 설치 예시:

```powershell
npx skills add obra/superpowers
```

개별 Superpowers 스킬 설치 예시:

```powershell
npx skills add https://github.com/obra/superpowers --skill brainstorming
npx skills add https://github.com/obra/superpowers --skill verification-before-completion
```

## 참고 링크

- LLM Wiki Skill: https://github.com/lewislulu/llm-wiki-skill
- OpenAI Skills Catalog: https://github.com/openai/skills
- Graphify: https://github.com/safishamsi/graphify
- Graphify PyPI: https://pypi.org/project/graphifyy/
- gstack: https://github.com/garrytan/gstack
- gstack office-hours: https://github.com/garrytan/gstack/blob/main/office-hours/SKILL.md
- Superpowers: https://github.com/obra/superpowers
- Superpowers Skills: https://skills.sh/obra/superpowers
