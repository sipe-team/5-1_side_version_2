# minjee kim

## 소개
- 이름: 김민지
- 역할: 미정
- 브랜치: `docs/minjeekim`

---

## 활동 기록

### Week 01

#### 1. 기존 SIDE 분석

##### 1-1. 기술 스택 및 개발 환경

- 주요 기술 스택 : React 18 + TypeScript
- 패키지 매니저 : pnpm
- 패키지 빌드: tsup
- 테스트: Vitest
- 포맷/린트: Biome + ESLint
- 문서화 : storybook + docusaurus

> docusaurus의 경우 github actions에 별도 배포 X
> 로컬 실행 기준 설치 가이드, Avator 컴포넌트 섹션만 있는 상태)

##### 1-2. 토큰

- 위치 : `packages/tokens/src/`
- 선언된 토큰 종류 : color, font

> Input은 `constants/`에 color·spacing·typhography를 분리
> Checkbox는 `size`, `checkStyle` 등 별도 상수 파일 사용

##### 1-3. 구현된 컴포넌트

- side 번들에 포함 컴포넌트 (`index.ts` 기준) : Badge, Button, Card, Divider, Input, Radio, Skeleton, Switch, Tokens, Tooltip, Typography, Flex
- 번들 미포함 컴포넌트 : Avatar, Checkbox, Grid, Icon

#### 2. 분석 결과 해야할 일

- CSS 하드코딩 : 직접적으로 색상 값을 하드코딩으로 설정해둔 곳이 있어서 해당 부분에 대해서 규칙을 정하거나 token으로 정의해서 token에서만 가져오게 해야하지 않을까 생각합니다.
- 토큰 단일화 : Input의 경우 `constants/`로 typography가 정의되어 있어서 이 부분도 통일성을 위해 수정이 필요할 것 같습니다.
- 문서화 : 현재 문서 세팅에 대해서 설치 가이드와 Avatar에 대한 샘플 작업만 되어 있어서 문서화가 필요할 것 같습니다.

#### 3. 다른 디자인 시스템 레퍼런스

##### 3-1. 구름 오픈소스 디자인 시스템 (Vapor UI)

> 공식 문서 : https://vapor-ui.goorm.io/
> Github : https://github.com/goorm-dev/vapor-ui
> Figma : https://www.figma.com/community/file/1508829832204351721
> 발표 PPT (SSOT 분리 외) : figma.com/slides/C1RsX4c8fGM3pzVyfLxeMY/Q2-25-Maker-Collective-Seoul--for-goorm-?node-id=360-1750&t=MqRJJ88ACpPpdaKQ-1&fuid=1199160618206770857

- 토큰 : 간격 등은 이름 있는 스케일(000, 025, 050 …)로 정의하고, Vanilla Extract에서 CSS 변수(scaleFactorVar)와 곱해 반응형/스케일 조정 여지를 둠
- 스타일링 : vanilla extract를 사용하고 있으나 실제 디자인 시스템을 사용할 때는 tailwind도 사용 가능하게 처리되어있음

> 실제 사용해봤을 때 경험
> figma와 code가 잘 연결되어있는 느낌
> Text 사용할 때는 헤맸던 기억이 납니다
> 시각적 대비 관련해서 까다롭게 잡혀 있는 느낌이어서 색상 커스텀할 때 어려웠어요.

##### 3-2. 채널톡 오픈소스 디자인 시스템 (bezier-react)

> Github : https://github.com/channel-io/bezier-react
> chromatic : https://main--62bead1508281287d3c94d25.chromatic.com/
> figma : https://www.figma.com/design/L6PRKsJuNrNIQdf2I3lOSs/Apps--%E2%80%94-Buzzvil-Design-System--Community-?node-id=4870-19731&p=f&t=9PyV4Gnp1CgYLjMh-0

#### 4. 의견

- 새로운 도메인 URL : `side.sipe.team`
	- 이유 : side라는 디자인 시스템 이름과 sipe가 직관적으로 보이면 좋지 않을까 해서...

- 희망 역할 : 제가 개발 경험도, 디자인 시스템 경험도 많지 않아서 어렵네요.. 오픈소스 기여 경험은 문서 기여 경험이 미약하게나마 있긴 합니다.

- 기술 방향 : 6주보다 더 걸릴 수 있는 미션이지만 SIPE 홈페이지를 포함해서 SIPE에서 만들 서비스에서도 반영될 수 있도록 작업되면 좋을 것 같습니다.

- 미션 관련 디파짓 : 이미 토요일 11:59까지인걸 잊고 늦게 올린 지각자는 말이 없습니다. 따르겠습니다.

---

### Week 02~03

