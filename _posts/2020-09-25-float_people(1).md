---
title: "코로나로 인한 서울시 지역별 유동인구 변화 분석(1)"
date: 2020-09-25
categories: "Do myself"
tags: 
#excerpt: "Do myself"
comments: true
published: false
---

코로나로 인한 공포와 사회적 거리두기로 인해 홍대, 명동 등 항상 사람들로 바글거리는 동네들이 텅텅 빈 사진을 본 적이 있다.  
개미 한 마리 없는 듯한 깨끗한 거리 사진에서 자영업자들의 한숨이 느껴질 것만 같은..  
반면, 강남이나 종로에 들렀을 적에는 생각보다 사람이 많아서, 여긴 코로나가 없는 동네인가-? 싶었던 적이 있다.  
  
코로나 이후 실제로 유동인구가 줄었는지 궁금해졌고, 지역별로 변화의 차이가 있는 지 직접 데이터로 들여다보기로 했다.  
  
  
다음의 순서로 분석 계획을 세웠다.  
  
----
1. 유동인구 데이터 구하기  
2. 유동인구 변화 추이 확인하기  
3. 전년 대비 동기간 유동인구 비교해보기  
---

  
  
### 1. 유동인구 데이터 구하기
가장 먼저 데이터를 찾아볼 만한 곳은 [공공데이터포털](https://www.data.go.kr/).  
왠만한 공공데이터는 여기서 찾아볼 수 있는데, 아쉽게도 적합한 유동인구 데이터가 없어서, 이번에는 [SKT 빅데이터 허브](https://www.bigdatahub.co.kr/product/list.do?menu_id=1000157)에서 제공하는 서울 유동인구 데이터를 사용하기로 했다. 
현재 19년 3월부터 20년 8월까지의 데이터를 다운 받을 수 있다.  
  
데이터를 확인해보자  

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```
(늘 불러두는 그 친구들)  
  

분석할 유동인구 데이터 불러온 후 연도별로 묶어서 각각 raw19, raw20에 저장하고,  
raw19와 raw20을 합쳐서 raw에 저장  

```python
#2019년 3월~12월
raw1903 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1903.csv')
raw1904 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1904.csv')
raw1905 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1905.csv')
raw1906 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1906.csv')
raw1907 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1907.csv')
raw1908 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1908.csv')
raw1909 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1909.csv')
raw1910 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1910.csv')
raw1911 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1911.csv')
raw1912 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1912.csv')

#2020년 1월~8월
raw2001 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2001.csv')
raw2002 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2002.csv')
raw2003 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2003.csv')
raw2004 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2004.csv')
raw2005 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2005.csv')
raw2006 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2006.csv')
raw2007 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2007.csv')
raw2008 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2008.csv')
```
  
```python

#2019년 3월~12월 데이터
raw19 = pd.concat([raw1903 , raw1904, raw1905, raw1906, raw1907, raw1908, raw1909, raw1910, raw1911, raw1912], axis=0, ignore_index=True)

#2020년 1월~8월 데이터
raw20 = pd.concat([raw2001, raw2002, raw2003, raw2004, raw2005, raw2006, raw2007, raw2008], axis=0, ignore_index=True)

#2019년 3월~2020년 8월 데이터
raw = pd.concat([raw19, raw20], axis=0, ignore_index=True)
```

이제 raw 데이터가 어떻게 생겼는 지 살펴보자  

#### raw 데이터의 상위 5개 행 확인  
```python
raw.head()
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95011004-75580400-0668-11eb-8a47-d4942139dcb3.png" width ="70%">  
  
  
#### info() 함수로 raw 데이터 정보 확인  
```python
raw.info()
```  
`*결과*`  
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3939900 entries, 0 to 3939899
Data columns (total 7 columns):
 #   Column      Dtype 
---  ------      ----- 
 0   일자          int64 
 1   시간(1시간단위)   int64 
 2   연령대(10세단위)  int64 
 3   성별          object
 4   시           object
 5   군구          object
 6   유동인구수       int64 
dtypes: int64(4), object(3)
memory usage: 210.4+ MB
```
  
데이터프레임의 정보를 슬- 보아하니,   
컬럼은 '일자', '시간(1시간단위)', '연령대(10세단위)', '성별', '시', '군구', '유동인구수' 로 되어있었다.  
그리고  
**- '일자' 데이터 타입이 int64이고,**  
**- 유동인구가 한 날짜에 대해 '시간대' * ' 연령대' * '성별' * '군구' 로 세분화**  
되어있는 것을 확인할 수 있었다.    

#### '일자'는 유동인구 추이를 그래프로 나타낼 때, datetime 타입이여야 예쁘게 그려지므로 데이터 타입을 바꿔주기로 한다.  
```python
raw.일자 = pd.to_datetime(raw.일자, format='%Y%m%d')
```  
  
  
그리고, 나는 이번 분석에서 지역별 일일 총 유동인구 추이만 분석해보기로 하고  
'시간대', '연령대', '성별'로 세분화된 유동인구를 '일일 총 유동인구' 하나로 합쳤다.  
  
```python
# 각 군구별로 시간대, 연령대, 성별 구분 없이 일일 총유동인구수로 나타내기

raw19_1 = raw19.groupby(by=['일자','군구']).sum()
raw20_1 = raw20.groupby(by=['일자', '군구']).sum()
raw_1 = raw.groupby(by=['일자', '군구']).sum()

# 시간, 연령대 컬럼 제외하기
raw19 = raw19_1[['유동인구수']].reset_index()
raw20 = raw20_1[['유동인구수']].reset_index()
raw = raw_1[['유동인구수']].reset_index()

raw.head()
```
  
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95011188-e0eea100-0669-11eb-9ced-129843df3c89.png" width="70%">  
  
  
#### '군구' 컬럼에 몇 가지 행정구가 있는 지 확인  
```python
print(raw.군구.unique())

len(raw.군구.unique())
```  
  
`*결과*`  
```
['양천구' '종로구' '중랑구' '강동구' '성동구' '송파구' '영등포구' '노원구' '금천구' '도봉구' '서대문구' '구로구'
 '동대문구' '중구' '관악구' '강북구' '성북구' '광진구' '마포구' '서초구' '은평구' '강서구' '용산구' '강남구'
 '동작구']
25
```
  
  
### 2. 유동인구 변화 추이 확인하기   
이제 그래프 시각화를 통해 '군구'별로 유동인구가 어떻게 변화해왔는지 살펴보자  
```python
plt.figure(figsize=(20,5))
sns.lineplot(x='일자', y='유동인구수', data=raw, hue='군구')
```  

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95011234-404cb100-066a-11eb-9184-c7129630a7b4.png" width="70%">  


25개 행정구 데이터가 한번에 그려지니 어지러운 느낌이다.  
하지만 큰 맥락에서, 모든 행정구가 주기적으로 감소/증가를 반복하는 것을 알 수 있고,   
유동인구가 급감, 급증하는 특정 날짜들이 보인다.  

군구별로 개별 그래프를 그려보자  
```python
sns.relplot(x='일자', y='유동인구수', data=raw, kind='line', hue='군구', col='군구', col_wrap=3)
```  
  
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95011278-77bb5d80-066a-11eb-8734-4061a7e2e260.png" width="70%">  
  
X축이 일자, Y축이 유동인구수인데,  

- y축의 높이로 유동인구 규모를 파악할 수 있다.  

👉 강남구, 송파구가 평소 유동인구가 많은 편  

- 그래프의 두께로 유동인구 변화 폭을 파악할 수 있다.  
👉 강남구, 서초구, 영등포구, 종로구, 중구가 증감폭이 큼 👉 출퇴근이 많은 사무지역 느낌?  
  
  
이번에는 2020년의 유동인구 변화 추이를 살펴보자  
```python
plt.figure(figsize=(20,5))
plt.title('20년도')
sns.lineplot(x='일자', y='유동인구수', data=raw20, hue='군구')
```
  
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95011303-a2a5b180-066a-11eb-8ec1-a7261efd4ca3.png" width = "70%">  
  
  
1월 말 쯤 대부분의 행정구에서 유동인구가 급감한 시기가 있고,  

2월 말 쯤 대부분의 행정구에서 유동인구가 급증한 날이 있다.  

(이 외에도 3월 초중순, 5월 초, 5월 말 등 몇몇 눈에 띄는 곳들이 있다.)  
  
정확한 날짜를 알아보자  

#### 1월 말 유동인구가 급감한 날짜 (강남구를 대표로 확인)  
```python

# 1월 데이터만 추출하기
a = raw20[raw20.일자.isin(pd.date_range('2020-01-01', '2020-01-31'))]

# '강남구'데이터만 추출하기
gangnam = a[a.군구 == '강남구']

# 그래프로 나타내기
plt.figure(figsize=(20,3))
sns.lineplot(x='일자', y='유동인구수', data=gangnam)
```  
  
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95011326-d54faa00-066a-11eb-8c24-4b8703afede2.png" width="70%">  
  
1월 25일에 가장 유동인구가 적었던 것을 알 수 있다. (1월 25일은 설 명절이었고, 코로나 이슈가 붉어지기 시작한 때이다_!)  
  
  
#### 2월에 유동인구수가 급증한 날짜  
내가 가진 데이터의 전체 기간 중 유동인구수가 최대인 날이므로, max함수를 사용해서 알아보았다.  
```python
raw.max()

# 유동인구수가 max인 날짜 알아보기
raw[raw.유동인구수 == 23917450]
```  
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95011367-265f9e00-066b-11eb-9d5b-bc4a296fd7c4.png" width="50%">  
2월 23일이 유동인구가 급증한 날이었다.  
내 기억으로는 우리나라에서 코로나 공포가 점점 심화?되는 시기인데,, 위키피디아에 검색해봐도 코로나 외에는 특이점이 보이지 않았다. 👀  
  
<img src = "https://user-images.githubusercontent.com/50826051/95011380-3ecfb880-066b-11eb-8e30-654a953b2a4d.png" width="70%>  
  
네이버 검색어 트랜드를 통해서도 2월 23일 전후로 '코로나' 검색이 급증한 것을 볼 수 있다.   
아무래도 코로나와 관련이 있어 보이고, 코로나에 대한 우려와 공포로 유동인구가 줄어들 것만 같은데,, 급증한 이유가 뭘까?  
.  
.  
.  
고민해보니 당시 마스크 대란이 있었던 것이 생각났다_!  
네이버 검색어 트랜드에 '마스크'도 추가해서 그래프를 보았다.   
  
<img src = "https://user-images.githubusercontent.com/50826051/95011397-5e66e100-066b-11eb-978d-d5f863ae4840.png" width="70%">  
  
'마스크' 또한 '코로나'와 같이 동일한 기간에 검색량이 pick였던 것을 확인할 수 있었다.  
마스크를 구하기 위해 거리로 나온 사람들의 데이터가 반영된 것이 아닐까....? 유추해보았다.  
  
하지만 나의 초기 예상과 달리, 그래프 상으로는 코로나로 인해 장기적으로 평균 유동인구 수가 줄어든 것으로 보이지는 않았다.  
  
이유가 무엇일지 생각해보았을 때,  
 1) 동기간 19년 유동인구 데이터와 비교해본다(대조군을 만든다)  
 2) 시간대별 유동인구로 세분화해서 데이터를 살펴본다  
 3) 행정구를 행정동 단위로 세분화해서 데이터를 살펴본다  
이 세 가지를 고려하면 유동인구 변화를 좀 더 면밀히 살펴볼 수 있을 것 같다!  
  
To be continue...  
  
  





