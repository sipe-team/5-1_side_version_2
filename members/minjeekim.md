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
- 진행 내용:
- 회의 내용 반영:
- PR 링크:
