---
title: 'about'
date: 2020-9-26 22:33:13
lang: 'ko'
---

# 김민정 (MJ KIM)

**저는 `______` 프론트개발자 입니다.**

1. 좋은 코드, 간결한 코드를 작성하려는
2. 공부하는 것을 좋아하고 기록하고 공유하는
3. 협업과 커뮤니티를 좋아하는
4. 어떤 사람이 되고 싶은지 고민하고 그런 사람이 되기 위해 노력하는

**저는 `______` 조직을 선호합니다.**

1. 언제나 투명하고 솔직한 의사소통이 이루어지는
2. 구성원 간 신뢰를 기반으로 자율적으로 일하는
3. 불필요한 커뮤니케이션을 줄여 효율적으로 움직이는
4. 한 명이 앞서 나가는 것보다 다 같이 성장하는

|                  |                               |
| ---------------: | ----------------------------- |
|       **GitHub** | <https://github.com/howdy-mj> |
|    **Tech Blog** | <https://howdy-mj.me/>        |
| **General Blog** | <https://kim-mj.tistory.com/> |
|        **Email** | <hi.minjungkim@gmail.com>     |

<br />

# Experience

## 루트에너지(Rootenergy)

재생에너지 크라우드 펀딩 플랫폼 | 20.08 - | Frontend Developer

- TypeScript, React, Redux, Rematch
- SCSS
- Git flow, Notion, Slack으로 업무 진행
- HTTP, Web Server와 WAS 세미나 발표

## 플랜즈 커피(Planz Coffee)

자동화 음료 리테일 서비스 | 20.05 - 20.06 | Intern

- webpack으로 icon 크기 최적화 번들링 작업
- Storybook으로 Icon, Font 구축 및 배포
- react-spring, Node.js, Storybook 세미나 발표

## BRPartners

블록체인 컨설팅 | 18.10 - 20.01 | Manager

- 블록체인 프로젝트 검토 및 보고서 작성
- 암호화폐 거래소 시장 조사 및 기획
- 마케팅 기획안/제휴제안서 작성 및 진행
- 퍼포먼스 마케팅(SNS, APP, 연관검색어, 웹 배너 등)
- 중국어/영어 통번역

<br />

# Project

### BFRun

|                  |                                                                             |
| ---------------: | --------------------------------------------------------------------------- |
|       **period** | 2020.06.23 - 2020.07.17 (기획 및 디자인 1주, 개발 3주)                      |
| **introduction** | 웹 개발 입문자가 보면 좋을 유명 크리에이터분들의 동영상(유튜브) 모음 사이트 |
|        **stack** | JavaScript, Next.js, React, React Hooks, react-router, Styled-Component     |
|         **repo** | https://github.com/one-iron/BFRun                                           |

#### What We did

- Notion을 활용해 스케쥴 및 할 일 관리
- Ovenapp.io로 디자인
- REST API 사용

#### What I did

- Next.js 초기 세팅(ESLint, Prettier, Styled-component 등)
- 반응형 웹 구현 (Web, Tablet, Mobile)
- 구글 로그인 구현
- `getStaticProps`로 render 전 카테고리 및 메인 화면 데이터 불러오기
- 카테고리 개발
  - Contents와 Stacks, Creator는 공존 불가
  - Stacks는 최대 3개만 고를 수 있도록 제한
  - Stacks와 Creator를 같이 고를 경우 교집합으로 필요한 API를 axios로 불러옴
- Nav 설정
- 최대한 시맨틱 태그를 준수하려 노력

### GDAC

|                  |                                                                       |
| ---------------: | --------------------------------------------------------------------- |
|       **period** | 2020.05.11 - 2020.05.22 (약 2주)                                      |
| **introduction** | 국내 암호화폐 거래소 지닥 클론                                        |
|        **stack** | JavaScript, React, React Hooks, react-router, Styled-Component, Redux |
|         **repo** | [wedac](https://github.com/howdy-mj/wedac-frontend)                   |
|         **demo** | [youtube](https://youtu.be/LdF1LG_R4Uo)                               |

#### What We did

- 매일 Scrum 회의 진행 및 트렐로로 팀원 간 업무 파악
- Git으로 버전 관리 및 rebase로 commit 통합
- REST API 사용

#### What I did

- CRA 초기 세팅(ESLint, Prettier, react-router, Styled-component)
- 반응형 웹 구현 (Web, Tablet, Mobile)
- 메인, Nav, Footer, 회원가입, 로그인, 내 설정, 잔고, 지갑, 거래내역 페이지 개발
- 회원가입, 로그인 구현
  - 아이디, 비밀번호, 비밀번호 중복확인 정규식으로 조건 체크
  - 카카오톡 회원가입, 소셜 로그인 구현
  - 로그인 여부에 따라 Nav 및 특정 페이지 차단
- REST API로 fetch 한 정보를 보여주기
  - fetch한 '전일대비' 값이 양수, 음수, 0일 경우 조건식으로 '현재가' 및 '전
    일대비'의 텍스트 색상을 props로 넘겨주어 적용
- Redux로 state 관리

### Paulbassett

|                  |                                                         |
| ---------------: | ------------------------------------------------------- |
|       **period** | 2020.04.20 - 2020.05.01 (약 2주)                        |
| **introduction** | 카페 폴 바셋 클론                                       |
|        **stack** | JavaScript, React(Class), react-router, SCSS            |
|         **repo** | [Paulba3](https://github.com/howdy-mj/PaulBa3-frontend) |
|         **demo** | [youtube](https://youtu.be/a1vKyWHA8pE)                 |

#### What We did

- 매일 Scrum 회의 진행 및 트렐로로 팀원 간 업무 파악
- Git으로 버전 관리 및 충돌 해결
- REST API 사용

#### What I Did

- CRA 초기세팅(ESLint, Prettier, react-router, SCSS 설치)
- Nav, Footer, 메인, 메뉴, 스토어 페이지 개발
- Main 페이지
  - Mock data를 `.json`으로 만들어 메인배너 불러오기
  - 배너 자동 넘김 및 넘김 버튼 클릭 구현
  - ref()를 활용한 화면전환 스크롤 구현
- 동적라우팅으로 카테고리 및 제품 상세 페이지 이동 구현
- Store 페이지
  - Google Map API를 활용해 현재 위치 기준의 폴바셋 지점 표시
  - 해당 시/도, 구/군에 있는 지점의 단계별 클릭 리스트 구현
- AWS EC2 배포

<br />

# Education

### Wecode

20.03 - 20.06 | WeCode 부트캠프 과정 수료

### 북경대학교 (Peking University)

12.09 - 16.07 | 중어중문학

<div style="text-align: center" class="final">

_감사합니다._

<sub><sup>Front-End Engineer, <a href="https://github.com/howdy-mj">@howdy-mj</a></sup></sub>

</div>
