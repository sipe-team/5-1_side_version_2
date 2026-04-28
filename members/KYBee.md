# KYBee

## 소개
- 이름: KYBee
- 역할: 
- 브랜치: 

---

## 활동 기록

### Week 01 ~ 02

#### 진행 내용
1. 디자인 시스템 이해  
2. `side` 분석  
3. 수행 방향 정리  

---

#### 1. 디자인 시스템 학습

##### 1-1. 디자인 시스템의 4요소

- **Tokens**: 색상, 간격, 폰트 크기 등 공통 값을 이름으로 정의한 기준
- **Components**: 버튼, 입력창, 배지 등 반복 사용되는 UI 단위
- **Patterns**: 여러 컴포넌트를 조합한 화면 구성 패턴
- **Docs (Storybook)**: 사용법, 상태, 예시를 정리한 문서
- 컴포넌트를 상태별로 확인하는 기준 환경 역할

##### 1-2. headless vs headful

- **Headless UI**: 스타일이 포함되지 않은 UI 컴포넌트
- 접근성, 상태 관리, 이벤트 처리 등 동작 로직만 제공
- HTML 구조와 스타일은 사용하는 쪽에서 직접 구현

- 특징
  - 디자인 자유도 높음
  - 서비스 요구사항에 맞게 커스터마이징 가능
  - 디자인 시스템 토큰 및 스타일과 결합하여 사용

- **Headful UI**: 스타일과 UI 형태까지 포함된 완성형 컴포넌트
- 기본 디자인과 레이아웃이 함께 제공됨

- 특징
  - 즉시 사용 가능
  - 빠른 개발에 유리
  - 전체 UI 일관성 유지에 적합
  - 커스터마이징에는 제약 존재

- **디자인 시스템 관점에서의 차이**

- Headless 방식
  - 동작 로직만 제공하고, UI는 디자인 시스템을 기반으로 직접 구성하는 방식
  - 토큰, 스타일 시스템과 조합하여 사용하는 구조

- Headful 방식
  - 디자인 시스템이 컴포넌트의 UI까지 함께 제공하는 방식
  - 정의된 스타일을 그대로 사용하는 구조


##### 1-3. 디자인 시스템 필요성

- UI 중복 개발 방지
- 서비스 전반 일관성 유지
- 변경 시 전체 반영 가능
- 디자이너-개발자 간 공통 규칙 공유

> 백엔드 관점에서 lib 을 만들어 배포하는 것과 유사

##### 1-4. 테스트 비교

- **백엔드 테스트**: 입력 대비 결과 검증, 비즈니스 로직 정확성 중심

- **UI 테스트 (Storybook 기반)**: 상태별 UI 표현 검증, 인터랙션 및 화면 일관성 중심

- 백엔드 → 정답 검증
- UI → 깨짐 방지 및 일관성 유지

##### 1-5. 참고 링크

- SEED Design System: https://seed-design.io/  
- LINE Design System: https://designsystem.line.me/  
- Toss Design System: https://tossmini-docs.toss.im/tds-mobile/components/menu/  

---

#### 2. 기존 side 분석

##### 2-1. 구조

- 디자인 시스템 모노레포 구조
- 공통 UI 부품 및 스타일 규칙 관리
- `@sipe-team/side` 라이브러리 형태로 제공
- `packages/*` → 컴포넌트 및 토큰 패키지
- `packages/side` → 통합 패키지
- `www` → 문서 사이트

##### 2-2. side ↔ sipe.team 관계

- `sipe.team` → side 를 사용하는 실제 서비스
- `side` → 공통 UI 제공 역할
- `@sipe-team/side` 의존성으로 사용
- 전역 스타일 import 후 컴포넌트 사용

##### 2-3. 사용 현황

- 일부 공통 컴포넌트 사용 중
  - `Flex`, `Typography`, `color`, 일부 `Button`
- 완전한 디자인 시스템 기반 구조 아님
- 서비스 내부 별도 구현 존재
  - 별도 Button 컴포넌트
  - 로컬 SCSS 사용
  - 래퍼 컴포넌트 유지

##### 2-4. 테스트 및 개발 환경

- 컴포넌트별 `*.test.tsx`, `*.stories.tsx` 구성
- `vitest` 기반 테스트 실행

##### 2-5. 배포 구조

- GitHub Actions 기반 CI/CD
- Changesets 기반 버전 관리
- GitHub Packages 배포
- main 반영 시 자동 릴리즈

##### 2-6. 현재 상태 정리

- 디자인 시스템 일부 적용 상태
- 공통 시스템과 서비스 코드 혼재
- 책임 경계 불명확
- 완전한 소비 구조로 정착되지 않은 단계

---

#### 3. 수행 방향 정리

##### 3-1. 공통 디자인 토큰 정리
- 색상, 간격, 폰트 크기 등 자주 사용하는 값을 의미 중심으로 다시 정리해볼 수 있음.

##### 3-2. 핵심 공통 컴포넌트 정리
- `Button`, `Input`, `Badge`처럼 반복 사용되는 기본 UI를 `side` 기준으로 더 명확하게 정리해볼 수 있음.

