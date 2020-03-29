.. role:: raw-html-m2r(raw)
   :format: html


serving
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

Tensorflow Serving을 통해 예측, 모델의 서빙 상태를 확인하는 명령어

설명
----------------------------------------------------------------------------------------------------

배포된 모델에 대해 예측하거나 서빙 상태를 확인할 수 있습니다. 배포는 `적재 <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/mlmodel.html#mlmodel-import>`_ , `배포 <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/mlmodel.html#mlmodel-deploy>`_ 를 참조해주세요.


serving predict
----------------------------------------------------------------------------------------------------

배포된 모델에 대해 예측합니다. 예측 컬럼, 예측 확률을 출력합니다. tag 옵션이 있으면 예측되는 tag 값도 추가적으로 출력합니다.

DSL 방식과 설정파일 제출 방식을 지원합니다.

관련 USECASE 
 - `외부 모델 활용 <http://docs.iris.tools/manual/IRIS-Usecase/ml-serving/index.html>`_
 - `내부 모델 활용 <http://docs.iris.tools/manual/IRIS-Usecase/ml/index.html>`_

Parameters
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

DSL 방식 1

- feature옵션에 예측에 필요한 정보를 모두 입력합니다.

``serving predict model_name feature=[(feature, Conv1_input, double, 28, 28, 1)] version=11 tag=(zero, one, two, three, four, five, six, seven, egiht, nine, ten)``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - model_name
     - 예측할 모델명
     - 
     - mnist_v1
     - 문자열
     - O
   * - feature
     - 특징 컬럼명, 입력 텐서 이름, 입력 텐서 자료형, 입력 텐서 모양의 리스트
     - 
     - [(body, body, float, 10), (tags, tags, double, 12), (feature, Conv1_input, int, 28, 28, 1)]
     - list of tuple
     - O
   * - version
     - 모델의 서빙 버전
     - 최근 버전
     - 3
     - 숫자형
     - 
   * - tag
     - 태그값
     - 
     - ('T-shirt/top', 'Trouser', 'Pullover', 'Dress', 'Coat', 'Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot')
     - 튜플형
     - 


DSL 방식 2

- feature 옵션을 col, dtype, shape, layer_name으로 나누어 입력합니다.

``serving predict model_name col=(feature) shape=[(28,28,1)] dtype=(float) layer_name=(Conv1_input) version=11 tag=(zero, one, two, three, four, five, six, seven, egiht, nine, ten)``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - model_name
     - 예측할 모델명
     - 
     - mnist_v1
     - 문자열
     - O
   * - col
     - 특징 컬럼명
     - 
     - (body, tags, title)
     - 튜플형
     - O
   * - shape
     - 입력 텐서 모양의 리스트
     - 
     - [(-1), (10), (-1)]
     - list of tuple
     - O     
   * - dtype
     - 입력 텐서 자료형
     - (float, float, ...) col에 정의된 특징 컬럼의 개수
     - (float, double, int)
     - 튜플형
     - 
   * - layer_name
     - 입력 텐서 이름
     - col과 같은 값으로 간주합니다.
     - (body, tags, title)
     - 튜플형
     - 
   * - version
     - 모델의 서빙 버전
     - 최근 버전
     - 3
     - 숫자형
     - 
   * - tag
     - 태그값
     - 
     - ('T-shirt/top', 'Trouser', 'Pullover', 'Dress', 'Coat', 'Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot')
     - 튜플형
     - 

설정파일 제출 방식

- 설정파일을 객체저장소에 업로드하고, 업로드 경로를 입력하여 예측합니다.
- `설정 파일 포맷 <https://www.tensorflow.org/tfx/serving/api_rest#request_format_2>`_
- 설정 파일 예시

.. code-block:: none

   {
   "signature_name": "serving_default",
   "instances": [[[[0.0], [0.0], [0.0],..., [0.011764705882352941], [0.0], [0.0], [0.45098039215686275], [0.4470588235294118], [0.41568627450980394], [0.5372549019607843],..., [0.0], [0.0]]]]
   }


``serving predict model_name conf=OBJECTSTORAGE.{CONNECTOR_NAME}:{KEY} version=1 tag=(T-shirt/top, Trouser, Pullover, Dress, Coat, Sandal, Shirt, Sneaker, Bag, Ankle boot)``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - model_name
     - 예측할 모델명
     - 
     - mnist_v1
     - 문자열
     - O
   * - conf
     - 설정 파일
     - 
     - OBJECTSTORAGE.MINAI:USERS/root/clothes/predict/1.json
     - 문자열
     - O
   * - version
     - 모델의 서빙 버전
     - 최근 버전
     - 3
     - 숫자형
     - 
   * - tag
     - 태그값
     - 
     - ('T-shirt/top', 'Trouser', 'Pullover', 'Dress', 'Coat', 'Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot')
     - 튜플형
     - 

``CONNECTOR_NAME`` : Conncetor Name입니다. IRIS UI에서 연결정보 생성 후, 연결정보의 ``이름`` 컬럼에서 확인할 수 있는 값입니다.

``KEY`` : OBJECTSTORAGE의 key입니다. bucket은 생략해야 합니다.

Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

angora mnist test 데이터의 30개 레코드를 소스로하여 예측합니다. 숫자를 분류하는 모델이 사용되었습니다. layer_name은 학습시 input 텐서의 이름을 뜻합니다.

