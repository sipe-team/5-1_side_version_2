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

##### 개인 느낀점 (100자)

- 현업에서 안 해봤던, 그리고 의도적으로 시도하지 않으면 해보지 못할 것을 시도했고, 결국 목표를 이뤘다. 프론트엔드 개발자분들도 정말 대단하다고 느꼈다.

##### 팀

###### page1.

- 목표
  - 현재 SIDE 컴포넌트들의 한계를 분석하고, 더 범용적인 디자인 시스템 컴포넌트를 설계한다.

###### page2.

- 주차별 방향성
  - 1~2주차
    - 기존 코드·컴포넌트 구조 분석을 기반으로 마이그레이션 전략, 협업 룰, 디자인 시스템 방향성을 정립했습니다.
  - 3~4주차
    - 담당 컴포넌트 개발과 함께 PR 리뷰 문화 도입, 컴포넌트 고도화, 협업 프로세스 정착을 진행했습니다.
  - 5~6주차
    - 공홈 반영과 버그 개선을 통해 실서비스 적용, 코드 품질 향상, 테스트 코드 및 Changesets 기반 운영 체계 구축을 완료했습니다.

###### page3 ~ 6.

- 각자 담당 컴포넌트 정리 1 페이지
- 이건 금요일(5월 1일) 18시까지 정민님께 전달 예정

###### page7.

- 결과
  - 기존에 side 에 있던 컴포넌트 중 50% 개선
- 밑에 항목은 참고용입니다.
  - 개선한 항목(7개) ; image, tooltip, flex, button, card, icon, tokens
  - 전체 항목(14개) : input, checkbox, radio, image, tooltip, badge, skeleton, flex, button, card, icon, avatar, typography, tokens

---

### Week 07

#### 진행 내용
1. Week 05에서 정리하고 진행하던 `sipe.team` flex 마이그레이션을 추가로 이어서 적용함.
2. 안정적으로 변경하기 위해 테스트를 먼저 작성하고, 마이그레이션 후 필요한 테스트만 남기는 흐름으로 작업함.
3. `RecruitmentSummary`, `Badge`, `ContactSection`의 flex wrapper를 `@sipe-team/side`의 `Flex`로 전환함.
4. 반응형 flex 처리는 현재 `side/Flex`의 계약 범위 밖에 있어 SCSS에 유지하고, 추후 개선 대상으로 분리함.

