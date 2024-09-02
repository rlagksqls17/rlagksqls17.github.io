---
layout: post
title: "[후성유전학] 5. CHip Seq 데이터와 후성유전학과의 관계"
categories: project
tags: test
last_modified_at: 2024-09-02T18:00:00~23:00
---  


## 요약  
본인이 현재 업무 진행하며 다양한 시퀀싱 데이터들을 접했는데, 여기에 여러 종류의 시퀀싱 데이터에 대한 정보들을 기록하였다.
또한 이러한 시퀀싱 데이터가 후성유전학과는 어떠한 관계가 있는지 작성하기로 한다.  

여러번 나누어 해당 포스트에 올릴 계획이다.  


## ChIP-seq  
아래 출처에 대한 블로그의 글을 인용하니 참조바란다.

ChIP-seq은 단백질과 DNA간의 상호작용을 분석하는데 사용되는 방법으로, 유전자 발현을 조절하는 전사인자(transcription factor) 및 기타 단백질의 기작을 연구하는데 많이 사용된다.  

과정은 다음과 같다.  

하나. 알고자 하는 단백질에 DNA를 결합한 후 포름알데히드로 고정시킨다.  

둘. 세포를 Lysis하여 DNA를 분리한 후 500bp 이하로 조각낸다.  
=> 이때 DNA 조각은 단백질이 결합되어 있는 DNA와 단백질이 결합되지 않은 DNA로 나뉘게 된다.  

셋. 단백질에 특이적인 항체 (antibody)와 bead를 DNA에 붙여, 원심분리를 통해 단백질과 결합된 DNA만 분리한다.   

넷. 단백질과 DNA를 분리하여 순수한 DNA만 분리한다.  



[출처](https://blog.naver.com/sanigen/222175267534)  

---  

[이전글: DDS R package 원리 2](https://rlagksqls17.github.io/project/2024/08/28/methylation_cancer4-copy.html)  

