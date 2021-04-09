
Spark FP-Growth
====================================================================================================

FP-Growth 학습 알고리즘 설명 및 파라미터 설명서 입니다.

개요
----------------------------------------------------------------------------------------------------

FP-Growth((Frequent-Pattern Growth)  알고리즘은 데이터 그룹간에 빈번하게 발생하는 패턴을 찾는 알고리즘으로 연관규칙분석(Apriori) 분야에서 사용됩니다. 
(연관규칙분석은  "장바구니 분석" 이라는 별칭이 있습니다)

데이터로부터 자주 발생하는 패턴(빈발 패턴)을 찾아, 소비자가 구매하려는 아이템 옆에 빈발 패턴으로 묶인 아이템들을 같이 노출하여 구매확률을 높이는 마케팅 전략으로 많이 이용합니다.


설명
----------------------------------------------------------------------------------------------------

FP-Growth 모델을 만들고 input data frame을 모델에 적용시킵니다. FP-Growth 모델 학습에 필요한 파라미터들을 지정할 수 있으며 지정하지 않을 시 Default 값으로 설정됩니다.

Examples
----------------------------------------------------------------------------------------------------


* 구매 목록에 관한 데이터로 구매 transaction(TRANSATION_ID) 1건 당 구매한 아이템 목록(ITEM_LIST)입니다.
* 데이터의 형태

.. list-table::
   :header-rows: 1

   * - TRANSATION_ID
     - ITEM_LIST
   * - 1
     - Lassi,Coffee Powder,Butter,Yougurt,Ghee,Cheese
   * - 2
     - Ghee,Coffee Powder
   * - 3
     - Lassi,Tea Powder,Butter,Cheese
   * - 
     - ,,,


* fit fpgrowth - 학습모델 생성


.. code-block:: none

   ... | fit fpgrowth FEATURES  아이템컬럼명  minSupport=0.1 minConfidence=0.1 into 저장할모델이름

   example )
   ... | fit fpgrowth FEATURES  ITEM_LIST  minSupport=0.1 minConfidence=0.1 into EDU_MODEL_FPGrowth_01




.. list-table::
   :header-rows: 1

   * - items
     - freq
     - space
     - antecedent
     - consequent
     - confidence
   * - ['Coffee Powder', 'Ghee']
     - 2578
     - \|
     - ['Bread']
     - ['Panner']
     - 0.46498
   * - ['Butter', 'Sugar']
     - 2571
     - \|
     - ['Yougurt']
     - ['Coffee Powder']
     - 0.46429
   * - [,,]
     - ..
     - \|
     - [..]
     - [..]
     - ..

.. code-block:: none  

   - items : 목록패턴
   - freq : items 가 나온 빈도수
   - antecedent : 선행 item
   - consequent : 결과 item
   - confidence : 결과 item 의 신뢰도
  


* predict : 생성한 학습 모델을 통해 신규데이터 또는 test 데이터를 예측하는 명령어

.. code-block:: none

   ... | predict 학습모델소유자.학습모델이름 아이템컬럼명

   example )
   ... | predict demo.EDU_MODEL_FPGrowth_01  ITEM_LIST




.. list-table::
   :header-rows: 1

   * - TRANSATION_ID
     - ITEM_LIST
     - prediction
   * - 100
     - Yougurt,Milk,Sugar
     - [Butter, Panner, Cheese, Tea Powder, Sweet, Bread, Coffee Powder, Ghee, Lassi]
   * - 1206
     - Sugar,Butter,Milk
     - [Lassi, Coffee Powder, Sweet, Bread, Ghee, Yougurt, Cheese, Tea Powder, Panner]




Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | fit FPGrowth FEATURES fields INTO_model

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - FEATURES
     - 학습에 사용될 특징 column을 입력 받습니다.
     - 필수
   * - fields
     - 특징 column들의 이름입니다.
     - 필수
   * - params
     - 알고리즘 setting 파라미터들입니다.
     - 옵션
   * - INTO_model
     - ``INTO model_name``\ 으로 이루어져 있습니다. 경로 (\ **/Biris/angora/ml**\ )에 모델 메타 데이터와 함께 저장합니다.
     - 필수
   * - minSupport
     - frequent item에 대한 threshold값 입니다.
     - 옵션
   * - minConfidence
     - pattern 에 대한 threshold값 입니다.
     - 옵션
   * - numPartitions
     - partition 갯수 입니다.
     - 옵션


Parameters BNF
----------------------------------------------------------------------------------------------------

.. code-block:: none

   SparkFPGrowth_command : FEATURES fields params INTO_model

   fields : field
           | fields COMMA field

   field : WORD

   params : param
           | params param

   param : WORD EQUALS WORD
           | WORD EQUALS DOUBLE

   INTO_model : INTO WORD

   WORD : \w+
   COMMA : \,
   FEATURES : FEATURES | features
   INTO : INTO
   EQUALS : \=
   DOUBLE : [-+]?[0-9]+(\.([0-9]+)?([eE][-+]?[0-9]+)?|[eE][-+]?[0-9]+)


   params : minSupport=0.3, minConfidence=0.8, numPartitions=None
