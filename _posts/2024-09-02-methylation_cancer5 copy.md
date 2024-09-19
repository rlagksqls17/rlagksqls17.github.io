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

sequencing 과정은 다음과 같다.  

하나. 알고자 하는 단백질에 DNA를 결합한 후 포름알데히드로 고정시킨다.  

둘. 세포를 Lysis하여 DNA를 분리한 후 500bp 이하로 조각낸다.  
=> 이때 DNA 조각은 단백질이 결합되어 있는 DNA와 단백질이 결합되지 않은 DNA로 나뉘게 된다.  

셋. 단백질에 특이적인 항체 (antibody)와 bead를 DNA에 붙여, 원심분리를 통해 단백질과 결합된 DNA만 분리한다.   

넷. 단백질과 DNA를 분리하여 순수한 DNA만 분리한다.  

---  

하나. 시퀀싱으로 얻은 read를 해당 종의 genome 서열에 mapping 한다.  

둘. Visualization을 통해 read들이 mapping되는 특이적인 위치 (peak)를 확인한다.  

셋. 해당 위치에 가까이 있는 유전자 정보를 이용해 functional annotation, GO analysis 등 다양한 분석을 수행한다.  

[출처](https://blog.naver.com/sanigen/222175267534)  

---  

## 그렇다면 ChIP-seq과 Methylation과의 연관성이 있을까?  

PRC2 단백질은, 프로모터에서 (특히 CGI 프로모터),   

DNA hypermethylation을 해주는 기능이 있다고 알려져 있다 [(참조논문)](nature.com/articles/s41467-021-22720-0#Sec2).   

따라서 **단백질이 해당 genome의 어디 부위에 달라붙었는지 확인할 수 있는 ChIP-Seq**을 PRC2 단백질에 대해 수행하고, 같은 부위에 메틸화가 어떻게 되어 있는지 (즉 hypomethylation인지 hypermethylation인지) 확인할 수 있다면, PRC2가 사람의 어느 genome 부위에 메틸화 기능을 수행하는가를 알 수 있을 것이다. 예를 들면 줄기세포나, 사람의 상피세포 등 말이다.  

---

[이전글: DDS R package 원리 2](https://rlagksqls17.github.io/project/2024/08/28/methylation_cancer4-copy.html)  

[다음글: Bis-SNP 소프트웨어](https://rlagksqls17.github.io/project/2024/09/19/BisSNP.html)  
