---
title: "kaggle_titanic_(3)Feature Engineering"
date: 2020-05-03 15:00
categories: "STUDY"
tags:
#excerpt: "STUDY"
comments: true
---

지난 포스트:point_right: [(2)데이터 시각화](https://masunii.github.io/study/kaggle-titanic(2)/)

------------------------------------------------------------------------------
### Feature Engineering  
#### 1. Name  
##### Name 이름 앞 호칭(?)(Mr, Mrs, Miss 등) 정보를 통해 성별과 결혼 여부를 파악해보자
```javascript
train_test_data = [train, test] #combining train and test dataset.
for dataset in train_test_data:
  dataset['Title'] = dataset['Name'].str.extract(' ([A-Za-z]+)\.', expand=False)
  ```
> [A-Za-z]: 알파벳 모두
str.extract(pat,flag,expand):
pat: extract할 조건
flag: ㅇㅅㅇ. default는 0
expand: True인 경우 하나의 column단위로 나타내고, False인 경우 label 단위로 나타냄

>> .str.extract(' ([A-Za-z]+).', expand=False): 대문자로 시작하여 이후에는 소문자가 나열되며, '.'을 만나면 탐색을 멈추고 string을 출력한다
  
  
```javascript
train['Title'].value_counts()
```

`*결과*`
```
Mr          517
Miss        182
Mrs         125
Master       40
Dr            7
Rev           6
Major         2
Mlle          2
Col           2
Lady          1
Sir           1
Ms            1
Countess      1
Mme           1
Capt          1
Jonkheer      1
Don           1
Name: Title, dtype: int64
```

##### 문자 value 를 숫자로 맵핑하기. (why? 머신러닝에서는 숫자 value만 사용하기 때문에)
```javascript
title_mapping = {"Mr": 0, "Miss": 1, "Mrs": 2,
                "Master": 3, "Dr": 3, "Rev": 3, "Col": 3, "Major": 3, "Mlle": 3, "Countess": 3, 
                "Ms": 3, "Lady": 3, "Jonkheer": 3, "Don": 3, "Dona": 3, "Mme": 3, "Capt": 3, "Sir": 3 }
for dataset in train_test_data:
  dataset['Title'] = dataset['Title'].map(title_mapping)
```

```javascript
bar_chart('Title')
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84147790-f1ff1b00-aa98-11ea-9973-01e193790689.png">  
> 0 = Mr 사망 비율 높음  
1 = Miss, 2 = Mrs 상대적으로 사망 비율 낮음

##### 더 이상 'Name' 정보는 분석에 필요하지 않으므로 삭제.
```javascript
train.drop('Name', axis=1, inplace=True)
test.drop('Name', axis=1, inplace=True)
```  

#### 2. Sex  

##### 문자 value를 숫자로 맵핑하기
```javascript
sex_mapping = {"male": 0, "female": 1}
for dataset in train_test_data:
  dataset['Sex'] = dataset['Sex'].map(sex_mapping)
```

```javascript
bar_chart('Sex')
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84148033-56ba7580-aa99-11ea-9830-9e23cbdec787.png">  
> female(1)의 생존 비율이 높음  

#### 3. Age

##### NaN 값을 'Title'별 평균 나이로 변환
```javascript
train["Age"].fillna(train.groupby("Title")["Age"].transform("median"), inplace=True)
test["Age"].fillna(test.groupby("Title")["Age"].transform("median"), inplace=True)
```

##### 연령별 생존여부를 그래프로 나타내기
```javascript
facet = sns.FacetGrid(train, hue="Survived", aspect =4)
facet.map(sns.kdeplot, 'Age', shade = True)
facet.set(xlim=(0, train['Age'].max()))
facet.add_legend()

plt.show()
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84148400-e5c78d80-aa99-11ea-9511-c4bffcca1684.png">

> hue: 색상으로 구분되는 범례 feature

##### 연령 구간을 좁혀서 더 자세히 보기
```javascript
facet = sns.FacetGrid(train, hue="Survived", aspect =4)
facet.map(sns.kdeplot, 'Age', shade = True)
facet.set(xlim=(0, train['Age'].max()))
facet.add_legend()
plt.xlim(40, )
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84148551-21faee00-aa9a-11ea-8f13-90a71118062e.png">

##### 연령대 구간을 5개로 나눠보자
```javascript
train['AgeBand'] = pd.cut(train['Age'], 5)
train[['AgeBand', 'Survived']].groupby('AgeBand', as_index=False).mean().sort_values(by='AgeBand', ascending=True)
```

`*결과*`  

|   |          AgeBand | Survived |
|--:|-----------------:|---------:|
| 0 |   (0.34, 16.336] | 0.542857 |
| 1 | (16.336, 32.252] | 0.327345 |
| 2 | (32.252, 48.168] | 0.439024 |
| 3 | (48.168, 64.084] | 0.434783 |
| 4 |   (64.084, 80.0] | 0.090909 |  


##### 연령대를 그룹화하여 맵핑하기
```javascript
for dataset in train_test_data:
  dataset.loc[dataset['Age'] <= 16, 'Age'] = 0
  dataset.loc[(dataset['Age'] > 16) & (dataset['Age'] <= 32), 'Age'] = 1
  dataset.loc[(dataset['Age'] > 32) & (dataset['Age'] <= 48), 'Age'] = 2
  dataset.loc[(dataset['Age'] > 48) & (dataset['Age'] <= 64), 'Age'] = 3
  dataset.loc[dataset['Age'] > 64, 'Age'] = 4
```

```javasccript
bar_chart('Age')
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84148884-96359180-aa9a-11ea-9eec-6c5abb6ab26b.png">  
> 16~32세 그룹의 사망 비율이 높음

#### 4. Embarked

##### 탑승 위치에 따라 Pclass가 다른 지 확인해보자
```javascript
Pclass1 = train[train['Pclass']==1]['Embarked'].value_counts()
Pclass2 = train[train['Pclass']==2]['Embarked'].value_counts()
Pclass3 = train[train['Pclass']==3]['Embarked'].value_counts()
df = pd.DataFrame([Pclass1, Pclass2, Pclass3])
df.index = ['1st class', '2nd class', '3rd class']
df.plot(kind='bar', stacked=True, figsize=(10,5))
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84149059-d09f2e80-aa9a-11ea-8471-22a247aa795e.png">  
> Q 선착장에서 탑승한 사람은 거의 3rd class만 구매  
S 선착장에서 탑승한 사람이 전체 승객의 과반수 이상으로 보임. 'Embarked' cloumn에 NaN 값이 있다면 S로 입력해도 될 것 같음.  

##### 'Embarked' 컬럼의 NaN 값을 'S'로 변환
```javascript
for dataset in train_test_data:
  dataset['Embarked'] = dataset['Embarked'].fillna('S')
```

```javascript
train['Embarked'].value_counts()
```
`*결과*`
```
S    646
C    168
Q     77
Name: Embarked, dtype: int64
```

```javascript
embarked_mapping = {"S": 0, "C": 1, "Q": 2}
for dataset in train_test_data:
  dataset['Embarked'] = dataset['Embarked'].map(embarked_mapping)
```

```javascript
bar_chart('Embarked')
```
`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84149364-515e2a80-aa9b-11ea-956b-29e9244307af.png">  
> 0 = S 선착장에서 탑승한 사람의 사망비율이 높음  

#### 5. Fare

##### NaN값을 각각 Pclass의 평균값으로 변환
```javascript
train["Fare"].fillna(train.groupby("Pclass")["Fare"].transform("median"), inplace=True)
test["Fare"].fillna(test.groupby("Pclass")["Fare"].transform("median"), inplace=True)
```  
```javascript
facet = sns.FacetGrid(train, hue="Survived", aspect =4)
facet.map(sns.kdeplot, 'Fare', shade = True)
facet.set(xlim=(0, train['Fare'].max()))
facet.add_legend()
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84149577-a601a580-aa9b-11ea-9196-26eda78a9564.png">  
> 저렴한 티켓 소지자의 사망 비율이 높음

```javascript
facet = sns.FacetGrid(train, hue="Survived", aspect =4)
facet.map(sns.kdeplot, 'Fare', shade = True)
facet.set(xlim=(0, train['Fare'].max()))
facet.add_legend()
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84149691-cfbacc80-aa9b-11ea-848e-a9d9e97afe8f.png">  

##### Fare 구간을 4개로 나눠보자 
```javascript
train['Fareband'] = pd.qcut(train['Fare'], 4)
train[['Fareband','Survived']].groupby('Fareband').mean().sort_values(by='Survived', ascending=False)
```

`*결과*`  

|        Fareband | Survived |   |
|----------------:|---------:|---|
| (31.0, 512.329] | 0.581081 |   |
|  (14.454, 31.0] | 0.454955 |   |
|  (7.91, 14.454] | 0.303571 |   |
|  (-0.001, 7.91] | 0.197309 |   |  

##### Fare을 구간별로 그룹화하여 맵핑하기
```javascript
for dataset in train_test_data:
  dataset.loc[dataset['Fare'] <= 7.91, 'Fare'] = 0
  dataset.loc[(dataset['Fare'] > 7.91) & (dataset['Fare'] <= 14.454), 'Fare'] = 1
  dataset.loc[(dataset['Fare'] > 14.454) & (dataset['Fare'] <= 31.0), 'Fare'] = 2
  dataset.loc[dataset['Fare'] >=31, 'Fare'] = 3
```

```javascript
bar_chart('Fare')
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84149916-21635700-aa9c-11ea-8468-2797c8879b6e.png">  

#### 6. Cabin  
##### Cabin의 알파벳 정보를 기준으로 분류 (Cabin 컬럼 value를 Cabin value의 첫번째 문자로 치환하기)  
```javascript
for dataset in train_test_data:
  dataset['Cabin'] = dataset['Cabin'].str[:1]
```

```javascript
Pclass1 = train[train['Pclass']==1]['Cabin'].value_counts()
Pclass2 = train[train['Pclass']==2]['Cabin'].value_counts()
Pclass3 = train[train['Pclass']==3]['Cabin'].value_counts()
df = pd.DataFrame([Pclass1, Pclass2, Pclass3])
df.index = ['1st class', '2nd class', '3rd class']
df.plot(kind='bar', stacked=True, figsize=(10,5))
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84150132-65565c00-aa9c-11ea-849b-15f0641cf398.png">  
> C, B, A: 1st Class에만 해당  
G: 3rd Class에만 해당

##### 문자 Value를 숫자로 맵핑하기  
```javascript
cabin_mapping = {'A': 0, 'B': 0.4, 'C': 0.8, 'D': 1.2, 'E': 1.6, 'F': 2.0, 'G': 2.4, 'T': 2.8}
for dataset in train_test_data:
  dataset['Cabin'] = dataset['Cabin'].map(cabin_mapping)
```  
> 소수점으로 맵핑한 이유: 다른 feature들과의 전체 gap 정도를 맞추기 위해서.  
#예를들어, sex는 0,1 두 가지로만 구분되어, gap이 1-0=1인데, Cabin은 8가지로 구분되어 0,1,2,...7이 되면, gap이 7-0=7이 됨.  
#이렇게되면 머싱러닝 시 정상적으로 비교되지 않을 수 있음.  
#Title이 0부터 3까지이므로, 이에 근사하게 0.4 간격으로 0부터 2.8까지 맵핑하기로 한다.  

```javascript
bar_chart('Cabin')
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84150361-b9614080-aa9c-11ea-97f5-a39a2725a2f5.png">  
> B, D, E 의 사망비율이 적음  

##### Cabin의 NaN값을 각 Pclass의 평균값으로 변환  
```javascript
train["Cabin"].fillna(train.groupby("Pclass")["Cabin"].transform("median"), inplace=True)
test["Cabin"].fillna(train.groupby("Pclass")["Cabin"].transform("median"), inplace=True)
```

#### 7. Family size  
##### family size를 가늠할 수 있는 'SibSp'와 'Parch' 두 feature를 하나로 합쳐보자.
```javascript
train["FamilySize"] = train["SibSp"]+train["Parch"] + 1
test["FamilySize"] = test["SibSp"]+test["Parch"] + 1
```
> 왜 끝에 +1 하는거지?
1 값이 '나' 자신을 카운트하는건가?  

##### 위 내용을 그래프로 나타내기
```javascript
facet = sns.FacetGrid(train, hue="Survived", aspect =4)
facet.map(sns.kdeplot, 'FamilySize', shade = True)
facet.set(xlim=(0, train['FamilySize'].max()))
facet.add_legend()
```

`*결과*`  
<img src = "https://user-images.githubusercontent.com/50826051/84150852-6d62cb80-aa9d-11ea-88ab-19e8c0cace38.png"> 
> 1일 때 사망비율이 가장 높음


#### 마무리 - 필요 없는 column 삭제  
```javascript
features_drop = ['Ticket', 'SibSp', 'Parch']
train = train.drop(features_drop, axis = 1)
test = test.drop(features_drop, axis = 1)
train = train.drop(['PassengerId', 'AgeBand', 'Fareband'], axis = 1)
```

#### data set 재정의  
```javascript
train_data = train.drop('Survived', axis=1)
target = train['Survived']

train_data.shape, target.shape
```

`*결과*`  
```
((891, 8), (891,))
```


다음 포스트:point_right: [(4)Machine Learning Modelling](https://masunii.github.io/study/kaggle-titanic(4)/)
