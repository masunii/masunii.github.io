---
title: "edwith_python_(12)K-beauty 판매 데이터 살펴보기"
date: 2020-06-13 10:00:00
categories: "STUDY"
comments: TRUE
toc: TRUE
toc_sticky: TRUE
---

edwith 부스트코스 [파이썬으로 시작하는 데이터 사이언스](https://www.edwith.org/boostcourse-ds-510/joinLectures/28137) 를 공부하는 과정입니다.    
  
지난 포스트 :point_right: [edwith_python_(11)scatterplot, lmplot, distplot으로 데이터 시각화하기](https://masunii.github.io/study/edwith_%EA%B1%B4%EA%B0%95%EB%8D%B0%EC%9D%B4%ED%84%B0(6)/)  

--------------------------------------------------------
분석해볼 점  
* K-beauty는 성장하고 있을까?  
* 해외직접판매를 한다면 어느 국가로 판매 전략을 세우면 좋을까?  

## 1. 라이브러리 로드

##### 분석에 사용할 라이브러리 불러오기
```javascript
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```

## 2. 한글폰트 설정
```javascript
#google colab에서 한글을 그래프에 표현하려면 아래의 설정이 필요!

import matplotlib as mpl

#한글 깨지지 않도록 설정
!apt -qq -y install fonts-nanum > /dev/null

import matplotlib.font_manager as fm

fontpath = '/usr/share/fonts/truetype/nanum/NanumBarunGothic.ttf'
font = fm.FontProperties(fname=fontpath, size=9)
fm._rebuild ()

plt.rc('font', family='NanumBarunGothic') 
plt.rc("axes", unicode_minus=False)

#글씨가 선명하게 보이도록 설정
%config InlineBackend.figure_format = 'retina'
```

## 3. 데이터 불러오기

이번 포스트부터는 공공 데이터 포털의 2014 1/4분기 ~ 2019 4/4 분기까지의 데이터 활용! [다운로드 받기](http://kosis.kr/statHtml/statHtml.do?orgId=101&tblId=DT_1KE10081&vw_cd=MT_ZTITLE&list_id=JF&seqNo=&lang_mode=ko&language=kor&obj_var_id=&itm_id=&conn_path=MT_ZTITLE)

##### 구글 드라이브 마운트
```javascript
from google.colab import drive
drive.mount('/content/drive')
```

##### 분석할 데이터를 df_raw 변수로 지정하여 불러오기
```javascript
df_raw = pd.read_csv("/content/drive/My Drive/colab/edwith_study/온라인쇼핑_해외직접판매액.csv", encoding="cp949")
```

### 3.1 데이터 미리보기

##### 데이터 프레임의 행,열 개수 알아보기
```javascript
df_raw.shape
```

`*결과*`  
```
(450, 27)
```

##### head를 통해 데이터 미리보기
```javascript
df_raw.head()
```

`*결과*`  

|   | 국가(대륙)별 |           상품군별 |  판매유형별 | 2014 1/4 | 2014 2/4 | 2014 3/4 | 2014 4/4 | 2015 1/4 | 2015 2/4 | 2015 3/4 | 2015 4/4 | 2016 1/4 | 2016 2/4 | 2016 3/4 | 2016 4/4 | 2017 1/4 | 2017 2/4 | 2017 3/4 | 2017 4/4 | 2018 1/4 | 2018 2/4 | 2018 3/4 | 2018 4/4 | 2019 1/4 | 2019 2/4 | 2019 3/4 | 2019 4/4 |   |
|--:|-------------:|-------------------:|------------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---|
| 0 |         합계 |               합계 |          계 |   148272 |   153641 |   163999 |   213216 |   269101 |   271784 |   268421 |   450568 |   511395 |   494391 |   560853 |   726741 |   768504 |   574551 |   749612 |   858240 |   833672 |   897205 |   921586 |   974060 |  1250236 |  1344404 |  1525396 |  1879499 |   |
| 1 |         합계 |               합계 |      면세점 |        - |        - |        - |        - |        - |        - |        - |        - |        - |        - |        - |        - |   610257 |   441096 |   610630 |   677884 |   665613 |   725319 |   761836 |   770656 |  1070693 |  1157158 |  1336372 |  1655635 |   |
| 2 |         합계 |               합계 | 면세점 이외 |        - |        - |        - |        - |        - |        - |        - |        - |        - |        - |        - |        - |   158247 |   133455 |   138982 |   180356 |   168059 |   171886 |   159750 |   203404 |   179543 |   187246 |   189024 |   223864 |   |
| 3 |         합계 | 컴퓨터 및 주변기기 |          계 |     4915 |     4052 |     3912 |     3529 |     2903 |     2697 |     3804 |     4048 |     4211 |     3693 |     3160 |     3270 |     2610 |     2043 |     2018 |     2158 |     5236 |     3854 |     4320 |     4511 |     3702 |     4038 |     3670 |     3826 |   |
| 4 |         합계 | 컴퓨터 및 주변기기 |      면세점 |        - |        - |        - |        - |        - |        - |        - |        - |        - |        - |        - |        - |        5 |        3 |        5 |        1 |        2 |        6 |        1 |      215 |        2 |        0 |        1 |       70 |   |


> e: 추정치  
p: 잠정치  
'-' : 자료없음  
... : 미상자료  
x : 비밀보호  

##### "국가(대륙)별" 데이터 빈도수 카운팅
```javascript
df_raw["국가(대륙)별"].value_counts()
```
`*결과*`  
```
대양주           45
기타            45
미국            45
일본            45
아세안(ASEAN)    45
중남미           45
중동            45
EU            45
중국            45
합계            45
Name: 국가(대륙)별, dtype: int64
```

##### 미국 데이터만 따로 나타내기
```javascript
df_raw[df_raw["국가(대륙)별"] == "미국"]
```

## 4. 분석과 시각화를 위한 tidy data 만들기  

깔끔한 데이터(Tidy data)  
Jeff Leek가 쓴 책 The Elements of Data Analytic Style에서 정의한 깔끔한 데이터는 아래와 같은 특징을 가짐  

* 각 변수는 개별의 열(column)으로 존재한다.
* 각 관측치는 행(row)를 구성한다.
* 각 표는 단 하나의 관측기준에 의해서 조직된 데이터를 저장한다.
* 만약 여러개의 표가 존재한다면, 적어도 하나이상의 열(column)이 공유되어야 한다.

##### 분기 컬럼을 하나의 "기간"이라는 컬럼을 만들어 넣고, 분기별 컬럼 값(판매액)을 "백만원" 이라는 컬럼을 만들어 넣기
```javascript
df = df_raw.melt(id_vars=["국가(대륙)별", "상품군별", "판매유형별"], var_name="기간", value_name="백만원")
df.head()
```

`*결과*`  

|   | 국가(대륙)별 |           상품군별 |  판매유형별 |     기간 | 백만원 |
|--:|-------------:|-------------------:|------------:|---------:|-------:|
| 0 |         합계 |               합계 |          계 | 2014 1/4 | 148272 |
| 1 |         합계 |               합계 |      면세점 | 2014 1/4 |      - |
| 2 |         합계 |               합계 | 면세점 이외 | 2014 1/4 |      - |
| 3 |         합계 | 컴퓨터 및 주변기기 |          계 | 2014 1/4 |   4915 |
| 4 |         합계 | 컴퓨터 및 주변기기 |      면세점 | 2014 1/4 |      - |  

> id_vars: 그대로 유지할 열  

.  
.  
다음 포스트 :point_right: [edwith_python_(13)데이터 전처리하기](https://masunii.github.io/study/edwith_K-beauty(2)/)  
