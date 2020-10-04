---
title: "코로나로 인한 서울시 지역별 유동인구 변화 분석(2)"
date: 2020-09-28
categories: "분석생활"
tags: 
#excerpt: "분석생활"
comments: true
published: false
---

[앞선 글](https://masunii.github.io/%EB%B6%84%EC%84%9D%EC%83%9D%ED%99%9C/float_people(1)/) 에서 서울시 지역별 일일 유동인구 추이를 그래프로 살펴보았습니다.

오늘은 추후 스탭로 언급했던 태스크 중에서  
**동기간 19년 유동인구 데이터와 비교해본다(비교군을 만든다)**  
를 해보려고 합니다.  

#### _언제나 그렇듯_ 분석에 사용할 라이브러리 불러오기  
```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```
  
#### 유동인구 raw 데이터 생성하기  
SKT 빅데이터 허브에서 서울시 유동인구 데이터를 다운 받습니다.  
현재 19년 3월부터 20년 8월까지 다운로드가 가능한데,  
19년과 20년을 월별로 비교하기 위해서, 2개년도 데이터가 모두 존재하는 3월~8월 데이터만 사용하기로 합니다.  
```python
#2019년 3월~12월
raw1903 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1903.csv')
raw1904 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1904.csv')
raw1905 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1905.csv')
raw1906 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1906.csv')
raw1907 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1907.csv')
raw1908 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_1908.csv')

#2020년 1월~8월
raw2003 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2003.csv')
raw2004 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2004.csv')
raw2005 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2005.csv')
raw2006 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2006.csv')
raw2007 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2007.csv')
raw2008 = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/유동인구_2008.csv')
```
  
연도별로 각각 raw19, raw20에 저장하고, raw19와 raw20을 합쳐 raw_1에 저장  
```python
#2019년 3월~8월 데이터
raw19 = pd.concat([raw1903 , raw1904, raw1905, raw1906, raw1907, raw1908])

#2020년 3월~8월 데이터
raw20 = pd.concat([raw2003, raw2004, raw2005, raw2006, raw2007, raw2008], axis=0, ignore_index=True)


#raw19와 raw20 을 합쳐서 raw_1 생성
raw_1 = pd.concat([raw19, raw20], axis=0, ignore_index=True)
raw_1.head()
```  
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95021075-d2c07500-06a9-11eb-8aa3-e455b075cb95.png" widtb="30%">  
  
#### groupby를 이용해 군구별 일일 총 유동인구를 계산하고, 필요없는 컬럼인 '시간(1시간단위)'와 '연령대(10세단위)' 은 제거.  
```python
# 시간대, 연령대 컬럼 제거
temp = raw_1.groupby(by=['일자', '군구']).sum().reset_index()
raw_1 = temp.drop(['시간(1시간단위)', '연령대(10세단위)'], axis=1)
raw_1.head()
```  
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95021107-026f7d00-06aa-11eb-9d59-e7d5d4d673fa.png" width="40%">  
  
  
#### 군구별 2019년, 2020년 유동인구 비교 그래프 그리기
일자별 그래프를 그리기 위해 '일자'컬럼의 데이터 타입을 datetime으로 변경하고, 이 컬럼을 활용해 '연도', '월', '일' 컬럼을 생성합니다.  
```python

# '일자'컬럼의 데이터 타입을 datetime으로 변경
raw_1['일자'] = pd.to_datetime(raw_1['일자'], format='%Y%m%d')

# '연도', '월', '일' 컬럼 생성
raw_1['연도'] = raw_1.일자.dt.year
raw_1['월'] = raw_1.일자.dt.month
raw_1['일'] = raw_1.일자.dt.day

raw_1.head()
```
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95021148-35b20c00-06aa-11eb-9a98-c74e2cb5bd81.png" width="40%">  
  
#### relplot을 이용해, '군구'별 그래프 그리기  
```python
GunGu = raw_1.군구.unique()

for i in GunGu:
  df = raw_1[raw_1.군구 == i]
  sns.relplot(data=df, x='일', y='유동인구수', col='월', hue='연도', kind='line', legend='full', col_wrap=6, height=3, ci=None).fig.suptitle(i)
```
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95021254-e3251f80-06aa-11eb-8c74-f98108d731ab.png" width="80%">  
<img src = "https://user-images.githubusercontent.com/50826051/95021289-14055480-06ab-11eb-929d-73e132de28c9.png" width="80%">  
<img src = "https://user-images.githubusercontent.com/50826051/95021300-2e3f3280-06ab-11eb-88f1-246f0fef35d9.png" width="80%">  
_스압이 심한 관계로 일부 그래프만 본문에 삽입..(원래는 25개)_  
  
증감을 반복하는 주기는 19년과 20년이 같은데, (주기적으로 감소하는 때가 주말)  
19년의 요일과 20년의 요일이 같지 않기 때문에 그래프가 밀려보이는 것을 감안해야 합니다.  
2019년과 비교해보니, 일부 행정구를 제외하고 대체로 유동인구가 줄어든 것을 볼 수 있었습니다.  
특히 3월, 4월에 눈에 띄게 줄어든 것을 볼 수 있는데, 5~7월에 어느정도 회복되는 모습이며,   
8월 15일 광복절 집회가 있었던 시기를 기점으로 유동인구가 또다시 줄어드는 것을 볼 수 있습니다.  
  
  
#### 행정구별 월평균 유동인구 추이 그래프 그리기  
이번에는 그래프를 좀 간소화하여 월별 유동인구 추이를 파악해보겠습니다.   
```python
# 월평균 유동인구수 구하기
temp = raw_1.groupby(by=['군구', '연도', '월']).mean().reset_index()
raw_1 = temp.drop('일', axis=1)

# 그래프 그리기
raw_1 = raw_1.set_index('군구')

fig, axes = plt.subplots(nrows=5, ncols=5, figsize=(15,15))

for i, f in enumerate(raw_1.index.unique()):
    r = int(i / 5) #행별로 그래프 배치하기
    c = i % 5 #열별로 그래프 배치하기
    df19 = raw_1[(raw_1.index == f) & (raw_1.연도 == 2019)]
    line19 = sns.lineplot(data=df19, x='월', y='유동인구수', label='2019', ax=axes[r][c])
    df20 = raw_1[(raw_1.index == f) & (raw_1.연도 == 2020)]
    line20 = sns.lineplot(data=df20, x='월', y='유동인구수', label='2020', ax=axes[r][c])
    line20.set_ylim(0,1.5e+07)
    line20.set_title(f)

fig.tight_layout()
```
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95021387-b291b580-06ab-11eb-908f-49a638674a6c.png" width="80%">  
  
그래프를 보니, 2개 그룹으로 행정구를 분류할 수 있을 것 같습니다.  
group1) 19년도 대비 20년도에 유동인구가 감소한 그룹  
-> 강남구, 서초구, 영등포구, 용산구, 종로구, 중구 등...  
group2) 19년도 대비 20년도에 유동인구가 증가한 그룹  
-> 강동구, 노원구, 양천구, 은평구 등...  
  
