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

[다음 링크](https://angeloyeo.github.io/2020/02/13/Students_t_test.html) 보고 정리하였다.  
