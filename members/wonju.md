# wonju

> 이름: 이원주
> 역할: 미정 
> 브랜치: `docs/wonju`

---

## 활동 기록

### Week 01
----

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
