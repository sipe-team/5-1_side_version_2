# froggy1014

## 소개
- 이름: Evan
- 역할: 인프라 / 빌드 환경 / DX
- 브랜치: `dev/side-v2`

---

## 활동 기록

### Week 01

#### 1. 기존 side 분석

##### 1-1. 빌드 환경

- 패키지 빌드: `tsup` (ESM + CJS 동시 출력)
- 스타일 컴파일: `vanilla-extract` — CSS-in-TS로 빌드 타임에 정적 CSS 생성
- 테스트: `vitest` + `@testing-library/react` (happy-dom 환경)
- Storybook: 루트 설정 없이 각 패키지별 `.storybook/` 분산 관리 → PR마다 설정 조금씩 달라지는 문제 존재

##### 1-2. 패키지 구조

- `packages/*` 하위에 컴포넌트 단위로 패키지 분리
- `packages/tokens` → vanilla-extract contract vars로 디자인 토큰 관리
- `packages/theme` → `ThemeProvider`로 런타임 테마 전환
- 각 패키지별 `package.json` 스크립트가 제각각이라 일관성이 없음
  - `build`, `typecheck`, `test:coverage` 누락된 패키지 다수 존재

##### 1-3. CI/CD 상태

- GitHub Actions 기반 / Changesets로 버전 관리
- `pnpm --filter` 기반으로 변경된 패키지만 CI 실행
- Storybook은 별도 워크플로우로 배포 (`storybook.side.sipe.team`)
- 일부 `pnpm filter` 명령에 `--if-present` 누락으로 CI 오류 발생 이력 있음

#### 2. 방향성

v2에서 제가 기여하고 싶은 방향은 **컴포넌트를 만들기 좋은 환경**을 만드는 것입니다.

화려한 기능보다, 새 컴포넌트를 추가할 때 / PR 올릴 때 / CI 돌릴 때 마찰이 없는 구조를 먼저 잡는 게 중요하다고 생각합니다.

구체적으로 관심 있는 영역:

| 영역 | 방향 |
| --- | --- |
| Storybook | 루트 단일 설정으로 통합, 패키지별 중복 제거 |
| 패키지 스크립트 | 전 패키지 일관된 스크립트 구조 (`build`, `typecheck`, `test`, `clean`) |
| 패키지 정책 | `package.json` 필드 일관성 자동 검사 도구 도입 |
| 린트/포맷 | Biome 단일화 (ESLint 잔재 제거) |

> 컴포넌트가 아무리 잘 설계되어 있어도 개발 환경이 불편하면 기여하기 어렵습니다.  
> 특히 여러 사람이 함께 PR을 올리는 모노레포는 패키지 구조 일관성이 무너지면 나중에 자동화가 어려워집니다.

---

### Week 02

#### 진행 내용

**Storybook 설정 통합**

각 패키지마다 `.storybook/` 디렉토리가 분산되어 있어 PR마다 설정이 조금씩 달라지는 문제를 해소했습니다.  
루트 단일 설정으로 통합하고, vanilla-extract가 정상 동작하도록 각 패키지에 `vite.config.ts`를 추가했습니다.

| 항목 | 기존 (per-package) | 변경 후 (root 통합) |
| --- | --- | --- |
| `.storybook/` 위치 | 각 패키지마다 존재 | 루트 단일 설정 |
| vanilla-extract 지원 | 패키지별 설정 제각각 | `vite.config.ts`로 통일 |
| 신규 패키지 추가 시 | `.storybook/` 직접 작성 필요 | `vite.config.ts`만 추가 |
| 설정 파일 총수 | 패키지 수 × n개 | 루트 1개 + 패키지당 1개 |

**패키지 스크립트 정규화**

`build`, `typecheck`, `test:coverage` 스크립트가 일부 패키지에 누락되어 있었고, 순서도 패키지마다 달랐습니다.  
루트에도 동일한 스크립트를 추가하고 전체적으로 순서를 맞췄습니다.

**불필요한 패키지 / 선언 제거**

- `plugin-figma-codegen` 패키지 제거 (미사용 상태로 방치)
- 미사용 ESLint 타입 선언 제거

> chore 작업이라 눈에 잘 안 띄지만, 스크립트 순서나 설정 파일이 제각각이면 나중에 자동화 도구를 붙일 때 노이즈가 생깁니다.  
> 이번에 한 번에 정리해서 이후 패키지 정책 자동 검사 도구 도입을 위한 기반을 만들었습니다.

#### PR 링크

- [chore: add vite.config.ts to packages for vanilla-extract support](https://github.com/sipe-team/side/pull/248)
- [chore: remove unused eslint type declarations #249](https://github.com/sipe-team/side/pull/249)
- [chore: remove per-package .storybook configs #250](https://github.com/sipe-team/side/pull/250)
- [chore: remove plugin-figma-codegen package #251](https://github.com/sipe-team/side/pull/251)
