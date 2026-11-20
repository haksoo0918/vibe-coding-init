# 바이브 코딩 프로젝트 초기화 킷 (Vibe Coding Init Kit)

> [!NOTE]
> 본 저장소는 **Google Antigravity 환경을 기본 기준으로** 작성되었습니다.  
> Antigravity의 `AGENTS.md`·`.agents/skills/` 자동 인식, `invoke_subagent` 서브 에이전트 위임, `/teamwork` 및 내장 슬래시 커맨드 기능을 중심으로 설계되었으며, 타 AI 코딩 도구(Cursor, Claude Code 등)에서도 핵심 규칙을 응용해 활용할 수 있습니다.

본 저장소는 AI 코딩 에이전트와 함께하는 개발 방식인 **바이브 코딩(Vibe Coding)을** 안전하고 체계적으로 진행할 수 있도록 돕는 **프로젝트 초기화 규칙 및 스킬 저장소입니다.**

본 프로젝트는 메인 에이전트가 직접 코딩하지 않고 기획/지휘/검토를 맡고, 서브 에이전트가 TDD로 실제 구현을 전담하는 **오케스트레이터-워커(Orchestrator-Worker) 분리 아키텍처를** 기본 원칙으로 합니다.

모든 가이드, 규칙 문서, 주석 예시, 커밋 컨벤션은 **한국어로** 표준화되어 있습니다.

---

## 📁 디렉토리 구조

```text
vibe-coding-init/
├── AGENTS.md                      # AI 에이전트 최우선 진입점 (오케스트레이터/워커 역할 정의)
├── PRD.md                         # 본 저장소의 제품 요구사항 정의서
├── README.md                      # 사람(개발자)을 위한 프로젝트 안내서 (Antigravity 가이드 포함)
├── CHANGELOG.md                   # 프로젝트 버전 및 변경 이력 관리
├── .gitignore                     # Git 제외 설정 파일 (신규 프로젝트 복사 겸용)
├── .agents/                       # Antigravity 표준 에이전트 스킬 저장소
│   └── skills/                    # 세션 진입 시 자동 인식되는 프로젝트 스킬
│       ├── vibe-workflow/         # 오케스트레이션 및 서브에이전트 위임 워크플로우 스킬
│       │   └── SKILL.md
│       └── tdd/                   # 서브에이전트용 테스트 주도 개발(TDD) 핵심 스킬
│           ├── SKILL.md
│           ├── mocking.md
│           └── tests.md
└── rules/                         # AI 에이전트 및 프로젝트 핵심 세부 규칙
    ├── agent-execution.md         # 메인-서브 분리 프로토콜, 위임 템플릿, PRD 권장 양식
    ├── language-markdown.md       # 한국어 작성 원칙 및 마크다운(**, ~ 서식) 깨짐 방지 가이드
    ├── git-workflow.md            # Git 워크플로우 (dev 유지, 사용자 요청 시 main 머지 & 푸시, CHANGELOG 양식)
    ├── security.md                # 환경 변수, 비밀키 보호 및 데이터 손실 방지 수칙
    ├── code-quality.md            # 서브에이전트 TDD 기본 원칙, 언어별 Lint 검증, KISS/YAGNI 기준
    └── css-style-guide.md         # 모던 CSS 스타일 가이드 (@layer, 변수, 네스팅, clamp 등)
```

---

## 👥 메인-서브 에이전트 역할 분담 모델

| 구분 | 메인 에이전트 (오케스트레이터) | 서브 에이전트 (워커) |
| :--- | :--- | :--- |
| **코드 작성 여부** | **직접 코드 수정/구현 절대 금지** | **실제 소스 코드 및 테스트 코드 작성 전담** |
| **핵심 업무** | • 요구사항 분석 및 2~3가지 방향 제시<br>• `PRD.md` 작성 및 사용자 승인 획득<br>• 작업 단위 분할 후 서브 에이전트 호출(`invoke_subagent`)<br>• 산출물 검토 및 `dev` 브랜치 자율 커밋 (`dev` 상태 유지)<br>• **사용자 요청 시에만** `CHANGELOG.md` 작성, `main` 머지 및 GitHub 푸시 | • 메인 에이전트의 지시사항 수신<br>• Seam(접합부) 식별 및 테스트 선작성(**Red**)<br>• 테스트 통과를 위한 최소 구현(**Green**)<br>• 언어별 Lint 검증 실행<br>• 작업 완료 및 변경 파일 보고 |

---

## ⚡ Google Antigravity 연동 기능 및 활용 가이드

본 초기화 킷은 **Google Antigravity의 내장 커스터마이징 및 멀티 에이전트 기능과** 긴밀하게 동작하도록 구성되어 있습니다.

