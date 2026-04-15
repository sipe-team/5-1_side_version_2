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

### Week 02

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