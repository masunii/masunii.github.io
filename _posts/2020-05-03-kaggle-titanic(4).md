---
title: "kaggle_titanic_(4)Machine Learning Modelling"
date: 2020-05-03 17:00
categories: "STUDY"
tags:
#excerpt: "STUDY"
comments: true
published: false
---

지난 포스트:point_right: [(3)Feature Engineering](https://masunii.github.io/study/kaggle-titanic(3)/)

------------------------------------------------------------------------------
### Machine learning Modelling  

* kNN 알고리즘: k Nearest Neighbor 알고리즘

* Classifier  
k-fold cross validation  
decision tree  
random forest: 여러 개의 decision tree가 속해있음  
Naive Bayes: 확률 사용  
SVM: Support Vector Machine  
validation: training dataset을 이용  

예) train data(891)을 이용하여 800개는 training에 사용, 89개를 validation으로 사용  


##### Classifier Modules 불러오기
```javascript
from sklearn.neighbors import KNeighborsClassifier
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.naive_bayes import GaussianNB
from sklearn.svm import SVC
```
##### numpy 불러오기
```javascript
import numpy as np
```

```javascript
from sklearn.model_selection import KFold
from sklearn.model_selection import cross_val_score
k_fold = KFold(n_splits=10, shuffle=True, random_state=0)
```

#### Cross Validation (K-fold)

##### kNN  
```javascript
clf = KNeighborsClassifier(n_neighbors = 13)

scoring = 'accuracy'
score = cross_val_score(clf, train_data, target, cv=k_fold, n_jobs=1, scoring=scoring)
print(score)
```

##### kNN score
```javascript
round(np.mean(score)*100, 2)
```
`*결과*`  
```
82.49
```

#### Decision Tree
```javascript
clf = DecisionTreeClassifier()
scoring = 'accuracy'
score = cross_val_score(clf, train_data, target, cv=k_fold, n_jobs=1, scoring=scoring)
print(score)
```

##### decision tree score
```javascript
round(np.mean(score)*100, 2)
```

`*결과*`  
```
82.27
```

#### Random Forest
```javascript
clf = RandomForestClassifier(n_estimators=100)
scoring = 'accuracy'
score = cross_val_score(clf, train_data, target, cv=k_fold, n_jobs=1, scoring=scoring)
print(score)
```
##### Random Forest score
```javascript
round(np.mean(score)*100, 2)
```
`*결과*`  
```
82.83
```

#### Naive Bayes
```javascript
clf = GaussianNB()
scoring = 'accuracy'
score = cross_val_score(clf, train_data, target, cv=k_fold, n_jobs=1, scoring=scoring)
print(score)
```

##### Naive Bayes score
```javascript
round(np.mean(score)*100, 2)
```

`*결과*`  
```
77.88
```

#### SVM
```javascript
clf = SVC()
scoring = 'accuracy'
score = cross_val_score(clf, train_data, target, cv=k_fold, n_jobs=1, scoring=scoring)
print(score)
```

##### SVM score
```javascript
round(np.mean(score)*100, 2)
```
`*결과*`  
```
82.83
```

#### Testing

##### Random Forest 의 점수가 가장 높으므로, Random Forest로 TEST하여 제출하자
```javascript
clf = RandomForestClassifier(n_estimators=100)
clf.fit(train_data, target)

test_data = test.drop("PassengerId", axis=1).copy()
prediction = clf.predict(test_data)
```

##### 제출할 파일 생성
```javascript
submission = pd.DataFrame({
    "PassengerId": test["PassengerId"],
    "Survived": prediction
})

#구글드라이브에 csv형태로 저장
submission.to_csv("gdrive/My Drive/colab/titanic/submission_R.csv", index=False)
```

##### 생성된 파일 확인하기
```javascript
submission = pd.read_csv("gdrive/My Drive/colab/titanic/submission_R.csv")
submission.head()
```

`*결과*`  

|   | PassengerId | Survived |
|--:|------------:|---------:|
| 0 |         892 |        0 |
| 1 |         893 |        1 |
| 2 |         894 |        0 |
| 3 |         895 |        1 |
| 4 |         896 |        0 |  










