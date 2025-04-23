---
layout: post
title: "[통계] student t distribution"
categories: project
tags: test
last_modified_at: 2025-03-12T18:00:00~23:00
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


## 스튜던트의 t 분포  
논문을 보다보면 단순히 t-test를 하는 것 뿐만이 아니라, 사람들이 스스로 고안해서 만든 여러 test로부터 t 통계량을 얻고 그것을 normal distribution이나 student t distribution에 적용하여 p-value를 적용하는 경우가 많았다. 이미 기본적인 개념을 알고 있긴 하지만 좀 더 상세하게 공부해보고 싶었다.   

[다음 링크](https://angeloyeo.github.io/2020/02/13/Students_t_test.html) 보고 요약 및 정리하였다.  

본인이 생각한 내용은 *기울임체*로 적어두었으니 공부에 참조바란다.  

검정 통계량은 통계적 가설의 진위 여부를 검정하기 위해 표본으로부터 계산하는 통계량이다.  

연구를 진행하다보면 두 표본 집단 간의 차이를 비교해야 할 때가 생길 수 있다. 가령, 신약을 개발한 뒤 약의 효과를 확인해보고 싶다면 어떤 실험을 계획할 수 있을까?  

우선 임상적으로 문제가 없는 사람들을 최대한 많이 모은 뒤, 그들을 두 그룹으로 최대한 랜덤하게 나눈 다음, 한 그룹에는 플라시보 약을 주고 한 그룹에는 새로 개발한 약을 준 다음 약효를 확인하면 될 것이다.  

그 결과 각 그룹에서 평균 수치를 얻을 수 있다면, 그 두평균값의 차이를 확인하면 통계적으로 비교한 것으로 볼 수 있을까? 아니다. 이 두 평균값은 표본 평균인데, 이 두 표본 평균들은 오차를 항상 수반하고 있다는 사실을 잊어선 안된다. 다시 말해 표본 평균이기 때문에 발생하는 오차를 염두해두어야 한다. 표본에서 얻은 평균이 모집단의 진짜 평균을 정확히 반영하지 않기 때문에, 오차라는 개념이 필요하고, 이 불확실성을 정량화한 것이 표준오차이다.   

따라서 차이에 불확실도를 나누는 방식으로 통계적 차이 지표를 만들면 된다.  

표본 평균 차이의 통계적 지표: 차이 / 불확실도  

여기서 말하는 불확실도는 두 표본 그룹의 평균간 차이의 불확실도를 의미한다.  

t 통계량의 의미는 위에서 확인한 통계적 지표와 같은 의미를 가진다. 즉 t 통계량은 대략적으로 이 정도 차이나고 오류는 이정도야 라는 것을 의미하는 일종의 지표라고 할 수 있다.  

t value는 다음과 같이 수식으로 쓸 수 있다.  

$$t = \frac{\bar{X_1} - \bar{X_2}}{S(\bar{X_1} - \bar{X_2})}$$
