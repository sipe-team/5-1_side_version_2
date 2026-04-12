# 📌 Side v2 Week 03 컴포넌트 팀 회의 보고서  
📅 2026.04.09

---

## 1. 회의 목적

- [ ] 컴포넌트 팀의 역할 및 책임 정의
- [ ] 컴포넌트 개발 및 협업 프로세스 정립
- [ ] 디자인 시스템 적용 범위 및 우선순위 도출

---

## 2. 주요 안건

- [ ] 컴포넌트 팀 업무 범위 정의
- [ ] 팀원 역할 분담
- [ ] 개발 및 협업 프로세스 수립
- [ ] 개발 일정 (1차 스프린트 및 이후 계획)

---

## 3. 컴포넌트 팀 업무 정의

### 3.1 기존 정의된 업무
- [ ] 컴포넌트 구현
- [ ] sipe 홈페이지 마이그레이션
- [ ] 디자인 토큰 설계 요청
- [ ] 버전 업데이트 (side / sipe)
- [ ] 스타일링

---

### 3.2 이번 회의에서 확정된 업무

#### 📌 분석 및 정의 단계
- 토큰 설계 디자인 요청
- sipe 홈페이지 현재 컴포넌트 사용 현황 분석
- 추가 필요 컴포넌트 정의
- 전체 컴포넌트 리스트 정리 (현재 + TODO)

#### 📌 실행 단계
- 컴포넌트별 담당자 지정
- 개발 프로세스 수립

#### 📌 검증 및 적용 단계
- 컴포넌트 개발 후 실제 서비스 적용 테스트
- 전체 컴포넌트 개발 완료 후 배포
- 최신 버전 업데이트 및 마이그레이션 진행

---

## 4. 컴포넌트 현황

### 4.1 현재 디자인 시스템 사용 컴포넌트
- Button
- Flex
- Typography
- Color

---

### 4.2 디자인 시스템 전환 대상 (커스텀 구현)

#### 🧱 Atom / Primitive
- ExternalLink
- Image
- Badge
- CardWrapper
- HamburgerButton
- Toast

---

#### 📐 Layout / Container
- Layout
- ContentWithTitle
- DraggableContainer

---

#### 🧩 Composite UI
- Accordion / AccordionItem
- Table
- GlowArea
- Chip (Button 기반)
- Tabs (Button 기반)
- Carousel (swiper 기반)
- Tooltip (직접 구현)

---

#### 🃏 Card / List 패턴
- Card
- UserCard
- ActiveCard
- ActiveVideoCard

---

#### 📊 데이터 시각화
- RecruitBarChart
- ExperienceChart
- CompanyChart
- JobRoleChart

---

## 5. 문제 정의

현재 구조에서 확인된 주요 문제는 다음과 같다.

### 🚨 1) 디자인 시스템 미사용
- 기존 DS가 있음에도 실제 서비스에서 활용되지 않음

### 🚨 2) 컴포넌트 중복 구현
- Button, Card, Badge 등 핵심 컴포넌트가 여러 형태로 존재

### 🚨 3) 구현 방식 불일치
- Tooltip, Icon, Avatar 등이 각각 다른 방식으로 구현됨



## 6. 담당 컴포넌트 지정

| 이름 | 담당 |
| --- | --- |
| 김민지 | input, checkbox, radio, switch, **Image** |
| 전민지 | tooltip, badge, skeleton, **Layout** |
| 김영빈 | flex, grid, divider, **ExternalLink**  |
| 오소현 | button, card, icon |
| 이원주 | avatar, typography, tokens, **ContentWithTitle** |


## 7. 다음 전체 회의때 까지 해올 내용
- [ ] 담당 컴포넌트 코드 분석
- [ ] 담당 컴포넌트가 홈페이지 코드에 어떻게 마이그레이션 할지 계획세우기
- [ ] 개발 할 수 있다면 하기
