---  
title: "[Sub project] Accurate diagnosis of atopic dermatitis by combining transcriptome and microbiota data with supervised machine learning"
excerpt: "last_modified_at: 2023-02-14"
layout: post
use_math: True
last_modified_at: 2023-02-14T08:16:00~18:00
---  

현재 서브 프로젝트로 진행 중인 일을 간단히 요약하여 이곳에 정리하고,
이 프로젝트를 진행하면서 비동기 프로그래밍을 파이썬으로 사용해보려고 한다.  

다음의 논문의 내용들을 따라해보며 기술들을 익히고, 다른 연구들도 가능한지 생각해보려고한다.  

논문의 citation은 다음과 같다.  
> Jiang, Z., Li, J., Kong, N. et al. Accurate diagnosis of atopic dermatitis by combining transcriptome and microbiota data with supervised machine learning. Sci Rep 12, 290 (2022). https://doi.org/10.1038/s41598-021-04373-7  

---  

최종 수정 날짜 : 23.02.14 22:23  

이전에 작성했던 내용은 큰 틀이었는데, 이번엔 용어 하나하나 비집고 들어가며 검색해보고, 논문의 문장을 이해되기 쉽도록 문장을 좀 더 추가하였다.  

본인이 이 논문을 읽는 목적은 머신러닝 기술이 생물학적 데이터에 어떻게 적용되는지 익히기 위해서이므로, method에 내용이 치중되었다.    

## Introduction  

Atopic dermatitis (AD) : type of inflammatory skin disease resulting in red, itchy, swollen, cracked, and irritated skin, which is a severe from of eczema  

=> 어린이들에게서 특히 발병률이 높다.  

=> AD 아동들은 천식, 정신질환등 다른 질병을 앓을 수 있다.  

=> 이를 치료할 수 있는 일부 약물들이 있지만, systemic immunosuppressant (전신 면역억제제)에 국한되어 있고, 일부 효능이 좋은 약물들은 그 비용이 비싸다.   

---  

최근에 colonic epithelial cell (결장 상피 세포)가 host-microbial interation (숙주-세균 간 상호작용)에 연관되어 중요한 역할을 한다는 보고가 있다.  

이에 더하여, host genes expression과 gut microbiota의 상관괸계 분석은 human disease의 예측과 발병을 시키는데 중요한 기회를 가진다. AD에서도 말이다.    

