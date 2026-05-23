# 프로젝트 에이전트 운영 규칙

이 프로젝트는 `sources/`에 수집한 원자료를 LLM Wiki로 정리하고, Graphify로 관계를 시각화한 뒤, gstack과 Superpowers를 활용해 아이디어를 검증하고 실행 가능한 산출물로 발전시키는 개인 지식 기획 작업공간이다.

기본 응답과 작성물은 한국어로 작성한다. 제품명, 회사명, 라이브러리명, 명령어처럼 영어 표기가 자연스러운 항목은 원문을 유지한다.

## Graphify 우선 규칙

이 프로젝트에는 `graphify-out/`에 god node, community structure, cross-file relationship을 담은 지식 그래프가 있다.

사용자가 `/graphify`를 입력하면 다른 작업보다 먼저 `skill` tool을 `skill: "graphify"`로 호출한다.

규칙:

- 소스 파일을 읽거나, grep/glob 검색을 하거나, 코드베이스/문서 구조 질문에 답하기 전에 항상 `graphify-out/GRAPH_REPORT.md`를 먼저 읽는다. 그래프가 이 프로젝트의 1차 지도다.
- `graphify-out/wiki/index.md`가 있으면 raw file을 바로 읽기보다 해당 wiki index를 먼저 탐색한다.
- `llm-wiki/wiki/**/*.md` 내용은 기본적으로 한국어로 유지한다. H1 제목, 섹션 제목, 요약, 핵심 포인트, Obsidian 스타일 `[[...]]` 링크도 한국어를 우선한다.
- 목차나 운영 파일은 그래프 콘텐츠 노드로 보여주지 않는다. 시각 지식 그래프에서는 `llm-wiki/AGENTS.md`, `llm-wiki/wiki/index.md`, `llm-wiki/wiki/log.md`, raw manifest 파일을 제외한다.
- HTML 지식 그래프는 파일 간 wiki link를 방향 화살표로 보여줘야 한다. `A -> B`는 `A.md`가 `B.md`로 해석되는 Obsidian 스타일 링크를 포함한다는 뜻이다. 양방향 링크는 양쪽 페이지가 서로 링크한다는 뜻이다.
- 로컬 HTML 그래프를 재생성할 때는 `node scripts/build-wiki-graph.mjs`를 우선 사용한다. 이 스크립트는 `graphify-out/graph.json`, `graphify-out/graph.html`, `graphify-out/wiki-graph.json`, `graphify-out/wiki-graph.html`을 생성하며, 한국어 UI label과 arrow marker를 포함한다.
- "X와 Y가 어떻게 연결되는가" 같은 cross-module 질문은 grep보다 `graphify query "<question>"`, `graphify path "<A>" "<B>"`, `graphify explain "<concept>"`를 우선 사용한다. 이 명령들은 단순 파일 검색이 아니라 EXTRACTED + INFERRED edge를 따라간다.
- Windows workspace에서 `graphify` shim이 경로 문제로 실패하면 `C:\Users\com\AppData\Roaming\uv\tools\graphifyy\Scripts\python.exe -m graphify ...`를 사용한다.
- `sources/`, `llm-wiki/wiki/`, `llm-wiki/outputs/`, Graphify 생성 스크립트처럼 사용자 지식 콘텐츠나 그래프 생성 규칙에 영향을 주는 파일을 수정한 뒤에는 `C:\Users\com\AppData\Roaming\uv\tools\graphifyy\Scripts\python.exe -m graphify update . --force`를 실행해 그래프를 최신 상태로 유지한다. 이 업데이트는 AST-only라 API 비용이 없다.
- `README`, `AGENTS.md`, `.gitignore`처럼 운영 설명이나 에이전트 행동 규칙만 바꾸는 경우에는 사용자 지식 그래프 변경이 아니므로 Graphify 갱신을 생략한다.

## 질문 유형별 작업 방식

### 로컬 HTML 목업과 인앱 브라우저 사용

사용자가 HTML 목업, 로컬 웹 화면, `localhost`, `127.0.0.1`, 또는 인앱 브라우저 표시를 요청하면 다음 순서로 처리한다.

1. Browser 플러그인이 사용 가능한 경우 먼저 Browser skill 지침을 확인하고 Codex 인앱 브라우저를 우선 사용한다.
2. 단일 HTML 파일을 바로 보여줘야 하면 `file://`보다 로컬 정적 서버를 우선한다. Windows 환경에서는 다음 방식이 가장 단순하다.

```powershell
C:\Python314\python.exe -m http.server 8766 --bind 127.0.0.1
```