##### 3-3. 문서와 사용 규칙 보강
- Storybook과 문서 사이트에서 컴포넌트 예시뿐 아니라 사용 기준까지 함께 정리할 수 있음.

##### 3-4. 서비스 코드 공통화 후보 정리
- 현재 `sipe.team` 내부에 있는 UI 중 반복되는 부분을 찾아 `side`로 옮길 후보를 정리해볼 수 있음.

##### 3-5. 배포 및 운영 흐름 정리
- `npm` 배포 기준으로 디자인 시스템 변경이 버전 업데이트 후 서비스에 반영되는 흐름까지 함께 정리해볼 수 있음.

---

### Week 03

#### 진행 내용
1. 컴포넌트 팀 회의에 참여함.
2. 맡은 컴포넌트의 활용 내용과 현재 상황을 분석함.

#### 맡은 컴포넌트
- flex
- grid
- divider
- ExternalLink 

#### 관련 링크
- [회의 내용](https://www.notion.so/osohyun/33de1a3a120480e3a848edd3586009c0)
- [활용 분석](https://kybee.notion.site/2026-04-11-SIDE-33f3008406748078be88c245ccb00e6e?source=copy_link)

---

### Week 04

#### 진행 내용
1. `Flex` 컴포넌트 이슈를 생성하고 현재 계약을 정리함.
2. 계약 기준으로 Storybook 옵션과 테스트 범위를 보강함.
3. 작업 내용을 PR로 정리하고 머지까지 진행함.

#### 관련 링크
- [이슈](https://github.com/sipe-team/side/issues/227)
- [PR](https://github.com/sipe-team/side/pull/241)

#### Flex 계약 정리표

| 항목 | 내용 |
| --- | --- |
| 렌더링 규칙 | 기본적으로 `div`로 렌더링되고, `asChild=true`이면 wrapper 없이 자식 element 기준으로 렌더링됨 |
| 전달 동작 | `ref`, `className`, `style`, 그 외 `div` props를 최종 렌더링 element에 전달함 |
| 지원 Props | `direction`, `align`, `justify`, `wrap`, `basis`, `grow`, `shrink`, `gap`, `inline`, `asChild` |
| 기본값 | `direction=row`, `align=normal`, `justify=normal`, `wrap=nowrap`, `inline=false` |
| 스타일 적용 방식 | `direction`, `align`, `justify`, `wrap`은 사전 정의된 값만 사용하고, `basis`, `grow`, `shrink`, `gap`은 inline style로 적용함 |
| display 규칙 | `inline=true`이면 `inline-flex`, `inline=false`이면 `flex`로 렌더링됨 |
| 추가 보장 | `style`은 기본 inline style과 병합되고, `asChild=true`이면 자식 태그를 유지한 채 flex 스타일이 적용됨 |

---

### Week 05

#### 진행 내용
1. `sipe.team` 내부의 flex 레이아웃 사용 패턴을 분석함.
2. 분석 결과를 마이그레이션 가능 범위 기준으로 유형화함.
3. 우선 적용 가능한 1유형 대상을 정리하고 `side/Flex` 기준의 전환 방향을 설정함.

#### 관련 링크
- [이슈](https://github.com/sipe-team/side/issues/253)

#### flex 마이그레이션 유형 정리

| 유형 | 내용 |
| --- | --- |
| 1 유형 | 단순 flex wrapper를 `side/Flex`로 바로 치환할 수 있는 경우 |
| 2 유형 | 이미 `Flex`를 사용하고 있지만 CSS에 남아 있는 flex 관련 선언을 함께 정리할 수 있는 경우 |
| 3 유형 | 반응형 처리나 중첩 구조로 인해 전체 치환보다는 부분 적용이 적절한 경우 |

#### Week 05 정리

- `sipe.team`에서 flex가 사용되는 위치를 먼저 분석해 일괄 치환이 아닌 유형별 접근이 필요하다고 판단함.
- 단순 wrapper 중심의 1유형을 우선 대상으로 잡아 실제 마이그레이션 범위를 줄이고 적용 가능성을 높이는 방향으로 정리함.
- 이후 테스트 보강과 실제 치환 작업을 이어갈 수 있도록 기준 이슈를 문서화함.
- Week 05 범위에서는 분석과 분류 기준 정리까지 마무리했고, 다음 주차부터는 우선순위 대상에 대한 실제 적용과 회의 논의 사항 반영으로 넘어갈 수 있는 상태를 만들었음.

---

### Week 06

#### 진행 내용
1. week-05 에서 이어진 작업 마무리
2. 오프라인 모임에서 발표 관련하여 논의 진행, 일정 정리 밑 발표 준비

#### 관련 링크
- [오프라인 논의 스레드](https://sipe-team.slack.com/archives/C0AN5S2T458/p1777387027150819?thread_ts=1777208790.103449&cid=C0AN5S2T458)
- [Week 05 작업 PR](https://github.com/sipe-team/sipe.team/pull/185)

#### 발표 내용 
- TBD