### 1. 자동 인식 구조 (`AGENTS.md` 및 `.agents/skills/`)
- **`AGENTS.md`**: 워크스페이스 루트에 위치하면 Antigravity가 세션 시작 시 최우선 지침으로 자동 로드합니다. 메인 에이전트가 코드를 직접 수정하지 않고 기획·위임·검토 역할만 수행하도록 강제합니다.
- **`.agents/skills/*/SKILL.md`**: Antigravity 표준 프로젝트 스킬 디렉토리입니다. 각 `SKILL.md`의 YAML 프론트매터(`name`, `description`)를 기반으로 작업 맥락에 맞는 스킬(`vibe-workflow`, `tdd`)이 자동으로 탐색·활성화됩니다.

### 2. 서브 에이전트 위임 방식 (`invoke_subagent` vs `/teamwork`)

| 구분 | `invoke_subagent` (내장 도구) | `/teamwork` (슬래시 커맨드) |
| :--- | :--- | :--- |
| **호출 주체** | 메인 에이전트가 내부 도구로 직접 호출 | 사용자가 채팅창에 `/teamwork` 입력하여 실행 |
| **동작 방식** | 독립된 컨텍스트의 단일/개별 서브 에이전트를 백그라운드로 실행하고 완료 결과를 비동기로 보고받음 | 역할별(`Frontend`, `Backend`, `QA` 등) 다중 자율 에이전트 팀을 구성해 병렬 협업 |
| **권장 상황** | 일반 기능 구현, 버그 수정, 순차적·모듈 단위 TDD 작업 | 대규모 신규 기능, 프론트엔드+백엔드+인프라 동시 병렬 작업 |
| **위임 규격** | [rules/agent-execution.md](rules/agent-execution.md) 제3장 표준 위임 프롬프트 템플릿 적용 | 메인 에이전트가 로드맵 기반 팀 구성을 제안하고 사용자 승인 후 병렬 실행 |

### 3. 바이브 코딩 단계별 추천 슬래시 커맨드

| 슬래시 커맨드 | 활용 단계 | 설명 |
| :--- | :--- | :--- |
| `/plan` | 1~2단계 (분석/PRD) | 복잡한 요구사항을 구현 전에 단계별 계획으로 구체화할 때 사용 |
| `/grill-me` | 1~2단계 (설계 검토) | 에이전트와의 인터랙티브 질의응답을 통해 설계 결정과 예외 케이스를 사전에 확정 |
| `/teamwork` | 4단계 (병렬 구현) | 대규모 기능이나 풀스택 작업을 여러 자율 에이전트 팀으로 병렬 처리 |
| `/boost` | 고난도 설계/구현 | 깊은 추론, 다각도 검토, 엄격한 검증이 필요한 복잡한 작업에 사용 |
| `/compact` | 장시간 세션 관리 | 대화가 길어졌을 때 컨텍스트를 요약·압축하여 응답 품질과 속도 유지 |
| `/learn` | 작업 완료 후 | 반복 실수 교정이나 복잡한 환경 설정을 다음 세션에도 유지하도록 규칙화 |

### 4. 타 AI 코딩 도구 적용 시 참고

본 저장소는 Google Antigravity 기준이지만, 규칙 문서(`rules/*.md`)와 TDD·Git 워크플로우는 다른 도구에도 응용할 수 있습니다.

| 기능 / 경로 | Google Antigravity (기본) | Cursor / Claude Code 등 타 도구 참고 |
| :--- | :--- | :--- |
| 진입점 규칙 파일 | 루트 `AGENTS.md` 자동 로드 | Claude Code는 `CLAUDE.md`로 복사·연결, Cursor는 `.cursor/rules/` 연계 고려 |
| 스킬 디렉토리 | `.agents/skills/` 자동 인식 | 도구별 스킬 경로 정책에 맞게 심볼릭 링크 또는 복사 필요 |
| 서브 에이전트 호출 | `invoke_subagent` 도구 사용 | 각 도구의 자체 서브 에이전트 호출 방식(예: Claude Code의 `Agent` 등)에 맞게 문구 조정 |
| 병렬 팀 명령어 | `/teamwork` 슬래시 커맨드 | Antigravity 전용 명령어이므로 타 도구에서는 일반 프롬프트 지시로 대체 |

---

## 🚀 새로운 프로젝트에 적용하는 방법

### 방법 1. 신규 프로젝트 시작 (`git clone` 후 정리 — 권장)

저장소를 새 프로젝트 이름으로 바로 클론한 뒤, 기존 `.git` 이력과 본 저장소 전용 문서만 지우고 새 Git으로 초기화하는 가장 빠른 방법입니다.

1. 저장소를 클론하고 프로젝트 폴더로 이동합니다:
   ```bash
   git clone https://github.com/haksoo0918/vibe-coding-init.git my-new-project
   cd my-new-project
   ```

