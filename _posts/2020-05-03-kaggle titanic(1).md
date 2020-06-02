---
title: "kaggle-titanic_(1)데이터 로드 및 살펴보기"
date: 2020-05-03
categories: "STUDY"
tags: 
#excerpt: "STUDY"
comments: true
---

작년 언젠가 kaggle에 대해 알게 되었을 때 처음으로 데이터 분석에 관심을 갖게 되었던 것 같다.  
당장이라도 titanic 문제를 풀어보고 싶었지만, 당시에는 아예 프로그래밍 언어에 대해 알지 못했었기에, 그저 '동경하는 어떤 프로젝트' 로 존재해왔다.  
  
그러다가 작년부터 R과 SQL에 대해서 조금씩 공부를 하다보니 언어에 대해 조금 감이 생겼고, 일단 'titanic은 나도 공부하면 할 수 있겠는데?' 라는 생각이 들었다._(데이터 분석 생초보 대상 부트캠프에서 titanic 문제로 교육을 한다는 광고를 봐서 자신감이 생겼던 듯 하다.)_    

python은 기본 문법만 조금 공부해봤는데, 문법으로만 공부를 하니 자꾸 까먹어서 비효율적이라는 생각이 들었다.  
kaggle- titanic 문제로 공부를 한다면, 배경 상황을 통한 '문제 정의'부터 '어떤 문제를 해결하기 위해 어떤 문법을 사용할 수 있는 지' 등 데이터 분석 시 고민해봐야할 부분들도 경험해볼 수 있을 것 같았다.  

한국인이 제작한 좋은 [유튜브 walkthrough 영상](https://www.youtube.com/watch?v=aqp_9HV58Ls)을 찾게되어 다시 찬찬히 따라해보았다.  

--------------------------------------------------------------------------------------------------------------------  
  
분석 목표: titanic호 탑승자 정보를 이용하여 생존 여부 예측하기  
  

>시작 전 colab 사용 꿀팁 :bulb:
Ctrl + MM: cell을 markdown으로 전환  
Ctrl + MY: cell을 code로 전환  
Ctrl + MB: 바로 아랫줄에 cell 생성  
Ctrl + MA: 바로 윗줄에 cell 생성  
Ctrl + MD: cell 삭제
  
### 분석할 DATA 살펴보기  
```javascript
#파이썬에서 사용하는 데이터분석 라이브러리인 Pandas와 numpy 불러오기
import pandas as pd
import numpy as np
```
```javascript
#구글 드라이브에 저장된 데이터 연결하기
from google.colab import drive
drive.mount('/content/gdrive')
```
```javascript
#data 불러오기
train = pd.read_csv("gdrive/My Drive/colab/titanic/train.csv")
train.head()
```
```javascript
test = pd.read_csv("gdrive/My Drive/colab/titanic/test.csv")
test.head()
```
>train 데이터와 구성이 거의 같음. 'Survived' 컬럼이 없음. 


```
*   DATA DICTIONARY  
Pclass: ticket class  
SibSp: number of Siblings / Spouses aboard the Titanic  
Parch: number of Parents / children aboard the Titanic  
ticket: ticket number  
Cabin: cabin number  
Embarked: Port of embarkation  C: Cherbourg, S: Southampton, Q: Queenstown  
NaN: Not a number. 표현 불가능한 수치형 결과. 머신러닝 데이터로 사용하기 위해서는 평균값을 넣거나 삭제 필요
```

```javascript
#train 데이터의 행,열 정보 출력
train.shape
```
```javascript
#train 데이터 정보 출력
train.info()
```
```
**결과**
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 891 entries, 0 to 890
Data columns (total 12 columns):
 #   Column       Non-Null Count  Dtype  
---  ------       --------------  -----  
 0   PassengerId  891 non-null    int64  
 1   Survived     891 non-null    int64  
 2   Pclass       891 non-null    int64  
 3   Name         891 non-null    object 
 4   Sex          891 non-null    object 
 5   Age          714 non-null    float64
 6   SibSp        891 non-null    int64  
 7   Parch        891 non-null    int64  
 8   Ticket       891 non-null    object 
 9   Fare         891 non-null    float64
 10  Cabin        204 non-null    object 
 11  Embarked     889 non-null    object 
dtypes: float64(2), int64(5), object(5)
memory usage: 83.7+ KB
```
>891명의 승객정보가 있음을 확인.
Age, Cabin, Embarked에 유실된 값(NaN)이 있는 것을 확인할 수 있다. 머신러닝 전에 전처리 필요.

```javascript
#train 데이터의 결측치 개수 출력
train.isnull().sum()
```
```
**결과**
PassengerId      0
Survived         0
Pclass           0
Name             0
Sex              0
Age            177
SibSp            0
Parch            0
Ticket           0
Fare             0
Cabin          687
Embarked         2
dtype: int64
```

test 데이터도 train 데이터와 동일하게 행,열 정보 및 결측치 개수를 파악
```javascript
test.shape
```
```javascript
test.info()
```
```
**결과**
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 418 entries, 0 to 417
Data columns (total 11 columns):
 #   Column       Non-Null Count  Dtype  
---  ------       --------------  -----  
 0   PassengerId  418 non-null    int64  
 1   Pclass       418 non-null    int64  
 2   Name         418 non-null    object 
 3   Sex          418 non-null    object 
 4   Age          332 non-null    float64
 5   SibSp        418 non-null    int64  
 6   Parch        418 non-null    int64  
 7   Ticket       418 non-null    object 
 8   Fare         417 non-null    float64
 9   Cabin        91 non-null     object 
 10  Embarked     418 non-null    object 
dtypes: float64(2), int64(4), object(5)
memory usage: 36.0+ KB
```

```javascript
test.isnull().sum()
```
```
**결과**
PassengerId      0
Pclass           0
Name             0
Sex              0
Age             86
SibSp            0
Parch            0
Ticket           0
Fare             1
Cabin          327
Embarked         0
dtype: int64
```
>Age, Fare, Cabin에 결측값이 있는 것 확인  

다음 포스팅에는 데이터 시각화를 올려 보겠다.
