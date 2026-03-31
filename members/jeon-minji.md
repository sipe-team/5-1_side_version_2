# 1. 현 상황 분석하기

- sipe.team
    - 프레임 워크 : "next": "^14.2.11",
    - 디자인 시스템 :  "@sipe-team/side": "^0.2.5",
        
        주로 Flex(레이아웃), Typography(텍스트), color(색상 토큰) 세 가지를 조합해서
        사용하고 있고, Button은 RecruitmentStatusSection에서만 사용
        
    - 스타일 초기화 : sanitize.css
        - 디자인 시스템의 토큰을 사용하고 있지 않음.
    - 버튼 : side와 variant가 1:1매칭되지 않음.
    - 아코디언 애니메이션 / 글로우효과 : framer-motion": "^11.0.3
        - motion에 대한 토큰 없음
    - 캐러셀 : "swiper": "^11.0.5",
        - 디자인 시스템 추가 필요
    - 토스트 : "react-simple-toasts": "^5.10.1",
        - 디자인 시스템 추가 필요
- side
    - 토큰을 export 가능하게
    - button을 sipe.team을 고려하여 확장
    - 캐러셀/ 토스트 등 사용하고 있지만 없는 컴포넌트 추가
    - 

# 2. 다른 DS 분석하기

* headless : https://ark-ui.com/

* radix primitive 위에 radix ui : https://www.radix-ui.com/themes/docs/theme/overview

# 3. 고민

1. tailwind로 방향을 바꿀 것인가? 
    1. 코드작성의 속도는 빨라질 수 있음 
    2. [sipe.team](http://sipe.team) 이나 hub 등에서 tailwind 의존성이 생기게됨. sipe.team과 hub도 tailwind로 일관된 아키텍쳐를 가져가는게 좋지 않을까? 
    3. 범용 라이브러리로의 확장을 고려한다면 tailwind는 제약이 될 수 있음. 
2. headless로 갈것인가? 
    1. sipe.team의 디자인이 지속적으로 디벨롭될 가능성이 높다면 headless 였으면 좋겠음 
    2. headful일 경우, 새로운 컴포넌트나, 새로운 디자인이 추가 될 경우 override가 필요하고, important가 불가피 하게 늘어나는게 우려됨. 

| 구분 | Headless 디자인 시스템 | Headful 디자인 시스템 | 디자인 템플릿 |
| --- | --- | --- | --- |
| 공유 방식 | 패키지 | 패키지 | 복붙 |
| 스타일 | 없음 | 있음 | 있음 |
| 통제 | 중간 | 강함 | 없음 |
| 자유도 | 높음 | 낮음 | 매우 높음 |
| 대표 느낌 | Radix | 사내 UI | shadcn |

# 4. 상세 작업 계획 아이디어

> → radix 기반으로 디자인을 입히는 방식으로 작업 / 기존 순수 css 사용
> 
> - `@sipe-team/side-core`
>     - Radix primitive (접근성, 키보드 상태로직)
> - `@sipe-team/side-tokens`
>     - base/semantic 토큰, CSS vars
> - `@sipe-team/side-theme-default`
>     - `.btn`, `.btn-primary`, `.accordion-item` 같은 기본 룩
1. 코드 컨벤션 정리 
    1. token naming
2. 토큰 정리  
    1. sizing 
    2. spacing
    3. color
    4. box shadow
    5. border
    6. opacity
    7. font 
3. 토큰 export 가능한 형태로 → side에서 token을 사용할 수 있도록 
4. Tokens Studio로 피그마 <→ 코드 토큰 연동
5. sipe 홈페이지에서 사용하고 있는 컴포넌트 순서대로
    - button
    - toast
    - accordion
    - navigation
    - footer
    - layout
    - glowarea
    - carousel
    - card
    - flex
6. 컴포넌트 작업 완료시 figma to code 연동
    - figma to code
        
        디자인 시스템을 사용하는 프로덕트에서 개발자가 Figma를 코드로 구현하는 것을 돕기 위해 사용됩니다. (dev모드에서만 가능)  
        
        Figma의 Dev Mode에서 코드를 확인하여 디자인 시스템을 사용할 수 있도록 합니다.
        
        1. Figma에 적용된 스타일과 실제 코드에 적용된 컴포넌트 스타일이 일치하지 않을 때
        2. Figma / 문서 / 코드 간 컨텍스트 전환이 복잡할 때
        3. Figma의 디자인 컴포넌트가 코드의 어떤 컴포넌트를 사용해야 할지 모를 때
        4. Figma mcp의 사용시 시너지 효과 
7. sipe.team에 적용
8. 미사용 컴포넌트는 후순위 

# 5. 내가 맡고 싶은 업무

- CICD 등의 인프라, Dx 개선과 정리 업무

# 6. 그외

- 릴리즈 주기 및 버저닝
    - 예) 1달에 한번
- 테스트 계획
- 마일 스톤
- pr룰
    - 페어로 리뷰담당자 선정 및 approve 필수
- 목표
    
    1차 목표 : [sipe.team](http://sipe.team) 에 적용하기 
    
    2차 목표 : sipe hub에도 적용할 수 있을 정도의 확장
    
    3차 목표 : 범용 디자인 시스템?