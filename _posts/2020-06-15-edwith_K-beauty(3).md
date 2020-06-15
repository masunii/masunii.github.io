---
title: "edwith_python_(14)판매 데이터 시각화하기"
date: 2020-06-15 10:00:00
categories: "STUDY"
comments: TRUE
toc: TRUE
toc_sticky: TRUE
---

edwith 부스트코스 [파이썬으로 시작하는 데이터 사이언스](https://www.edwith.org/boostcourse-ds-510/joinLectures/28137) 를 공부하는 과정입니다.    
  
지난 포스트 :point_right: [edwith_python_(13)데이터 전처리하기](https://masunii.github.io/study/edwith_K-beauty(2)/)  

--------------------------------------------------------

## 6. K-Beauty 시각화  

### 6.1 전체 상품군 판매액  

##### "판매유형별" 데이터는 일부 기간에 대해 "계"만 존재하므로, "계" 데이터만 df_total 변수에 담기(면세점, 면세점 외 데이터는 제외)  
```javascript
df_total = df[df["판매유형별"] == "계"].copy()
df_total.head()
```

`*결과*`  

|    | 국가(대륙)별 |           상품군별 | 판매유형별 |     기간 | 백만원 | 연도 | 분기 |
|---:|-------------:|-------------------:|-----------:|---------:|-------:|-----:|-----:|
| 48 |         미국 | 컴퓨터 및 주변기기 |         계 | 2014 1/4 | 2216.0 | 2014 |    1 |
| 51 |         미국 | 가전·전자·통신기기 |         계 | 2014 1/4 | 2875.0 | 2014 |    1 |
| 54 |         미국 |         소프트웨어 |         계 | 2014 1/4 |   47.0 | 2014 |    1 |
| 57 |         미국 |              서 적 |         계 | 2014 1/4 |  962.0 | 2014 |    1 |
| 60 |         미국 |          사무·문구 |         계 | 2014 1/4 |   25.0 | 2014 |    1 |  

##### 연도별 판매액을 linplot으로 나타내기
```javascript
sns.lineplot(data=df_total, x="연도", y="백만원")
```  

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84617387-b4234c00-af09-11ea-9370-2e54c91dc686.png" width="50%">  

##### 연도별 판매액을 "상품군별"로 구분하여 linplot으로 나타내기  
```javascript 
sns.lineplot(data=df_total, x="연도", y="백만원", hue="상품군별")
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84617447-e634ae00-af09-11ea-9bce-824ea257f1eb.png" width="70%">  

> plt.legend( ~~) : 범례값의 위치 지정  
그래프 그리는 함수 뒤에 작성해야 적용됨  

##### 위에 그린 그래프를 자세히 보기 위해 서브플롯으로 나타내기  
```javascript
sns.relplot(data=df_total, x="연도", y="백만원", hue="상품군별", kind="line", col="상품군별", col_wrap=4)
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84619155-4b3ed280-af0f-11ea-86f0-c8ff071d5e2f.png">  

> 화장품 판매액이 다른 상품군에 비해 너무 커서 그래프 비교가 어려움  

##### isin 을 사용하여 "화장품"과 "의류 및 패션관련 상품" 만 제외하여 df_sub이라는 변수에 담아서 다시 서브플롯 나타내기  
```javascript
df_sub = df_total[~df_total["상품군별"].isin(["화장품", "의류 및 패션관련 상품"])].copy()

sns.relplot(data=df_sub, x="연도", y="백만원", hue="상품군별", kind="line", col="상품군별", col_wrap=3)
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84617553-4a577200-af0a-11ea-960d-cba5bb61411c.png">  

### 6.2 화장품의 온라인쇼핑 해외직접판매액

##### "상품군별"이 "화장품"인 데이터만 df_cosmetic이라는 변수에 담기
```javscript
df_cosmetic = df_total[df_total["상품군별"] == "화장품"].copy() 
df_cosmetic.head()
```

`*결과*`  

|     |  국가(대륙)별 | 상품군별 | 판매유형별 |     기간 |  백만원 | 연도 | 분기 |
|----:|--------------:|---------:|-----------:|---------:|--------:|-----:|-----:|
|  72 |          미국 |   화장품 |         계 | 2014 1/4 |  3740.0 | 2014 |    1 |
| 117 |          중국 |   화장품 |         계 | 2014 1/4 | 32235.0 | 2014 |    1 |
| 162 |          일본 |   화장품 |         계 | 2014 1/4 |  1034.0 | 2014 |    1 |
| 207 | 아세안(ASEAN) |   화장품 |         계 | 2014 1/4 |   398.0 | 2014 |    1 |
| 252 |            EU |   화장품 |         계 | 2014 1/4 |   937.0 | 2014 |    1 |  


##### "연도"와 판매액을 "분기"로 구분하여 lineplot으로 나타내기
```javscript
sns.lineplot(data=df_cosmetic, x="연도", y="백만원", hue="분기")
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84617636-9bfffc80-af0a-11ea-8652-3887796d7b11.png" width="50%">

##### 화장품 판매액에 대한 기간별 금액 데이터 시각화 하기
```javascript
plt.figure(figsize=(15,4))
plt.xticks(rotation=30)
sns.lineplot(data=df_cosmetic, x="기간", y="백만원")
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84617682-c0f46f80-af0a-11ea-934c-aa7a424e4a76.png">  

> 화장품 판매액이 꾸준히 증가한 것을 확인할 수 있음  
plt.xticks(rotation= 30): x축 값을 30도로 회전하여 나타내기  


##### 화장품 판매액에 대한 기간별 금액 데이터를 "국가(대륙)별"로 구분하여 시각화 하기
```javascript
plt.figure(figsize=(15,4))
plt.xticks(rotation=30)
sns.lineplot(data=df_cosmetic, x="기간", y="백만원", hue="국가(대륙)별")
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84618539-76c0bd80-af0d-11ea-9bef-694213738e8c.png">  

> 중국에 대한 판매액이 크게 증가해 온 것을 확인할 수 있음

##### 화장품 판매액에 대한 기간별 금액 데이터를 "국가(대륙)별"로 구분하되, "중국"은 제외하고 시각화 하기
```javascript
plt.figure(figsize=(15,4))
plt.xticks(rotation=30)
sns.lineplot(data=df_cosmetic[df_cosmetic["국가(대륙)별"] != "중국"], x="기간", y="백만원", hue="국가(대륙)별")
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84618583-9821a980-af0d-11ea-9e8f-8c1d8b1a257e.png">  

> 아세안(초록색) 판매액이 최근 급성장한 확인할 수 있음  

##### 화장품 판매액에 대한 기간별 금액 데이터를 "판매유형별"로 구분하여 시각화 하기
```javascript
plt.figure(figsize=(15,4))
plt.xticks(rotation=30)
df_sub = df[df["판매유형별"] != "계"].copy()
sns.lineplot(data=df_sub, x="기간", y="백만원", hue="판매유형별", ci=None)
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84618796-2eee6600-af0e-11ea-9fc6-e6368ecc8e0d.png">  

> 판매유형별 데이터는 raw data에 2017년부터 존재하므로, 그래프가 2017년부터 그려짐  

### 6.3 의류 및 패션관련 상품 온라인쇼핑 해외직접판매액

##### 의류 데이터만 df_fashion 이라는 변수에 담아주기
```javascript
df_fashion = df[(df["상품군별"] == "의류 및 패션관련 상품") & (df["판매유형별"] == "계")].copy()
df_fashion.head()
```

`*결과*`  

|     |  국가(대륙)별 |              상품군별 | 판매유형별 |     기간 |  백만원 | 연도 | 분기 |
|----:|--------------:|----------------------:|-----------:|---------:|--------:|-----:|-----:|
|  66 |          미국 | 의류 및 패션관련 상품 |         계 | 2014 1/4 |  9810.0 | 2014 |    1 |
| 111 |          중국 | 의류 및 패션관련 상품 |         계 | 2014 1/4 | 12206.0 | 2014 |    1 |
| 156 |          일본 | 의류 및 패션관련 상품 |         계 | 2014 1/4 | 13534.0 | 2014 |    1 |
| 201 | 아세안(ASEAN) | 의류 및 패션관련 상품 |         계 | 2014 1/4 |  3473.0 | 2014 |    1 |
| 246 |            EU | 의류 및 패션관련 상품 |         계 | 2014 1/4 |  1364.0 | 2014 |    1 |  


##### "의류 및 패션관련 상품"에 대해 "기간"별 판매액을 "국가(대륙)별"로 구분하여 시각화하기
```javascript
plt.figure(figsize=(10,4))
plt.xticks(rotation=30)
sns.lineplot(data=df_fashion, x="기간", y="백만원", hue="국가(대륙)별")
```  
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84618876-69f09980-af0e-11ea-9cef-b24c1357657a.png" width="70%">  

> 중국 판매액이 꾸준히 증가한 것을 확인할 수 있음

##### "의류 및 패션관련 상품"에 대해 "기간"별 판매액을 "판매유형별"로 구분하여 시각화하기
```javascript
df_fashion2 = df[(df["상품군별"] == "의류 및 패션관련 상품") & (df["판매유형별"] != "계")].copy()

plt.figure(figsize=(10,4))
plt.xticks(rotation=30)
sns.lineplot(data=df_fashion2, x="기간", y="백만원", hue="판매유형별", ci=None)
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84618909-87256800-af0e-11ea-89e2-31f69b802695.png" width="50%">  

> 면세점 판매액보다 면세점 이외 판매액이 대체로 높았으나, 최근에는 면세점 판매액이 면세점 이회 판매액과 비슷한 규모로 성장함
