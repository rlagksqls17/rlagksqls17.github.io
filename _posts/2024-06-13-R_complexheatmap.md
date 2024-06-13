---
layout: post
title: "R - Complexheatmap 이용한 시각화"
categories: proejct
tags: test
last_modified_at: 2024-06-13T08:00:00~18:00
---


## 핵심 요약
Complexheatmap을 사용하면 좀 더 쉽고 이쁜 히트맵을 만들 수 있다.  

![Imgur](https://imgur.com/xlfs6Cb.jpg)  


위와 같은 시각화를 얻어낼 수 있다.
극히 일부분의 데이터이기 때문에 분석은 불가하지만 시각화에 도움이 되셨으면 좋겠다.  

X축 레이블은 따로 표기하지 않았고, 각 샘플을 의미한다.
Y축 레이블은 각각의 종 이름이다.  
색상은 value, 즉 해당 종의 전체 리드 수를 log 10으로 변환해서 그라데이션으로 나타냈다.  
heterogeneity를 어느정도 확인할 수 있다.  


## Complexheatmap 이용한 히트맵 그리기  
저번에 논문 발표하고나서, 내 논문 데이터 해석에 대해서, 심사위원 교수님이 상대풍부도가 아닌 절대풍부도로도 확인해보았냐고 물어보셨다. 그렇게 질문하신 이유는 다음과 같다.  

아직 발간이 안된 논문이라 완전한 내용을 여기에 공유할 수는 없지만, 대략적으로 설명해보자면, 나는 약물을 섭취하기 전 후의 장내 균 구성변화를 연구했었다. 이때 약물을 섭취하고 나서 어떤 세균의 풍부도가 유의하게 증가함을 확인하였지만, 그 풍부도가 상대풍부도, 즉 해당 세균 A의 풍부도만 고려하는 것이 아닌, 다른 전체 세균의 풍부도에 비해 그 세균 A의 풍부도는 상대적으로 얼마나 되는지를 나타내는 척도로 살펴봤었다. 즉 간단하게 말하면 상대풍부도는 전체 세균 내 특정 세균 A가 존재하는 비율 (%)이다.  

따라서 그 세균 A의 절대적인 양을 볼 수는 없기 때문에, 아무리 상대풍부도가 증가했다고 말한다한들, 비율이기 때문에 데이터 해석에 있어서 왜곡이 발생할 수 있다.   

따라서 교수님이 이 점을 파악하시고 "절대풍부도로도 확인을 해보았냐"고 질문하신 것 같다.  

---  

물론 절대풍부도로도 이미 확인을 해봤기 때문에, 발표 당일에도 확인을 했다고 답변을 드렸다.  

근데 시각화는 따로 하지 않아서, 오늘 시간을 내서 시각화를 해보려고 한다.  

---  

## 데이터 확인  

먼저 데이터는 다음과 같다. 각 세균들의 read count 데이터인데, log10으로 변환을 해줬다. 많은 column과 raw가 있는데 여기서는 축약했다.

|제목|샘플 1|샘플 2|샘플 3|...|샘플 100|
|:---:|:---:|:---:|:---:|:---:|:---:|
|종 1|2.2|NaN|3.2|NaN|5|
|...||||||
|종 1200|1.2|3|2.5|4.0|4.1|  


각 raw : 식별된 종을 말한다.  
각 column : 샘플을 말한다.  
각 value : 해당 샘플에서 식별된 종의 sequencing read count (절대풍부도와 같음)을 log10으로 변환한 값을 의미 (즉 해당 종의 DNA일 것으로 추정되는 데이터의 수를 의미)한다.  

모든 기존의 raw 데이터는 사전에 python pandas 및 numpy 사용해, 위와 같은 테이블 형식으로 만들어주었다.  

---  

## R visualization  

이후 R로 시각화를 한다. 

각 세균의 value를 바탕으로 heatmap을 그릴건데, [complexheatmap](https://jokergoo.github.io/ComplexHeatmap-reference/book/index.html)이 설명 가장 잘 되어 있고 사용하기 편하다. 무엇보다 시각화가 유연하다.  

complexheatmap은 R package이다. 따라서 다음코드를 이용해서 직접 R 내에서 설치를 해줘야한다.  

```R
if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

BiocManager::install("ComplexHeatmap")

install.packages("circlize")
install.packages("dplyr")
```  
  
---

이후 라이브러리를 불러온다.  

```R
library(ComplexHeatmap)
library(circlize)
library(dplyr)
```  

---  

그런다음 엑셀데이터를 불러오고, matrix 형식으로 바꾼다음, 자르고 싶은 행과 열은 자르고, 행의 name을 본인이 원하는 방식으로 설정해준다.

```R
occ_df_raw = read.csv("테이블이 있는 경로/테이블 이름") 
occ_data <- as.matrix(occ_df_raw)
occ_data = apply(occ_data[,-1], 2, as.numeric)
rownames(occ_data) <- occ_df_raw[, 1]
```

---  


시각화를 시킬 데이터프레임을 만들어준다.
내가 분석할 데이터는 블로그 기록용으로 전체 중 일부 샘플만 잘라왔다, 샘플이 각 다른 그룹에 속해있다. 즉 같은 샘플이지만 약 섭취 1개월 후, 2개월 후, 3개월 후 등의 시간에 따른 그룹 (longitudinal)으로 다시 나뉘어 있고, 대조군 또한 존재하기 때문에, 총 6개 그룹으로 구성되어 있다.  

(실험군 : 약 섭취 1개월, 2개월, 3개월  
대조군 : 약 섭취 1개월, 2개월, 3개월).

이전의 처음 불러왔던 데이터프레임은 그 그룹들 전체가 속해있는 것이기 때문에, 그룹별로 시각화를 나눠서 수행하고 싶다면, 바로 위 문장에서 기록한 그룹 정보처럼 각 그룹별로 데이터프레임을 다시 나눠줘야 한다.   

```R
G1 <- occ_data[, c('sample 1', 'sample 2', 'sample 3', 'sample 4', 'sample 5', 'sample 6', 'sample 7', 'sample 8')]
G2 <- occ_data[, c('sample 1', 'sample 2', 'sample 3', 'sample 4', 'sample 5', 'sample 6', 'sample 7', 'sample 8')]
G3 <- occ_data[, c('sample 1', 'sample 2', 'sample 3', 'sample 4', 'sample 5', 'sample 6', 'sample 7', 'sample 8')]

P1 <- occ_data[, c('sample 1', 'sample 2', 'sample 3', 'sample 4', 'sample 5', 'sample 6', 'sample 7', 'sample 8')]
P2 <- occ_data[, c('sample 1', 'sample 2', 'sample 3', 'sample 4', 'sample 5', 'sample 6', 'sample 7', 'sample 8')]
P3 <- occ_data[, c('sample 1', 'sample 2', 'sample 3', 'sample 4', 'sample 5', 'sample 6', 'sample 7', 'sample 8')]

G1_df <- as.matrix(G1)
G2_df <- as.matrix(G2)
G3_df <- as.matrix(G3)
P1_df <- as.matrix(P1)
P2_df <- as.matrix(P2)
P3_df <- as.matrix(P3)
```

---  

그다음에는 complexheatmap 패키지 이용한 코드를 작성한다.  

6개 각각의 시각화 코드를 작성했다.

클러스터링은 따로 안해줬고 (cluster_columns = FALSE), 
척도 및 척도별 색상은 colorRamp2를 이용해서 설정한다.  
NaN값이 있을경우 회색으로 설정해준다 (na_col = "gray").

이외 fontsize 및 히트맵 각 셀별 높이와 폭을 조정 (unit() 함수 및 gpar() 함수 이용)해준다.

```R
G1_dw = Heatmap(G1_df, cluster_columns = FALSE, cluster_rows = FALSE, col = colorRamp2(c(4, 4.75, 5.5, 6.25, 7),
                                                                                       c('#FFFFFF','#F5DA81', '#FF8000','#FF0000', '#B40404')),
                na_col = "gray", border = TRUE, row_names_side = 'left',
                cell_fun = function(j, i, x, y, width, height, fill) {
                  grid.rect(x = x, y = y, width = width, height = height, 
                            gp = gpar(col = "#848484", fill = 0))},
                column_title = "G1",
                row_title = "",
                width = unit(3.5, "cm"),
                height = unit(13, "cm"),
                column_title_gp = gpar(fontsize = 35),
                row_title_gp = gpar(fontsize = 35),
                column_names_gp = gpar(fontsize = 0),
                row_names_gp = gpar(fontsize = 15, fontface = 'italic'))

G2_dw = Heatmap(G2_df, cluster_columns = FALSE, cluster_rows = FALSE,  col = colorRamp2(c(4, 4.75, 5.5, 6.25, 7),
                                                                                        c('#FFFFFF','#F5DA81', '#FF8000','#FF0000', '#B40404')),
                na_col = "gray", border = TRUE, row_names_side = 'left',
                cell_fun = function(j, i, x, y, width, height, fill) {
                  grid.rect(x = x, y = y, width = width, height = height, 
                            gp = gpar(col = "#848484", fill = NA))},
                column_title = "G2",
                row_title = "",
                width = unit(3.5, "cm"),
                height = unit(13, "cm"),
                column_title_gp = gpar(fontsize = 35),
                row_title_gp = gpar(fontsize = 35),
                column_names_gp = gpar(fontsize = 0),
                row_names_gp = gpar(fontsize = 15, fontface = 'italic'))

G3_dw = Heatmap(G3_df, cluster_columns = FALSE, cluster_rows = FALSE, col = colorRamp2(c(4, 4.75, 5.5, 6.25, 7),
                                                                                       c('#FFFFFF','#F5DA81', '#FF8000','#FF0000', '#B40404')),
                na_col = "gray", border = TRUE, row_names_side = 'left',
                cell_fun = function(j, i, x, y, width, height, fill) {
                  grid.rect(x = x, y = y, width = width, height = height, 
                            gp = gpar(col = "#848484", fill = NA))},
                column_title = "G3",
                row_title = "",
                width = unit(3.5, "cm"),
                height = unit(13, "cm"),
                column_title_gp = gpar(fontsize = 35),
                row_title_gp = gpar(fontsize = 35),
                column_names_gp = gpar(fontsize = 0),
                row_names_gp = gpar(fontsize = 15, fontface = 'italic'))


P1_dw = Heatmap(P1_df, cluster_columns = FALSE, cluster_rows = FALSE,  col = colorRamp2(c(4, 4.75, 5.5, 6.25, 7),
                                                                                        c('#FFFFFF','#F5DA81', '#FF8000','#FF0000', '#B40404')),
                na_col = "gray", border = TRUE,
                cell_fun = function(j, i, x, y, width, height, fill) {
                  grid.rect(x = x, y = y, width = width, height = height, 
                            gp = gpar(col = "#848484", fill = NA))},
                column_title = "P0",
                row_title = "Genes",
                width = unit(3.5, "cm"),
                height = unit(25, "cm"),
                column_title_gp = gpar(fontsize = 35),
                row_title_gp = gpar(fontsize = 35),
                column_names_gp = gpar(fontsize = 0),
                row_names_gp = gpar(fontsize = 8))

P2_dw = Heatmap(P2_df, cluster_columns = FALSE, cluster_rows = FALSE,  col = colorRamp2(c(4, 4.75, 5.5, 6.25, 7),
                                                                                        c('#FFFFFF','#F5DA81', '#FF8000','#FF0000', '#B40404')),
               na_col = "gray", border = TRUE,
               cell_fun = function(j, i, x, y, width, height, fill) {
                 grid.rect(x = x, y = y, width = width, height = height, 
                           gp = gpar(col = "#848484", fill = NA))},
               column_title = "P1",
               row_title = "Genes",
               width = unit(3.5, "cm"),
               height = unit(25, "cm"),
               column_title_gp = gpar(fontsize = 35),
               row_title_gp = gpar(fontsize = 35),
               column_names_gp = gpar(fontsize = 0),
               row_names_gp = gpar(fontsize = 0))

P3_dw = Heatmap(P3_df, cluster_columns = FALSE, cluster_rows = FALSE,  col = colorRamp2(c(2, 3, 4, 5, 6),
                                                                                        c('#FFFFFF','#F5DA81', '#FF8000','#FF0000', '#B40404')),
               na_col = "gray", border = TRUE,
               cell_fun = function(j, i, x, y, width, height, fill) {
                 grid.rect(x = x, y = y, width = width, height = height, 
                           gp = gpar(col = "#848484", fill = NA))},
               column_title = "P2",
               row_title = "Genes",
               width = unit(3.5, "cm"),
               height = unit(25, "cm"),
               column_title_gp = gpar(fontsize = 35),
               row_title_gp = gpar(fontsize = 35),
               column_names_gp = gpar(fontsize = 0),
               row_names_gp = gpar(fontsize = 0))
```

---  

그렇게 하고나서 시각화를 해준다.  
위에서 작성한 6개 각각의 시각화 코드를, 하나의 combined_dw 변수에 저장해주고, draw() 함수를 이용한다. 

```R
combined_dw = G1_dw + G2_dw + G3_dw + P1_dw + P2_dw + P3_dw
draw(combined_dw, gap = unit(10, "mm"), heatmap_legend_side = "bottom", annotation_legend_side = "bottom")
```

---  

이후 ppt 사용해서 수정해주면..  

![Imgur](https://imgur.com/xlfs6Cb.jpg)  


위와 같은 시각화를 얻어낼 수 있다. 6개 그룹 중 3개 그룹에 대한 시각화이고, 
극히 일부분의 데이터이기 때문에 분석은 불가하지만 시각화에 도움이 되셨으면 좋겠다.    