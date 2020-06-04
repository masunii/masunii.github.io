---
title: "edwith[python](2)Pandas"
date: 2020-06-02
categories: "STUDY"
comments: TRUE
---

edwith 부스트코스 [파이썬으로 시작하는 데이터 사이언스](https://www.edwith.org/boostcourse-ds-510/joinLectures/28137) 를 공부하는 과정입니다.  
지난 포스트 :point_right: [edwith[python](1)분석환경 구성 & 파이썬 기초 문법](https://masunii.github.io/study/edwith_%EB%B6%84%EC%84%9D%ED%99%98%EA%B2%BD%EA%B5%AC%EC%84%B1%ED%95%98%EA%B8%B0/)
  
  --------------------------------------------------------
  
### Pandas 
 
 Pandas: **Pan**el **da**ta **s**ystem 의 약자  
       : 행과 열로 이루어진 데이터 객체를 만들어 다룰 수 있는 데이터 분석 라이브러리  
       : 엑셀보다 대용량 데이터를 빠르게 처리할 수 있음. 주피터 노트북에 소스코드를 작성해두고 기존의 소스코드를 재사용할 수 있음(반복되는 작업에 유용함)  


```javascript
import pandas as pd

df = pd.DataFrame(
{"a" : [4, 5, 6],
"b" : [7, 8, 9],
"c" : [10, 11, 12]},
index = [1, 2, 3])


#series 데이터 형태로 출력
df["a"]

#dataframe 형태로 출력
df[["a"]]

#2개 이상의 컬럼을 불러올 때는 반드시 dataframe 형태로 출력해야 함(series 형태로 불러오면 에러 발생함)
df[["a", "b"]]

#특정 조건을 충족하는 dataFrame 출력
df[df["a"] > 5]
```

**Summarize Data**
```javascript
df_1 = pd.DataFrame(
{"a" : [5, 5],
"b" : [7, 8 ],
"c" : [11, 12]},
index = [1, 2])

#df_1의 a컬럼 값을 카운트
df_1["a"].value_counts()

#df_1의 길이 출력
len(df_1)
```
  
**Sort_Values, Drop**
```javascript
#b컬럼을 기준으로 내림차순 정렬하여 출력
df.sort_values("b", ascending=False)

#c컬럼 드롭하여 출력. 'c'는 컬럼이므로, axis = 1 로!
df.drop(["c"], axis=1)
```
  
   
**Groupby, pivot**
```javascript
#a컬럼을 기준으로 b컬럼의 mean, sum, count 값 출력
df.groupby(["a"])["b"].agg(["mean", "sum", "count"])
df.groupby(["a"])["b"].describe()

#a컬럼을 인덱스로, 피벗테이블 만들기
pd.pivot_table(df_1, index="a")
#a 컬럼에 5가 2개 중복으로 있기 때문에 b,c 컬럼의 값은 mean값으로 계산되어 나타남 (default)

#위 코드에서 aggfunc 코드를 추가하여, mean 값(default) 대신 sum 값을 출력할 수도 있음
pd.pivot_table(df_1, index="a", aggfunc="sum")

```
  
   
**Plotting**
```javascript
#그래프 그리기
#df_1.plot. + tab키를 누르면, 다양한 그래프 형태 선택 가능
#그래프 형태 뒤에 반드시 () 붙이기
df_1.plot.bar()
```
  
**파일경로**
```javascript
#주피터 노트북이 있는 파일 경로 출력
#분석할 데이터 파일을 주피터 노트북이 있는 경로에 저장해두는 것이 좋음
%pwd
```  

다음 포스트 :point_right: [edwith[python](3)데이터 살펴보기](https://masunii.github.io/study/edwith_%EA%B3%B5%EA%B3%B5%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%83%81%EA%B6%8C%EB%B6%84%EC%84%9D(1)/)
