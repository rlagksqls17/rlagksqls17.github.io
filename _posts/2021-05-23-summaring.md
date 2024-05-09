---
title: "데이터 결합 및 요약"
excerpt: "last_modified_at: 2021-05-23"
categories:
  - Blog
layout: post
tags:
  - Blog
last_modified_at: 2021-05-23T08:19:00~21:00
---  

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Frlagksqls17.github.io%2Fblog%2Fsummaring%2F&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)  

데이터 결합 및 요약에 대한 R과 파이썬 언어를 정리해보았다.  
목차는 아래 내용과 같다.  
[학습 링크](https://colab.research.google.com/drive/1ZZ9ObK68h2XGQj7UwDez6aFCGh5Sh1lR?usp=sharing)  

데이터 결합  

> 행 결합  
>> rbind()와 pd.concat()  

> 열 결합  
>> cbind()와 pd.concat()  

> 병합  
>> merge()와 pd.merge()  

데이터 요약  

> aggregation  
>> aggregate()와 DataFrame.groupby().함수   

> 빈도표  
>> table()과 DataFrame.groupby().함수  

> 상대빈도표  
>> prob.table()과 pd.crosstab()  

> 조건에 맞는 변수값 조회  
>> subset()과 pd.query()  

> apply 계열 함수  
>> [apply, lapply, sapply, vapply, tapply] 와 pd.apply() + lambda x  
