---  
title : "나의 두 번째 프로젝트 - 중간 과정 기록"  
excerpt : "last_modified_at: 2021-10-10"  
categories:  
- Blog  
layout: post
tags:  
- ['코딩배우기', '코딩교육', '코딩부트캠프', '코딩국비지원', '엘리스AI트랙', '팀프로젝트', '데이터분석']
katex: True  
toc: True  
toc_sticky: False    
toc_label: "목차"  
last_modified_at: 2021-09-07T08:01:00~03:00  
---  

포트폴리오 웹 서비스를 구현하는 첫 프로젝트를 마치고, 한달 동안 나는 엘리스에서 제공하는 기초 데이터 분석을 학습했다.  
또한 남는 시간에는 데이터 분석 스터디를 하나 만들어 운영하고, 다른 데이터 분석 스터디에도 참여하여 데이터 분석 심화과정도 조금씩 학습해왔다.  

처음에 생소했던 R언어에 익숙해져 갈 때 즈음 점점 진로에 대한 고민이 커지기 시작했다.  
그때 시기가 한창 엘리스에서 이력서 첨삭이나 취업 관련 세미나를 하던 때라서 아무래도 나의 진로에 대해서 더 진지하게 고민해보지 않았나 싶다.  

사실 이번 두 번째 프로젝트를 시작하기 전에는 백엔드와 데이터분석에 관심이 많았고, 따라서 데이터분석 대학원 진학과 백엔드 직종 취업 중에서 상당히 많은 고민을 했었다.  
결론적으로 백엔드 취업을 목표로 하기로 막연하게 정했었다.  
하지만 이번 프로젝트를 진행하면서 내 진로에 대한 방향성이 어느 정도 잡혀나가는 느낌이 들고 있다.  

다음과 같이 취업 계획을 구상했다.     

1. 빅데이터 분석 기사 자격증을 취득한다.  
- 이번 겨울에 빅데이터 분석 기사 실기 시험을 볼 예정이다.  

2. 자바 스프링을 공부하여 백엔드의 기초 및 심화 공부를 한다.  
- 아직 본인이 백엔드에 대한 내용을 너무 모른다고 생각하기 때문이다.  

3. 엘리스 커리큘럼이 끝나면 java spring, React, MySQL을 이용해 나만의 포트폴리오를 만들고 배포하여 다음의 내용들을 넣을 것이다.  
- 여태껏 진행했던 프로젝트와 개인 데이터 분석 결과를 정리한 페이지를 구현한다.  
- 데이터 시각화 라이브러리를 사용하여 내 포트폴리오를 보는 사람들에게 가독성 있는 결과를 보여준다.  
- java spring에 대해 알기 쉽게 정리한 페이지를 작성한다.  

4. 포트폴리오를 구현하며 매일 시간을 정해서 진로 탐색 및 정보 검색을 꾸준히 진행한다.  

**즉 데이터분석, 프론트엔드, 백엔드를 전반적으로 공부하되 백엔드와 데이터분석을 심층적으로 공부하려고 한다.**    
**이번 프로젝트를 진행하면서 나 자신이 어느정도 데이터 분석과 시각화에 관심이 있다는 것을 확신했다.**    

이제 2주 동안 엘리스에서 진행한 팀 프로젝트의 전체적인 과정을 적어보면서 왜 이러한 진로를 선택하게 되었는지 적어보도록 하겠다!   


# 엘리스 두 번째 프로젝트 시작!  

프로젝트의 주제는 배달 데이터 분석 웹 서비스이고, 진행기간은 9월 27일 ~ 10월 15일이다.  
현재(10월 10일) 중간발표를 마친 후의 과정들을 기록하려고 한다.  

이번에 진행한 프로젝트는 팀 프로젝트라서 팀을 먼저 구성해야 했는데, 운이 좋게도 다른 레이서 분과 내가 운영하고 있는 스터디 팀원 분들이 함께 해보자고 말씀해주셔서 빠르게 팀이 결성되었다. 사실 저번에 운영했었던 스터디 팀원 분들의 장점을 눈여겨보고 프로젝트 때 같이 해봤으면 좋겠다라는 생각을 했었는데, 이렇게 먼저 제안을 해주셔서 너무 감사했다.  

예상보다 빠르게 팀이 결성되었던 덕분에 바로 역할 분담을 나누게 됬는데, 어느정도 팀의 방향성을 잡는 일과 의견 조율 및 역할 분담시키기 등은 할 수 있을 것 같아서 팀장을 맡게되었다. 또한 한창 데이터분석을 공부하고 있었기 때문에 데이터 분석 및 백엔드 서브를 해보기로 결정했다.  