![Imgur](https://imgur.com/21W6mwr.jpg)  

[사진 출처 링크](https://m.edaily.co.kr/news/Read?newsId=02207446625901104&mediaCodeNo=257&utm_source=https://www.google.com/)  


=> 예를 들어 AD에서는 IL-17 유전자와 Streptococcus간에 관련성 보고가 있다.  

=> 그러나 이 gut transcriptome과 microbiota에 기반한 머신 러닝 AD 예측 분석 연구는 그 수가 적다.  



---  

AD를 판별할 수 있는 기준은 이미 worldwide하게 개발이 되었고, 사용이 되고 있지만, 사람에 따라 나타날 수 있는 morphology, distribution, irregularity가 워낙 다양하고, 객관적이고 효과적인 지표가 부족하여 질병의 중증도 평가에 문제점이 되고 있다. 


이 문제는 의사가 AD의 진단, 심각도에 따라 치료에 대한 결정을 내릴 필요가 있기때문에 더 부각된다.  

---  

이 논문에서는 장 상피 결장 세포의 전사체와 장내 미생물 데이터를 이용하여 AD의 정확하고 자동화된 진단을 위한 machine learning classifier를 개발한다.  

---

## Method  


### Sample Collection and disease diagnosis  

이 연구에서, 연구자들은 88 paitents (AD), 73 healty, 총 161 subject의 gut epithelial colonocytes와 gut microbiota data의 transcriptome을 수집했다.  

=> 환자 AD의 심각성에 따라 SCORAD index가 매겨졌다.   

=> ImmunoCAP system을 이용하여 혈장 내 immunoglobulin level이 측정되었다.  

> immunoglobulin과 atopy의 관련성은 다음의 인용문장에서 찾아볼 수 있다.  

> The serum level of immunoglobulin E (IgE) was found to be significantly greater than normal ...  (Lennart Juhlin et al., 1969, *Arch Dermatol*)  

### Transcriptome and microbiota data  

Transcriptome data는 각 대변 표본의 exfoliated colonocytes로부터 mRNA를 얻었다.  
Microbiota data는 fecal sample에서 얻었다.  

> exfoliated colonocyte가 뭐냐면, 사람이 분변을 싸면 거기에 딸려나오는 사람의 장 상피층이다. transcriptome data는 사람의 전사체 시퀀스 데이터이기 때문에, 사람의 세포가 필요하다. 그래서 사람의 장 상피세포로부터 얻은것이다. 반면 microbiota data는 박테리아, 바이러스, 곰팡이를 포함하여 환경에서 발견되는 모든 미생물들의 시퀀스 데이터 (*물론 여기서는 박테리아가 중심이라서 박테리아 시퀀스들을 찾을 것이다.*) 를 총칭한다. 특히 gut에 있는 미생물이 필요하기 때문에, 분변에 딸려나오는 미생물들을 찾기위해 분변 샘플을 채취하였을 것이다.   

==> 이 샘플에서 16S rRNA gene을, 두 개의 시퀀싱 플랫폼에서 얻었고,  

> 두 플랫폼이란 얘기는 여기서 Roche/454 FLX Titanium system과 MiSeq을 의미한다. 즉 이것은 뽑아낸 데이터를 시퀀싱하기 위해 필요한 기계의 이름일 뿐이다. 두 개의 기계를 썼다.   

==> 두 시퀀싱 플랫폼 간의 직접적인 비교가 어렵기 때문에, common phylum과 genus까지에만 중점을 두었다.    

> 조사를 조금 해봤는데, 다음 문장을 읽어보자.  

> Roche 454 sequencing can sequence much longer reads than illumina    

> 즉 454 sequencing이 더 길게 시퀀스를 읽을 수 있다. 하지만 454 sequencing의 단점은 'AAAAA'와 같이 homopolymer repeat을 sequencing 하기가 어렵다는 것이다. **즉 두 개의 시퀀싱 플랫폼간에 시퀀싱 방식과 장단점이 다르다.** 즉 시퀀스의 세세한 정확도가 높지는 않겠고, 따라서 논문에서는 strain, species까지 구별하긴 어려웠을 것이다.  

### AD machine learning classifier   

연구자들은 transcriptome과 microbiota data를 사용하여 아토피 피부염 상태를 예측하는 지도 학습 머신러닝 파이프라인을 구축했다.  

이때 파이프라인은 Python3.7, scikit-learn package를 사용했다.  

#### Preprocessing 

transcriptome data set (n = 161)  
microbiota data set (n = 161)  
==> both sample (n = 160)  

> *여기서 하나의 사람당, 두 개의 data (transcriptome data, mircrobiota data)가 존재해야 한다. 그것을 both sample이라하는데, 그것이 160개라는 것이다.*  

논문에서는 누락된 데이터가 transcriptome data 한 개, microbiota data 한 개가 누락 되었다고 언급한다.  

> 근데 이렇게 되면 아까 설명한것과 좀 다른 것이 아닌지.. 누락된 데이터가 두 개이므로, both sample은 159개가 되어야 맞는 것인데, 일단 method 중심으로 읽기로 했으므로 이건 좀 두고 봐야 할 것 같다.  

sample 1 : transcriptome data (O) - microbiota data (X)  

sample 2 : transcriptome data (X) - microbiota data (O)  

따라서 연구자들은 이 결측치 데이터를 완전무작위결측 (MCAR)로 판단하고, 결측치를 다른 데이터의 mean값으로 처리했다. (이 처리는 microbiota data만 했고, transcriptome data는 대치할 gene 수가 많아서 해당 샘플은 제거해버렸다.)   

> 여기서 완전무작위결측이 잘 기억이 안나서 조사좀 해봤다. (일부 내용 ChatGPT 검색)   

> MCAR (Missing Completely at Random, 완전무작위결측) :  결측값이 변수의 성격과 전혀 무관하게 발생한 경우  

> MAR (Missing at Random, 무작위결측) : 결측의 발생은 오로지 관측된 값에 의해서만 설명되며, 결측된 값과는 독립일 것이라 가정한 상태  

> NMAR (Not Missing at Random, 비무작위결측) : 결측값이 전혀 임의적으로 발생한 것이 아니며, 관측된 값과 결측된 값 모두에 영향을 받는 상태  

> 사실 위 세 개 용어를 읽어봐도 뭔 말인지 이해가 안간다. 조금 쉽게 예를 들어보겠다.  

> 상황 하나를 가정해보자. 피실험자 (아토피 or 정상)가 병원에 가서 샘플을 제공하고, 연구자는 그 샘플들을 다 받아서 시퀀싱 돌린 뒤에 주어진 시퀀스가 아토피에 가능성이 있는지 정상에 가능성이 있는지 예측하는 머신러닝 모델을 만든다고 가정하자.   

> MCAR example : 피실험자 (아토피 or 정상)가 연구자의 병원에 가서 샘플을 제공했는데, 연구자가 그 샘플 해동시켜놓고, 해당 샘플이 그 장소에 있는지는 인식 못한채 그 샘플 위에 뜨거운 컵라면을 올려놓았다 (물론 그럴리는 절대 없겠고, 예시이다). 그럼 샘플이 망할 것이다. 그렇다면 이 결측은 피실험자가 아토피인지 정상인지 상관없이 순전히 연구자의 실수로 인한 것이므로, 즉 결측값이 변수의 성격과 전혀 무관하게 발생했기 때문에 MCAR이다.  

> MAR example : 피실험자 (아토피 or 정상)가 샘플을 오늘 병원에가서 제출하기로 했는데, 까먹고 안 갔다. 이렇게 된다면 아토피 샘플이나 정상 샘플 중 하나가 빌 것이고, 그 결측의 발생은 피실험자가 샘플을 제출하지 않았기 때문이라는 것을 연구자는 이미 알고 있다. 즉 관찰 가능한 변수 (아토피 9 정상 10)에 의해서 결측의 발생이 일어났음을 알고 있기 때문에 MAR이다.   

> NMAR example : 만약에 아토피가 걸린 사람들이, 자신이 아토피가 걸린 것을 숨기고 정상으로 속여서 제출하는 경향이 많다는 연구결과가 있다고 해보자. 그렇다면 연구자들은 피실험자가 아토피인지 정상인지 모른다 (그냥 정상으로 보이긴 하지만 여전히 의심쩍다). "분명히 내가 모집할 때는 아토피 10명, 정상 10명이었는데, 샘플 수집하고 보니까 정상이 15명, 아토피가 5명이다. 왜 아토피에서 결측이 5개가 났지? 정상 중에 아토피가 숨어있나?"  이런 상황이 되면 관찰된 변수도 결측에 영향을 받고 있는걸 알고, (아토피 5, 정상 15), 관찰불가능한 변수도 결측에 영향을 받고 있는걸 알기 때문에, (아토피는 정상으로 숨길 가능성이 높다.) 즉 **인과구조**에 의해 발생했기 때문에 세세하게 추가조사를 해야하는 NMAR이다.

> 혹시 예시가 잘못되었다면 댓글 부탁드린다.  

이후 이들을 training set (n = 131)과 test set (n = 30)으로 split 했다.  
==> 층화추출법 사용  
==> 이후 min-max normalization  

> *왜 161개가 되는지 모르겠다. 하나만 삭제됬으면 160개가 아닌가?*  


#### Feature selection  

질병이 40000개 이상의 유전자와 강하게 연관될 가능성은 거의 없기 때문에 대부분의 유전자는 질병과 관련이 없거나 무시할만한 영향을 가진다.  

따라서 훈련 데이터 세트에 대한 feature selection이 필요하다.  

고려한 측면은 다음과 같다.  

1) optimal number of features to be selected in the entire dataset   

2) exact features chosen from the original training set   

