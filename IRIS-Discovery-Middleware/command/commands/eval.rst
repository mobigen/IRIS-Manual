eval
====

개요
----

기계 학습(Machine Learning) 관련 명령어로 기계 학습 모델의 정확도, 예측오차 등을 계산하여 출력합니다.

설명
----

검증용 데이터(또는 test 데이터) 로 학습 모델을 통해 구한 예측 결과와 실제(정답) 데이터를 비교하여 학습 모델의 정확도 및 예측 오차를 계산하는 명령어 입니다.

eval 의 input 데이터는 predict 명령어의 결과입니다.

학습 모델의 type(spark / tf) 과 유형(classification / regression) 에 따라 다른 통계량을 출력합니다.

    Spark ML 을 통한 학습 모델 / Classification 유형 -> 학습 모델의 정확도(accuracy) 를 출력합니다.
    
    Spark ML 을 통한 학습 모델 / Regression 유형 ->  MSE(평균예측오차) , RMSE(root MSE) 를 출력합니다.
    
    Tensorflow 2.0 을 이용한 deep learning 모델 / Classification 유형 -> 학습 모델의 정확도(accuracy) 를 출력합니다.

Examples
--------


Spark ML / Classification 모델
''''''''''''''''''''''''''''''''
predict 명령어를 통해서 예측된 label 과 원본 label 의 값을 비교하여 정확도를 계산합니다.

.. code-block:: none
  
   ... | predict  spark타입_학습모델  feature1,feature2,,,,    
       | eval  classification  label컬럼  예측결과컬럼


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

Spark ML / Regression 모델
''''''''''''''''''''''''''''''''
predict 명령어를 통해서 예측된 label컬럼의 값과 원본 label값의 오차를 이용하여 MSE(평균제곱오차), RMSE(Root MSE) 를 계산합니다.

.. code-block:: none
  
   ...  | predict  spark타입_학습모델  feature1,feature2,,,,   
        | eval  regression  label컬럼  예측결과컬럼


.. list-table:: 평가 결과
   :header-rows: 1

   * - MSE
     - RMSE
   * - 0.010
     - 0.100
 
     
Tensorflow 2.0 / deep Learning
''''''''''''''''''''''''''''''''''''''''''
1. Tensorflow 2.0 / deep Learning 모델은 Classification 모델이므로, 예측된 label 과 원본 label 의 값을 비교하여 모델의 정확도를 출력합니다.
2. Tensorflow 2.0 / deep Learning 모델은 Serving 된 모델을 통해서 예측을 실행하므로, serving predict 명령어로 예측합니다.

.. code-block:: none

   ... | serving predict  user=사용자계정  name=학습모델이름 col=feature컬럼이름 shape=[(28,28,1)]  tag=(0,1,2,3,4,5,6,7,8,9) 
       | eval  classification  label컬럼  interpreted

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


Parameters
----------

.. code-block:: none

  eval 유형 label_column prediction_column

.. list-table:: 파라미터 정보
   :header-rows: 1

   * - 파라미터 이름
     - 설명
     - 필수/선택
   * - 유형
     - [str] 모델 학습시 사용한 알고리즘, 종류: spark = [Classification, Regression, Recommendation], Tensorflow=[Classification]
     - 필수
   * - label_column
     - [str] 테스트 데이터의 원본 라벨 컬럼명, 예측 라벨 컬럼명(prediction_column) 과 비교 할 때 사용됩니다.
     - prediction 명령어 후 eval 명령어 사용시 필수
   * - prediction_column
     - [str] predict 명령어를 통해 예측한 라벨 컬럼명, 원본 라벨 컬럼명(label_column) 과 비교를 할 때 사용됩니다.
     - prediction 명령어 후 eval 명령어 사용시 필수
   
