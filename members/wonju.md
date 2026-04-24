# wonju

> 이름: 이원주
> 역할: 미정 
> 브랜치: `docs/wonju`

---

## 활동 기록


### Week 04

## 맡은 작업

- 토큰 아키텍처 및 네이밍 컨벤션 정의 (side#219) ✅ — PR #246
- Primitive 토큰 정의 (side#220) ✅ — PR #248
- Semantic 토큰 초안 설계 (side#221) — 이번 주 목표

---

## 진행 내용

### 토큰 아키텍처 및 네이밍 컨벤션 정의 (side#219)

레퍼런스 디자인 시스템(Toss tds, Atlassian, Adobe Spectrum, shadcn, Radix) 비교 분석 후 계층 구조 확정:

- 2계층 구조 채택: Primitive / Semantic
- 네이밍: JSON 키는 `.` 중첩 (`color.text.primary`), CSS 변수명은 하이픈 (`--side-color-text-primary`)
- Semantic 원칙: Primitive alias만 참조, 원시값 직접 사용 금지
- 다크 모드가 기본값: `:root`에 dark 값을 기본 주입, `[data-mode="light"]`로 라이트 모드 전환
- theme switching: `data-mode` (light/dark) + `data-theme` (SIPE 기수별 브랜드 색) 이중 attribute
- 의사결정 기록: `.github/decisions/token-architecture.md`, `.github/decisions/token-naming-convention.md`

### Primitive 토큰 정의 (side#220)

v1 감사 결과를 기반으로 전 영역 Primitive 토큰 정의:

| 영역 | 주요 내용 |
|------|-----------|
| Color | 기존 SIPE v1 팔레트 확장, 계열별 10단계 (50~950) |
| Spacing | 4px 베이스 10단계 (4 / 8 / 12 / 16 / 20 / 24 / 32 / 40 / 48 / 64) |
| Radius | 숫자 키 4단계 + full (2 / 4 / 8 / 12 / full) |
| Typography | font-family, size, weight, line-height, letter-spacing |

W3C DTCG JSON 포맷 준수, v1 감사에서 발견된 하드코딩 값(8px, 12px, 0.2s 등) 커버 검증 완료.

---

### Week 03

## 맡은 작업

- 시멘틱 토큰 작업 전체 설계
- 에픽 및 하위 태스크 이슈 생성 (side#217 에픽, #218~#226 하위 이슈)
- v1 토큰 사용 현황 감사 (side#218) ✅ — PR #243

---

## 진행 내용

### 시멘틱 토큰 에픽 설계 및 이슈 생성

SIPE Design System v2 토큰 시스템 구축 에픽 및 9개 하위 이슈 생성:

| 이슈 | 제목 | PR |
|------|------|----|
| side#217 (에픽) | 토큰 시스템 구축 (Primitive / Semantic 2계층 + Figma 동기화) | — |
| side#218 | v1 토큰 사용 현황 감사 | #243 ✅ |
| side#219 | 토큰 아키텍처 및 네이밍 컨벤션 정의 | #246 ✅ |
| side#220 | Primitive 토큰 정의 | #248 |
| side#221 | Semantic 토큰 초안 설계 | — |
| side#222 | Style Dictionary 빌드 파이프라인 구축 | — |
| side#223 | GitHub Actions CI 구축 | — |
| side#224 | Tokens Studio + Figma Variables 연동 | — |
| side#225 | 샘플 컴포넌트 토큰 마이그레이션 | — |
| side#226 | 문서화 및 마이그레이션 가이드 | — |

다크 모드는 이번 스코프 외이나 언제든 확장 가능한 구조로 설계.

### v1 토큰 사용 현황 감사 (side#218)

주요 발견 사항:

- **토큰 우회**: Tooltip, Avatar가 hex 직접 하드코딩
- **토큰 미사용**: color, fontSize, fontWeight, lineHeight 정의는 있으나 컴포넌트 import 거의 없음
- **누락 토큰**: spacing, border-radius, z-index, animation(duration/easing) 전무
- **불일치 누적**: `border-radius: 8px vs 12px`, `transition: 0.2s vs 0.3s vs 150ms` 등 컴포넌트마다 제각각

감사 결과를 `.github/decisions/` 에 md 파일로 기록.

---

### Week 02

## 맡은 작업

- `avatar`, `typography`, `ContentWithTitle` 컴포넌트 및 토큰 현안 파악

---

## 컴포넌트 현안 파악

### Avatar (`packages/avatar`)

기본 구조(size, shape, fallback, asChild)는 갖춰져 있음. 보완이 필요한 지점:

- `Avatar.module.css`에 `background-color: #e2e8f0`, `color: #2d3748` 하드코딩
  → 시멘틱 토큰으로 교체 필요 (`--color-bg-avatar`, `--color-text-avatar-fallback`)
- size/shape 값이 JS 함수로 인라인 style에 주입되는 구조
  → CSS Variables로 통일하면 토큰 연동이 더 자연스러움
- fallback이 이미지 실패 시 `alt` 텍스트를 그대로 노출하는 방식
  → 이니셜 처리(e.g. "이원주" → "이") 등 UX 개선 여지 있음
- Group Avatar(여러 아바타 겹침), 상태 배지(온라인/오프라인) 미구현
  → sipe.team People 섹션 등에서 필요 여부 확인 후 추가 검토

### Typography (`packages/typography`)

fontSize/fontWeight/lineHeight는 `@sipe-team/tokens`를 참조하여 비교적 깔끔하게 구현됨. 보완 지점:

- `color` prop이 `ComponentProps<'p'>`에서 상속된 HTML `color` 어트리뷰트
  → 타입이 `string`으로 너무 열려 있어, 시멘틱 토큰 키를 받도록 좁힐 필요 있음
- 기본 태그가 `<p>` 고정이라 heading(h1~h6) 등 다른 태그는 `asChild`로만 대응 가능
  → `as` prop 또는 `variant` prop(heading/body/caption 등) 추가를 검토
- 텍스트 말줄임(`truncate`), 정렬(`align`) 등 자주 쓰는 유틸리티 prop 없음
  → sipe.team 실사용 패턴 보고 최소한으로 추가 검토

### ContentWithTitle (sipe.team 로컬 → side 이관)

`sipe.team/src/components/atoms/ContentWithTitle/index.tsx`에 로컬 컴포넌트로 존재.
Activity, Recruit, People, FAQ 등 여러 페이지 섹션에서 사용 중.

현재 구현 요약:
```tsx
// Flex(column, center) + Typography(title) + children 구조
<ContentWithTitle title="제목" titleColor={color.white} titleSize={36}>
  {children}
</ContentWithTitle>
```

side 이관 시 정리할 사항:
- 현재 `titleColor` 기본값이 `color.white` (Base Token 직접 참조)
  → 시멘틱 토큰(`--color-text-section-title` 등)으로 교체
- sipe.team은 SCSS 사용, side는 CSS Modules 사용 → CSS 파일 포맷 변환 필요
- `titleSize` prop 타입이 `36 | 12 | 14 | ...` 리터럴 유니언 → `FontSize` 타입 재사용으로 정리

---

## 시멘틱 토큰 작업 계획

### 현황

`packages/tokens/src/`에는 `colors.ts`(Base Token 110개), `fonts.ts`만 존재.
`semantic.ts` 없음. 컴포넌트들이 hex 직접 참조하거나 Base Token을 직접 씀.

### 초안: 담당 컴포넌트 기준 필요 토큰 목록

| 시멘틱 토큰 | Base Token 참조 | 사용처 |
|-------------|-----------------|--------|
| `color-bg-avatar` | `gray200` (`#e4e4e7`) | Avatar fallback 배경 |
| `color-text-avatar-fallback` | `gray700` (`#3f3f46`) | Avatar fallback 텍스트 |
| `color-text-default` | `gray900` | Typography 기본 색 |
| `color-text-subtle` | `gray600` | Typography 보조 텍스트 |
| `color-text-section-title` | `white` | ContentWithTitle 제목 |
| `color-bg-surface` | `gray50` | 섹션 배경 |

### 디자이너 협업 프로세스

```
1. 개발 측 초안 작성 (위 목록 기준)
2. 디자이너(양정민님) Figma 변수명과 대조 → 이름 합의
3. packages/tokens/src/semantic.ts 작성
4. avatar, typography, ContentWithTitle에 파일럿 적용
5. 나머지 컴포넌트로 점진 확대
```

### Week 01

## 들어가기 전

미션 방향에 대해서 개인적인 의견을 아래와 같이 작성해 보았는데요.
제 시간에 회의에 참여하지 못하는 상황이라 또 다른 의견이 있다면 언제든 열려 있고 따를 의향 있습니다! 논의 잘 부탁드립니다.

### 레포지토리 현황 요약

### 기술 스택

| 영역 | 도구 |
|------|------|
| 패키지 관리 | pnpm 9.7 + workspaces (catalog로 버전 중앙관리) |
| 빌드 | tsup 8 (ESM + CJS 이중 출력) |
| 스타일 | CSS Modules + CSS Variables |
| 타입 | TypeScript strictest |
| 테스트 | Vitest + Testing Library |
| 문서 | Storybook 8 + Docusaurus 3 |
| 릴리즈 | Changesets (main 푸시 → 자동 버전 PR) |

### 현재 패키지 구성

```
packages/
├── tokens/          ← 색상 팔레트 + 폰트 스케일
├── reset/           ← CSS 정규화
├── typography/      ← 텍스트 컴포넌트
├── icon/
├── button, input, checkbox, radio, switch,
│   avatar, badge, card, divider, skeleton, tooltip
├── flex, grid       ← 레이아웃
├── side/            ← 전체 통합 메타 패키지
└── plugin-figma-codegen/  ← Figma 코드젠 플러그인
```

### 눈에 띄는 현황

- 시멘틱 토큰 부재: `colors.ts`에 팔레트(Base Token)만 있고, 의미 기반 토큰(Semantic Token) 레이어가 부재. (예: Button에서 `'#00ffff'` 하드코딩 사용 중)
- 다크모드 미지원: CSS 전체에 `prefers-color-scheme`, `[data-theme]` 관련 코드 부재.
- Docusaurus 문서 미완성: 컴포넌트 11개 중 avatar 1개만 문서화됨.
- tsup 유지보수 중단: 후계자인 tsdown으로 마이그레이션 여지 있음.
- Figma 코드젠 플러그인: 동작하지만 23줄로 보강의 여지 있음.

---

## 논의 안건별 의견

### 1. Headless vs Headful

Headful 유지에 한 표 던져봅니다.

Headless 전환의 문제:

- 기존 컴포넌트 11개를 모두 재작성해야 함
- DS를 사용하는 팀도 스타일을 처음부터 직접 구현해야 함
- 직장인 동아리 특성상 사용자 측도 퀵하게 사용하고자 하는 니즈가 예상됨

커스터마이징 문제는 Composition 패턴 사용으로 풀어볼 수 있을 것 같아요. 현재 side는 이미 `asChild` / `Slot` 패턴이 구현되어 있고, CSS Variables 기반 구조도 갖춰져 있어서 Headless 전환 없이도 커스터마이징 자유도를 높일 수 있을 것 같습니다.

---

### 2. Radix / shadcn 기반 전환

shadcn 모델 전환은 반대, Radix 활용 확대는 찬성합니다.

shadcn 모델의 문제:

shadcn은 npm 패키지 설치가 아니라 소스코드를 내 프로젝트에 복사해오는 방식이라 각 팀이 복사한 소스를 자유롭게 수정하면 SIPE 브랜드 일관성을 보장하기 어려울 것 같다는 생각입니다.

```
현재 side: npm install @sipe-team/button → 버전 관리, 브랜드 강제 가능
shadcn 방식: 소스 복사 → 각 팀이 마음대로 수정 → 브랜드 분기
```

DS의 핵심 가치인 일관성이 약해질 수 있어요.

Radix 활용 확대는 좋은 방향인 것 같아요. 현재는 `@radix-ui/react-slot` 하나만 사용 중인데, 접근성 구현이 복잡한 컴포넌트(Select, Dialog, Popover, Tooltip 등)를 추가할 때 Radix Primitive를 베이스로 활용하면 a11y 구현 부담을 많이 줄일 수 있을 것 같습니다.

---

### 3. 디자인 토큰 & Figma 연동

시멘틱 토큰 도입이 먼저인 것 같아요, 자동화는 그다음.

현재 문제:

`colors.ts`에 팔레트 110개가 있지만 "이 색이 어디에 쓰이는지"를 코드에서 알 수 없어요. Button은 토큰을 쓰지 않고 `'#00ffff'`를 하드코딩하고 있고요.

시멘틱 토큰 도입 효과:

| 상황 | 지금 | 도입 후 |
|------|------|---------|
| 브랜드 컬러 변경 | 컴포넌트 전체 수색 | 토큰 1줄 수정 |
| 신규 컴포넌트 개발 | 110개 팔레트에서 직접 선택 | 시멘틱 목록에서 선택 |
| 다크모드 (미래) | 전체 컴포넌트 재작성 | 테마 파일만 교체 |
| Figma 협업 | hex 값으로 소통 | 이름으로 소통 |

제안하는 토큰 레이어:

```
Base Token (현재 colors.ts)
    gray50, cyan300, red500 ...

Semantic Token (추가 필요)
    color-primary        → color.cyan300
    color-text-default   → color.gray900
    color-danger         → color.red500
```

Figma 연동 방식:

Style Dictionary 기반 완전 자동화는 세팅 비용이 꽤 큰 것 같아요. 6주 안에 현실적으로 할 수 있는 것은:

1. Figma 토큰 이름과 코드 토큰 이름 합의 (디자이너와 협업 필수)
2. `packages/tokens/src/semantic.ts` 파일 추가
3. 각 컴포넌트가 하드코딩 대신 시멘틱 토큰 참조하도록 교체

---

## 6주 미션 팀 구성 제안

총 6명을 3개 트랙으로 나눠 병렬 진행하기

### 트랙 구성

| 트랙 | 인원 | 담당 |
|------|------|------|
| A. 컴포넌트 추가 | 2명 (디자이너 필요) | 신규 컴포넌트 구현 + Storybook |
| B. 문서 작성 | 2명 | Docusaurus 문서 + 기존 컴포넌트 문서 보강 |
| C. 시멘틱 토큰 | 2명 (디자이너 필요) | 토큰 설계 + 다크모드 기반 (2차 병행) |

### 트랙별 주의사항

A ↔ B 의존성 주의
현재 문서가 없는 상태라 B 트랙은 기존 컴포넌트 11개 문서 작성 + 문서 배포 세팅만으로도 초반이 충분히 바빠서 병목은 없을 것 같아요.

C 트랙 디자이너 리소스 병목
디자이너가 1명뿐이라 A 트랙과 C 트랙에 동시 투입이 어렵다. C 트랙의 핵심인 시멘틱 토큰 이름 합의는 디자이너가 반드시 참여해야 하므로, 아래와 같이 범위를 나눈다.

```
1차 미션 (6주) — C 트랙 범위
  - Figma 토큰 이름 합의
  - semantic.ts 구조 설계 및 파일 추가
  - 기존 컴포넌트 일부에 시멘틱 토큰 적용 (파일럿)

2차 미션 병행 — C 트랙 이월 범위
  - 전체 컴포넌트 시멘틱 토큰 적용
  - 다크모드 구현
  - Style Dictionary 자동화 검토
```

## 우선순위 요약

```
🟢 1차 미션 필수
  - 신규 컴포넌트 추가 (Radix 활용, Compound Pattern)
  - 기존 컴포넌트 Docusaurus 문서 완성
  - 시멘틱 토큰 구조 설계 + 파일럿 적용

🟡 1차 미션 여유 있으면
  - tsup → tsdown 마이그레이션

🔴 2차 미션 이월
  - 전체 컴포넌트 시멘틱 토큰 적용
  - 다크모드 구현
  - Style Dictionary 자동화
```