일반적으로 널리 사용되는 feature selection에는 세 가지 유형이 있다.  

1) Filter  

2) Wrapper  

3) Embedded    

카이제곱 검정, RFE (Recursive Feature Elimination), RFC (Random Forest Classifier)가 효율적이기 때문에 각 유형별로 이 세 가지 방법을 선택했다.  

이때 RFE는 모델의 결과가 필요하므로 (*voting도 해주나..?*)  
로지스틱 회귀, SVM및 RFC를 모델로 선택했다.  
==> 앞으로 이 조합들을 각각 LR-RFE, SVM-RFE, RFC-RFE로 부른다.  

이후에는 overfitting 문제를 해결한다.    

----------------------------------------------------------------------------------  

Cross-validation은 overfitting을 방지하는데 중요하다.  

feature selection을 위한 CV는 두 개의 plan을 사용했다. (Plan A, Plan B)  

Plan A : 5 by 5 nested cross-validation  

1. outer CV, inner CV 진행  
outer CV와 inner CV의 개념은 해당 논문의 Supplement Fig. S1A와, [이 링크](https://seeyapangpang.tistory.com/9)를 살펴보면 좋을 것이다.  

이후 outer CV에서 겹치는 feature를 계산했다. (*inner CV에서의 검증보다 outer CV에서의 검증이 잘 될 때 더 유익하다.*)  

최종 겹쳐진 집합의 feature 수를 nA로 나타내고, training split CV에서의 모든 feature은 분류모델에 의해 할당된 가중치에 따라 순위가 매겨진다.  


