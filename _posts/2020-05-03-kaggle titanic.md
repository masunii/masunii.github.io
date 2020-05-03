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
마침 초보자를 위한 좋은 [titanic tutorial](https://www.kaggle.com/alexisbcook/titanic-tutorial)을 찾게되어 walkthrough 형식으로 공부를 해 볼 생각이다.    
일단 시작!

--------------------------------------------------------------------------------------------------------------------
목표: titanic호 탑승자의 정보를 이용하여, 생존 여부 예측하기  

#### 1. data tap에서 data 다운받기  

train.csv: 탑승객 정보가 들어있는 파일 (survived 컬럼 값이 0이면 died, 1이면, survived)  
test.csv: train에서 얻은 패턴 정보를 바탕으로 생존 여부를 예측할 탑승객 정보가 들어있는 파일 (survived 컬럼 값이 없음)  
gender_submission.csv: 예측값 제출할 양식  
  
#### 2. notebook tap에서 문제를 풀 notebook 생성하기  
  
코드 실행: '코드 셀 왼쪽의 run 버튼 누르기' or 'shift + enter 누르기'  
pd: pandas의 줄임. 파이썬 모듈. 데이터 파일의 table을 notebook에 로드하는 역할을 함.
  
* data load하기  
```javascript
train_data = pd.read_csv("/kaggle/input/titanic/train.csv")
train_data.head()
```
```javascript
test_data = pd.read_csv("/kaggle/input/titanic/test.csv")
test_data.head()
```

* 생존 패턴 탐색하기  
  - 성별 기준  
```javascript  
women = train_data.loc[train_data.Sex == 'female']["Survived"]  
rate_women = sum(women)/len(women)  

print("% of women who survived:", rate_women)  
```
```javascript  
men = train_data.loc[train_data.Sex == 'male']["Survived"]  
rate_men = sum(men)/len(men)  

print("% of men who survived:", rate_men)
```
>table.loc: table의 행,열 추출할 때 사용. (예: table.loc[10,:] -> 10행에 해당하는 모든 열의 데이터)    

위 두 cell을 실행하면 각각 여성: 74%, 남성: 19%로 생존했음을 확인할 수 있음  

  - machine learning  
random forest 모델을 만들자  
```javascript  
from sklearn.ensemble import RandomForestClassifier  

y = train_data["Survived"]  

features = ["Pclass", "Sex", "SibSp", "Parch"]  
X = pd.get_dummies(train_data[features])  
X_test = pd.get_dummies(test_data[features])  

model = RandomForestClassifier(n_estimators=100, max_depth=5, random_state=1)  
model.fit(X, y)  
predictions = model.predict(X_test)  

output = pd.DataFrame({'PassengerId': test_data.PassengerId, 'Survived': predictions})  
output.to_csv('my_submission.csv', index=False)  
print("Your submission was successfully saved!")  
```  
>이건 무슨말인지 모르겠다. 따로 공부해야할 듯  


-------------------------------------------------------------------------------------------------------
읭, 이렇게 끝. 정말 입문 튜토리얼이구나!?



