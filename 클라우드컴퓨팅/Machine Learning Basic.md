### AI, ML, and DL
+ AI: 전체 개념으로, 사람의 지능의 기능을 모방하는 시스템을 포괄해 일컫는다.
+ ML: AI의 한 분야로, 데이터를 통해 학습하고 예측할 수 있는 알고리즘을 개발한다.
+ DL: ML의 하위 분야로, 인공 신경망을 통해 대량의 데이터를 처리하고 학습하는 기술

![500](Pasted%20image%2020241023222005.png)
#### ML vs DL
ML은 주어진 데이터를 인간이 전처리하는 과정을 포함한다. 일반 목적에 맞게 설계되어 있으며, 데이터에서 자율적으로 특징을 추출할 수 없다. 사람이 데이터를 처리해 컴퓨터가 이해할 만한 특징들을 전처리한 후 기계가 데이터의 특성을 인식하고 학습하여 문제를 해결하도록 만드는 기술이다.

DL은 인간이 데이터를 전처리하지 않고, 방대한 양의 특징들을 가진 데이터를 신경망에 적용함으로써 기계가 자율적으로 특징을 추출하고, 분석해 문제를 해결하도록 만드는 기술이다.

![500](Pasted%20image%2020241023222320.png)
### Terms of ML
+ Sample: 훈련 데이터 인스턴스의 모음 (row)
+ Feature: 예측을 하거나 문제를 해결하는 데 사용되는 데이터의 개별 측정 가능한 속성(column)
+ Training Dataset: 모델을 훈련하는 데 사용되는 데이터
+ Test Dataset: 모델이 정확한 예측을 수행하는 능력을 측정하는 데 사용되는 데이터
+ Preprocessing: 모델 훈련 전 데이터를 처리하는 단계. cleaning, scaling, encoding, 특징 선택, 데이터 분할, 정규화 및 불균형 데이터 처리 등 모두를 포함한다.
+ Target: 모델이 예측하거나 분류하고자 하는 데이터 내 특징 혹은 레이블
### Types of ML
+ Supervised Learning(지도 학습): 이미 정답이 있는 데이터를 통해 학습한 후 특징을 통해 정답을 예측하도록 하는 학습 방법.
+ Unsupervised Learning(비지도 학습): 예측하고자하는 target이 없는 데이터를 입력해 모델이 직접 각 인스턴스들을 분류하고, 구분하도록 하는 학습 방법.
### ML Dataset
ML model을 학습시키기 위해서는 고품질의 데이터셋이 필요하다. 뛰어난 성능의 ML algorithm이 있더라도, 고품질의 훈련 데이터 없이는 좋은 결과를 얻기 어렵다. 

Data Quality(데이터 품질)은 데이터가 해결하고자 한느 문제를 얼마나 잘 반영하는 지에 대한 정도를 말한다. 문제 해결의 성능에 즉각적으로 영향을 주는 요소이다. 

이러한 좋은 품질의 데이터를 수집하는 방법은 크게 두 가지 방법이 있다.
1. 직접 수집: 설문조사, 센서, 사용자 입력 등 다양한 방법을 통해 필요한 데이터를 직접 수집
2. 공개 데이터셋 활용: 연구 및 실험에 유용하게 활용되도록 공개되어 있는 정재된 데이터를 활용

#### Scikit-learn Dataset
Scikit-learn(sickits.learn, sklearn)은 python을 위한 무료 오픈소스 ML 라이브러리이다. sickit-learn의 데이터셋 패키지는 여러 ML 데이터셋의 축소 버전(toy dataset)을 제공한다. (Iris, Boston, Digits 등)