3. 서버는 HTML 파일이 있는 폴더에서 실행한다. 예를 들어 `.superpowers/brainstorm/pharma-sales-mockup.html`을 보여줄 때는 `.superpowers/brainstorm`을 working directory로 두고, URL은 `http://127.0.0.1:8766/pharma-sales-mockup.html` 형태로 안내한다.
4. 백그라운드 서버가 샌드박스 안에서 바로 종료될 수 있으므로, 사용자가 실제로 봐야 하는 로컬 서버는 필요하면 승인 요청 후 샌드박스 밖에서 실행한다.
5. URL을 안내하거나 브라우저에 열기 전에 반드시 `Invoke-WebRequest -UseBasicParsing <url>`로 `200 OK`와 응답 길이를 확인한다.
6. 사용자가 이미 열어둔 포트가 죽어 있으면 새 포트를 안내하기보다 가능하면 같은 포트에서 서버를 다시 띄운 뒤 새로고침을 요청한다.
7. 인앱 브라우저 자동화가 로컬 URL을 `ERR_BLOCKED_BY_CLIENT`, `ERR_CONNECTION_REFUSED`, 또는 Browser Use URL policy로 막으면 우회하지 않는다. 서버 상태, 포트, 바인딩 주소를 확인한 뒤에도 막히면 그 제한을 설명하고, 사용자의 승인 하에 기본 데스크톱 브라우저로 열거나 정적 이미지/스크린샷 대안을 제공한다.
8. 로컬 HTML 목업은 제품 코드와 구분해 `.superpowers/brainstorm/` 같은 임시 작업 폴더에 둔다. 장기 보존할 설계 산출물이 필요하면 `llm-wiki/outputs/docs/`에 별도 문서로 정리한다.

### 프로젝트 구조나 현재 상태를 묻는 질문

참고 순서:

1. `graphify-out/GRAPH_REPORT.md`
2. `graphify-out/wiki/index.md`가 있으면 해당 index
3. `readme.md`
4. 필요한 경우에만 관련 원본 파일

응답 방식:

- 그래프의 god node, community, surprising connection을 먼저 활용해 큰 지형을 설명한다.
- 필요한 파일 경로는 절대 경로 링크로 제시한다.
- 변경이 필요 없는 질문이면 파일을 수정하지 않는다.

### 특정 개념, 기업, 시장, 기술 관계를 묻는 질문

참고 순서:

1. `graphify-out/GRAPH_REPORT.md`
2. `llm-wiki/wiki/index.md`
3. 관련 `llm-wiki/wiki/concepts/`, `entities/`, `sources/` 문서
4. 관계형 질문이면 `graphify query`, `graphify explain`, `graphify path`

활용 도구:

- Graphify: 개념 간 연결, 핵심 노드, 경로 확인
- LLM Wiki: 기존 요약, 출처, 개념 노트 확인

성과물 저장:

- 새 개념 정리가 필요하면 `llm-wiki/wiki/concepts/`에 저장한다.
- 기업/인물/제품 정리가 필요하면 `llm-wiki/wiki/entities/`에 저장한다.
- 원천 자료 요약이면 `llm-wiki/wiki/sources/`에 저장한다.
- 변경했다면 `llm-wiki/wiki/index.md`와 `llm-wiki/wiki/log.md`도 함께 갱신한다.

### 자료 ingest 또는 원천 자료 정리를 요청한 경우

참고 순서:

1. `sources/`
2. `llm-wiki/wiki/index.md`
3. `llm-wiki/wiki/log.md`
4. 기존 `llm-wiki/wiki/sources/`, `concepts/`, `entities/`, `ideas/`

활용 도구:

- LLM Wiki 또는 `llm-wiki-ideation`: 원자료를 wiki note로 변환하고 cross-link 생성
- Graphify: ingest 이후 관계 그래프 갱신

성과물 저장:

- 웹/SNS/PDF 원자료 요약은 `llm-wiki/wiki/sources/`에 저장한다.
- 반복 등장하는 주제는 `llm-wiki/wiki/concepts/`에 저장한다.
- 회사, 제품, 인물, 조직은 `llm-wiki/wiki/entities/`에 저장한다.
- 판단이나 선택 근거는 `llm-wiki/wiki/decisions/`에 저장한다.
- 원자료 자체는 `sources/`에서 수정하지 않는다.

### 아이디어, 기획, 전략, 사업화 질문

참고 순서:

1. `graphify-out/GRAPH_REPORT.md`
2. `llm-wiki/wiki/index.md`
3. 관련 `concepts/`, `entities/`, `ideas/`, `decisions/`
4. 필요하면 `graphify query "<아이디어와 관련된 질문>"`

아이디어를 받을 때의 기본 참조 방식:

- 첫 참조 파일은 항상 `graphify-out/GRAPH_REPORT.md`다. 전체 지식 지도를 보고 god node, 많이 연결된 파일, 관련 커뮤니티를 먼저 확인한다.
- 다음으로 `llm-wiki/wiki/index.md`를 읽어 관련 concept, entity, idea, decision 문서를 찾는다.
- AI 에이전트 관련 아이디어라면 우선 `llm-wiki/wiki/concepts/enterprise-ai-agent-adoption.md`, `llm-wiki/wiki/concepts/ai-agent-selection-checklist.md`, `llm-wiki/wiki/concepts/ai-agent-market-map.md`를 확인한다.
- 이미 유사한 아이디어가 있으면 `llm-wiki/wiki/ideas/`의 기존 아이디어 문서를 먼저 읽고, 새 아이디어를 기존 노트와 연결한다.
- wiki만으로 근거가 부족하거나 출처 확인이 필요하면 `llm-wiki/wiki/sources/`와 `sources/` 원자료를 확인한다.