> 💡요약
>
> - 담당 컴포넌트 : image / input / checkbox / radio / switch
> - 작업 우선순위 : image > input > checkbox, radio, switch

## image

- 사용 현황
    - sipe.team : 사용
        - 공통 래퍼 : **`@/components/molecules/Image`**
            - activity 페이지
                - ActiveCard : 썸네일 `fill` + `objectFit="cover"`, 프로필 고정 `32×32`
                - ActiveVideoCard : 썸네일 `fill` + `cover`
                (태블릿 사이즈 이하 너비가 잘리는 문제 발생)
            - about 페이지
                - swiper 슬라이드 이미지
            - people 페이지
                - UserCard 프로필 이미지
        - next/image 직접 사용 : 홈페이지 히어로 / 스폰서 이미지
    - figma : 없음
    - side 디자인 시스템 : 없음
- 로컬 구현 코드 분석
    - 현재 `sipe.team`의 이미지 정책은 `molecules/Image` 중심이나, 일부는 `next/image` 직접 사용으로 이원화됨
    - `fill` 사용 시 부모 레이아웃 의존도가 높아, 이미지 API와 레이아웃 책임 분리 필요
    - fallback / alt / sizes 등의 기본 정책은 있으나, 페이지/컴포넌트별 일관 규칙 문서화 필요
- 결정 필요한 사항
    - people 페이지 내 UserCard 프로필 이미지는 Avatar가 아니라 Image인데 Avatar로 변경할 필요가 없는지
    - 현재 구현은 next/image로 되어있는데 프레임워크 중립 img 기반으로 갈지 결정 필요
    - 현재 호출할 때마다 숫자 하드코딩(`height={220}` 등)으로 관리 중인데 토큰화 또는 variant로 정리할 필요는 없는지
- 개선 방향
    - 현재 로컬 이미지 컴포넌트에서만 활용하던 스타일을 디자인 시스템 컴포넌트로 전환
        - 범용(`img`) vs Next 전용 래퍼 분리
    - 썸네일 레이아웃(태블릿 브레이크포인트)과 이미지 정책(cover vs contain, 고정 비율)을 스펙으로 고정 후 컴포넌트/스타일 반영
- 요청사항
    - figma: 이미지 플레이스홀더, 카드 썸네일 비율, 크롭 규칙(cover 기준점), 브레이크포인트별 최대 너비 등 스펙 정의 요청

## Input

- 사용 현황
    - sipe.team : 미사용 (form 입력 UI 없음)
        - 향후 활용 가능성 : `joing us` 페이지 / `activity` 내 자료 검색
    - figma : 없음
- 디자인시스템 코드 분석
    - 로컬 토큰 상수 활용 중 (`constants/color.ts | spacing.ts | typography.ts`)
