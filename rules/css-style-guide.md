# 모던 CSS 스타일 가이드 (Modern CSS Style Guide)

본 문서는 일관되고 유지보수하기 쉬우며 확장성 높은 모던 웹 스타일시트 작성을 위한 CSS 컨벤션 가이드입니다.  
(참조: [이학수 개발 블로그 - 모던 CSS 작성 가이드](https://blog.haksoo0918.workers.dev/post/2026/css-style-guide/))

---

## 1. 핵심 아키텍처 및 계층 구조 (`@layer`)

명시도(Specificity) 경쟁 및 스타일 덮어쓰기 충돌을 근본적으로 방지하기 위해 CSS 캐스케이드 레이어(`@layer`)를 사용하여 스타일의 우선순위를 명시적으로 제어합니다.

### 1.1 계층 순서 선언
스타일시트 최상단에서 우선순위 순서를 선언합니다 (뒤에 선언된 레이어가 앞의 레이어보다 높은 우선순위를 가집니다):

```css
@layer reset, base, components, utilities;
```

### 1.2 계층별 역할과 정의

| 레이어 | 설명 | 주요 포함 내용 |
| :--- | :--- | :--- |
| **`reset`** | 브라우저 기본 스타일 초기화 | `box-sizing: border-box`, 기본 마진/패딩 제거, 미디어 요소 초기화 |
| **`base`** | HTML 기본 태그 스타일링 | 전역 폰트 패밀리, 기본 배경/글자색, 본문 텍스트 라인 높이, 스무딩 |
| **`components`** | 재사용 가능한 UI 컴포넌트 | 버튼, 카드, 네비게이션바, 모달, 폼 컨트롤 등 컴포넌트 스타일 |
| **`utilities`** | 단일 목적의 유틸리티 클래스 | 텍스트 정렬, 디스플레이 제어, 간격 유틸리티 등 (최우선 적용) |

---

## 2. 디자인 토큰 및 전역 변수 (`:root`)

색상, 간격 스케일, 타이포그래피 등의 공통 값은 특정 레이어에 종속되지 않고 어디서나 상속/참조될 수 있도록 **`:root`에 단독 선언합니다.**

```css
:root {
  /* 색상 팔레트 */
  --color-primary: #0066ff;
  --color-primary-hover: #0052cc;
  --color-bg-base: #ffffff;
  --color-bg-muted: #f4f4f5;
  --color-text-main: #18181b;
  --color-text-sub: #71717a;
  --color-border: #e4e4e7;

  /* 간격(Spacing) 스케일 */
  --space-1: 0.25rem; /* 4px */
  --space-2: 0.5rem;  /* 8px */
  --space-3: 0.75rem; /* 12px */
  --space-4: 1rem;    /* 16px */
  --space-6: 1.5rem;  /* 24px */
  --space-8: 2rem;    /* 32px */

  /* 타이포그래피 */
  --font-sans: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  --font-mono: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
}
```

---

## 3. 모던 CSS 필수 기능 권장 사항

### 3.1 네이티브 CSS 네스팅 (Native Nesting)
- Sass, Less 등의 외부 전처리기 없이 브라우저 표준 네이티브 네스팅 문법을 적극 활용합니다.
- 과도한 중첩(Deep Nesting)은 피하고 3단계 이내로 제한합니다.

```css
@layer components {
  .card {
    background-color: var(--color-bg-base);
    border: 1px solid var(--color-border);
    padding: var(--space-4);

    & .card-title {
      font-size: 1.25rem;
      color: var(--color-text-main);
    }

    &:hover {
      border-color: var(--color-primary);
    }
  }
}
```

### 3.2 현대적 가상 클래스 (`:is()`, `:where()`, `:has()`)
- **`:where()`**: 명시도 0으로 기본 스타일을 정의할 때 활용 (오버라이드가 매우 쉬움).
- **`:is()`**: 선택자 그룹화로 중복을 줄이고 코드를 간결화할 때 활용.
- **`:has()`**: 부모 선택자 또는 형제 상태에 따른 조건부 스타일링 (JavaScript 없이 순수 CSS로 상태 제어 가능).

```css
/* 특정 요소를 포함하는 카드에 강조 스타일 부여 */
.card:has(.badge-danger) {
  border-color: #ef4444;
}

/* 입력 필드에 포커스된 경우 폼 전체 스타일 변경 */
.form-group:has(:focus-visible) {
  box-shadow: 0 0 0 2px var(--color-primary);
}
```

### 3.3 유체 타이포그래피 및 유체 함수 (`clamp()`)
- 복잡하고 많은 미디어 쿼리를 일일이 작성하는 대신 `clamp()`를 활용하여 뷰포트 너비에 따라 부드럽게 반응하는 유체 타이포그래피와 반응형 간격을 구성합니다.

```css
:root {
  /* 최소 1rem(16px), 뷰포트 비례 2.5vw, 최대 1.5rem(24px) */
  --text-fluid-base: clamp(1rem, 0.8rem + 1vw, 1.5rem);
  --space-fluid-lg: clamp(1.5rem, 1rem + 2.5vw, 3rem);
}
```

### 3.4 논리적 속성 (Logical Properties)
- 물리적 방향(`left`, `right`, `top`, `bottom`) 대신 다국어 지원 및 글쓰기 모드 대응이 가능한 논리적 속성을 우선 사용합니다.
  - `margin-left` / `margin-right` ➔ `margin-inline`
  - `padding-top` / `padding-bottom` ➔ `padding-block`
  - `top`, `right`, `bottom`, `left` ➔ `inset` / `inset-inline` / `inset-block`

### 3.5 서브그리드 (Subgrid)
- 중첩된 자식 요소가 부모의 Grid 트랙 라인에 정확히 정렬되어야 하는 경우 `grid-template-columns: subgrid;`를 활용합니다.