관련 내용이 없을 때의 처리:

- `GRAPH_REPORT.md`, `index.md`, 관련 wiki 폴더에서 유사 노트를 찾지 못하면 "현재 wiki에는 직접 관련된 기존 문서가 없다"고 명시한다.
- 이 경우에도 작업을 멈추지 않고 사용자 아이디어 자체를 1차 원문으로 삼아 문제, 대상 사용자, 가정, 검증 질문, 첫 실험을 정리한다.
- 새 아이디어로 재사용할 가치가 있으면 `llm-wiki/wiki/ideas/`에 새 노트를 만들고, 개념 정리가 필요하면 `llm-wiki/wiki/concepts/`에 새 노트를 만든다.
- 사실 확인이나 시장/기업 근거가 필요한데 로컬 자료가 없으면 사용자에게 원자료 추가를 요청하거나, 최신 정보가 필요한 경우 웹 검색을 통해 확인한다.
- 새 노트를 만들거나 wiki 구조가 바뀌면 `llm-wiki/wiki/index.md`, `llm-wiki/wiki/log.md`를 갱신하고 Graphify를 다시 갱신한다.

활용 도구:

- LLM Wiki: 기존 지식과 아이디어를 연결
- gstack `office-hours`: 약점, 시장성, 실행 가능성, 반박 질문으로 압박 검증
- Superpowers `brainstorming`: 2-3개 접근안을 비교하고 추천안 도출

성과물 저장:

- 아이디어 노트는 `llm-wiki/wiki/ideas/`에 저장한다.
- 의사결정이 생기면 `llm-wiki/wiki/decisions/`에 저장한다.
- 실행 가능한 문서 초안은 `llm-wiki/outputs/docs/`에 저장한다.
- 발표 자료 초안은 `llm-wiki/outputs/slides/`에 저장한다.
- 자동화 코드나 Apps Script 산출물은 `llm-wiki/outputs/apps-script/`에 저장한다.

### 실행계획, PoC, 구현, 자동화 요청

참고 순서:

1. 관련 wiki idea, decision, concept 문서
2. 기존 `llm-wiki/outputs/docs/` 산출물
3. 실제 코드나 스크립트가 있으면 `scripts/` 및 관련 파일

활용 도구:

- Superpowers `brainstorming`: 요구사항과 접근안 정리
- Superpowers `writing-plans`: 구현 단계가 여러 개인 경우 실행계획 작성
- Superpowers `test-driven-development`: 코드 변경이나 자동화 로직 구현 시 테스트 우선 검토
- Superpowers `verification-before-completion`: 완료라고 말하기 전 검증
- gstack `office-hours`: PoC의 가정, 리스크, 반론 검토

성과물 저장:

- 설계서, 실행계획, 운영 문서는 `llm-wiki/outputs/docs/`에 저장한다.
- 코드 산출물은 성격에 따라 `scripts/` 또는 `llm-wiki/outputs/apps-script/`에 저장한다.
- 작업 결과가 wiki 지식 구조에 영향을 주면 관련 `ideas/`, `decisions/`, `concepts/` 문서를 갱신한다.

### 문서, 보고서, 발표자료 요청

참고 순서:

1. `llm-wiki/wiki/index.md`
2. 관련 source/concept/entity/idea/decision 문서
3. Graphify의 핵심 노드와 연결 관계

활용 도구:

- LLM Wiki: 근거 자료와 연결 구조 확보
- gstack `office-hours`: 주장과 논리의 약점 점검
- Superpowers `brainstorming`: 대상 독자, 목적, 구조, 톤 정리

성과물 저장:

- 보고서/제안서/기획서는 `llm-wiki/outputs/docs/`에 저장한다.
- 발표자료 초안이나 slide outline은 `llm-wiki/outputs/slides/`에 저장한다.
- 산출물 상단에는 목적, 대상 독자, 사용한 주요 wiki 문서, 작성일을 적는다.

## 성과물 저장 원칙

- 사용자가 저장 위치를 명시하면 그 위치를 우선한다.
- 사용자가 위치를 명시하지 않으면 위의 질문 유형별 기본 위치에 저장한다.
- `sources/`, `llm-wiki/`, `graphify-out/`의 실제 내용은 개인 작업물로 보고 GitHub에 올리지 않는다. 폴더 구조 유지를 위한 `.gitkeep`만 추적한다.
- 모든 wiki 문서는 Obsidian에서 탐색하기 쉽게 `[[...]]` 링크를 적극 사용한다.
- 새 문서를 만들거나 기존 wiki 구조를 바꾸면 `llm-wiki/wiki/index.md`와 `llm-wiki/wiki/log.md`를 갱신한다.
- 작업 완료 전에는 필요한 검증 명령을 실행하고, 실행하지 못한 검증은 이유를 명확히 말한다.
