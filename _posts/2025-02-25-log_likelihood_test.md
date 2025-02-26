---
layout: post
title: "[통계] log likelihood test"
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
[저번 글]("https://rlagksqls17.github.io/project/2025/01/16/comparison_beta_coefficient.html")에서는 특정 독립변수가 종속변수에 미치는 영향이 서로 다른 조건에서의 두 linear regression에서 어떻게 다른지를 test하는 방법과 코드를 포스트했다. 이번 글에서는 **독립변수 set이 서로 다른 두 linear regression 중 어떤 regression formula가 더 종속변수를 잘 설명할 수 있는가를 test**하는, log-likelihood test에 대해 포스트하려고한다.
