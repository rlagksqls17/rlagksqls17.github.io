---
layout: post
title: "[통계] Testing eqaulity of coefficients from two different regressions - 2"
categories: project
tags: test
last_modified_at: 2025-01-16T18:00:00~23:00
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
[저번 글](https://rlagksqls17.github.io/project/2024/11/30/LR_ztest.html) 에서 조건에 따라 서로 다른 효과를 보이는 regression 식을 어떻게 식별할 수 있을지에 대한 해결법을 제시하였고, 결과적으로 다음의 식을 이용하기로 했었었다.  

$$z=\frac{\beta_{conditionA} - \beta_{conditionB}}{\sqrt{\sigma^2_{conditionA} + \sigma^2_{conditionB}}}$$  

위 식을 파이썬으로 구현한 코드를 아래 기록해 놓으려고 한다.  

## 코드  

함수로 작성했고, input값은 두 개의 데이터프레임이다. 각 데이터프레임에는 다음과 같은 컬럼이 있다고 가정한다.  

column "age" : 나이와 관련된 컬럼 (서브 독립변수)  
column "sex" : 성별과 관련된 컬럼 (서브 독립변수)  
column "cov1" : 다른 공변량 1 (키나 몸무게와 같은)  (서브 독립변수)    
column "variable" : 선형회귀에서 beta coefficient를 직접적으로 비교하고자 하는 메인 독립변수  
column "predict" : 선형회귀에서의 종속변수, input값으로 넣어줄 여러 컬럼 중 하나로, 이 값 또한 넣어줌으로써 exp 값을 예측하려고 함  


```python  
def comparison_beta(df1, df2):
	# 주어진 값을 숫자형으로 변형
    df1 = df1.apply(pd.to_numeric) 

	# 종속변수
    X = sm.add_constant(df1[['age', 'sex', 'cov1', 'variable']])
    
	# 독립변수
	Y = df1['predict']

	# df1 에서의 linear regression model  
    model = sm.OLS(Y, X).fit()

	# coefficient 및 표준편차 계산
    df1_coef_genotype = model.params['variable']
    df1_stde_genotype = model.bse['variable']


	# df2도 위의 df1과 같이 수행
    df2 = df2.apply(pd.to_numeric)
    X = sm.add_constant(df2[['age', 'sex', 'cov1', 'variable']])
    Y = df2['predict']
    model = sm.OLS(Y, X).fit()
	
	# coefficient 및 표준편차 계산
    df2_coef_genotype = model.params['variable']
    df2_stde_genotype = model.bse['variable']

	# 이전 글에서 정의 내린 식 활용한 z값 및 pvalue 계산
    z = (df1_coef_genotype - df2_coef_genotype) / np.sqrt((df1_stde_genotype ** 2) + (df2_stde_genotype ** 2))
    p_value = 2 * norm.sf(abs(z))

    return p_value
```  


