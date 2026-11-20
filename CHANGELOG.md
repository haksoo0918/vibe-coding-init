# 변경 이력 (Changelog)

본 프로젝트의 모든 주요 변경 사항은 이 문서에 기록됩니다.  
버전 관리는 [유의적 버전(Semantic Versioning)](https://semver.org/lang/ko/) 규약을 준수합니다.

---

## [1.6.0] - 2026-11-20
### 추가 및 변경됨
- **경로 표기 표준화 및 상대 링크 오류 수정**:
  - 눈에 보이는 모든 파일 경로 및 링크 텍스트를 **프로젝트 루트 기준 경로(예: `rules/...`, `.agents/...`)로** 통일
  - `rules/agent-execution.md`, `rules/code-quality.md`, `rules/language-markdown.md`의 깨진 상대 경로 링크(`../../` ➔ `../`) 수정
- **서브 에이전트 위임 표준 프롬프트 템플릿 신설**:
  - `rules/agent-execution.md`에 메인 에이전트가 `invoke_subagent` 호출 시 누락 없이 전달할 수 있는 표준 위임 규격 추가
- **단일 진실 공급원(SSOT) 정립 및 중복 축약**:
  - `AGENTS.md`, `templates/AGENTS.template.md`, `README.md`의 커밋 태그 테이블을 간결화하고 `rules/git-workflow.md`로 안내 통합
- **저장소 환경 정비**:
  - 프로젝트 루트에 `.gitignore` 추가
  - 루트 `PRD.md`에 신규 프로젝트용 `templates/PRD.template.md` 사용 안내 배너 추가

---

## [1.5.0] - 2026-11-06
### 변경됨
- **`dev` 브랜치 유지 및 사용자 요청 기반 `main` 머지/푸시 규정 확립**:
  - 평상시 모든 개발과 커밋은 `dev` 브랜치에 머무르며 자율 커밋만 진행하도록 제한
  - `main` 브랜치 머지는 에이전트가 임의로 진행하지 않으며, **오직 사용자가 명시적으로 요청했을 때만** 수행하도록 변경
  - 사용자 요청 시 수행 절차: `CHANGELOG.md` 버전 관리 기록 ➔ `main` 머지 ➔ 원격(GitHub 등) 저장소 `git push` 일괄 처리
- **문서 동기화**:
  - `AGENTS.md`, `rules/git-workflow.md`, `rules/agent-execution.md`, `.agents/skills/vibe-workflow/SKILL.md`, `templates/AGENTS.template.md`, `README.md`, `PRD.md`에 개정된 릴리즈/푸시 규칙 반영

---

## [1.4.0] - 2026-11-06
### 추가 및 변경됨
- **오케스트레이터-워커(메인-서브 에이전트) 분리 아키텍처 도입**:
  - **메인 에이전트**: 기획, 2~3가지 방향 제시, PRD 관리, 사용자 승인, 서브에이전트 호출(`invoke_subagent`), 산출물 검토 및 최종 머지 전담 (**소스 코드 직접 수정 엄격 금지**)
  - **서브 에이전트**: 메인 에이전트의 지휘 하에 TDD(Red ➔ Green), 실제 소스 코드 작성 및 언어별 Lint 검증 전담
- **`/teamwork` 명령어 연계 가이드 수립**:
  - 대규모 신규 기능 또는 풀스택 병렬 협업을 위한 멀티 에이전트 팀워크 가이드를 `README.md` 및 `PRD.md`에 수록
- **전체 문서 동기화**:
  - `AGENTS.md`, `rules/agent-execution.md`, `.agents/skills/vibe-workflow/SKILL.md`, `templates/AGENTS.template.md`, `README.md`에 메인-서브 역할 분담 체계 반영

---

## [1.3.0] - 2026-11-06
### 추가됨
- **TDD(테스트 주도 개발) 기본 개발 원칙 채택**:
  - `.agents/skills/tdd` 스킬을 프로젝트의 기본 개발 표준으로 통합
  - 기능 구현 및 버그 수정 시 **실패하는 테스트 작성(Red) ➔ 통과하는 최소 구현(Green) ➔ 리팩토링 및 린트 검증** 루프를 의무화
- **규칙 문서 갱신**:
  - `AGENTS.md` 및 `rules/agent-execution.md`: 5단계 워크플로우 4단계에 TDD 구현 프로세스 구체화
  - `rules/code-quality.md`: Seam(공개 접합부) 중심 테스트, 수직 슬라이싱(Vertical Slicing), 안티패턴(구현 결합, 동어반복) 방지 수칙 수립
  - `.agents/skills/vibe-workflow/SKILL.md`: `tdd` 스킬과의 필수 연계 워크플로우 명시
- **템플릿 및 README 갱신**:
  - `templates/AGENTS.template.md`, `README.md`, `PRD.md`에 TDD 기본 개발 원칙 반영

---

## [1.2.0] - 2026-11-06
### 변경됨
- **스킬 디렉토리 표준 일원화 (`.agents/skills/`)**:
  - 에이전트 도구 자동 인식을 위해 기존 `skills/` 디렉토리를 에이전트 표준 경로인 `.agents/skills/`로 통합
  - `vibe-workflow` 스킬을 `.agents/skills/vibe-workflow/`로 이동하고 내부 상대 경로 갱신
  - 새로 설치된 `tdd` 스킬과 함께 일관된 스킬 관리 체계 확립
- **문서 동기화**:
  - `PRD.md`, `AGENTS.md`, `templates/AGENTS.template.md`, `README.md`에 `.agents/skills/` 디렉토리 구조 및 적용 방법 반영

---

## [1.1.0] - 2026-11-06
### 추가됨
- **AI 에이전트 최우선 진입점 (`AGENTS.md`)**:
  - AI 어시스턴트 세션 시작 시 최우선 자동 인식을 위한 마스터 인스트럭션 문서 신규 구축
  - 핵심 페르소나, 한국어 원칙, 5단계 프로토콜 요약 및 `rules/*.md` 라우팅 인덱스 테이블 구성
- **신규 프로젝트용 AGENTS 템플릿 (`templates/AGENTS.template.md`)**:
  - 프로젝트별 패키지 설치, 빌드, 테스트, 린트 명령어 영역이 포함된 보일러플레이트 템플릿 제공
- **README.md 및 PRD.md 갱신**:
  - `AGENTS.md` 자동 인식 활용법 및 역할 분배 명세 반영

---

## [1.0.0] - 2026-11-04
### 추가됨
- **바이브 코딩 초기화 킷 최초 릴리즈**
- **에이전트 실행 프로토콜 (`rules/agent-execution.md`)**:
  - 작업 전 다양한 작업 방향(대안) 및 트레이드오프 제시 의무화
  - PRD 우선 갱신 및 사용자 명시적 승인 원칙
  - 언어별 린터(Linter) 필수 실행 검증
  - `dev` 브랜치 기본 사용 및 자율 커밋
  - `main` 머지 직전 `CHANGELOG.md` 버전 관리 기록 절차 수립
- **언어 및 마크다운 규칙 (`rules/language-markdown.md`)**:
  - 한국어 전용 작성 원칙
  - 한글 마크다운 `**`(굵게), `~`/`~~`(취소선), 인라인 코드 렌더링 깨짐 방지 서식 가이드 추가
- **Git 워크플로우 (`rules/git-workflow.md`)**:
  - 개발용 `dev` 브랜치 및 배포용 `main` 브랜치 분리 운용
  - `dev` 브랜치에서의 에이전트 자율 커밋 규칙
  - `main` 머지 전 CHANGELOG 작성 필수 절차
  - 한글 커밋 태그 컨벤션 (`기능:`, `수정:`, `문서:`, `리팩토링:` 등)
- **모던 CSS 스타일 가이드 (`rules/css-style-guide.md`)**:
  - 이학수 개발 블로그 가이드 요약 반영
  - `@layer` 4단계 계층(`reset`, `base`, `components`, `utilities`)
  - `:root` 전역 변수(디자인 토큰) 관리
  - 네이티브 CSS 네스팅, 모던 가상 클래스(`:has()`, `:is()`, `:where()`), `clamp()` 유체 디자인, 논리적 속성
- **코드 품질 및 보안 (`rules/code-quality.md`, `rules/security.md`)**:
  - 언어별 Lint 도구 적용 기준 명시
  - 자격 증명 하드코딩 금지 및 데이터 손실 방지 수칙
- **신규 프로젝트용 템플릿 (`templates/`)**:
  - `PRD.template.md`, `CHANGELOG.template.md`, `.gitignore.template`, `env.example.template`
- **프로젝트 안내서 (`README.md`)**