프로젝트는 다음과 같은 일과로 진행했다.  
1. 오전스크럼 진행 (오전 10시, 엘리스 기본 일과)  
2. 개발 (엘리스 기본 일과)  
3. 점심스크럼 진행 (오후 1시, 내가 팀원 분들에게 요청드린다.)  
4. 개발 (엘리스 기본 일과)  
5. 오후스크럼 진행 (오후 5시, 팀 회의로 결정)  
6. 코치님 오피스아워 진행 (오후 7시 ~ 9시, 엘리스 기본 일과)  
7. 오피스아워 후 스크럼 (오후 9시, 대부분 내가 강제적으로 요청을 한다 ㅎㅎ..)  

전체적인 일과를 보면 스크럼 진행이 굉장히 많은데, **팀장인 내가 다른 팀원들의 일 진행 상황을 수시로 확인하고, 서로 소통하며 역할 분담을 하는게 팀원분들이 더 효율적으로 일을 할 수 있도록 하는 방법이라고 생각했다.**  

#### 오피스 아워 진행 디스코드 화면  
![Imgur](https://imgur.com/95Tbnd7.jpg)  



## 팀프로젝트 - 1주차  
1주차에는 전체적인 프로젝트 기획 및 기본 개발 환경 구축을 진행했다.  

데이터 분석 결과를 띄우는 웹 서비스이니만큼 처음에는 팀원들이랑 같이 대표 주제를 논의했고,  
코로나 확진자 전 후로 배달주문건수의 변화를 사용자에게 보여주는 웹 서비스를 만들기로 결론지었다.  

코로나에 대한 사람들의 두려움으로 밖에 직접 나가서 여러 사람들이랑 외식을 하는 것은 꺼리게 되고, 오히려 집에서 직접 요리를 해먹거나 배달을 더 시켜먹지 않을까라는 생각에서였다.  

이렇게 기획 아이디어를 도출한 뒤, 팀원들 각자 관련된 데이터를 마구 수집하였다.  
**도출한 주제 범위가 워낙 협소해서 그렇게 쓸 만한 데이터를 많이 뽑아낼수는 없었다.**  
수집한 데이터는 유로 데이터를 제외하고 다음 [github 링크](https://github.com/rlagksqls17/teamProject_dataAnalysis-for-deliveryService)에 공유한다.  
데이터를 사용할 분들은 사용전에 README.md에 명시된 출처를 꼭 참조해주셨으면 한다.  

수집한 데이터들을 바탕으로 어떠한 인사이트를 도출할 수 있을지 팀원들이랑 회의하고, 다음과 같은 인사이트를 나타내보기로 결정하였다.  
1. 코로나 전후로 실제 음식점 방문고객수와 배달건수의 변화가 있었는지 확인    
2. 코로나 전후로 확진자수와 배달 주문건수의 관계성이 존재하는지 확인  
3. 배달상점데이터를 분석해 각 세부지역별 존재하는 배달 음식점의 분포를 분석  
4. 사용자 선호 맛집 가격 분석 데이터를 통해 요일별-성별별 인기 업종 데이터 분석  

이렇게 위에서 나열한 정보들만 보면 상당히 일사천리로 진행된 것처럼 보이지만, 이 의견들과 인사이트를 도출하는데에도 많은 어려움이 있었다.  
1. 의견을 조율하는 입장에서 팀원들의 다른 의견들을 어떻게 합치고 타협해야할지 고민이 많았다.  
- 이는 팀원들과의 많은 소통과 제안, 수용을 통해 해결할 수 있었다. **일단 팀원분들의 성향이 모두 배려와 이해를 잘해주셨기 때문에 가능했지 않았을까 생각한다.**  
2. 분석 주제에 맞는 데이터가 너무 부족했다.  
- 이는 **엘리스 회사에서 직접 유료데이터를 구매해 우리 팀에게 지원을 해주심으로써 해결이 되었다.**  

이렇게 도출한 기획 아이디어를 바탕으로 와이어프레임을 구축했는데, 팀원 분들 중 한 분이 디자인에 굉장히 능통하신 분이라서 금세 와이어프레임을 구축할 수 있었다.  
**팀 프로젝트를 진행하면서 각자의 장점들을 발견하고, 이 장점들을 적극 활용하여 팀원분들이 자기자신의 능력을 펼칠수 있도록 하는게 팀플에서 중요한 점 중 하나라고 생각했다.**  

## 팀프로젝트 - 2주차  
2주차에는 본격적인 코드 작성과 데이터 분석을 진행했다.  

팀장의 입장에서 어떻게 팀원분들에게 역할 분담을 하면 더 효율적일 수 있는지 너무나 많은 고민을 하였고, 새벽에 혼자 나와서 깊은 고민을 했었다.  
일을 한 번 했는데 방향성이 어긋나기라도 한다면, 현재 진행한 일은 아무것도 아닌게 되버린다고 생각했기 때문이다.  

실질적으로 일을 지속적으로 할 수 있는 사람은 나를 포함한 네 명이었고, 기간별로 각자 다음 파트들을 역할 분담하게 했다.  

#### 기본작업  
팀원 A, 팀원 B (데이터분석)  
- 수집한 데이터 전처리  
- 분석 결과 데이터의 형식을 백엔드에 알려주어 API를 만들 수 있도록 함  
  

팀원 C (프론트엔드)  
- 스켈레톤 페이지 구축 (홈, 서비스 소개, 데이터 분석 페이지)  
- CSS 기초 디자인 적용  

  
팀원 D (백엔드)  
- 데이터 분석결과 그래프 이미지와 결과 문장을 프론트로 넘겨주도록 하는 API를 제작   

#### 중간작업  
팀원 A (데이터 분석, 프론트엔드 서브), 팀원 B (데이터 전처리 및 분석)  
- 전처리한 데이터 분석 및 시각화 (관련 데이터 분석결과는 추후에 다른 글로 포스팅하려고 한다.)  
- 프론트엔드에서 Nivo 라이브러리 이용한 데이터 시각화  
  

팀원 C (프론트엔드 메인)  
- leaflet이라는 라이브러리 이용하여 React에서 지도를 표시하도록 함  
- 지도의 각 지역을 클릭했을 때 해당 지역 데이터 분석결과를 백엔드에 요청하는 코드 작성      
  


팀원 D (프론트엔드 서브 및 배포)  
- 팀원 C와 같이 협업하여 지도 기능을 빠르게 구현하도록 함  
- 배포를 주기적으로 진행하여 실제 환경에서 테스트하도록 함  

#### 최종작업  
팀원 A (프론트엔드 데이터 시각화)
- Nivo 라이브러리 이용한 프론트엔드 데이터 시각화, 범례 설정 및 색상 선택  
- 발표준비  
  


팀원 B (데이터 분석)  
- 데이터 시각화 자료를 토대로 전체적인 데이터 분석결과 텍스트를 작성하도록 함  
  


팀원 C (프론트엔드)  
- MUI 라이브러리를 이용한 웹 디자인  
- 발표자료 제작  
  

  
팀원 D (백엔드)  
- 배포 진행 및 코드 수정시 재빌드, Docker 배포 시도  

#### 홈페이지 이미지  
![Imgur](https://imgur.com/PlwA6vI.jpg)  

#### 서비스소개페이지 이미지  
![Imgur](https://imgur.com/A7ghMhJ.jpg)  

#### 데이터분석페이지 이미지  
![Imgur](https://imgur.com/h6k3ec4.jpg)


# 프로젝트를 진행하며 깨달은 점  

팀원분들에게 역할을 분담하기 전에, 매일밤에 나는 각자 팀원들이 진행한 데이터 분석 결과와 코드를 찬찬히 살펴보고, 현재 상황을 이해하였다. 그후에 팀원들에게 회의를 요청하여 내일 진행할 일들에 대해서 설명해주고 역할분담을 어떤식으로 진행하면 좋을지에 대해서 서로 논의했다.  

**결국 팀원들간의 소통이 팀플에서 가장 중요했던 것이라고 생각한다.**  

지금까지는 팀원 분들의 능력도 너무 좋으셨고, 내가 기획한 역할 분담을 잘 따라와주신 덕분에 만족할 만한 결과를 내었다고 생각한다.  

프로젝트를 진행하며 처음으로 몇 백만개의 데이터 전처리와 분석, Nivo 라이브러리를 이용한 데이터 시각화를 경험했는데 너무 재밌었다.  
여태껏 배운 것을 바탕으로 개인적으로 다시 한번 심층적인 분석을 해보고 싶었다.  
**즉 프로젝트를 진행하면서 내가 데이터분석에 흥미를 느끼고 있구나라는 것을 확실하게 느끼게 되었다.**  
데이터분석의 분야를 잘 선택하는 것이 앞으로의 숙제인 것 같다. **생각만 하기보다는 직접 프로젝트를 진행하면서 느껴보려 한다.**
백엔드는 아직 경험해본게 많이 없어서 좀 더 경험이 필요하다고 생각했다.  

여러가지 얻고 있는게 많은 프로젝트였고, 남은 한 주동안 열심히 진행하여 마무리해보려고한다.  
최종 발표를 진행하고 난 뒤에 다시 포스팅 하도록 하겠다.  
:)  