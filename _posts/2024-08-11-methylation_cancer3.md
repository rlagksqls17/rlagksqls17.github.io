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

![Imgur](https://imgur.com/Dqp9Abv.jpg)



---  