![500](Pasted%20image%2020241023223527.png)
#### AWS Open Access Dataset
Amazon에서 운영하는 ML 공개 데이터셋으로, AWS에 배포된 애플리케이션과 쉽게 통합할 수 있다.
+ Biology: human genome project와 같은 널리 알려진 데이터셋을 포함
+ Chemistry: 다양한 화학 분자 공식과 관련된 데이터를 포함
+ Economics: 인구 조사와 같은 다양한 경제 관련 데이터 제공
+ Encyclopedias: 위키피디아 및 기타 여러 출처의 데이터를 제공
#### Kaggle.com Dataset
Kaggle.com은 ML 대회를 주최하는 가장 유명한 웹사이트 중 하나이다. 이 사이트 또한 사용가능한 다양한 데이터셋을 제공하고 있다. Kaagle 경연 대회에서 사용되는 자체 데이터셋도 제공한다. Pandas DataFrame 형식으로 제공하고 있다.
#### UCI Machine Learning Repository
UC Irvine의 Machine Learning and Intelligent Systems Center에서 운영하는 서비스이다. 450개 이상의 공개 데이터셋을 공유하고 있으며 ML 데이터셋 저장소 중 가장 오래된 곳이다. 데이터는 모델 생성을 위해 사용하기 전 전처리가 필요하다. Pandas 혹은 Scikit-learn을 사용해 데이터 분석이 가능하다.
### Data Exploration
ML 모델을 만들 때 첫 번째 작업 중 하나는 변수에 대한 초기 정보를 수집하는 것이다. 
#### Loading Raw Data
```python
import numpy as np
import pandas as pd

url = './datasets/titanic_dataset/original/train.csv'
df_titanic = pd.read_csv(url)
```
#### Checking Data Shape
```python
print(df_titanic.shape) # (891, 12)
```
#### Checking for Missing Values
Data scientists가 흔히 겪는 문제 중 하나는 결측값(missing value, NaN) 처리이다. Raw data는 종종 하나 이상의 열에서 결측값을 포함하고 있으며, 이는 입력 오류나 실제 관측값의 부족 등 여러 가지로 발생할 수 있다. CSV 파일을 Pandas DataFrame으로 불러올 때 결측값은 'NaN'으로 표시된다.

```python
pd.set_option('display.max_columns', None)
df_titanic.head()
```

![](Pasted%20image%2020241023224703.png)

결측값을 식별하는 다양한 방법 중 하나는 `info()` 함수를 사용하는 것이다.

![500](Pasted%20image%2020241023225223.png)

다른 방법은 `isnull()` 함수를 사용하는 방법도 존재한다.

![500](Pasted%20image%2020241023225202.png)
#### Index Setup
Dataframe에서 인덱스의 존재 여부를 확인하려면 `df.index.name`을 출력해보면된다. 특정 속성을 index로 설정할려면 아래와 같은 방법을 사용하면 된다. 기존 속성을 index로 바꾸게 된다면, column이 하나 감소하게 된다.

![](Pasted%20image%2020241023225459.png)
#### Target Variable and Feature
ML 학습을 하기 위해 target와 feature를 따로 분리해 저장할 필요가 있다.

![500](Pasted%20image%2020241023225624.png)

그 후 target 값들이 1:1 비율인지를 확인해보는 것이 좋다. 아래와 같이 수치로 확인할 수도 있지만, Historgram을 통해도 확인이 가능하다.

![500](Pasted%20image%2020241023225731.png)

![500](Pasted%20image%2020241023225831.png)

물론 각 feature가 어떻게 분산되어 있는지도 확인하는 것이 좋다.

![500](Pasted%20image%2020241023225924.png)

Historgram을 그릴 때 다음과 같은 다양한 기법을 통해 이해를 도울 수 있다.
+ 당양한 bin 크기를 적용하는 것은 특징의 분포를 이해하는 데 도움이 될 수 있다. `df.hist(column='Age', figsize=(5, 5), bins=2)`
+ `value_counts()` 함수는 범주형 데이터의 histogram을 나타내는 데 사용될 수 있다.

`discribe()` 함수를 통해 feature의 통계치를 확인해 볼 수 있다. 또한, `df.boxplot(figsize=(10, 6))`을 통해 boxplot의 형태로 각 특징을 그려볼 수도 있다.

![500](Pasted%20image%2020241023230251.png)
#### Correlation Analysis
각 Feature들끼리의 상관관계(Correlation)를 다음과 같은 방법으로 분석할 수 있다.

![500](Pasted%20image%2020241023230456.png)

![500](Pasted%20image%2020241023230507.png)

### Data Preprocessing Technique
#### Handling Missing Values
Titanic 데이터에서 결측값은 Age, Cabin, Embarked feature에서 존재한다. 각 feature에 따라 적절한 처리가 필요하다. 

Age는 숫자 데이터이고, 중앙값은 28이다. Pandas에서 `fillna()` 함수를 사용하여 결측값을 새로운 값으로 대체할 수 있다. Age의 결측값들을 중앙 값인 28로 대체한다.
```python
median_age = df_titanic_features['Age'].median()
df_titanic_features['Age'].fillna(median_age, inplace=True)
```

Embarked는 범주형 데이터이다. Embarked에는 두 개의 결측값이 존재한다. 범주형 데이터의 경우 가장 많이 등장한 값으로 결측값을 대체한다.
```python
embarked_value_counts = df_titanic_features['Embarked'].value_counts(dropna=True)
most_common_value = embarked_value_counts.index[0]
df_titanic_features['Embarked'].fillna(most_common_value, inplace=True)
```

Cabin은 매우 많은 결측값이 존재한다. 그래서 Embarked의 경우처럼 가장 많이 등장한 값으로 대체할시 편향(Bias) 문제가 발생할 수 있다. 이러한 경우 새로운 feature로 boolean 값인 CabinIsKnown으로 Cabin을 대체해 사용하는 것이 좋다.
```python
df_titanic_features['CabinIsKnown'] = ~df_titanic_features.Cabin.isnull()
df_titanic_features.drop(['Cabin'], axis=1, inplace=True)
```
#### Creating New Features
SibSp와 Parch feature는 다음과 같은 feature이다.
+ SibSp: 함께 여행하는 형제자매/배우자의 수를 나타내는 숫자 변수
+ Parch: 함께 여행하는 부모 및 자녀의 수를 나타내는 숫자 변수

두 feature를 하나로 묶어 함께 여행한 가족의 수를 나타내는 FamilySize feature를 생성할 수 있다.
```python
df_titanic_features['FamilySize'] = df_titanic_features.SibSp + df_titanic_features.Parch
```

Age와 Fare feature는 특정 범위를 가진 숫자 데이터이다. 범위를 가진 숫자 데이터의 경우 데이터를 그룹화하는 것이 종종 도움이 된다. Pandas의 `cut()` 함수를 사용해 AgeCategory라는 새로운 범주형 특성을 생성할 수 있다.
```python
bins_age = [0, 20, 30, 40, 50, 150]
labels_age = ['<20', '20-30', '30-40', '40-50', '>50']
df_titanic_features['AgeCategory'] = pd.cut(df_titanic_features.Age, 
											bins=bins_age, 
											labels=labels_age, 
											include_lowest=True)
df_titanic_features['FareCategory'] = pd.cut(df_titanic_features.Fare, 
											q=4,
											labels=['Q1', 'Q2', 'Q3', 'Q4'], 
											include_lowest=True)
```
#### Transforming Numeric Features
데이터를 학습시킬 때 각 숫자 feature 데이터의 범위가 모두 다르기 때문에 정규화 및 표준화가 필요하다.
+ Normalization: 0 to 1
+ Standardization: 평균(mean) = 0, 분산(variance) = 1

```python
from sklean import preprocessing

minmax_scaler = preprocessing.MinMaxScaler()
ndNormalizedAge = minmax_scaler.fit_transform(df_titanic_features['Age'].values)
df_titanic_features['NormalizedAge'] = pd.DataFrame(ndNormalizedAge)

standard_scaler = preprocessing.StandardScaler()
ndStandardizedAge = standard_scaler.fit_transform(df_titanic_features['Age'].values)
df_titanic_features['StandardizedAge'] = pd.DataFrame(ndStandardizedAge)
```
#### One-Hot Encoding Categorical Features
One-Hot Encoding은 범주형 데이터를 숫자 데이터로 변환하는 방법이다. 범주형 데이터를 숫자 데이터로 변환하는 이유는 많은 ML algorithm이 범주형 데이터를 직접 처리할 수 없기 때문이다.

sex_male과 sex_female은 매우 강한 negative correlation을 나타낸다. 따라서 이들 중 하나에 대한 정보를 알면 다른 지표를 예측하는 데 충분하다. 또한, 불필요한 feature들이 ML model 훈련에서 제거시켜 의미 있는 특성과 숫자 데이터만 포함된 데이터 프레임을 생성할 수 있다.

```python
corr_matrix = df_titanic_features[['Sex_mail', 'Sex_female']].corr()

df_titanic_features_numeric = df_titanic_features.drop(['Name', 'Ticket', 'Sex_female', 'CabinIsKnown_False'], axis=1)
```

### Breast Cancer Diagnosis Using SVM and KNN
#### Data Preprocessing and Exploration
```python
import numpy as np  
from sklearn import preprocessing  
from sklearn.neighbors import KNeighborsClassifier  
from sklearn.svm import SVC  
from sklearn import model_selection  
from sklearn.metrics import classification_report  
from sklearn.metrics import accuracy_score  
from pandas.plotting import scatter_matrix  
import matplotlib.pyplot as plt  
import pandas as pd

# Loading dataset  
url = "https://archive.ics.uci.edu/ml/machine-learning-databases/breast-cancer-wisconsin/breast-cancer-wisconsin.data"  
names = ['id', 'clump_thickness', 'uniform_cell_size', 'uniform_cell_shape',  
        'marginal_adhesion', 'single_epithelial_size', 'bare_nuclei',  
        'bland_chromatin', 'normal_nucleoli', 'mitoses', 'class']  
df = pd.read_csv(url, names=names)

# Data Preprocessing 
df.replace('?', -99999, inplace=True)  
print(df.axes) 
# [RangeIndex(start=0, stop=699, step=1), Index(['id', 'clump_thickness', 'uniform_cell_size', 'uniform_cell_shape',
#       'marginal_adhesion', 'single_epithelial_size', 'bare_nuclei',
#       'bland_chromatin', 'normal_nucleoli', 'mitoses', 'class'],
#       dtype='object')]

df.drop(['id'], 1, inplace=True)
# Data Column Check  
print(df.loc[10])
# clump_thickness           1
# uniform_cell_size         1
# uniform_cell_shape        1
# marginal_adhesion         1
# single_epithelial_size    1
# bare_nuclei               1
# bland_chromatin           3
# normal_nucleoli           1
# mitoses                   1
# class                     2
# Name: 10, dtype: object

# Check Data Shape  
print(df.shape) # (699, 10)
# Description of dataset  
print(df.describe())

# Histogram of features  
df.hist(figsize = (10, 10));  
plt.show()

# Scatter matrix for correlation  
scatter_matrix(df, figsize = (18,18));  
plt.show()
```
#### Seperation of dataset
```python
# feature, target seperation  
X = np.array(df.drop(['class'], 1))  
y = np.array(df['class'])  
  
X_train, X_test, y_train, y_test = model_selection.train_test_split(X, y, test_size=0.2)  
  
# parameter setup seed = 8  
scoring = 'accuracy'
```
#### Model Training
```python
 Model Definitionmodels = []  
models.append(('KNN', KNeighborsClassifier(n_neighbors = 5)))  
models.append(('SVM', SVC(gamma="auto")))  
  
# Model Evaluation results = []  
names = []  
  
for name, model in models:  
    kfold = model_selection.KFold(n_splits=10)  
    cv_results = model_selection.cross_val_score(model, X_train, y_train, cv=kfold, scoring=scoring)  
    results.append(cv_results)  
    names.append(name)  
    msg = "%s: %f (%f)" % (name, cv_results.mean(), cv_results.std())  
    print(msg)

# KNN: 0.967825 (0.019209)
# SVM: 0.957110 (0.031109)
```
#### Evaluation of Model
```python
# Evaluation of Models Using Test set  
  
for name, model in models:  
    model.fit(X_train, y_train)  
    predictions = model.predict(X_test)  
    print(name)  
    print(accuracy_score(y_test, predictions))  
    print(classification_report(y_test, predictions))
```

![500](Pasted%20image%2020241023234130.png)

### Diabetes Prediction
```python
import numpy as np  
import pandas as pd
import sklearn  
import keras
```
#### Data exploration and data preprocessing
```python
url = "https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.data.csv"  
names = ['n_pregnant', 'glucose_concentration', 'blood_pressure (mm Hg)', 'skin_thickness (mm)', 'serum_insulin (mu U/ml)', 'BMI', 'pedigree_function', 'age', 'class']  
  
df = pd.read_csv(url, names=names)

df.describe()
```

![](Pasted%20image%2020241023234439.png)

```python
df[df['glucose_concentration'] == 0] # 혈당값이 0인 행들을 필터링해 보자.
```

![](Pasted%20image%2020241023234524.png)

먼저 결측값을 `NaN`(`np.nan`) 값으로 코딩한 후에, 결측값이 있는 행들을 제거하려고 한다. 이렇게 하기 위해서 검색 대상 열을 정의한다. `n_prgenancy`, `age`, `class`를 제외한 열들을 선택한다.

```python
columns = ['glucose_concentration', 'blood_pressure (mm Hg)', 'skin_thickness (mm)', 'serum_insulin (mu U/ml)', 'BMI']

for col in columns:  
    df[col].replace(0, np.nan, inplace=True)  
df.describe()
```

![](Pasted%20image%2020241023234633.png)

이제 결측값을 모두 `NaN`으로 처리했다. 이제 결측값을 가진 사례들을 다음과 같은 코드로 제거한다.

```python
df.dropna(inplace=True)  
df.describe()

# 데이터프레임을 numpy 배열로 변환 dataset = df.values  
print(dataset.shape) # (392, 9)
X = dataset[:, 0:8]  
Y = dataset[:, 8].astype(int)
print(X.shape)  # (392, 8)
print(Y.shape)  # (392, )
print(X[:5])  
print(Y[:5])
```
#### Data Normalization
```python
from sklearn.preprocessing import StandardScaler  
scaler = StandardScaler().fit(X)
# 훈련 데이터의 변환과 출력  
X_standardized = scaler.transform(X)  
  
data = pd.DataFrame(X_standardized)  
data.describe()
```

![](Pasted%20image%2020241023234814.png)
#### Define Model
```python
from sklearn.model_selection import GridSearchCV, KFold  
from keras.models import Sequential  
from keras.layers import Dense  
from keras.wrappers.scikit_learn import KerasClassifier  
from keras.optimizers import Adam

def create_model():  
    #  케라스 모델 정의   
	model = Sequential()  
    model.add(Dense(8, input_dim = 8, kernel_initializer='normal', activation='relu'))  
    model.add(Dense(4, input_dim = 8, kernel_initializer='normal', activation='relu'))  
    model.add(Dense(1, activation='sigmoid'))  
   
    # 모델 컴파일   
    adam = Adam(lr = 0.01)  
    model.compile(loss = 'binary_crossentropy', optimizer = adam, metrics = ['accuracy'])  
    return model 

model = create_model()  
print(model.summary())
```

![500](Pasted%20image%2020241023235835.png)

Hyperparameter는 DL model을 훈련할 때 변경할 수 있으며, 모델의 성능에 영향을 미칠 수 있는 변수를 말한다. Dropout 여부, 학습률(lr), epoch크기, batch 크기 등을 말하며, 이 값들이 최적화되어야지 높은 성능을 얻을 수 있다.
#### scikit-learn  Grid Search Hyperparameter Tuning
```python
# 최적 배치 크기와 에포크를 정하기 위한 그리드 탐색  
# 랜덤 시드 정의  
seed = 6  
np.random.seed(seed)  
  
# 모델 정의  
def create_model():  
    # 케라스 모델 생성  
    model = Sequential()  
    model.add(Dense(8, input_dim = 8,   
kernel_initializer='normal', activation='relu'))  
    model.add(Dense(4, input_dim = 8,   
kernel_initializer='normal', activation='relu'))  
    model.add(Dense(1, activation='sigmoid'))  
  
    # 모델 컴파일   
	adam = Adam(lr = 0.01)
	model.compile(loss = 'binary_crossentropy', optimizer = adam, metrics = ['accuracy'])  
    return model  
  
# 모델 생성  
model = KerasClassifier(build_fn = create_model, verbose = 1)  
  
# 그리드 탐색 매개변수 정의 
batch_size = [10, 20, 40]  
epochs = [10, 50, 100]  
  
# 그리드 탐색 매개변수를 딕셔너리로 만들기 
param_grid = dict(batch_size=batch_size, epochs=epochs)  
  
# GridSearchCV 빌드와 적합  
grid = GridSearchCV(estimator = model, param_grid = param_grid,   
cv = KFold(random_state=seed, shuffle=True), verbose = 10)  
grid_results = grid.fit(X_standardized, Y)  
  
# 결과 보고  
print("Best: {0}, using {1}".format(grid_results.best_score_, grid_results.best_params_))  
means = grid_results.cv_results_['mean_test_score']  
stds = grid_results.cv_results_['std_test_score']  
params = grid_results.cv_results_['params']  
for mean, stdev, param in zip(means, stds, params):  
    print('{0} ({1}) with: {2}'.format(mean, stdev, param))
```
#### Reducing Overfitting with Dropout
DL는 많은 수의 훈련 샘플을 사용하는 것이 일반적이다. 훈련 중 모델의 복잡성이 증가함에 따라 DL의 성능이 향상된다. 이는 레이어 수를 증가시키거나 레이어 내 뉴런 수를 늘리는 등의 작업을 포함할 수 있다. 그러나 이러한 모든 과정은 DL의 훈련 과정에서 과적합(Overfitting)을 초래할 수 있다. 

이 문제를 해결하는 일반적인 방법은 dropdout이 있다. 

```python
# 최적 배치 크기와 에포크를 정하기 위한 그리드 탐색  
# 필요한 패키지 임포트  
from keras.layers import Dropout  # 임포트 라인에 추가   
# 랜덤 시드 정의  
seed = 6  
np.random.seed(seed)  
  
# 모델 정의 
def create_model(learn_rate, dropout_rate): # 학습률과 드롭아웃 비율을 인자로  
    # 카라스 모델 생성  
    model = Sequential()  
    model.add(Dense(8, input_dim = 8, kernel_initializer='normal', activation='relu'))  
    model.add(Dropout(dropout_rate))        # 드롭아웃 레이어 추가   
    model.add(Dense(4, input_dim = 8, kernel_initializer='normal', activation='relu'))  
    model.add(Dropout(dropout_rate))        # 드롭아웃 레이어 추가     
    model.add(Dense(1, activation='sigmoid'))  
      
    # 모델 컴파일   
    adam = Adam(lr = learn_rate)            # 학습률에 대한 변수   
    model.compile(loss = 'binary_crossentropy', optimizer = adam, metrics = ['accuracy'])  
    return model  
    
# 모델 생성 
model = KerasClassifier(build_fn = create_model, epochs = 50,   
batch_size = 10, verbose = 0)  
                          
  
# 그리트 탐색 매개변수 정의  
learn_rate = [0.001, 0.01, 0.1]  
dropout_rate = [0.0, 0.1, 0.2]  
  
# 그리드 탐색 매개변수를 딕셔너리로 변환 
param_grid = dict(learn_rate=learn_rate, dropout_rate=dropout_rate)  
  
# GridSearchCV 빌드와 적합  
grid = GridSearchCV(estimator = model, param_grid = param_grid,   
cv = KFold(random_state=seed, shuffle=True), verbose = 10)  
grid_results = grid.fit(X_standardized, Y)  
  
# 결과 보고  
print("Best: {0}, using {1}".format(grid_results.best_score_, grid_results.best_params_))  
means = grid_results.cv_results_['mean_test_score']  
stds = grid_results.cv_results_['std_test_score']  
params = grid_results.cv_results_['params']  
for mean, stdev, param in zip(means, stds, params):  
    print('{0} ({1}) with: {2}'.format(mean, stdev, param))
```
#### 최적 매개변수 찾기
```python
# activation, init 그리드 탐색   
# 랜덤 시드 지정  
seed = 6  
np.random.seed(seed)  
  
# 모델 정의 
def create_model(activation, init):  # 인자 지정   
	# 모델 생성  
    model = Sequential()  
    model.add(Dense(8, input_dim = 8, kernel_initializer= init, activation=activation))
    model.add(Dense(4, input_dim = 8, kernel_initializer= init, activation=activation))
    model.add(Dense(1, activation='sigmoid'))  
      
    # 모델 컴파일   
    adam = Adam(lr = 0.001)   # 학습률을 하드코딩함.  
    model.compile(loss = 'binary_crossentropy', optimizer = adam, metrics = ['accuracy'])  
    return model  
  
# 모델 생성  
model = KerasClassifier(build_fn=create_model, epochs=100, batch_size=20, verbose=0)  
  
# 그리드 탐색 매개변수 정의 
activation = ['softmax', 'relu', 'tanh', 'linear']  
init = ['uniform', 'normal', 'zero']  
  
# 그리드 탐색 매개변수를 딕셔너리로 변환 
param_grid = dict(activation = activation, init = init)  
  
# GridSearchCV 빌드와 적합  
grid = GridSearchCV(estimator = model, param_grid = param_grid,   
cv = KFold(random_state=seed, shuffle=True), verbose = 10)  
grid_results = grid.fit(X_standardized, Y)  
  
# 결과 보고 print("Best: {0}, using {1}".format(grid_results.best_score_, grid_results.best_params_))  
means = grid_results.cv_results_['mean_test_score']  
stds = grid_results.cv_results_['std_test_score']  
params = grid_results.cv_results_['params']  
for mean, stdev, param in zip(means, stds, params):  
    print('{0} ({1}) with: {2}'.format(mean, stdev, param))
```
#### 뉴런 개수 최적화
```python
# 히든 레이어의 뉴런의 최적 개수를 찾기 위한 그리드 탐색   
# 랜덤 시드 설정 
seed = 6  
np.random.seed(seed)  
  
# 모델 정의 
def create_model(neuron1, neuron2):  
	# 케라스 모델 생성  
	model = Sequential()  
	model.add(Dense(neuron1, input_dim = 8, kernel_initializer= 'uniform',   
activation= 'linear'))  
	model.add(Dense(neuron2, input_dim = neuron1, kernel_initializer= 'uniform',   
activation= 'linear'))
	model.add(Dense(1, activation='sigmoid'))  
      
    # 모델 컴파일  
    adam = Adam(lr = 0.001)  
    model.compile(loss = 'binary_crossentropy', optimizer = adam, metrics = ['accuracy'])  
    return model  
  
# 모델 생성  
model = KerasClassifier(build_fn=create_model, epochs=100, batch_size=20, verbose=0)  
  
# 그리드 탐색 매개변수 설정 
neuron1 = [4, 8, 16]  
neuron2 = [2, 4, 8]  
  
# 그리드 탐색 매개변수를 딕셔너리로 변화 
param_grid = dict(neuron1 = neuron1, neuron2 = neuron2)  
  
# GridSearchCV 빌드와 적합  
grid = GridSearchCV(estimator = model,   
param_grid = param_grid,   
cv = KFold(random_state=seed, shuffle=True), refit = True, verbose = 10)  
grid_results = grid.fit(X_standardized, Y)  
  
# 결과 보고  
print("Best: {0}, using {1}".format(grid_results.best_score_, grid_results.best_params_))  
means = grid_results.cv_results_['mean_test_score']  
stds = grid_results.cv_results_['std_test_score']  
params = grid_results.cv_results_['params']  
for mean, stdev, param in zip(means, stds, params):  
    print('{0} ({1}) with: {2}'.format(mean, stdev, param))
```
#### 최적의 초매개변수를 사용해 예측하기
```python
# 최적 초매개변수를 가지고 예측값 생성  
import numpy as np
from sklearn.metrics import classification_report, accuracy_score  
y_pred = grid.predict(X_standardized)

print(y_pred.shape) # (382, 1)
print(y_pred[:5]) # [[0], [1], [0], [1], [1]]


print(accuracy_score(Y, y_pred))
print(classification_report(Y, y_pred))
```

![500](Pasted%20image%2020241023235625.png)
#### Bonus
```python
example = df.iloc[1]  
print(example)

prediction = grid.predict(X_standardized[1].reshape(1, -1))  
print(prediction) # [[1]]
```
