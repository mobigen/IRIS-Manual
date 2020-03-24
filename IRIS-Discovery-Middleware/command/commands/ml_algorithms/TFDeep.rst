TFDeep
======

개요
----

Tensorflow 2.0의 keras를 이용한 간단한 네트워크를 이용해 딥러닝 모델을 학습하는 명령어

설명
----

TensorFlow 2.0 을 이용해 간단한 딥러닝 모델을 학습하는 명령어입니다.

네트워크 구성은 설정파일에 작성하여야 하며, 특정 데이터소스(현재 objectstorage)에 업로드 한 후, 연결정보를 구성해야 합니다.

Examples
--------

fit 명령어를 이용해 Tensorflow Deep learning 네트워크를 사용한 학습 예제입니다.
학습에 사용된 데이터 읽어와 사용합니다. 구성은 아래와 같습니다. (fit 명령어 앞에 사용된 명령어의 데이터는 사용하지 않습니다.)

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

.. list-table:: 반환 결과
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
----------

.. code-block:: none

  fit deep (epochs=[정수])? (batch_size=[정수])? (train_validation_ratio=[0<=실수<=1])? (continuous=[boolean])? (retrain=[boolean])? config=[datasource_name].[connector_name]:[config_file path] INTO [model name]

.. list-table:: 파라미터 정보
   :header-rows: 1

   * - 파라미터 이름
     - 설명
     - 필수/선택
   * - epochs
     - [int] 학습의 반복 횟수
     - 옵션, Default=5
   * - batch_size
     - [int] 학습 데이터의 배치 크기, 예) 1000 rows 의 데이터, batch_size=100 -> (1000 / 100 = 10)개의 배치 데이터 -> 1 epoch 당 100 rows 데이터를 10번 학습
     - 옵션, Default=32
   * - train_validation_ratio
     - [float] Train 데이터를 분할 하여, 학습시 validation data 로 사용 하기 위한 비율, (**현재는 지원되지 않습니다.**)
     - 옵션, Default=0.8
   * - continuous
     - [bool] 이전에 학습된 모델에 새로운 데이터를 넣어 연속하여 학습 진행
     - 옵션, Default=False 
   * - retrain
     - [bool] 이전에 학습된 모델이 있으면 삭제하고 새 학습을 진행
     - 옵션, Default=False
   * - config
     - [str] 딥러닝 네트워크 구성 정보, loss function, optimizer, 데이터 정보, 데이터 위치 등을 구성한 설정 파일, 현재 objectstorage 만 지원 합니다.\ :raw-html-m2r:`<br />`\ [작성 포멧] 데이터소스명.컨넥터이름:설정파일경로 예) objectstorage.MINIO_SOURCE:USERS/test/config.json
     - 필수
   * - model_name
     - 학습 후 저장 할 이름, 같은 이름으로 학습은 1번만 가능하며 여러번 시도 시 에러가 발생합니다. 같은 이름으로 재학습을 원한다면 retrain 옵션, 연속 학습을 원한다면 continuous 옵션을 사용해야 합니다.
     - 필수

Parameters BNF
--------------

.. code-block:: none

   arguments : options model_name
   model_name : INTO words
   options : option
           | options option
   option : WORD EQUAL WORD
          | WORD EQUAL NUMBER
   words : WORD
         | words WORD
         | SQ words SQ
         | DQ words DQ

   NUMBER : r'\d+(.\d+)?'
   SQ : r"\'"
   DQ : r'\"'
   EQUAL : r'='
   WORD : r'[^\s|^\=]+'
   INTO : '(?i)into'
