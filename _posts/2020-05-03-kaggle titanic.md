---
title: "kaggle-titanic"
date: 2020-05-03
categories: "STUDY"
tags: 
excerpt: "STUDY"
comments: true
---

작년 언젠가 kaggle에 대해 알게 되었을 때 처음으로 데이터 분석에 관심을 갖게 되었던 것 같다.  
당장이라도 titanic 문제를 풀어보고 싶었지만, 당시에는 아예 프로그래밍 언어에 대해 알지 못했었기에, 그저 '동경하는 어떤 프로젝트' 로 존재해왔다.  
  
그러다가 작년부터 R과 SQL에 대해서 조금씩 공부를 하다보니 언어에 대해 조금 감이 생겼고, 일단 'titanic은 나도 공부하면 할 수 있겠는데?' 라는 생각이 들었다._(데이터 분석 생초보 대상 부트캠프에서 titanic 문제로 교육을 한다는 광고를 봐서 자신감이 생겼던 듯 하다.)_    
그리고 오늘이 바로 실행하는 첫 날이다 :clap: :clap:

python은 기본 문법만 조금 공부해봤는데, 문법으로만 공부를 하니 자꾸 까먹어서 비효율적이라는 생각이 들었다.  
kaggle- titanic 문제로 공부를 한다면, 배경 상황을 통한 '문제 정의'부터 '어떤 문제를 해결하기 위해 어떤 문법을 사용할 수 있는 지' 등 데이터 분석 시 고민해봐야할 부분들도 경험해볼 수 있을 것 같았다.  
마침 초보자를 위한 좋은 [titanic tutorial](https://www.kaggle.com/alexisbcook/titanic-tutorial)을 찾게되어 따라하면서 공부를 해 볼 생각이다.    
일단 시작! -> 다 해봤는데, 너무 맛보기용 간단 튜토리얼이라서 당황..하지 않고,  
한국인이 제작한 좋은 [유튜브 walkthrough 영상](https://www.youtube.com/watch?v=aqp_9HV58Ls)을 찾게되어 다시 찬찬히 따라해보았다.  

--------------------------------------------------------------------------------------------------------------------  

목표: titanic호 탑승자 정보를 이용하여 생존 여부 예측하기  
 
[나의 colab notebook 주소:point_right:](https://colab.research.google.com/drive/1_c-7itnvISFRh_K8Qr-5dFyyr-1qRlXa)
 
kaggle 제출까지 완료했다.
![kaggle ranking](assets/images/titanic_ranking.jpg)  
kaggle에서 제공하는 튜토리얼만 보고 제출했을 때보다 예측도가 올랐다!

동영상을 보면서 따라한 수준이라 이해가 많이 높지는 않지만, 어느정도 감을 잡은 듯 하다.    
(이와 비슷한 과제가 생긴다면 응용할 수 있을 것 같다.)

메모들  
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


