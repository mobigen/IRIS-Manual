Eval
====

개요
----

학습된 모델을 이용해 예측 결과를 반환하는 명령어

설명
----

학습된 모델을 이용해 데이터를 예측한 결과의 정확도를 계산하는 명령어 입니다.

Spark ML 을 통한 학습 모델은 Classification, Regression 에 관한 정확도를 보여줍니다.
Tensorflow 2.0 을 이용한 학습 모델은 해당 모델의 정확도와 손실률을 보여줍니다.

Examples
--------

eval 명령어를 이용해 예측의 정확도를 계산하는 예제입니다.

Spark ML
''''''''
predict 명령어를 통해서 예측된 label 과 원본 label 의 값을 비교하여 정확도를 계산합니다.

.. code-block:: none

   ... | predict ... | eval classification label prediction

.. list-table:: 평가 결과
   :header-rows: 1

   * - all_count
     - correct_count
     - wrong_count
     - accuracy
   * - 1000
     - 800
     - 200
     - 80

Tensorflow 2.0
''''''''''''''
1. predict 명령어를 통해서 예측된 label 과 원본 label 의 값을 비교하여 정확도를 계산합니다.

.. code-block:: none

   ... | predict ... | eval classification label prediction

.. list-table:: 평가 결과
   :header-rows: 1

   * - all_count
     - correct_count
     - wrong_count
     - accuracy
   * - 1000
     - 800
     - 200
     - 80

2. 학습된 tensorflow 모델을 직접 사용해 앞에서 넘어온 test 데이터셋으로 예측 및 평가를 진행 합니다.

   label: 원본 데이터 라벨

   rate: 예측에 사용될 데이터 비율

   repeat: 평가 반복 횟수

.. code-block:: none

   ... | eval deep test_model label=label rate=0.8 repeat=3

.. list-table:: 평가 결과
   :header-rows: 1

   * - no
     - losses
     - metrics
   * - 0
     - {'loss': 1.69789723}
     - {'acc': 0.6887238}
   * - 1
     - {'loss': 1.7142421}
     - {'acc': 0.702323124}
   * - 2
     - {'loss': 1.7442421}
     - {'acc': 0.69460124}

Parameters
----------

.. code-block:: none

  eval mode (label_column prediction_column | model_name options)

.. list-table:: 파라미터 정보
   :header-rows: 1

   * - 파라미터 이름
     - 설명
     - 필수/선택
   * - mode
     - [str] 모델 학습시 사용한 알고리즘, 종류: Spark ML=[Classification, Regression], Tensorflow=[Deep]
     - 필수
   * - label_column
     - [str] 테스트 데이터의 원본 라벨 컬럼명, 예측 라벨 컬럼명(prediction_column) 과 비교룰 할 때 사용됩니다.
     - prediction 명령어 후 eval 명령어 사용시 필수
   * - prediction_column
     - [str] predict 명령어를 통해 예측한 라벨 컬럼명, 원본 라벨 컬럼명(label_column) 과 비교를 할 때 사용됩니다.
     - prediction 명령어 후 eval 명령어 사용시 필수
   * - model_name
     - [str] 학습 모델명, Tensorflow 모델을 이용해서 예측을 할 때 사용됩니다. (**Spark ML 모델을 현재 지원하지 않습니다.**)
     - eval 명령어만 사용시 필수
   * - options
     - [str] model_name 이 지정되었을 때 사용하는 옵션\ :raw-html-m2r:`<br />`\ [종류] feature(테스트 데이터 feature 컬럼명), label(테스트 데이터 label 컬럼명), rate(예측시 사용될 데이터 비율, 0~1 사이 실수), repeat(평가 반복 횟수, 정수)\ :raw-html-m2r:`<br />`\  [사용법] (option=option_value )+, 예) feature=image label=label rate=0.7 repeat=5
     - 옵션

Parameters BNF
--------------

.. code-block:: none

   eval_command : mode model_name_or_label options
                | mode model_name_or_label prediction
   mode : WORD
   model_name_or_label : WORD
   options : option
           | options option
   option : WORD EQUAL WORD
          | WORD EQUAL NUMBER
   prediction : WORD

   NUMBER : r'\d+(.\d+)?'
   WORD : r'[^\s|^\=]+'
   EQUAL : r'='
