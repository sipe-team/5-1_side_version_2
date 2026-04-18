## 사이프 디자인 시스템 피그마

: 전달받은 구글 이메일로 로그인 후 사용해주세요

https://www.figma.com/design/EVxUFhOfCTmOXsgED5RU9B/SIDE--sipe-design-system?node-id=0-1&t=z41vHrVWyqYcNIyb-1

## 1. 피그마 컴포넌트 현황

### Foundation

| 항목 | 세부 내용 |
| --- | --- |
| **Color** | Atomic (Gray / Red / Pink / Purple / Cyan / Blue / Teal / Green / Yellow / Orange 각 050-950) + Theme 기수별 (1st-4th) + Semantic (text / background / accent/ gradient / warning / danger / positive) |
| **Typography** | Pretendard Variable, font-size 050(12px)-900(48px), weight 400·500·600·700, line-height regular(150%) / compact(130%) |
| **Spacing** | 기본 단위 4px, space-050(4px) - space-900(60px) |
| **Breakpoints** | `$sm` 0px(mobile) / `$md` 780px(tablet) / `$lg` 1060px(desktop), SCSS mixin 4종 |
| **Icon** | Icon Button (size sm·lg / color white·gray·black·theme / disabled) + SNS Icons (Instagram · GitHub · YouTube · LinkedIn · link 등) |
| **Logo** | LOGO 단독형 + LOGO w.slogan, 기수별 컬러 토큰 적용 구조 |

---

### Component

| 컴포넌트 | Props 요약 |
| --- | --- |
| **Button** | variant (filled / ghost / outlined) · size (sm / lg) · disabled · interaction (normal / hovered / pressed) · 기수 theme 색상 연동 |
| **Card** | 기본 이미지 카드 · 활동 리뷰 페이지 카드 · 사이퍼 소개 페이지 카드 · 스케줄 표시 카드 |
| **Nav** | Header — breakpoint (xs-sm / md-lg / xl), state (recruiting / closed) + Footer — breakpoint (xs-sm / md-lg / xl) |
| **Chip** | variant (filled / outlined / ghost) |
| **LabelBlock** | variant (solid) · size (lg) · badge (none / text) · icon (none / left / right / both) · color (dark / theme) |
| **Accordion** | expand (false / true) · verticalpadding (lg) |
| **Badge** | variant (solid) · size (sm / lg) · icon (none / left / right / both) · color (white / primary / gray / danger) · theme (general / 1st / 2nd / 3rd / 4th) |
| **Imagecard** | imagetile (filled / empty) |

---

## 2. sipe.team 사이트 분석 결과

> 4기 기준 피그마 문서 작성 이후 홈페이지가 5기 버전으로 업데이트되어, 
디자인시스템에 추가 반영이 필요한 항목들을 정리합니다.
> 

### 페이지별 변경사항

### 홈 `/`

- 5기 기준 **오렌지 테마**로 전면 변경 (기존 피그마엔 1-4기 테마만 존재)
- **StatCard** 3개 추가 — 누적 지원자 수 / 총 참여자 수 / 누적 미션 수
- CTA 버튼 2개 병렬 배치 — `6기 모집 알림 신청(filled)` + `링크 공유하기(outlined)`
- **FloatingActionButton** 신규 — 우측 하단 카카오채널 채팅 버튼 (전 페이지 공통)

### About `/about`

- 기존 구조 유지 (Goal / Mission / Culture / 주요활동 / 후원사 / FAQ / Contact)
- 주요활동 탭 버튼 (정규 미션 / 사이프챗 / 사담콘 / 내친소 / 그 외 행사) → **Tab 컴포넌트** 필요 확인

### Recruit `/recruit` ⭐ 신규 페이지

- **지원자격** — 체크 아이콘 + 텍스트 형태의 ChecklistItem 4개
- **모집 일정** — 가로 스크롤 DraggableContainer 안에 ScheduleCard 5개 (서류 접수 → 합격자 발표 → 오프라인 인터뷰 → 최종 합격자 발표 → 정규 활동 시작)
- **활동 안내** — 회차별 날짜 + 이벤트명 목록 (11회차, Table 또는 리스트 형태)
- **이전 기수 구성원 현황** — 인터랙티브 도넛 차트 3종 (연차별 / 회사별 / 직군별), 클릭 시 항목 하이라이트
- **자주 묻는 질문** — Accordion 동일하게 사용

### People `/people`

- 기수 필터 추가 — `4기 / 3기 / 2기 / 1기` 링크 탭 → **Tab 컴포넌트** 필요
- UserCard 구조 유지 (프로필 이미지 + 이름·직군 + 소셜 링크 + 자기소개 + 활동후기)

### Activity `/activity`

- 콘텐츠 탭 추가 — `발표 영상 / 블로그` → **Tab 컴포넌트** 필요
- 영상 콘텐츠 **VideoCard** 형태 — 썸네일 이미지 + 제목 + `보러가기` 버튼


### 체감하고 있는 시급한 부분

- 컬러 시멘틱 토큰 재정의
- 5기 버전 컬러 토큰 정의
- 바뀐 사이트 기준으로 필요한 컴포넌트 우선순위대로 디자인
