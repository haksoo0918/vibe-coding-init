# 바이브 코딩 프로젝트 초기화 킷 (Vibe Coding Init Kit)

본 저장소는 AI 코딩 에이전트와 함께하는 개발 방식인 **바이브 코딩(Vibe Coding)**을 안전하고 체계적으로 진행할 수 있도록 돕는 **프로젝트 초기화 템플릿 및 규칙 저장소**입니다.

본 프로젝트는 메인 에이전트가 직접 코딩하지 않고 기획/지휘/검토를 맡고, 서브 에이전트가 TDD로 실제 구현을 전담하는 **오케스트레이터-워커(Orchestrator-Worker) 분리 아키텍처**를 기본 원칙으로 합니다.

모든 가이드, 규칙 문서, 주석 예시, 커밋 컨벤션은 **한국어**로 표준화되어 있습니다.

---

## 📁 디렉토리 구조

```text
vibe-coding-init/
├── AGENTS.md                      # AI 에이전트 최우선 진입점 (오케스트레이터/워커 역할 정의)
├── PRD.md                         # 본 저장소의 제품 요구사항 정의서
├── README.md                      # 사람(개발자)을 위한 프로젝트 안내서 (/teamwork 가이드 포함)
├── CHANGELOG.md                   # 프로젝트 버전 및 변경 이력 관리
├── .gitignore                     # Git 제외 설정 파일
├── .agents/                       # 에이전트 표준 환경 및 스킬 저장소
│   └── skills/                    # 에이전트 자동 로드 프로젝트 스킬
│       ├── vibe-workflow/         # 오케스트레이션 및 서브에이전트 위임 워크플로우 스킬
│       │   └── SKILL.md
│       └── tdd/                   # 서브에이전트용 테스트 주도 개발(TDD) 핵심 스킬
│           ├── SKILL.md
│           ├── mocking.md
│           └── tests.md
├── rules/                         # AI 에이전트 및 프로젝트 핵심 세부 규칙
│   ├── agent-execution.md         # 메인-서브 분리 실행 프로토콜 및 위임 프롬프트 템플릿
│   ├── language-markdown.md       # 한국어 작성 원칙 및 마크다운(**, ~ 서식) 깨짐 방지 가이드
│   ├── git-workflow.md            # Git 워크플로우 (dev 유지, 사용자 요청 시 main 머지 & 푸시)
│   ├── security.md                # 환경 변수, 비밀키 보호 및 데이터 손실 방지 수칙
│   ├── code-quality.md            # 서브에이전트 TDD 기본 원칙, 언어별 Lint 검증, KISS/YAGNI 기준
│   └── css-style-guide.md         # 모던 CSS 스타일 가이드 (@layer, 변수, 네스팅, clamp 등)
└── templates/                     # 신규 프로젝트 세팅용 보일러플레이트 템플릿
    ├── AGENTS.template.md         # 신규 프로젝트용 AGENTS 템플릿 (오케스트레이터 모델)
    ├── PRD.template.md            # 신규 프로젝트용 빈 PRD 템플릿
    ├── CHANGELOG.template.md      # 신규 프로젝트용 CHANGELOG 템플릿
    ├── .gitignore.template        # 권장 .gitignore 템플릿
    └── env.example.template       # 환경 변수 예시 템플릿
```

---

## 👥 메인-서브 에이전트 역할 분담 모델

| 구분 | 메인 에이전트 (오케스트레이터) | 서브 에이전트 (워커) |
| :--- | :--- | :--- |
| **코드 작성 여부** | **직접 코드 수정/구현 절대 금지** | **실제 소스 코드 및 테스트 코드 작성 전담** |
| **핵심 업무** | • 요구사항 분석 및 2~3가지 방향 제시<br>• `PRD.md` 작성 및 사용자 승인 획득<br>• 작업 단위 분할 후 서브 에이전트 호출(`invoke_subagent`)<br>• 산출물 검토 및 `dev` 브랜치 자율 커밋 (dev 상태 유지)<br>• **사용자 요청 시에만** `CHANGELOG.md` 작성, `main` 머지 및 GitHub 푸시 | • 메인 에이전트의 지시사항 수신<br>• Seam(접합부) 식별 및 테스트 선작성(**Red**)<br>• 테스트 통과를 위한 최소 구현(**Green**)<br>• 언어별 Lint 검증 실행<br>• 작업 완료 및 변경 파일 보고 |

---

## 🤝 `/teamwork` 명령어 활용 가이드

Antigravity의 **`/teamwork`** 기능은 여러 자율 에이전트가 팀(Team)을 이루어 대규모 개발 작업을 병렬로 수행할 수 있도록 돕는 슬래시 커맨드입니다.

- **언제 사용하나요?**
  - 대규모 신규 기능 개발 또는 풀스택 프로젝트 (프론트엔드 + 백엔드 + 인프라 병렬 작업)
  - 독립된 단위 작업이 많아 여러 에이전트가 동시에 작업할 때
