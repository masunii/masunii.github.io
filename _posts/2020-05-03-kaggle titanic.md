---
title: "kaggle-titanic"
date: 2020-05-03
categories: "STUDY"
tags: 
excerpt: "STUDY"
comments: true
---

작년 언젠가 kaggle에 대해 알게 되었을 때, 처음으로 데이터 분석에 관심을 갖게 되었던 것 같다.  
당장이라도 titanic 문제를 풀어보고 싶었지만,_(퍼즐게임을 엄청 좋아함. 비슷한 관점에서 흥미가 생겼던 것 같다.)_ 당시에는 아예 프로그래밍 언어에 대해 알지 못했었기에, 나에게는 그저 '동경하는 어떤 프로젝트' 로 존재해왔다.  
  
그러다가 작년부터 R과 SQL에 대해서 조금씩 공부를 하다보니 언어에 대해 조금 감이 생겼고, 일단 'titanic은 나도 공부하면 할 수 있겠는데?' 라는 생각이 들었다._(데이터 분석 생초보 대상 강의에서 titanic 문제로 실습을 한다는 광고를 보기도 해서 자신감이 생겼던 듯 하다.)_    
그리고 오늘이 바로 실행하는 첫 날이다 :clap: :clap:

python은 기본 문법만 조금 공부해봤는데, 문법으로만 공부를 하니 자꾸 까먹어서 비효율적이라는 생각이 들었다.  
이 kaggle- titanic 문제로 공부를 한다면, 배경 상황을 통한 '문제 정의'부터 '어떤 문제를 해결하기 위해 어떤 문법을 사용할 수 있는 지' 등 데이터 분석 시 고민해봐야할 부분들도 경험해볼 수 있을 것 같았다.  
마침 초보자를 위한 좋은 [titanic tutorial](https://www.kaggle.com/alexisbcook/titanic-tutorial)을 찾게되어 따라하면서 공부를 해 볼 생각이다.    
일단 시작! -> 다 해봤는데, 너무 맛보기용 간단 튜토리얼이라서 당황..  

한국인이 제작한 좋은 [유튜브 walkthrough 영상](https://www.youtube.com/watch?v=aqp_9HV58Ls)을 찾게되어 다시 찬찬히 따라해보는 중이다.  

--------------------------------------------------------------------------------------------------------------------  

목표: titanic호 탑승자의 정보를 이용하여, 생존 여부 예측하기  

[나의 kaggle notebook 주소:point_right:](https://www.kaggle.com/kyeongah/let-s-do-it/edit/run/33224875) 


#### 1. data tap에서 data 다운받기  

train.csv: 탑승객 정보가 들어있는 파일 (survived 컬럼 값이 0이면 died, 1이면, survived)  
test.csv: train에서 얻은 패턴 정보를 바탕으로 생존 여부를 예측할 탑승객 정보가 들어있는 파일 (survived 컬럼 값이 없음)  
gender_submission.csv: 예측값 제출할 양식  
  
#### 2. notebook tap에서 문제를 풀 notebook 생성하기  
  
코드 실행: '코드 셀 왼쪽의 run 버튼 누르기' or 'shift + enter 누르기'  
pd: pandas의 줄임. 파이썬 모듈. 데이터 파일의 table을 notebook에 로드하는 역할을 함.
  
* data load하기  
```javascript
import pandas as pd
  
train = pd.read_csv("/kaggle/input/titanic/train.csv")
test = pd.read_csv("/kaggle/input/titanic/test.csv")
```
  
```javascript
train.head()
```
>.head(): dataset의 상위 5행을 출력해라  
>분석할 dataset이 어떻게 구성되어있는 지 파악  

```javascript
train.shape
```
>train dataset의 행, 열 개수 정보를 출력. 출력 예: (891, 12)  


>table.loc: table의 행,열 추출할 때 사용. (예: table.loc[10,:] -> 10행에 해당하는 모든 열의 데이터)    


