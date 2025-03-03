---
layout: post
title: "[통계] eQTL 분석과 log likelihood test"
categories: project
tags: test
last_modified_at: 2025-02-25T18:00:00~23:00
---  


<script type="text/javascript" async
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-MML-AM_CHTML">
</script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        extensions: ["tex2jax.js"],
        jax: ["input/Tex", "ourput/HTML-CSS"],
        tex2jax: {
            inlineMath: [ ['$', '$'], ["\\(", "\\)"] ],
            displayMath: [ ['$$', '$$'], ["\\[", "\\]"] ],
            processEscapes: true
        },
        "HTML-CSS": { availableFonts: ["TeX"] }
    });
</script>



## 개요  

[저번 글](https://rlagksqls17.github.io/project/2025/01/16/comparison_beta_coefficient.html)에서는 특정 독립변수가 종속변수에 미치는 영향이 서로 다른 조건에서의 두 linear regression에서 어떻게 다른지를 test하는 방법과 코드를 포스트했다. 이번 글에서는 **독립변수 set이 서로 다른 두 linear regression 중 어떤 regression formula가 더 종속변수를 잘 설명할 수 있는가를 test**하는, log-likelihood test에 대해 포스트하려고한다.  

모든 통계 방정식을 여기에 다 적지는 않았고, 대강 원리가 어떻게 돌아가는지만 기록해두려고 했다.  

출처: 데이터 분석을 위한 통계 (Bruce P. & Bruce A., 2021)  


## linear regression이란?    

![Imgur](https://imgur.com/z3rfhlY.jpg)  

lineare regression은, 그림에서처럼 일반적으로 한 변수와 다른 변수의 value 사이의 어떤 관계, 예를 들면 X (독립변수, 여기서는 SNP)가 증가하면 Y (종속변수, 여기서는 expression)가 증가, 혹은 반대로 X가 증가하면 Y는 감소하는 식의 관계에 대한 정량적 모델을 제공한다. 일반적으로, genotype에따라 expression이 변화하는가에 대한 linear regression 식인, eQTL (expression quantitative loci) lineare regression 식을 디자인 할때는, 독립변수인 genotype matrix와 종속변수인 gene expression matrix를 input으로 넣어줘야 그 변수 간의 관계에 대한 모델링이 가능하다. 즉, 위 그림과 같이  input으로 넣어진 데이터를 기반으로, 특정 독립변수에 대해 종속변수의 관계에 대한 정량적 척도인 회귀선이 예측된다.  

이때 그 회귀선의 기울기가 크면 클수록 genotype이 expression에 미치는 영향이 크다고 할 수 있다. 이 크기의 정도를 beta coefficient라 한다. linear regression은 이렇게, 주어진 독립변수를 통해 종속변수를 예측하는 용도 뿐만 아니라, 각 종속변수별 독립변수와의 관계 측정의 목적으로도 쓰일 수 있다.  


## 최소제곱법과 log-likelihood test  

![Imgur](https://imgur.com/PzBbM6C.jpg)

한편, linear regression에서 많이 쓰이는 알고리즘은 다음과 같다. 즉 모델을 만드는데 쓰인 expression 실제값 (종속변수)과, 완성된 모델에 genotype data (독립변수)를 넣어줬을 때의 회귀선 예측값 차이를 잔차라고 하는데, 이 잔차들의 제곱합을 최소화하는 방법인 **최소제곱법 (method of least squares)** 을 이용해 회귀선을 그림으로써 주어진 모델이 독립변수와 종속변수 간의 관계를 잘 정립할 수 있게끔 해준다. 지금 위의 그림이 어떻게 보면 잔차가 최소화되도록 회귀선을 그린 것이라고 볼 수 있다.  

![Imgur](https://imgur.com/SiS5L58.jpg)  

사실, gene expression에 관계가 있는 변수는 genotype 뿐만 아니라 TF-expression, TF-SNP interaction 등이 gene expression과 관계가 있을 수 있다.  

그렇다는 가정하에서, 만약 genotype, TF expression,  gene expression의 regression을 의미하는 Formula1과, TF-SNP의 상호작용, gene expression의 regression을 의미하는 Formula2를 서로 비교했을 때, 그 잔차들의 제곱합이 Formula2에서 더 적어진다면, 즉 독립변수와 종속변수의 관계를 의미하는 회귀선에 주어진 점들이 최대한 가깝게 찍혔다면, 이는 genotype이라는 변수보다 TF-SNP interaction의 변수가 expression과의 관계를 더 잘 설명한다고 해석할 수가 있게 된다.  

즉 특정 변수를 다른 변수로 대체하거나 추가했을 때 전체 모델의 잔차제곱합이 적어지냐, 많아지냐를 확인함으로써, 어떤 변수가 expression과의 관계를 더 잘 설명하는지를 알 수 있는 것이다. linear regression에서 모델의 설명력을 의미하는 log likelihood를 계산하는 방정식 자체가 잔차제곱합 value와 밀접하게 관련되어 있고,   

따라서 각기 다른 변수 set을 가지는 두 linear regression 식에서의 log likelhood를 비교함으로써, 어떤 변수 set이 expression 변화에 중요한 기여를 하는지 평가할 수 있다 (카이제곱검정 등). 여기까지 log likelihood ratio test의 기초적인 개념을 간략하게 정리해보았다. 
