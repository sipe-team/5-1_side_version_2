#  side ver.2 Week 03 0409 컴포넌트 팀 회의

## 회의 아젠다

- [ ] 컴포넌트 팀이 해야할 일 정리
- [ ] 컴포넌트 팀원 내부에서 역할 분담
- [ ] 컴포넌트 개발 프로세스 어떻게 가져갈 것 인지? 협업 프로세스 정리
- [ ] 컴포넌트 개발 일정 협의 (1차 스프린트 / 그 이후)

## 1. 컴포넌트 팀이 해야할 일

### 지난 주 회의 내용으로 정리한 것
- [ ] 컴포넌트 구현
- [ ] sipe 홈페이지 마이그레이션
- [ ] 토큰 설계 디자인 요청
- [ ] 버전 업데이트 (side/sipe 둘다)
- [ ] 스타일링

### 이번 회의로 확정된 해야할 일
- [ ] 토큰 설계 디자인 요청 
- [ ] Sipe 홈페이지에 현재 적용된 컴포넌트 목록 확인
- [ ] Sipe 홈페이지에 추가로 적용되어야할 필요한 컴포넌트 목록 확인
- [ ] 컴포넌트 리스트(현재 및 TODO 포함하여) 정리
- [ ] 컴포넌트 별로 담당자 지정 및 업무 배분
- [ ] 개발 프로세스 논의
- [ ] 각자 컴포넌트 완성 -> 실제 사이프 홈페이지에 적용해보면서 테스트 
- [ ] 모든 컴포넌트 개발 완성 -> 배포 -> 최신 버전 업데이트 -> 실제 마이그레이션 작업 진행

## 2. 컴포넌트 정리

### 1) 현재 sipe 홈페이지에서 sipe 사용중인 컴포넌트 목록
- Button
- Flex
- Typography
- color (컬러)

### 2) sipe 홈페이지에서 그냥 개발해서 사용중인 컴포넌트 목록 (side로 바꾸어야할)
#### Atom/Primitive 레벨(DS 후보)

- **ExternalLink** (`src/components/atoms/ExternalLink`)
- **Image** (`src/components/molecules/Image`)
- **Badge** (`src/components/atoms/Badge`)
- **CardWrapper** (`src/components/atoms/CardWrapper`)
- **HamburgerButton** (`src/components/global/HamburgerButton`)
- **Toast** (현재 `react-simple-toasts` 사용: `src/hook/useCopyToClipboard.ts`)

#### 레이아웃/컨테이너(DS 후보)

- **Layout** (`src/components/atoms/Layout`)
- **ContentWithTitle** (`src/components/atoms/ContentWithTitle`)
- **DraggableContainer** (`src/components/atoms/DraggableContainer`)

#### 복합 UI(DS 후보)

- **Accordion** (`src/components/molecules/accordion/Accordion`)
- **AccordionItem** (`src/components/molecules/accordion/AccordionItem`)
- **Table** (`src/components/molecules/Table`)
- **GlowArea** (`src/components/molecules/GlowArea`)
- **Chip** (현재 `src/components/molecules/Button`의 `buttonType="chip"`로 구현)
- **Tabs** (현재 `src/components/molecules/Button`의 `buttonType="chip"` 사용 패턴으로 구현)
- **Carousel** (현재 `swiper` 기반, 구현 위치: `src/components/organisms/about/ActivitiesSection`)
- **Tooltip** (현재 `src/components/organisms/recruit/RecruitBarChart` 내부 구현)

#### 카드/리스트 패턴(DS 후보)

- **Card** (`src/components/molecules/Card`)
- **UserCard** (`src/components/organisms/people/UserCard`)
- **ActiveCard** (`src/components/organisms/activity/ActiveCard`)
- **ActiveVideoCard** (`src/components/organisms/activity/ActiveVideoCard`)

#### 데이터 시각화(DS 후보)

- **RecruitBarChart** (`src/components/organisms/recruit/RecruitBarChart`)
- **ExperienceChart** (`src/components/organisms/recruit/ExperienceChart`)
- **CompanyChart** (`src/components/organisms/recruit/CompanyChart`)
- **JobRoleChart** (`src/components/organisms/recruit/JobRoleChart`)

### 3) side에 이미 있는데도(side 컴포넌트 미사용) 그냥 구현/대체해서 쓰고 있는 것들
- **Button**
  - 대체 구현: `src/components/molecules/Button` (menu/apply/chip 용도)
  - side는 `@sipe-team/side`의 `Button`이 있는데, 네비/칩/탭 등은 커스텀 버튼을 사용 중

- **Badge**
  - 대체 구현: `src/components/atoms/Badge`

- **Card**
  - 대체 구현:
    - `src/components/atoms/CardWrapper`
    - `src/components/molecules/Card`
    - `src/components/organisms/activity/ActiveCard`
    - `src/components/organisms/activity/ActiveVideoCard`
    - `src/components/organisms/people/UserCard` (내부적으로 카드 레이아웃)

- **Tooltip**
  - 대체 구현: `src/components/organisms/recruit/RecruitBarChart` 내부에 hover tooltip 직접 구현

- **Icon**
  - 대체 사용: DS `Icon` 컴포넌트 대신 `src/libs/assets/icons`, `src/libs/assets/logos`의 SVG 컴포넌트들을 직접 사용

- **Avatar**
  - 대체 사용: DS `Avatar` 대신 프로필을 `src/components/molecules/Image`(+ 일부 `next/image`)로 직접 렌더링 (`UserCard`, `ActiveCard` 등에서 사용)

