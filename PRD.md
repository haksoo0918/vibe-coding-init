> [!NOTE]
> 본 문서는 `vibe-coding-init` 저장소 자체의 제품 요구사항 정의서입니다.  
> 새로운 프로젝트에서는 `AGENTS.md`, `.agents/`, `rules/`만 복사한 뒤, 첫 기획 단계에서 AI 에이전트가 [rules/agent-execution.md](rules/agent-execution.md)의 표준 양식에 맞춰 `PRD.md`를 자동 생성하도록 진행하세요.

# PRD (제품 요구사항 정의서): 바이브코딩 프로젝트 초기화 킷

## 1. 개요
본 프로젝트는 **Google Antigravity 환경을 기본 기준으로** AI 에이전트와 함께하는 개발 방식인 **바이브 코딩(Vibe Coding)을** 시작할 때, 일관성 있고 통제 가능한 개발 환경을 빠르게 세팅하기 위한 **프로젝트 초기화 규칙 및 스킬 저장소입니다.** 모든 산출물과 문서는 **한국어로** 작성됩니다.

## 2. 목표
- 새로운 프로젝트 시작 시 루트의 `AGENTS.md`, `.agents/`, `rules/`(필요 시 `.gitignore`)만 복사하면 바로 적용할 수 있는 단순화된 단일 진실 공급원(SSOT) 구조 제공 (`templates/` 중복 파일 제거)
- **Google Antigravity 기준 기능 및 활용 가이드 체계화**:
  - `AGENTS.md` 및 `.agents/skills/` 자동 로드 구조 안내
  - `invoke_subagent`(단일/단계별 위임)와 `/teamwork`(대규모 병렬 팀 협업)의 동작 방식 및 선택 기준 명시
  - 바이브 코딩 단계별 추천 Antigravity 슬래시 커맨드(`/plan`, `/grill-me`, `/boost`, `/compact`, `/learn` 등) 안내
  - Cursor, Claude Code 등 타 AI 코딩 도구에 응용할 때의 차이점 및 호환 참고 제공
- **메인-서브 에이전트 분리 아키텍처(Orchestrator-Worker Architecture) 확립**:
  - **메인 에이전트(오케스트레이터)**: 기획, 방향 제시, PRD 승인, 작업 분할 및 서브 에이전트 호출, 결과 검토, 커밋 관리 (**직접 코드 수정 금지**).
  - **서브 에이전트(워커)**: **TDD(Red-Green) 개발, 실제 소스 코드 작성, 언어별 Lint 검증** 전담.
  - 서브 에이전트 위임 시 누락 없는 업무 전달을 위한 **표준 위임 프롬프트 템플릿** 제공.
  - `PRD.md` 및 `CHANGELOG.md`의 권장 문서 양식을 규칙 문서(`rules/agent-execution.md`, `rules/git-workflow.md`)에 통합하여 AI가 일관된 포맷으로 자동 생성하도록 지원.
- **Git 브랜치 운용 원칙 정립 (`dev` 유지 및 사용자 요청 기반 `main` 머지/푸시)**:
  - 기본적으로는 **`dev` 브랜치에서 일상적인 개발 및 자율 커밋만** 진행하고 머무름.
  - `main` 브랜치 머지는 에이전트가 임의로 진행하지 않으며, **오직 사용자가 명시적으로 요청했을 때만** 트리거.
  - 사용자 요청 시: **`CHANGELOG.md` 버전 갱신 ➔ `main` 머지 ➔ GitHub 원격 저장소 푸시(`git push`)** 순으로 일괄 처리.
- **문서 참조 안정성 및 경로 표준화**:
  - 텍스트 및 링크 라벨은 **프로젝트 루트 기준 경로(예: `rules/...`, `.agents/...`)로** 일관되게 표기하여 AI 및 사람의 가독성을 극대화하고 마크다운 상대 링크 오류 제거.
  - 한글 마크다운 문법 오류 방지, 언어별 린터(Linter) 활용, 모던 CSS 컨벤션을 포함한 세부 코드 품질 기준 확립.

## 3. 핵심 구성 요소 및 디렉토리 구조

```text
vibe-coding-init/
├── AGENTS.md                      # AI 에이전트 최우선 진입점 (마스터 인덱스 및 위임 규정)
├── PRD.md                         # 본 프로젝트 제품 요구사항 정의서
├── README.md                      # 사람(개발자)을 위한 초기화 안내서 (Antigravity 기능 가이드 포함)
├── CHANGELOG.md                   # 프로젝트 버전 및 변경 이력 관리
├── .gitignore                     # Git 제외 설정 파일 (신규 프로젝트 복사 겸용)
├── .agents/                       # Antigravity 표준 에이전트 스킬 저장소
│   └── skills/                    # 세션 진입 시 자동 인식되는 프로젝트 스킬
│       ├── vibe-workflow/         # 오케스트레이션 및 워크플로우 가이드 스킬
│       │   └── SKILL.md
│       └── tdd/                   # 서브에이전트용 TDD 핵심 스킬
│           ├── SKILL.md
│           ├── mocking.md
│           └── tests.md
└── rules/                         # 영역별 심층 세부 규칙 (Deep-dive Specifications)
    ├── agent-execution.md         # 메인-서브 분리, 위임 템플릿, PRD 표준 양식, 머지/푸시 원칙
    ├── language-markdown.md       # 한글 작성 원칙 및 마크다운(**, ~ 서식) 깨짐 방지 상세
    ├── git-workflow.md            # dev 기본 유지, 사용자 요청 시 main 머지 & 푸시, CHANGELOG 양식
    ├── security.md                # 환경 변수, 비밀키, 데이터 유실 방지 보안 수칙
    ├── code-quality.md            # TDD 기본 개발 원칙 및 린트 검증 상세 규정
    └── css-style-guide.md         # 모던 CSS 스타일 가이드 (@layer, 변수, 네스팅, clamp 등)
```

## 4. 단계별 추진 계획
1. **`templates/` 폴더 제거**: 중복 및 저효율 템플릿 5개 파일을 제거하여 루트 단일화(SSOT) 달성.
2. **규칙 문서에 표준 양식 통합**: [rules/agent-execution.md](rules/agent-execution.md)에 `PRD.md` 권장 양식을, [rules/git-workflow.md](rules/git-workflow.md)에 `CHANGELOG.md` 권장 양식을 간결하게 수록.
3. **`README.md` 전면 보완**:
   - 상단에 Google Antigravity 기준 제작 명시.
   - Antigravity 연동 기능 상세 가이드(`AGENTS.md`/스킬 자동 로드, `invoke_subagent` vs `/teamwork`, 추천 슬래시 커맨드, 타 도구 호환 참고) 섹션 확장.
   - `templates/` 제거에 맞춰 새 프로젝트 적용 복사 절차 단순화 및 디렉토리 트리 갱신.
4. **전체 마크다운 검증 및 커밋**: 수정된 전체 문서의 한글 마크다운 서식과 링크를 검증한 뒤 `dev` 브랜치에 커밋.