2. 기존 `.git` 폴더와 본 저장소 전용 문서(`PRD.md`, `CHANGELOG.md`, `README.md`)를 삭제합니다:
   - **macOS / Linux (bash/zsh)**:
     ```bash
     rm -rf .git PRD.md CHANGELOG.md README.md
     ```
   - **Windows (PowerShell)**:
     ```powershell
     Remove-Item -Recurse -Force .git, PRD.md, CHANGELOG.md, README.md
     ```
   > [!TIP]
   > `PRD.md`와 `CHANGELOG.md`는 지워두면 첫 기획 및 릴리즈 시점에 에이전트가 규칙 문서의 표준 양식에 맞춰 자동으로 새로 생성합니다. `README.md`는 삭제 대신 새 프로젝트 소개로 내용을 비워 덮어써도 됩니다.

3. 새 Git 저장소를 초기화하고 기본 브랜치(`main`, `dev`)를 구성합니다:
   ```bash
   git init
   git add .
   git commit -m "환경: 바이브 코딩 초기 규칙 및 스킬 설정"
   git branch -M main
   git checkout -b dev
   ```

### 방법 2. 이미 진행 중인 프로젝트에 적용

기존 프로젝트에 규칙만 추가하고 싶다면, 클론한 저장소에서 아래 3개 항목만 기존 프로젝트 루트로 복사하면 됩니다 (기존 `.gitignore`는 유지).

- `AGENTS.md`
- `.agents/`
- `rules/`

### 적용 후 시작하기

프로젝트 루트에 `AGENTS.md`, `.agents/`, `rules/`가 준비된 상태로 Antigravity를 열면 세션 진입 시 자동으로 오케스트레이터 규칙과 스킬이 로드됩니다. 첫 요청 시 에이전트가 대안 제시 ➔ `PRD.md` 자동 생성 ➔ 승인 대기 ➔ 서브 에이전트 TDD 위임 순서로 작업을 진행합니다.

---

## 🔄 바이브 코딩 5단계 핵심 워크플로우

1. **요청 분석 & 방향 제시 (Analyze & Present Options, 메인)**: 작업 성격을 분석하고, 2~3가지 대안과 장단점(트레이드오프)을 먼저 제시합니다.
2. **PRD 작성 (PRD Update, 메인)**: 소스 코드를 건드리기 전에 `PRD.md`에 요구사항과 서브에이전트 작업 계획을 기록합니다.
3. **사용자 승인 (Approval, 메인)**: 계획을 사용자에게 공유하고, **명시적 승인을** 받기 전까지 코드를 작성하거나 서브에이전트를 호출하지 않습니다.
4. **서브 에이전트 위임 TDD 구현 & 검증 (Implement, Lint & Auto Commit)**:
   - 메인 에이전트가 `invoke_subagent`(또는 `/teamwork`)로 서브 에이전트를 호출합니다.
   - 서브 에이전트가 **[.agents/skills/tdd](.agents/skills/tdd/SKILL.md)** 스킬에 따라 **Red ➔ Green ➔ Lint 검증을** 완료하고 보고합니다.
   - 메인 에이전트가 검토 후 `dev` 브랜치에 한글 커밋 태그로 **자율 커밋하며**, **`dev` 브랜치 상태를 유지합니다.**
5. **사용자 요청 시 ➔ CHANGELOG 작성, `main` 머지 및 GitHub 푸시 (Release & Push, 메인)**:
   - **사용자가 명시적으로 머지/푸시를 요청했을 때만 실행합니다.**
   - `CHANGELOG.md` 갱신 ➔ `main` 머지 ➔ `git push origin main` 및 `git push origin dev`를 차례대로 완수합니다.

---

## 🏷️ 한글 커밋 태그 요약 (상세: [rules/git-workflow.md](rules/git-workflow.md))

| 커밋 태그 | 설명 | 예시 |
| :--- | :--- | :--- |
| **기능** | 새로운 기능 추가 | `기능: 소셜 로그인 기능 추가` |
| **수정** | 버그 및 결함 수정 | `수정: 세션 만료 리다이렉트 오류 수정` |
| **테스트** | 테스트 코드 추가/보완 (TDD Red/Green) | `테스트: 사용자 인증 단위 테스트 추가` |
| **문서** | 문서 생성 및 변경 (PRD, CHANGELOG 등) | `문서: v1.6.0 릴리즈 CHANGELOG 업데이트` |
| **리팩토링** | 기능 변경 없는 코드 개선 | `리팩토링: 데이터 조회 쿼리 최적화` |
| **환경** | 빌드 설정 및 의존성 변경 | `환경: ESLint 및 Prettier 설정 추가` |
| **스타일** | 코드 포맷팅 및 서식 정리 | `스타일: 린트 포맷 규칙 적용` |
| **제거** | 미사용 파일/코드 삭제 | `제거: 레거시 유틸리티 함수 제거` |

---

## 📄 라이선스
[MIT License](LICENSE)