- **어떻게 사용하나요?**
  1. 사용자가 채팅창에 `/teamwork`를 입력합니다.
  2. 메인 에이전트가 로드맵을 기반으로 역할별 에이전트 팀(`Frontend Dev`, `Backend Dev`, `QA Tester` 등) 구성을 제안합니다.
  3. 승인 시 각 서브 에이전트들이 각자의 브랜치/영역에서 TDD 개발을 병렬로 수행하고 결과를 메인 에이전트가 취합합니다.

---

## 🚀 새로운 프로젝트에 적용하는 방법

### 방법 1. 직접 파일 복사 및 Git 초기화

1. 신규 프로젝트 디렉토리를 생성합니다:
   ```bash
   mkdir my-new-project
   cd my-new-project
   ```

2. 본 저장소의 `AGENTS.md`, `.agents/skills/`, `rules/` 폴더 및 템플릿을 복사합니다:
   ```bash
   # 에이전트 진입점, 스킬 및 규칙 복사
   cp /path/to/vibe-coding-init/templates/AGENTS.template.md ./AGENTS.md
   cp -r /path/to/vibe-coding-init/.agents ./.agents
   cp -r /path/to/vibe-coding-init/rules ./rules

   # 프로젝트 문서 및 설정 템플릿 복사
   cp /path/to/vibe-coding-init/templates/PRD.template.md ./PRD.md
   cp /path/to/vibe-coding-init/templates/CHANGELOG.template.md ./CHANGELOG.md
   cp /path/to/vibe-coding-init/templates/.gitignore.template ./.gitignore
   cp /path/to/vibe-coding-init/templates/env.example.template ./.env.example
   ```

3. Git 저장소를 초기화하고 기본 브랜치를 구성합니다:
   ```bash
   git init
   git add .
   git commit -m "환경: 바이브 코딩 초기 규칙, 스킬 및 템플릿 설정"
   git branch -M main
   git checkout -b dev
   ```

### 방법 2. AI 에이전트 자동 인식 활용

프로젝트 루트에 `AGENTS.md`와 `.agents/skills/` 폴더가 존재하면 Antigravity, Cursor, Claude Code 등 최신 AI 에이전트가 세션 진입 시 자동으로 오케스트레이터 모델을 인식하고, 직접 코딩하지 않고 서브 에이전트 위임 절차를 밟습니다.

---

## 🔄 바이브 코딩 5단계 핵심 워크플로우

1. **요청 분석 & 방향 제시 (Analyze & Present Options, 메인)**: 작업 성격을 분석하고, 2~3가지 대안과 장단점(트레이드오프)을 먼저 제시합니다.
2. **PRD 작성 (PRD Update, 메인)**: 소스 코드를 건드리기 전에 `PRD.md`에 요구사항과 서브에이전트 작업 계획을 기록합니다.
3. **사용자 승인 (Approval, 메인)**: 계획을 사용자에게 공유하고, **명시적 승인**을 받기 전까지 코드를 작성하거나 서브에이전트를 호출하지 않습니다.
4. **서브 에이전트 위임 TDD 구현 & 검증 (Implement, Lint & Auto Commit)**:
   - 메인 에이전트가 `invoke_subagent`(또는 `/teamwork`)로 서브 에이전트를 호출합니다.
   - 서브 에이전트가 **[.agents/skills/tdd](.agents/skills/tdd/SKILL.md)**에 따라 **Red ➔ Green ➔ Lint 검증**을 완료하고 보고합니다.
   - 메인 에이전트가 검토 후 `dev` 브랜치에 한글 커밋 태그로 **자율 커밋**하며, **`dev` 브랜치 상태를 유지**합니다.
5. **사용자 요청 시 ➔ CHANGELOG 작성, `main` 머지 및 GitHub 푸시 (Release & Push, 메인)**:
   - **사용자가 명시적으로 머지/푸시를 요청했을 때만 실행**합니다.
   - `CHANGELOG.md` 갱신 ➔ `main` 머지 ➔ `git push origin main` 및 `git push origin dev`를 차례대로 완수합니다.

---

## 🏷️ 한글 커밋 태그 요약 (상세: [rules/git-workflow.md](rules/git-workflow.md))

| 커밋 태그 | 설명 | 예시 |
| :--- | :--- | :--- |
| **기능** | 새로운 기능 추가 | `기능: 소셜 로그인 기능 추가` |
| **수정** | 버그 및 결함 수정 | `수정: 세션 만료 리다이렉트 오류 수정` |
| **테스트** | 테스트 코드 추가/보완 (TDD Red/Green) | `테스트: 사용자 인증 단위 테스트 추가` |
| **문서** | 문서 생성 및 변경 (PRD, CHANGELOG 등) | `문서: v1.6.0 릴리즈 CHANGELOG 업데이트` |
| **리팩토링**| 기능 변경 없는 코드 개선 | `리팩토링: 데이터 조회 쿼리 최적화` |
| **환경** | 빌드 설정 및 의존성 변경 | `환경: ESLint 및 Prettier 설정 추가` |
| **스타일** | 코드 포맷팅 및 서식 정리 | `스타일: 린트 포맷 규칙 적용` |
| **제거** | 미사용 파일/코드 삭제 | `제거: 레거시 유틸리티 함수 제거` |

---

## 📄 라이선스
MIT License
