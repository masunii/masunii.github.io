---
title: "kaggle_titanic_(2)데이터 시각화"
date: 2020-05-03 13:00
categories: "STUDY"
tags:
#excerpt: "STUDY"
comments: true
published: false
---

지난 포스트:point_right: [kaggle_titanic_(1)데이터 로드 및 살펴보기](https://masunii.github.io/study/kaggle-titanic(1)/)

------------------------------------------------------------------------------
### 데이터 시각화
```python
#데이터 시각화에 필요한 library 불러오기
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
sns.set() #setting seaborn default for plots
```

```python
#bar 그래프 그리기
#feature: 변수
def bar_chart(feature):
  survived = train[train['Survived']==1][feature].value_counts()
  dead = train[train['Survived']==0][feature].value_counts()
  df = pd.DataFrame([survived,dead])
  df.index = ['survived', 'dead']
  df.plot(kind='bar', stacked=True, figsize=(10,5))
```
  
#### 1.1 성별에 따른 생존비율  
```python
bar_chart('Sex')
```
<img src = "https://user-images.githubusercontent.com/50826051/83611356-d210b880-a5bb-11ea-933d-f27ae7df2c08.png">
> 여성이 남성에 비해 생존 비율이 높음  
    
#### 1.2 Pclass에 따른 생존비율  
```python
bar_chart("Pclass")
```  
<img src = "https://user-images.githubusercontent.com/50826051/83612081-e0130900-a5bc-11ea-8184-09fce3a3f515.png">
  
> Pclass가 높을수록 생존 비율이 높음  


#### 1.3 함께 탑승한 형제/자매 및 배우자 인원 수에 따른 생존비율  
```python
bar_chart("SibSp")
```
<img src = "https://user-images.githubusercontent.com/50826051/83612829-e8b80f00-a5bd-11ea-8dbe-9061ee277409.png">
  
> 혼자 탑승한 경우 사망 비율이 높음. 그 외의 것은 판단 어려움  
  
  
#### 1.4 함께 탑승한 부모, 자식 인원 수에 다른 생존비율  
```python
bar_chart("Parch")
```  
<img src = "https://user-images.githubusercontent.com/50826051/83613063-37fe3f80-a5be-11ea-98c4-cf8cbbabe276.png">
  
> SibSp와 비슷함 (혼자 탑승한 경우 사망 비율이 높음)  


#### 1.5 선착장 위치에 따른 생존비율  
```python
bar_chart("Embarked")
```  
<img src = "https://user-images.githubusercontent.com/50826051/83613170-611ed000-a5be-11ea-9737-2392bce39456.png"> 
  
> S 선착장 탑승자가 많고, 사망 비율이 높음  
  
  
다음 포스트:point_right: [kaggle_titanic_(3)Feature Engineering](https://masunii.github.io/study/kaggle-titanic(3)/)