#### 관련 링크
- [sipe.team PR](https://github.com/sipe-team/sipe.team/pull/186)

#### 적용한 마이그레이션 방식

| 단계 | 내용 |
| --- | --- |
| 테스트 추가 | 기존 렌더링과 링크 동작이 깨지지 않도록 필요한 테스트를 먼저 추가 |
| 마이그레이션 | flex wrapper를 `side/Flex`로 치환 |
| 테스트 정리 | 구현 세부사항에 가까운 과한 검증은 제거하고 필요한 테스트만 유지 |
| 검증 | 테스트, lint, prettier, Vercel preview 상태 확인 |

#### 이번 주 정리
- `sipe.team`의 flex 마이그레이션을 기존 작업에 이어 추가로 진행함.
- 한 번에 넓게 바꾸기보다, 작은 단위 PR을 만드는 방식이 안전하다고 판단함.
- 현재 `side/Flex`는 정적 flex 값에는 적용 가능하지만, 반응형 props는 아직 지원하지 않으므로 별도 설계가 필요하다고 판단함.

#### 다음 작업
- 남아 있는 `sipe.team` flex 사용처 중 이관 가능한 후보를 추가로 마이그레이션할 예정임.
- 추후에는 반응형 props를 `side/Flex` 계약에도 적용하고, 디자인 토큰을 고려해 `sipe.team`의 반응형 flex 처리도 함께 이관하는 방향을 계획하고 있음.

---

### Week 08

#### 진행 내용
1. 컴포넌트 팀 회의에 참여해 이후 스프린트 목표와 진행 방향을 정리함.
2. Week 07에서 이어서 `sipe.team` flex 마이그레이션을 추가로 진행함.
3. `UserCard`와 `ActiveVideoCard`를 대상으로 테스트를 먼저 작성하고, 이후 `@sipe-team/side`의 `Flex`로 wrapper를 전환함.
4. DOM 구조와 기존 태그가 바뀌지 않도록 `asChild`를 사용하고, 반응형 처리나 커스텀 링크 컴포넌트와 연결된 스타일은 이번 범위에서 유지함.
5. 변경 내용을 하나의 브랜치와 PR 단위로 정리함.

#### 관련 링크
- [sipe.team PR](https://github.com/sipe-team/sipe.team/pull/187)

#### 컴포넌트 팀 회의 정리
- 팀 목표는 디자인 토큰을 실제 컴포넌트에 적용해보는 경험을 쌓는 것으로 정함.
- 7월에는 이미 만든 컴포넌트를 대상으로 토큰 적용을 시작하기로 함.
- 초기 적용 범위는 typo, spacing 등 단순 값부터 변수로 치환하고, 컴포넌트 내부에 로컬로 선언된 토큰을 점진적으로 교체하는 수준으로 시작하기로 함.
- 필요한 토큰이 추가로 생기면 토큰 팀에 요청하고, 장기적으로는 요청 인터페이스와 프로세스도 함께 개선하기로 함.
- 최종적으로는 우리가 만든 토큰을 실제 반영하고, 요청부터 반영까지의 워크플로우를 1회 이상 완주하는 것을 목표로 함.

#### 스프린트 계획
- Sprint 0: 화요일 회의 전까지 week log 최신화
- Sprint 1: 진행 중인 Flex 마이그레이션 1차 마무리 및 토큰 적용 대상/범위 식별
- Sprint 2: Flex 토큰 적용

#### 이번 주 정리
- 회의를 통해 팀 차원의 토큰 적용 목표와 개인 스프린트 계획을 정리함.
- Flex 마이그레이션은 기존 방식대로 테스트를 먼저 작성하고 작은 단위로 이어서 진행함.
- 반응형 처리나 커스텀 컴포넌트와 연결된 스타일은 아직 별도 판단이 필요해 이번 범위에서는 유지함.

#### 다음 작업
- Sprint 1 범위로 진행 중인 Flex 마이그레이션을 1차 마무리할 예정임.
- Flex에 토큰을 적용할 수 있는 대상과 범위를 함께 식별할 예정임.

---

### 2nd-Sprint-1

#### 진행 내용
1. `sipe.team`에 남아 있는 flex 관련 SCSS와 `@sipe-team/side`의 `Flex` 사용처를 다시 전체 스캔함.
2. 현재 `Flex` 계약으로 옮길 수 있는 정적 flex 영역과, 반응형 계약이 필요해 후속 작업으로 두는 영역을 분리함.
3. 토큰 도입 전에도 전환 가능한 후보를 추가로 마이그레이션하고, 기존 렌더링을 보존하기 위한 테스트를 함께 작성함.
4. `side`의 `Flex`가 `{ sm, md, lg }` responsive 값을 받을 수 있도록 계약을 확장하는 PR을 별도로 정리함.

#### 관련 링크
- [sipe.team Flex 마이그레이션 PR](https://github.com/sipe-team/sipe.team/pull/188)
- [side Flex responsive 계약 확장 PR](https://github.com/sipe-team/side/pull/284)

#### flex 마이그레이션 상태 정리

| 분류 | 비율 | 내용 | 예시 |
| --- | ---: | --- | --- |
| 전환 완료/거의 완료 | 약 55% | 정적 flex wrapper 중심으로 `side/Flex`를 적용한 영역 | `Badge`, `RecruitmentSummary`, `UserCard`, `ActiveVideoCard`, `ActiveCard`, `Button`, `SponsorImage`, `RecruitBarChart`, `Recruit` charts wrapper |
| 후속 전환 대상 | 약 45% | 모바일/데스크톱에 따라 `gap`, `direction`, `justify`, `align` 등이 달라 responsive Flex 계약 적용 후 다시 보는 것이 적절한 영역 | `Footer`, `Navigation`, `Table`, `Card`, `ContactSection`, `ActivitiesSection` |

#### side Flex responsive 계약 정리

| 항목 | 내용 |
| --- | --- |
| 기존 호환 | `direction="column"`, `gap="12px"`처럼 기존 단일 값 사용 방식은 유지 |
| 추가 계약 | `direction`, `align`, `justify`, `wrap`, `gap`에서 `{ sm, md, lg }` 객체 값을 추가로 지원 |
| 처리 방식 | 현재는 Flex 패키지 내부 responsive 계약으로 처리 |
| 추후 방향 | `@sipe-team/tokens`에 정식 breakpoint token이 생기면 외부 사용 방식은 유지하고 내부 구현만 token 기반으로 교체할 수 있도록 정리 |

#### 이번 주 정리
- `sipe.team`에서는 토큰 도입 전에도 옮길 수 있는 정적 flex wrapper를 추가로 전환해 마이그레이션 범위를 넓힘.
- 남은 영역은 대부분 반응형 값과 연결되어 있어, `side/Flex`의 responsive 계약이 먼저 필요하다고 판단함.
- `side`에서는 기존 단일 값 prop을 유지하면서 responsive 객체 값을 받을 수 있는 방향으로 `Flex` 계약을 확장함.

#### 다음 작업
- `side`에 이번 responsive 계약을 반영하고, 이후 `sipe.team`에서 해당 계약을 사용하도록 수정할 계획임.