서울토박이피셜  
**group1**: 상업, 사무지역  
**group2**: 주거지역  
으로 예상이 되는데, 이것이 맞을지 데이터로 한번 살펴보기로 합니다.  
  
#### 군구별 용도별 건축물 개수 비교  
서울시 데이터포털에서 [서울시 건축허가 통계 데이터](https://data.seoul.go.kr/dataList/235/S/2/datasetView.do)를 찾아 다운받습니다.   
```python
GunGu_data = pd.read_csv('/content/drive/My Drive/colab/blogger/유동인구/행정구역정보.txt', engine='python', sep='\t')
temp = GunGu_data[GunGu_data.분류별 == '동수']
GunGu_data = temp[['자치구별', '용도별', '계']]

GunGu_data.head()
```
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95021444-fe445f00-06ab-11eb-9fca-faf4c06319f3.png" width="40%">  
행정구별 주거용, 상업용 건축물 동수(개수)를 비교해보겠습니다.    
  
#### 행정구별 주거용 건축물 수, 상업용 건축물 수 비교  
```python
# '계' 컬럼 타입을 정수로 변경하기
GunGu_data.계 = [int(x) for x in GunGu_data.계]
GunGu_data = GunGu_data[(GunGu_data.용도별 == '주거용') | (GunGu_data.용도별 == '상업용')]

fig, axes = plt.subplots(nrows=5, ncols=5, figsize=(10,10))

for i, f in enumerate(GunGu_data.자치구별.unique()):
    r = int(i / 5) #행별로 그래프 배치하기
    c = i % 5 #열별로 그래프 배치하기
    line = sns.barplot(data=GunGu_data[GunGu_data.자치구별==f], x='용도별', y='계', ax=axes[r][c])
    # line.set_ylim(0,1.5e+07)
    line.set_title(f)

fig.tight_layout()
```
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/95021483-33e94800-06ac-11eb-8e70-3985442cb198.png" width="80%">  
그래프 상으로 대부분의 행정구가 주거용건물 : 상업용건물 비율이 뚜렷하게 차이나는 것을 볼 수 있었습니다.  

**상업용 > 주거용** 이면 **'상업지역'**, **주거용 > 상업용** 이면 **'주거지역'** 으로 행정구 분류  
**상업지역**: 강남구, 강서구, 노원구, 동대문구, 마포구, 서초구, 성동구, 용산구, 영등포구, 종로구, 중구  
**주거지역**: 강동구, 강북구, 광진구, 관악구, 구로구, 금천구, 도봉구, 동작구, 서대문구, 성북구, 송파구, 양천구, 은평구, 중랑구  
  
앞에서 분류한 group1과 group2가 주거/상업지역에 중 어디에 해당되는지 파악해보면,  
토박이피셜이 어느정도 맞는 것을 확인할 수 있다.😉  
  
  
EDA 결과 정리⭐  
• 대부분의 행정구가 2019년에 비해 2020년에 유동인구가 감소  
• 2019년에 비해 2020년에 유동인구가 줄어든 행정구는 대부분 '상업지역',  
• 일부 2020년에 오히려 유동인구가 늘어난 행정구는 대부분 '주거지역'이었음  
• 유동인구는 3,4월에 큰 폭으로 감소, 5~7월에 회복세를 보이고, 8월 15일부터 다시 감소세   
  
재택근무, 온라인 수업 등으로 상업/사무 지역을 중심으로 유동인구가 감소한 것으로 생각되며,  
따라서 주거지역보다 상업지역의 상권이 유동인구 감소로 인한 매출 타격을 많이 받았을 것으로 예상됩니다.  
  
  
to be continue...