- layer_name을 잘못 입력하면 아래와 같이 에러 문구가 나옵니다. 에러 문구의 signiture를 참조하여 layer_name을 입력하면 됩니다. 아래 예시에서는 input 텐서의 이름을 feature로 잘못 주었고, input 텐서 이름을 Conv1_input로 주어야 합니다.

.. code-block:: none

   raise AngoraException(servingCommand.EXCEPTION_05.format(res.status_code, res.text, signature))
   angora.exceptions.AngoraException: Incorrect response[404]. Please check the signature. 
   "error": "Failed to process element: 0 key: feature of \'instances\' list. Error: Invalid argument: JSON object: does not have named input: feature" }
   signature=b\"The given SavedModel SignatureDef contains the following input(s):\\n  
   inputs['Conv1_input'] tensor_info:\\n      
   dtype: DT_FLOAT\\n      
   shape: (-1, 28, 28, 1)\\n      
   name: serving_default_Conv1_input:0\\n
   The given SavedModel SignatureDef contains the following output(s):\\n  
   outputs['Softmax'] tensor_info:\\n      
   dtype: DT_FLOAT\\n      
   shape: (-1, 10)\\n      
   name: StatefulPartitionedCall:0\\nMethod name is: tensorflow/serving/predict\\n\"\n

``model name = 'angora mnist test' | top 30 feature | serving predict mnist_v1 col=feature shape=[(28,28,1)] layer_name=Conv1_input version=12 tag=(zero, one, two, three, four, five, six, seven, egiht, nine, ten)``

출력 결과

- predictions은 output 텐서의 각 확률 값을 출력합니다.
- probability컬럼은 predictions 중 가장 높은 값을 출력합니다.
- interpreted는 tag 옵션이 있는 경우 predictions에서 가장 큰값의 index를 tag에서 선택하여 출력합니다.

.. list-table::
   :header-rows: 1

   * - label
     - tag
     - feature
     - predictions
     - probability
     - interpreted
   * - 0,0,0,0,0,1,0,0,0,0
     - five
     - 0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0...
     - [0.62, 0.01, 0.04...]
     - 0.62
     - five
   * - 1,0,0,0,0,0,0,0,0,0
     - zero
     - 0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0...
     - [0.14, 0.03, 0.03...]
     - 0.38
     - zero
   * - ...
     - ...
     - ...
     - ...
     - ...
     - ...

multi_in_out test 데이터를 소스로 예측합니다. 다중 컬럼을 입력으로하고 다중 컬럼을 출력합니다. 특징 컬럼과 input 텐서이름이 같다면 layer_name을 생략합니다. version을 최신 버전을 사용할거라면 생략합니다.

``model name = 'multi_in_out test' | serving predict multi_in_out col=(title, body, tags) shape=[(10), (10), (12)]``

출력 결과

- 다중 출력의 경우 각 출력의 텐서 이름을 컬럼명으로 하여 값을 출력합니다.

.. list-table::
   :header-rows: 1

   * - title
     - body
     - tags
     - department
     - priority
   * - [0.43, 0.77, 0.3, 0.19, 0.38, 0.37, 0.56, 0.48, 0.8, 0.4]
     - [0.9, 0.5, 0.16, 0.74, 0.9, 0.64, 0.37, 0.18, 0.08, 0.87]
     - [0.44, 0.45, 0.56, 0.63, 0.72, 0.28, 0.57, 0.19, 0.66, 0.47, 0.89, 0.37]
     - [-0.17423968, -0.243361622, -0.155712008, -0.312662631]
     - [-0.115426637]
   * - ...
     - ...
     - ...
     - ...
     - ...

설정파일을 제출하여 예측합니다. 옷을 분류하는 모델이 사용되었습니다.

``serving predict clothes conf=OBJECTSTORAGE.MIN_AI:USERS/namjals/clothes/predict/1.json version=1 tag=(T-shirt/top, Trouser, Pullover, Dress, Coat, Sandal, Shirt, Sneaker, Bag, Ankle boot)``

출력 결과

.. list-table::
   :header-rows: 1

   * - predictions
     - probability
     - interpreted
   * - [0.14, 0.03, 0.03...]
     - 0.38
     - Ankle boot
   * - ...
     - ...
     - ...


Parameters BNF
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: none

   serving_command : PREDICT WORD options
   options : option
           | options option
           |
   option : WORD EQ WORD
           | WORD EQ WORD_WITH_BRACKET
           | WORD EQ WORD_WITH_SQUARE_BRACKET

   WORD = [^ |^\|^\'|\"|^\=]+
   WORD_WITH_BRACKET =  \([^\|^\'|\"|^\=]+\)
   WORD_WITH_SQUARE_BRACKET = \[[^\|^\'|\"|^\=]+\]
   EQ = \=
   PREDICT = (?i)predict


serving status
----------------------------------------------------------------------------------------------------

서빙 중인 모델의 서빙 상태를 확인합니다. 

Parameters
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

``serving status model_name``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - model_name
     - 서빙 상태를 확인할 모델명
     - 
     - mnist_v1
     - 문자열
     - O

Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

mnist_v1모델의 서빙 상태를 확인합니다.

``serving status mnist_v1``

.. list-table::
   :header-rows: 1

   * - version
     - state
     - label
   * - 12
     - AVAILABLE
     - stable
   * - 11
     - AVAILABLE
     - unstable
   * - ...
     - ...
     - ...


Parameters BNF
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: none

   serving_command : STATUS WORD

   WORD = [^ |^\|^\'|\"|^\=]+
   STATUS = (?i)status