- 결정 필요한 사항
    - type=number 지원 필요 여부 (현재 지원 type에서 제외됨)
        
        > Input이 text 계열만 지원한다면, TextInput 네이밍도 대안
        ****(레퍼런스 : https://vapor-ui.goorm.io/docs/components/text-input)
        > 
    - textarea는 별도 컴포넌트로 만들 필요가 없는지
        
        > 당장은 사용처가 없어 필요없지만 추후 고도화 시 고려 필요 예상
        > 
- 개선 방향
    - 토큰 정렬 : 디자인 시스템 내 토큰으로 수정
- 요청 사항
    - 현재 figma 작업 페이지 내 정의가 없어 관련 스펙 정의 요청 (size, state, type 등)

## checkbox

- 사용 현황
    - sipe.team : 미사용 (form 입력 UI 없음 / side export에도 없음)
        - 향후 활용 가능성 : `joing us` 페이지
    - figma : 없음
- 디자인시스템 코드 분석
    - 로컬 토큰 상수 활용 중 (`constants/checkStyle.ts | size.ts`)
- 결정 필요한 사항
    - state 상태 스펙 포함 범위
- 개선 방향
    - 배포 추가 : side에 의존성 및 export 추가해 import 경로 통일
    - 토큰 정렬 : 로컬 checkStyle이 아닌 디자인 시스템 내 토큰으로 수정
- 요청 사항
    - 현재 figma 작업 페이지 내 정의가 없어 관련 스펙 정의 요청 (size, state, type 등)

## radio

- 사용 현황
    - sipe.team : 미사용 (form 입력 UI 없음)
        - 향후 활용 가능성 : `joing us` 페이지
    - figma : 없음
- 디자인시스템 코드 분석
    - 컴포넌트 내부적으로 sizeMap에 값을 하드코딩해서 사용 중
    - 색상/상태(checked, focus, invalid 등)에 대해 별도 설정 없이 네이티브 표현에 의존 중
- 결정 필요한 사항
    - 네이티브 기본 표현을 유지할지, 토큰 기반으로 설정할 것인지 논의 필요
- 개선 방향
    - 토큰 정렬 : 디자인 시스템 내 토큰으로 수정
- 요청 사항
    - 현재 figma 작업 페이지 내 정의가 없어 관련 스펙 정의 요청 (size, state 등)

## switch

- 현황
    - sipe.team : 미사용
        - 향후 활용 가능성 : `joing us` 페이지 / 페이지 내 다크모드 추가 시
    - figma : 없음
- 디자인시스템 코드 분석
    - 크기: `sm | md | lg`를 `constants/size.ts`에서 수치로 정의
    - 색상: `@sipe-team/tokens` 활용중
    - 현재 트랙 안에 썸의 위치가 어색해보이는 문제가 있어 코드 수정이 필요할 것으로 예상
- 개선 방향
    - 트랙 내 썸 위치 조정
- 요청 사항
    - 현재 figma 작업 페이지 내 정의가 없어 관련 스펙 정의 요청 (size, state, type 등)


### Week 04 - 05 

> 요약
> 이미지 컴포넌트 관련 레퍼런스 디자인 시스템 분석한 내용을 기반으로 기본 구현 방향(에러, 로딩 처리 방식) 관련 초기 작업을 진행 중입니다.
> 다음 회의 전까지 초안 컴포넌트 완성 + 논의 지점 정리해서 공유 예정입니다.

#### 레퍼런스 디자인 시스템 분석

Next.js 기반 서비스 특성상 Next.js Image 대응이 가능한 상용 디자인 시스템(Mantine, Chakra UI)을 우선 검토했습니다.
다만 로딩, 에러 처리 부분은 두 시스템만으로는 아쉬운 점이 있어 추가 레퍼런스를 더 살펴볼 예정입니다.

- 공통 : alt 필수 prop 강제
- 차이점 : fallback 처리 방식 (Mantine : fallback용 img를 별도 렌더링 / Chakra UI는 fallbackSrc로 URL을 받아 처리)

#### 기본 구현 방향
- component prop 패턴 채택 (img 기본, Next.js Image 주입 가능) : asChild + Slot 방식의 타입 문제(@ts-expect-error 억제 필요)를 피하기 위한 선택
  => 추후 변동 가능성 있음
- 에러 처리: 같은 DOM에서 resolvedSrc만 갱신하는 방식으로 구현
  (Mantine은 에러 시 fallback용 img를 별도로 렌더링하는 방식인데, 우리는 동일 DOM에서 resolvedSrc만 갱신하는 방식으로 단순화)
- 로딩 처리: Skeleton 조합 방식 채택 (Mantine, Chakra UI 동일 패턴)
- fit prop: cover / contain / fill 지원, 기본값 cover
- radius: 현재 none 고정, 추후 디자인 토큰 반영 예정

#### 다음주 할 일
- 로딩, 에러 처리 관련 레퍼런스를 추가로 검토
- 초안 컴포넌트 완성
- 논의가 필요한 지점은 PR로 따로 정리해서 팀원분들의 의견을 듣고 싶음

#### 2026.04.29 컴포넌트팀 발표 준비 회의록

- 프로젝트 주요 목표 및 방향성
  - SIDE 디자인시스템을 분석하고 범용적인 디자인 시스템 구축하자
  - 1차 목표 : 디자인시스템 분석, SIPE 홈페이지에 컴포넌트 적용
  - 목표 달성을 위한 구체적인 액션 : 홈페이지 기준 활용성 높은 컴포넌트 위주로 우선순위 설정 후 작업

- 주차별 작업
  - 1-2주차
    - 기존 SIDE 시스템 및 상용 디자인시스템 분석
    - 컴포넌트 설계 방식 논의 구체화 (headless vs headful)
    - SIDE ver2 방향성 및 MVP 범위 및 담당 컴포넌트 배정
  - 3-4주차
    - 담당 컴포넌트 분석 및 작업 계획 설정 -> 개별 작업
    - 모각작
    - 팀원 PR 리뷰
  - 5-6주차
    - 컴포넌트 본격 작업
    - 테스트코드 컨벤션 확정
    - 버그 픽스, 코드 퀄리티 및 테스트 코드 개선
    - 고도화한 컴포넌트 SIPE 반영

### Week 06 과제 - 발표 준비

#### 개인 느낀점 (100자 이내)

동료들의 작업 방식을 보고 피드백을 받으며, 혼자서는 생각 못했을 것들을 해보고 배울 수 있었습니다. 처음이라 어려웠지만 함께할 수 있어서 영광이었습니다. SIPE 최고!

#### 담당 컴포넌트 작업 내용 정리

> 요약
> as-is: SIDE DS에는 이미지 컴포넌트가 없었음. SIPE는 next/image 래퍼 컴포넌트를 정의해서 사용 중이었으나 사용처마다 에러/로딩 처리가 달랐음.
> to-be: img 기반 신규 컴포넌트 패키지 추가, Ant Design 등 디자인 시스템을 레퍼런스로 로딩·정상·대체·에러 네 가지 상태로 구분. fallbackSrc, placeholder로 동작 통일
> 남은 과업 : SIPE 홈페이지에 디자인시스템 컴포넌트 도입. 고도화

- 담당 컴포넌트 : image, input, checkbox, radio, switch
- 작업 방향성 : SIPE 홈페이지에 직접적으로 사용하고 있는 컴포넌트를 우선적으로 작업하기 위해 image부터 작업 결정
- `as-is`
  - SIDE 디자인시스템 : 이미지 컴포넌트 부재
  - SIPE 홈페이지
    - 8곳에서 이미지 요소 사용 중 (히어로 배경 이미지, Avatar 컴포넌트 대체 2곳 제외 5곳을 적용 범위로 설정)
    - 내부적으로 이미지 컴포넌트 선언 후 사용 중 : next/image 위에 얇게 올린 wrapper 형태
    - 문제 지점 : 사용처마다 에러 처리 방식이 다름 / 로딩 처리 방식 부재
- 과정
  - 상용 디자인 시스템(Ant Design, Next.js, Mantine, Chakra)을 레퍼런스로 스펙 구성
- `to-be`
  - 패키지로 Image 컴포넌트 신규 추가
  - img 기반 프레임워크 중립적 코어로 서비스 프레임워크 상관없이 동일한 동작과 API를 쓰게 함
  - 4가지 상태(loading, normal, fallback, error) 기반으로 로딩/성공/에러 상황 구분
  - fallbackSrc, 최종 실패 시 visibility: hidden으로 영역만 남기고 숨길 수 있도록 함
- 남은 과업
  - SIPE 홈페이지 도입
  - next/image 연동 전략 고민
  - 고도화 : 기본 에러 UI, 디자인 토큰 및 프리셋 등, src 변경 시 상태 리셋 등

### Week 06 - 작업 보고

> 요약
> 이미지 컴포넌트 초안 PR을 올렸습니다.
> - [PR 링크](https://github.com/sipe-team/side/pull/262)

#### 구현 완료 내용

- `useImageStatus` 훅 분리 : 에러/대체 이미지 상태 관리 (loading / normal / fallback / error 4단계)
  - fallback 1회 전환 + 무한루프 방지 (`hasTriedFallback` 플래그) — 레퍼런스에 없는 방식으로 직접 추가
  - `onError` 콜백 호출 보장
- Props : `src`, `alt` 필수 / `width`, `height`, `fit`, `fill`, `loading`(기본값 lazy), `placeholder`, `fallbackSrc` 지원
- 테스트 8개 케이스 작성
- Storybook 추가 (Basic / Fallbacks / WithPlaceholder / WithFill / Fits / ResponsiveWidth)
- 내부적으로 에러/로딩 상태를 관리하다 보니 next/image의 자체 처리와 충돌 가능성이 있어 이전 구현 방식에서 변경 있음
  (4주차에서 검토했던 component prop 방식은 next/image 자체 처리와의 충돌 가능성으로 이번 구현에서 제외, 대응 방식은 팀 논의 후 결정 예정)

#### 레퍼런스 추가 분석

4주차에서 Mantine, Chakra UI를 검토했고, 에러 처리와 로딩 처리 관련해서 추가 레퍼런스(Ant Design, Next.js)를 살펴봤습니다.

- 상태 4단계 분리 구조 : Ant Design 참고
- 에러 시 `visibility: hidden`으로 공간 유지 : Ant Design 참고
- `fallbackSrc` prop 제공 + `onError` 항상 호출 : Ant Design + Next.js 참고

#### 다음 주 할 일
- PR 피드백 반영 및 세부 작업 진행
- 토큰/radius 처리 방향 논의 (현재는 사용자가 직접 값을 입력하는 방식으로 열어둔 상태)

#### 팀과 논의하고 싶은 것
- 에러 표시 정책 : 현재 `visibility: hidden`으로 공간 유지 중. 기본 fallback UI를 내장하는 방향으로 가면 에러 상태가 더 명확해질 것 같아 디자인팀과 UI 형태 논의 후 추가하는 방향 제안
- next/image 대응 방식 : 단일 패키지 대응 vs `@sipe-team/image-next` 분리
- `aspectRatio` prop : 현재 서비스에서 비율 처리 케이스가 적어 제외했으나, 필요 시 추가하는 방향 고려