mymnist
=======

개요
----

MNIST 예제 데이터를 TensorFlow 모델을 이용 하여 학습, 예측, 평가 할 수 있는 MNIST 예제 전용 명령어 입니다.

설명
----

MNIST 예제 데이터를 딥러닝 할 수 있는 명령어이며, 다른 데이터는 학습할 수 없는 MNIST 전용 명령어 입니다.
Spark를 통해서 학습 데이터를 읽은 후 -> Tensorflow Dataset 으로 데이터를 전송 -> Tensorflow 모델을 이용하여 학습, 예측, 평가를 할 수 있습니다.

Examples
----------------------------------------------------------------------------------------------------

학습에 사용된 데이터는 아래와 같습니다.

.. list-table:: Image-Label 데이터
   :header-rows: 1

   * - image
     - label
   * - 0,0,0,...,0.12,0.92,0.23,...,0,0
     - 1,0,0,0,0,0,0,0,0,0
   * - 0,0,0,...,0.12,0.92,0.23,...,0,0
     - 0,0,0,0,0,1,0,0,0,0
   * - ...
     - ...

mymnist 명령어를 통한 학습 예제 입니다.

  batch_size : train 데이터의 배치 사이즈 입니다.

  epochs : train 을 반복할 횟수 입니다.

.. code-block:: none

   mymnist fit test_model batch_size=128 epochs=5

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

mymnist 명령어를 통한 예측 예제 입니다.

  batch_size : test 데이터의 배치 사이즈 입니다.

  interpret : 예측 결과의 label을 숫자 형태로 보여주는 옵션 입니다.

.. code-block:: none

   mymnist predict test_model batch_size=128 interpret=true

.. list-table:: 예측 결과
   :header-rows: 1

   * - image
     - label
     - prediction
     - interpreted
   * - 0,0,0,...,0.12,0.92,0.23,...,0,0
     - 1,0,0,0,0,0,0,0,0,0
     - 1,0,0,0,0,0,0,0,0,0
     - 0
   * - 0,0,0,...,0.12,0.92,0.23,...,0,0
     - 0,0,1,0,0,0,0,0,0,0
     - 0,0,1,0,0,0,0,0,0,0
     - 2

mymnist 명령어를 통한 평가 예제 입니다.

  batch_size : test 데이터의 배치 사이즈 입니다.

  repeat : 평가를 반복할 횟 수 입니다.

  rate : 평가에 사용될 랜덤 비율 입니다.

.. code-block:: none

   mymnist eval test_model repeat=2 batch_size=128 rate=0.8

.. list-table:: 평가 결과
   :header-rows: 1

   * - no
     - losses
     - metrics
   * - 1
     - {'loss': 0.2142421}
     - {'accuracy': 0.815123124}
   * - 2
     - {'loss': 0.1442421}
     - {'accuracy': 0.802123124}

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   mymnist operation model_name (epochs=[정수])? (batch_size=[정수])? (train_validation_ratio=[0<=실수<=1])? (continuous=[bool])? (rate=[0<=실수<=1])? (repeat=[정수])? (interpret=[bool])?

.. list-table::
   :header-rows: 1

   * - 파라미터 이름
     - 설명
     - 필수/선택
   * - operation
     - [fit, predict, eval] 중 택 1, 선택된 operation 에 따라 다른 동작
     - 필수
   * - model_name
     - 학습 후 저장 할 이름
     - 필수
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
   * - rate
     - 평가 동작 시 테스트 데이터의 랜덤 비율
     - 옵션, Default=0.7
   * - repeat
     - 평가 동작 시 평가를 반복 하려할 때 사용하는 옵션
     - 옵션, Default=1
   * - interpret
     - 예측 동작 후 라벨 데이터를 사람이 볼 수 있는 형태로 변환하는 옵션
     - 옵션, Default=False

Parameters BNF
----------------------------------------------------------------------------------------------------

.. code-block:: none

   mymnist_command : token token
                   | token token options
   options : option
           | options option
   option : token EQUAL token
   token : term
         | SQ terms SQ
         | DQ terms DQ
   terms : term
         | terms term
   term : WORD
        | NUMBER

   SQ = r'\''
   DQ = r'\"'
   EQUAL = r'\='
   NUMBER = r'\d+(.\d+)?'
   WORD = r'[^\s|^\=]+'
