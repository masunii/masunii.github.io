---
title: "edwith_python_(15)"
date: 2020-06-16 10:00:00
categories: "STUDY"
comments: TRUE
toc: TRUE
toc_sticky: TRUE
---

edwith 부스트코스 [파이썬으로 시작하는 데이터 사이언스](https://www.edwith.org/boostcourse-ds-510/joinLectures/28137) 를 공부하는 과정입니다.    
  
지난 포스트 :point_right: [edwith_python_(14)lineplot으로 판매 데이터 시각화하기](https://masunii.github.io/study/edwith_K-beauty(3)/)  

--------------------------------------------------------

### 6.4 데이터 집계하기  

##### 피벗테이블로 "국가(대륙)별", "연도"별 합계 금액을 표 형태로 구해서 result 변수에 담기
```javascript
result = df_fashion.pivot_table(index="국가(대륙)별", columns="연도", values="백만원", aggfunc="sum")
result
```

`*결과*`  

|          연도 |    2014 |     2015 |     2016 |     2017 |     2018 |     2019 |
|--------------:|--------:|---------:|---------:|---------:|---------:|---------:|
|  국가(대륙)별 |         |          |          |          |          |          |
|       EU      |  4485.0 |   3374.0 |   4899.0 |   3736.0 |   4114.0 |   3694.0 |
|      기타     |  9683.0 |   7248.0 |   5918.0 |  14387.0 |  23901.0 |   6309.0 |
|     대양주    |  3392.0 |   2349.0 |   3401.0 |   2266.0 |   2725.0 |   2387.0 |
|      미국     | 33223.0 |  38066.0 |  48451.0 |  50353.0 |  47875.0 |  55035.0 |
| 아세안(ASEAN) | 14936.0 |  19639.0 |  24478.0 |  22671.0 |  23068.0 |  31217.0 |
|      일본     | 48960.0 |  57594.0 |  79905.0 |  90584.0 | 136800.0 | 134243.0 |
|      중국     | 57531.0 | 142339.0 | 190932.0 | 225407.0 | 288848.0 | 330254.0 |
|     중남미    |   975.0 |    616.0 |    649.0 |    762.0 |    576.0 |    543.0 |
|      중동     |  1172.0 |   1018.0 |    968.0 |    772.0 |    879.0 |    924.0 |  

> 언제 어느 국가의 판매액이 높고 낮은 지 파악하기 어려움  

### 6.5 연산 결과를 시각적으로 나타내기
##### heatmap을 이용하여 피벗테이블로 구한 결과를 시각적으로 표현하기
```javascript
plt.figure(figsize=(8,4))
sns.heatmap(result, cmap="Blues", annot=True, fmt=".0f")
``` 

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84797628-01500c80-b035-11ea-9283-c4e0e95148ee.png" width="70%">  

> 중국에서 구매액이 가장 많고, 2019년으로 갈수록 판매액이 증가함  

> annot=True : heatmap에 숫자도 나타냄  
fmt=".0f" : 소수점없이 float 형태로 나타내기  


## 7. 전체 상품군별로 온라인쇼핑 해외직접판매액은 증가했을까?  

##### 위의 판매유형별 데이터의 "계"만 모은 df_total 변수를 통한 "연도"별 판매액을 시각화 하기
```javascript
sns.barplot(data=df_total, x="연도", y="백만원")
```
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84797784-2ba1ca00-b035-11ea-85ee-1bdf7c66d318.png" width="50%">  

##### "연도"별 판매액을 그리고, "국가(대륙)별"로 구분하여 나타내기  
```javascript
plt.figure(figsize=(8,4))
sns.lineplot(data=df_total, x="연도", y="백만원", hue="국가(대륙)별")
```

`*결과*` 
<img src = "https://user-images.githubusercontent.com/50826051/84797874-43794e00-b035-11ea-9655-9b3e82ce05c9.png" width="70%">  


##### "연도"별 판매액을 그리고, "상품군별"로 다른 색상으로 표현하기
```javascript
plt.figure(figsize=(8,4))
sns.lineplot(data=df_total, x="연도", y="백만원", hue="상품군별")
plt.legend(bbox_to_anchor=(1.01, 1), loc=2, borderaxespad=0.)
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84797957-5b50d200-b035-11ea-8d7c-cff6605852b3.png" width="70%">  



**결론** (?)    
- 최근 5년 데이터 상 K-beauty와 관련된 '화장품'과 '의류'의 해외 판매액은 증가 추세를 보임.  
- 특히 여러 국가 중 중국에서의 판매액 성장이 두드러짐.   







