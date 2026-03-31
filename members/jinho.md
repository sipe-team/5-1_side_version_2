# jinho

## 소개
- 이름: 염진호
- 역할:
- 브랜치:

## 활동 기록

### Week 01
----


## 의사결정 해야할 것

1. 디자인 시스템은 어디까지 커버할 것인가
    
    | 범위 | 설명 | 장점 | 단점 |
    | --- | --- | --- | --- |
    | SIPE 한정 | 현재 서비스에 필요한 것만 최적화 | 빠르고 현실적 | 재사용성과 확장성 낮음 |
    | SIPE를 커버하는 전형적 DS | 현재와 가까운 범용 웹 앱 수준 | 밸런스가 좋음 | 일정 수준의 설계 투자 필요 |
    | 범용 디자인 시스템 | 다양한 제품/팀/브랜드를 전제 | 확장성 최대 | 가장 무겁고 느림 |

    > 초기 목표 범위는 2번으로 설정
    > SIPE를 안정적으로 커버할 수 있는 전형적인 디자인 시스템을 우선 구축
    > 다수 브랜드/다수 제품군을 전제한 범용 디자인 시스템은 초기 범위에서 제외
    
2. 토큰 구조를 어디까지 계층화할 것인가
    
    1. Base token
    2. Semantic token
    3. Component token
    
    > 초기 공식 토큰 구조는 Base Token + Semantic Token의 2계층으로 정의
    > Semantic Token 레이어를 통해 디자인 의도를 의미 단위로 전달
    > 컴포넌트별로 혼재된 토큰 활용 전략 정비를 위한 기준 레이어로 사용
    
    > `Figma Variables → Token Studio (export JSON) → Style Dictionary → CSS vars + TS types 자동 생성`

    > Figma Variables를 기준으로 관리하고, export된 JSON은 repo에서 버전 관리

3. 마이그레이션 원칙은 어떻게 가져갈 것인가
    1. 기존 컴포넌트를 유지/개선할지
    2. 신규 방식으로 재작성할지
    3. deprecate할지 기준 정의

    > 기본 원칙은 유지/개선 우선
    > 기존 구조를 최대한 활용하되, 필요 시 재작성 및 deprecate 기준 별도 정의


4. 컴포넌트 구현/공급 방식 
    | 선택지 | 설명 | 장점 | 단점 |
    | --- | --- | --- | --- |
    | 1. 현재 방식 유지 | CSS Modules + 자체 구현 유지 | 기존 자산 재사용, 외부 의존성 최소화 | 복잡한 컴포넌트 품질 확보가 어려움 |
    | 2. Radix 도입 + CSS Modules 유지 | 복잡한 컴포넌트만 Radix primitives 위에 구축 | 기존 코드 변경 최소화, 접근성/인터랙션 위임 가능 | shadcn registry 생태계 활용이 제한적 |
    | 3. Radix + Tailwind + shadcn registry | 설치형 컴포넌트 배포 구조로 전면 전환 | DX 우수, 트렌드 부합, 복잡한 컴포넌트 확장성 높음 | 재작성 비용 큼, Tailwind 학습 필요, 배포 구조 재설계 필요 |

    > 장기 기술 방향은 3번으로 설정
    > Tailwind **고수들**이 많고, 향후 확장성과 생태계 활용 측면에서 유리


5. 배포는 어떻게?
    
    | 방식 | 의미 | 적합한 상황 |
    | --- | --- | --- |
    | npm package only | 지금과 같은 배포 방식 유지 | 내부 사용 중심, 기존 자산 최대 활용 |
    | shadcn registry only | 설치형 소스 배포 중심 | 외부 채택, Tailwind 친화 생태계 지향 |
    | 둘 다 운영 | npm 패키지와 registry 병행 | 장기적으로 확장성이 크지만 운영 난이도 높음 |
    
    >  장기적으로 npm package와 shadcn registry 병행 운영
    > 배포 채널별 역할 분리 및 CI/CD 구축 필요


## 기존 분석

- react18 base
- Token이 Base Token으로 구성
- 토큰 활용 전략 혼재
    - Typo - 전역 token 사용
    - Input - 로컬 재정의
    - Button - 파일에 재작성
- release/v1 에서 CSS Modules → vanilla-extract 전환 작업하는 중

## 해야할 일

- react19 base migration
- Token Semantic Layer 추가
- 디자인 시스템 커버 범위 크게 생각하기
    - 사이프 한정
    - 사이프를 커버하느 전형적인 디자인 시스템
    - 범용 디자인 시스템
- 토큰 활용 전략 재정비(?)
    - 토큰을 어디까지 TS object로 둘지 / CSS var로 내릴지 / 컴포넌트 variant token은 어디서 관리할지
- Documentation 추가 Or Storybook 보강
- @side-team/side sideEffects 확인(style.css)
- 

## 도메인 URL

- [storybook.side.sipe.team](http://storybook.side.sipe.team) (storybook)
- [side.sipe.team](http://side.sipe.team) (docs)

## 다른 디자인 시스템 분석

- 사내디자인 시스템
    
    > 따로 공유예정
    

## 희망역할

- Docs 구축
- shadcn process 구축
- 역할이 유사한 단위로 2~3명씩 스쿼드를 구성
- PR 승인 기준: 스쿼드 내 approve 1명 + 타 스쿼드 approve 1명

## 기술 방향

- **Radix + Tailwind + shadcn 커스텀 registrhy 방향**
    - 대부분이 tailwind 경험이 있음
    - 모든 컴포넌트 마이그레이션
    - npm & shadcn registry ( CI / CD만 짜두면 둘다 커버하는데 문제 없음 )

## 디자인 토큰 & 피그마 연동

- 디자인 토큰은 SSOT로 피그마쪽에서 관리
- Token Studio + Style Dictionary로 활동하면 좋을듯
- Semantic Layer만 하나 더 올리면 좋지 않을까?

## 디파짓

- PR 늦게 제출 벌금 1만원
- PR 미제출 벌금 3만원
- 이거 모아서 오프라인때 맛있는거 먹기 !

---

### Week 02
- 진행 내용:
- 회의 내용 반영:
- PR 링크:
