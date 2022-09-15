.. role:: raw-html-m2r(raw)
   :format: html


fit
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

이 명령어는 해당 데이터를 선택한 알고리즘으로 기계학습 모델을 만들어줍니다.

타입
----------------------------------------------------------------------------------------------------


설명
----------------------------------------------------------------------------------------------------

데이터를 학습하고 싶은 알고리즘(ex:classfication,regeression,clustering,ranking,...)을 선택하여 해당 알고리즘에 맞게 기계학습한 모델을 만들어줍니다. 이 때, 해당 알고리즘 각각의 하이퍼파라미터를 조정하여 사용자가 원하는 모델을 만들어줍니다.

Algorithms
''''''''''

각 알고리즘의 사용법은 관련 명령어 문서를 확인해주세요.

- **Spark ML Algorithms**

  1. `RandomForest Classification <ml_algorithms/RandomForestClassification.html>`_
  2. `RandomForest Regression <ml_algorithms/RandomForestRegression.html>`_
  3. `Spark DecisionTree Classification <ml_algorithms/SparkDecisionTreeClassification.html>`_
  4. `Spark DecisionTree Regression <ml_algorithms/SparkDecisionTreeRegression.html>`_
  5. `Spark FP-Growth <ml_algorithms/SparkFPGrowth.html>`_
  6. `Spark Kmeans <ml_algorithms/SparkKmeans.html>`_
  7. `generalizedLinearRegression <ml_algorithms/generalizedlinearregression.html>`_
  8. `linearRegression <ml_algorithms/linearregression.html>`_
  9. `logisticRegression <ml_algorithms/logisticregression.html>`_

- **TensroFlow Deep Learning**

  1. `TFDeep <ml_algorithms/TFDeep.html>`_ 

Examples
----------------------------------------------------------------------------------------------------

Spark ML
''''''''

LogisticRegression을 학습하는 예제 입니다.

예제 데이터는 다음과 같이 존재합니다.

.. list-table::
   :header-rows: 1

   * - a
     - b
     - s
     - d
     - Species
     - minmax_a
     - minmax_b
     - minmax_c
     - minmax_d
     - Label
   * - 5.1
     - 3.5
     - 4
     - 0.2
     - Iris-setosa
     - 0.222222
     - 0.625
     - 0.508475
     - 0.041667
     - 0
   * - 4.9
     - 3
     - 1.4
     - 0.2
     - Iris-setosa
     - 0.166667
     - 0.416667
     - 0.067797
     - 0.041667
     - 0
   * - 4.7
     - 3.2
     - 1.3
     - 0.2
     - Iris-setosa
     - 0.111111
     - 0.5
     - 0.050847
     - 0.041667
     - 0
   * - 4.6
     - 3.1
     - 1.5
     - 0.2
     - Iris-setosa
     - 0.083333
     - 0.458333
     - 0.084746
     - 0.041667
     - 0
   * - 5
     - 3.6
     - 1.4
     - 0.2
     - Iris-setosa
     - 0.194444
     - 0.666667
     - 0.067797
     - 0.041667
     - 0
   * - 7
     - 3.2
     - 4.7
     - 1.4
     - Iris-versicolor
     - 0.75
     - 0.5
     - 0.627119
     - 0.541667
     - 2
   * - 6.4
     - 3.2
     - 4.5
     - 1.5
     - Iris-versicolor
     - 0.583333
     - 0.5
     - 0.59322
     - 0.583333
     - 2
   * - 6.8
     - 3.2
     - 5.9
     - 2.3
     - Iris-virginica
     - 0.694444
     - 0.5
     - 0.830508
     - 0.916667
     - 1
   * - 6.7
     - 3.3
     - 5.7
     - 2.5
     - Iris-virginica
     - 0.666667
     - 0.541667
     - 0.79661
     - 1
     - 1


Fit으로 logicsticregression을 적용해 분류 하는 모델을 생성하는 명령어 예제입니다.

| FEATURES : 입력 변수입니다. (단순 선형 회귀의 x 변수)
| LABEL : 예측하는 항목입니다. (단순 션형 회귀의 y 변수)
| INTO : 학습모델의 이름을 설정합니다. (이후에 모델 이름을 통해서 모델에 접근 가능합니다.)
|  

.. code-block:: none

   ... | fit LogisticRegression FEATURES minmax_a,minmax_b,minmax_c,minmax_d LABEL label maxIter=100 regParam=0.1 fitIntercept=True INTO modelA

명령어 이후 모델 저장

.. list-table::
   :header-rows: 1

   * - index
     - category
     - algorithm
     - Model_name
     - saved_date
     - features
     - label
     - parameters
     - Evaluation
     - crossvalidation
     - grid_info
     - used_data_count
     - spent_seconds
     - user
   * - 1
     - Classfication
     - LogisticRegresssion
     - ModelA
     - 20190909102754
     - LAT, LON
     - SYS_OUT
     - (maxIter:100,regParam:0.01,...)
     - (Accuracy:99,pricison:99,recall:10,...)
     - {}
     - {}
     - 100
     - 5 sec
     - None

| 저장된 모델은 아래 dsl을 이용하여 확인하실 수 있습니다.

.. code-block:: none

  * | mlmodel list | where name = 'modelA'


TensorFlow
''''''''''

fit 명령어를 이용해 Tensorflow Deep learning 네트워크를 사용한 학습 예제입니다.
학습에 사용된 데이터는 아래와 같습니다.

.. list-table:: Image-Label 데이터
   :header-rows: 1

   * - image
     - label
     - tag
   * - 0,0,0,...,0.12,0.92,0.23,...,0,0
     - 1,0,0,0,0,0,0,0,0,0
     - zero
   * - 0,0,0,...,0.12,0.92,0.23,...,0,0
     - 0,0,0,0,0,1,0,0,0,0
     - five
   * - ...
     - ...
     - ...

fit 명령어에 아래와 같이 옵션을 지정하고, 모델명을 지정해 줍니다.

  batch_size : train 데이터의 배치 사이즈 입니다.

  epochs : train 을 반복할 횟수 입니다.

  config : 모델 구성 정보, 데이터 정보, 사용 알고리즘, 등 학습을 하기 위한 설정파일 위치 입니다. (minio 데이터소스 사용)

.. code-block:: none

   fit deep batch_size=128 epochs=5 config=objectstorage.MINIO_AI_SOURCE:/USERS/test/mnist/mnist_config.json into test_model

학습결과로 각 epoch 당 정확도(accuracy), 손실률(loss) 과 같은 정보를 반환합니다.

.. list-table:: 학습 결과
   :header-rows: 1

   * - epoch
     - losses
     - metrics
   * - 1
     - {'loss': 0.2142421}
     - {'accuracy': 0.15123124}
   * - 2
     - {'loss': 0.1442421}
     - {'accuracy': 0.32123124}
   * - 3
     - {'loss': 0.1042421}
     - {'accuracy': 0.55123124}
   * - 4
     - {'loss': 0.0942421}
     - {'accuracy': 0.71123124}
   * - 5
     - {'loss': 0.0542421}
     - {'accuracy': 0.85123124}

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   fitCommand : alg option

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - alg
     - *학습 알고리즘* 입니다.\ :raw-html-m2r:`<br />`\ 예 : LogisticRegression
     - 필수
   * - option
     - 해당 알고리즘의 내부 파라미터 및 모델 저장 이름입니다.\ :raw-html-m2r:`<br />`\ 예 : FEATURES fieldA, fieldB, LABEL target maxIter=100 regParam=0.1 fitIntercept=True INTO modelA
     - 필수


*학습 알고리즘*

.. list-table::
   :header-rows: 1

   * - 알고리즘
     - 지정파라미터
     - 필수요소
   * - LogisticRegression
     - Label, Features, regParam, maxIter, name
     - Label, Features, name
   * - SVM
     - Label, Features, regType, maxIter, name
     - Label, Features, name
   * - Decisontree
     - (Label), Features, maxDepth, name
     - (Label), Features, name
   * - RandomForest
     - (Label), Features, numTree, name
     - (Label), Features, name
   * - LinearRegression
     - Label, Features, regParam, name
     - Label, Features, name
   * - Kmeans
     - Features, numk,name
     - Features,numk,name
   * - FPGrowh
     - Features, minSupport, minConfidance, name
     - Features, name
   * - Deep
     - epochs, batch_size, train_validation_ratio, continuous, retrain, config, name
     - config, name


Parameters BNF
----------------------------------------------------------------------------------------------------

.. code-block:: none

   fit_command : alg options
   alg : WORD
   options : any
           | options any
   any : WORD
       | NUMBER
       | DOUBLE
       | EQUALS
       | COMMA
       | SPACE
       | DOT
       | TIMES
       | MINUS
       | LBRACKET
       | RBRACKET
       | ATSIGN
       | SLASH
       | COLON

   WORD = r'\w+'
   COMMA = r','
   TIMES = r'\*'
   MINUS = r'-'
   EQUALS = r'\='
   SPACE = r'\ '
   DOT = r'\.'
   LBRACKET = r'\['
   RBRACKET = r'\]'
   NUMBER = \d+
   DOUBLE = [-+]?[0-9]+(\.([0-9]+)?([eE][-+]?[0-9]+)?|[eE][-+]?[0-9]+)
   ATSIGN = r'@'
   SLASH = r'/'
   COLON = r':'

추가 개발 사항(Issue)
----------------------------------------------------------------------------------------------------


* Merge_dataframe 실행 시 df가 섞이는 현상이 발생함 sort 후 섞는 기능 추가.
* model metadata Evaluation에 summary 사용 불가, 여러 성능 지표 계산 기능 추가.
* 겹치는 함수 및 Tensorflow 확장성을 위해 내부 함수들을 fit단계로 올려할 것.

추가 개발 방향
----------------------------------------------------------------------------------------------------


* Running_curve : 데이터량에 따라 학습이 얼마나 잘 진행되고 있는지 알려줄 수 있는 데이터를 return값에 포함 시켜줍니다. Data-Discovery-Service내에서 따로 시각화해서 확인 할수 있게 설계합니다. 기본적인 기능 구현을 우선시하여 뒤로 밀린 개발사항입니다.
* Sampling : 학습 알고리즘 내부에서 알아서 training/test 데이터를 나눠주는지 확인하지 못 하였습니다. 만약 스스로 나누지 않는다면 구현해야할 사항입니다. 역시 우선순위는 뒤로 밀렸습니다.
* CrossValidation : 교차검증기능 역시 알고리즘 내부에서 자동으로 이루어지는지 확인해 봐야 합니다. 스스로 이루어지지 않을 시에는 옵션으로 구현해야합니다. 역시 우선순위는 뒤로 밀렸습니다.
* Overfit,Underfit : 두 가지 경우에 어떻게 해줄지 생각을 하고 설계 및 개발을 해줘야한다.
