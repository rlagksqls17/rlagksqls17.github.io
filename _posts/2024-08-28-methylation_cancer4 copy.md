---
layout: post
title: "[후성유전학] 4. DDS R package 원리 2"
categories: project
tags: test
last_modified_at: 2024-08-28T18:00:00~23:00
---  


## 요약  

![Imgur](https://imgur.com/ovyL9LK.jpg)  

위 [논문](https://academic.oup.com/bioinformatics/article/32/10/1446/1743267)을 읽고 번역 및 개인적으로 해석하여 DDS R package가 어떤식으로 돌아가는지 정리해두었다.  

해당 프로그램 알고리즘 내 여러 통계적인 지식들을 본인이 이해한대로 정리해두었으니, 무조건 맞지 않을 수 있다. 읽는데 유의했으면 좋겠다.  
 
[지난 글](https://rlagksqls17.github.io/project/2024/08/11/methylation_cancer3-copy.html)에서는 DMR이란 무엇이고, DMR을 식별하는데 유용한 DDS R package가 어떤 원리를 이용하는지에 대해, 베타이항분포의 개략적인 개념을 통해 설명하였다. 

이번 글에서는 결과적으로 이러한 베타이항분포 모델로부터, 어떻게 가설 검정을 위한 통계량 계산식을 유도할 수 있는지 작성해보도록 하겠다.  

---  

하... 너무 어렵다. 이걸 정리하는 것도 일인 것 같다. 본인이 이해한 내용이 맞는 건지 잘 모르겠는데, 직관적으로 대략 이런 느낌일 것 같아서 기록해보았다. **이 다음부터 쓸 글은 사실이 아닐 확률이 되게 높으니 아닌 것 같으면 댓글 부탁드립니다..**   

---

## 설명

지난 글에서, 우리는 Dataset d (d = 1, 2, 3, ..., D) 내의 모든 CpG site i (i = 1, 2, 3, ..., N)에 대해서, Y*id*를 메틸화된 read의 수, m*id*는 그 CpG site를 가지는 전체 read의 수를 말한다는 것을 알았다.   

또한 p*id*를 sample d 내 특정 CpG site i의 메틸화 수준이라고 부르기로 했다 ([지난 글](https://rlagksqls17.github.io/project/2024/08/11/methylation_cancer3-copy.html)을 꼭 살펴보자).  

이후, 저자는 Y*id*가 이항 분포 (특정 CpG site에서 메틸화가 되느냐 안되느냐 둘 중 한가지의 시행에 다른 확률의 분포)를 따른다고 가정했지만, 그 확률을 단순하게 추정할 수 없고 여러 생물학적 맥락에서 고려해야 하기 때문에, **사전 확률 분포, 즉 베타분포를 그림으로써 메틸화가 될 확률을 더 정확하게 추정하여야 한다고 주장한다.** 따라서 Y*id*는 베타이항분포 (beta-binomial distribution)를 따른다고 가정한다.    

**즉 Y*id* ~ beta-bin (mid, πid, ψi)**  

여기서 해당 분포의 평균값은 πid, 그리고 해당 분포의 분산 매개변수는 ψi라 한다 (m*id*는 그 CpG site를 가지는 전체 raed의 수).

---  

그 다음으로, 이제 저자는 베타이항분포를 이용하여 특정 데이터 셋 내의 메틸화 확률을 추정했으니, 이번에는 **링크함수** 개념을 도입한다. 즉 **베타이항분포를 통해 특정 CpG site 내 추정된 메틸화 확률 (πid)과, 그리고 메틸화 외 다른 연속형 메타데이터 공변량 (covirate)을 이용해서 그 데이터 셋 내의 특정 CpG site coefficient를 계산**하려 한다.

링크함수는 말 그대로 독립 변수와 종속 변수 사이의 관계를 적절히 모델링할 수 있도록 도와준다. 즉, 종속 변수 (여기서는 πid)가 따르는 분포의 특성에 맞게 변환을 적용하여, 선형 모델로 적합하게 만들어준다.   

**그러니까 말 그대로 종속변수 (πid) 그리고 다른 상수 (X_d, 여기서는 메틸화 수준 데이터 외 다른 연속형 데이터를 뜻함)를 선형적인 식으로 만들어 하여금 독립변수 (β_i)를 계산할 수 있게 만들어주는 일종의 도구함수라는 것 같다!!!!**  

저자는 여기서 'arcsine'이라는 link function을 사용했다. 본문에 따르면, 해당 link function이 binomial data내 분산 변동성을 안정화시키는 것으로 보고된 바 있어서, 저자는 이 link function을 사용했다고 한다.  

왜 이 link function을 사용했는지에 대해 궁금하다면.. 미안하다.. 수학적인 이론까지 건드리기 힘들다..  

어쨋거나 결과적으로 식은 다음과 같다.  

Y*id* ~ beta-bin (m*id*, π*id*, ψi),  

arsine(2π*id* - 1) = x_d * β_i  

---  

**즉, 저자는 위 식을 통해 실험적 요인 (x_d)이 메틸화 수준을 나타내는 계수 (β_i)에 얼마나 큰 영향을 미칠지, 사전에 미리 추정한 메틸화 확률 베타분포에 기반한 링크함수를 통해 측정하고자 하는 것이다.**  

**따라서 귀무가설은 다음과 같이 설정된다.**  

**H_0j:C^Tβ_i = 0 => 실험적인 요인들의 선형 결합 C^T (다수의 X_d 값들로부터 계산됨)이 CpG 메틸화 수준에 영향을 주지 않는다.**  

이걸 이용해서, 모든 실험적 요인 및 공변량 (experimental factor/covariate)에 대한 DML (Differentially Methylated Loci, 메틸화된 CpG site를 의미) 여부를 탐지할 수 있다고 한다.  

---  

너무 어렵다 ㅠㅠㅠㅠㅠㅠㅠㅠㅠㅠ

---  

[이전글: DDS R package 원리 1](https://rlagksqls17.github.io/project/2024/08/11/methylation_cancer3-copy.html)  

[다음글: Chip Seq data와 후성유전학과의 관계](https://rlagksqls17.github.io/project/2024/09/02/methylation_cancer5.html)