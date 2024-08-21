---
layout: post
title: "[후성유전학] 3. DMR 분석 - DDS R package 원리 (8.21. updated)"
categories: project
tags: test
last_modified_at: 2024-08-11T18:00:00~23:00
---  


## 요약  

![Imgur](https://imgur.com/ovyL9LK.jpg)  

위 [논문](https://academic.oup.com/bioinformatics/article/32/10/1446/1743267)을 읽고 번역 및 개인적으로 해석하여 DDS R package가 어떤식으로 돌아가는지 정리해두었다.  

해당 프로그램 알고리즘 내 여러 통계적인 지식들을 본인이 이해한대로 정리해두었으니, 무조건 맞지 않을 수 있다. 읽는데 유의했으면 좋겠다.  
 

## DMR이란?  
DMR (Differentially methylated region)이란, 예를 들어 A그룹 (약 복용)과 B그룹 (대조군)이 있을 때, 그 그룹 간의 메틸화 (methylation) 패턴이 다른 region들을 의미한다. 일반적으로 sequence data 내 CpG 사이트 (이전 글에서 설명)내 메틸화 존재 여부를 특정 소프트웨어 (예를 들어 Bismark, 다른 글에서 설명 예정)를 사용하여 확인하는데, 식별된 CpG 사이트들 중 몇몇 CpG 사이트들이 포함된 특정 region에서 만약 메틸화 패턴이 A그룹과 B그룹간에 다르다면, 그것을 DMR이라 칭한다.   

일단 DMR을 식별하고 나면, 해당 DMR이 포함된 유전자 (gene)들을 찾아서, 어떤 기능을 가지는 유전자들에 메틸화 패턴이 달라졌는지 확인할 수 있다. 만약 방대한 데이터 내에서 수많은 유전자들 내 존재하는 DMR들을 식별하기만 하면, 질병을 가지는 그룹에서 대조군과 비교하여 어떤 유전자들의 메틸화 패턴이 달라졌는지 시각화하여 연구해볼 수 있을 것이다.  

DSS R package를 이용하여 DMR을 식별할 수 있다. 

---  

## Methods  

*여기서부터 참조논문을 번역하였다.*  

### 2.1 The data model  

먼저, 어떤 input data가 D 개의 샘플들 내 N 개의 CpG 사이트에 대한 정보를 가진다고 가정해보자. 

Dataset *d* (*d* = 1, 2, 3, ..., *D*) 내의 모든 CpG site i (i = 1, 2, 3, ..., *N*)에 대해서, Y*id* 는 메틸화된 read의 수를, m*id*는 그 CpG site를 가지는 전체 read의 수를 말한다. *아래 그림을 살펴보자*  

![Imgur](https://imgur.com/10bIGrs.jpg)

sampling과 biological variation을 고려했을 때, Y*id*는 beta-binomial distribution을 따른다고 가정한다.  

---

여기서 잠깐, 본인이 통계에 대해서 정말 아무것도 모르지만, 그래도 beta binomial distribution에 대해 공부해봤다. 

**여기서 저자는 왜 Yid가 beta-binomial distribution을 따른다고 가정했을까?**

먼저 이항분포 (binomial distribution)에 대해 알아야 한다. 이항분포는 말 그대로 실패, 혹은 성공과 같이 결과가 두 가지로 한정되는 실험을 특정 횟수만큼 반복했을 때의 성공 횟수를 분포로 나타낸 것이다.  

아래 그림을 보자.  

![Imgur](https://imgur.com/FmZmNZT.jpg)

위 그림은 이항 분포의 예시이다. x축은 성공의 횟수, y축은 특정 횟수만큼 성공할 확률을 의미한다. figure legend의 각 색깔 점은 실험 디자인을 의미한다. 예를 들어, 빨간 색 점으로 표기된 분포는, 성공확률이 0.5로 알려져있는 시행을 40번 했을 때, 20번 성공할 확률, 30번 성공할 확률 등을 분포로 나타낸 것이다.  



---  