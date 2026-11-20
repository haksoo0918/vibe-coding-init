> [!NOTE]
> 본 문서는 `vibe-coding-init` 저장소 자체의 개발 요구사항 정의서입니다.  
> 새로운 프로젝트를 시작할 때는 [`templates/PRD.template.md`](templates/PRD.template.md)를 복사하여 사용하세요.

# PRD (제품 요구사항 정의서): 바이브코딩 프로젝트 초기화 템플릿

## 1. 개요
본 프로젝트는 AI 에이전트와 함께하는 개발 방식인 **바이브 코딩(Vibe Coding)**을 시작할 때, 일관성 있고 통제 가능한 개발 환경을 빠르게 세팅하기 위한 **프로젝트 초기화 템플릿 및 규칙 문서 저장소**입니다. 모든 산출물과 문서는 **한국어**로 작성됩니다.

## 2. 목표
- 새로운 프로젝트 시작 시 복사하거나 적용할 수 있는 표준화된 프로젝트 규칙, 프로토콜, 스킬 템플릿 제공
- **메인-서브 에이전트 분리 아키텍처(Orchestrator-Worker Architecture) 확립**:
  - **메인 에이전트(오케스트레이터)**: 기획, 방향 제시, PRD 승인, 작업 분할 및 서브 에이전트 호출, 결과 검토, 커밋 관리 (**직접 코드 수정 금지**).
  - **서브 에이전트(워커)**: **TDD(Red-Green) 개발, 실제 소스 코드 작성, 언어별 Lint 검증** 전담.
  - 서브 에이전트 위임 시 누락 없는 업무 전달을 위한 **표준 위임 프롬프트 템플릿** 제공.
  - 대규모 협업 시 `/teamwork` 커맨드와의 연계 가이드 제공.
- **Git 브랜치 운용 원칙 정립 (`dev` 유지 및 사용자 요청 기반 `main` 머지/푸시)**:
  - 기본적으로는 **`dev` 브랜치에서 일상적인 개발 및 자율 커밋**만 진행하고 머무름.
  - `main` 브랜치 머지는 에이전트가 임의로 진행하지 않으며, **오직 사용자가 명시적으로 요청했을 때만** 트리거.
  - 사용자 요청 시: **`CHANGELOG.md` 버전 갱신 ➔ `main` 머지 ➔ GitHub 원격 저장소 푸시(`git push`)** 순으로 일괄 처리.
- **문서 참조 안정성 및 경로 표준화**:
  - 텍스트 및 링크 라벨은 **프로젝트 루트 기준 경로(예: `rules/...`, `.agents/...`)**로 일관되게 표기하여 AI 및 사람의 가독성을 극대화하고 마크다운 상대 링크 오류 제거.
  - 문서 간 중복을 축약하고 단일 진실 공급원(SSOT) 원칙 적용.
- AI 에이전트의 최초 진입점(Entry Point)이자 마스터 라우터 역할을 하는 `AGENTS.md` 구축
- 에이전트 도구 표준 스킬 디렉토리인 `.agents/skills/`를 기준으로 모든 프로젝트 스킬(`vibe-workflow`, `tdd` 등)을 일원화하여 자동 로드 지원
- 한글 마크다운 문법 오류 방지, 언어별 린터(Linter) 활용, 모던 CSS 컨벤션을 포함한 세부 코드 품질 기준 확립

## 3. 핵심 구성 요소 및 디렉토리 구조

```text
vibe-coding-init/
├── AGENTS.md                      # AI 에이전트 최우선 진입점 (마스터 인덱스 및 위임 규정)
├── PRD.md                         # 본 프로젝트 제품 요구사항 정의서
├── README.md                      # 사람(개발자)을 위한 초기화 사용 안내서
├── CHANGELOG.md                   # 프로젝트 버전 및 변경 이력 관리
├── .gitignore                     # Git 제외 설정 파일
├── .agents/                       # 에이전트 표준 환경 및 스킬 저장소
│   └── skills/                    # 에이전트 자동 인식 프로젝트 스킬
│       ├── vibe-workflow/         # 오케스트레이션 및 워크플로우 가이드 스킬
│       │   └── SKILL.md
│       └── tdd/                   # 서브에이전트용 TDD 핵심 스킬
│           ├── SKILL.md
│           ├── mocking.md
│           └── tests.md
├── rules/                         # 영역별 심층 세부 규칙 (Deep-dive Specifications)
│   ├── agent-execution.md         # 메인-서브 분리, 위임 템플릿, 사용자 요청 기반 머지/푸시
│   ├── language-markdown.md       # 한글 작성 원칙 및 마크다운(**, ~ 서식) 깨짐 방지 상세
│   ├── git-workflow.md            # dev 기본 유지, 사용자 요청 시 main 머지 & 깃헙 푸시 상세
│   ├── security.md                # 환경 변수, 비밀키, 데이터 유실 방지 보안 수칙
│   ├── code-quality.md            # TDD 기본 개발 원칙 및 린트 검증 상세 규정
│   └── css-style-guide.md         # 모던 CSS 스타일 가이드 (@layer, 변수, 네스팅, clamp 등)
└── templates/                     # 새 프로젝트 세팅용 보일러플레이트 템플릿
    ├── AGENTS.template.md         # 신규 프로젝트용 AGENTS 템플릿
    ├── PRD.template.md            # 신규 프로젝트용 빈 PRD 템플릿
    ├── CHANGELOG.template.md      # 버전 이력 관리 템플릿
    ├── .gitignore.template        # 권장 .gitignore 템플릿
    └── env.example.template       # 환경 변수 예시 템플릿
```

## 4. 단계별 추진 계획
1. **루트 `.gitignore` 생성**: 본 저장소의 불필요 파일 추적 방지.
2. **상대 경로 및 라벨 표준화**: `rules/agent-execution.md`, `rules/code-quality.md`, `rules/language-markdown.md` 등의 링크 및 프로젝트 루트 기준 텍스트 통일.
3. **위임 프롬프트 템플릿 추가**: `rules/agent-execution.md`에 서브 에이전트 호출용 표준 규격 신설.
4. **중복 축약 및 SSOT 정립**: `AGENTS.md`의 커밋 태그 및 다이어그램 간결화.
5. **동기화 및 릴리즈 기록**: `README.md`, `CHANGELOG.md` 갱신.
