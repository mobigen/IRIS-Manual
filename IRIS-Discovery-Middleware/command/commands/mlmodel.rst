.. role:: raw-html-m2r(raw)
   :format: html


mlmodel
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

ML 모델을 관리하는 명령어

설명
----------------------------------------------------------------------------------------------------

학습 모델에 대해 목록 조회, 상세 조회, 삭제, 적재, 반출, 배포, 중지 기능을 제공합니다.


mlmodel list
----------------------------------------------------------------------------------------------------
전체 모델 목록을 조회합니다. 아래 내용을 확인 할 수 있습니다.

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 예
   * - id
     - 식별자
     - 1
   * - user
     - 유저명
     - root
   * - name
     - 모델명
     - mnist_v1
   * - type
     - 유형
     - spark, tf, sklearn, r 등
   * - category
     - 범주
     - classification, regression 등
   * - algorithm
     - 알고리즘
     - deep
   * - serving
     - 모델 서빙 상태
     - on / off
   * - create
     - 생성 시간
     - 20191001145628
   * - modified
     - 수정 시간
     - 20191002145628

Parameters
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

``mlmodel list``

Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

전체 모델 목록을 조회합니다.

``mlmodel list``

출력 결과

.. list-table::
   :header-rows: 1

   * - id
     - user
     - name
     - type
     - category
     - algorithm
     - serving
     - create
     - modified
   * - 1
     - root
     - mnist_v1
     - tf
     - classification
     - deep
     - off
     - 2019/11/19 00:11:22
     - 2019/11/19 00:11:33
   * - ...
     - ...
     - ...
     - ...
     - ...
     - ...
     - ...
     - ...
     - ...

Parameters BNF
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: none

   mlmodel_command : LIST
                   |
   
   LIST = r'(?i)list'

mlmodel summary
----------------------------------------------------------------------------------------------------

특정 모델의 상세 내용을 조회합니다. 아래 내용을 확인 할 수 있습니다.

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 예
   * - id
     - 식별자
     - 1
   * - user
     - 유저명
     - root
   * - name
     - 모델명
     - mnist_v1
   * - filename
     - 파일명
     - model.h5
   * - format
     - 포멧
     - h5, saved_model 등
   * - type
     - 유형
     - spark, tf, sklearn, r 등
   * - category
     - 범주
     - classification, regression 등
   * - algorithm
     - 알고리즘
     - deep
   * - feature
     - 특징 컬럼
     - x
   * - label
     - 레이블 컬럼
     - y
   * - parameter
     - 파라미터
     - { "epochs": 3, "batch_size" : 64, "train_validation_ratio" : 0.8 }
   * - evaluation
     - 학습 평가 결과
     - [ { "losses" : { "loss" : 12.345 , "val_loss" : 12.345 }, "metrics" : { "acc" : 12.345, "val_acc" : 12.345 } }, { "losses" : { "loss" : 12.345 , "val_loss" : 12.345 }, "metrics" : { "acc" : 12.345, "val_acc" : 12.345 } }, { "losses" : { "loss" : 12.345 , "val_loss" : 12.345 }, "metrics" : { "acc" : 12.345, "val_acc" : 12.345 } } ]
   * - cross_validation
     - 교차검증 옵션
     - {}
   * - grid_info
     - 그리드 옵션
     - {}
   * - train_cnt
     - 학습 건수
     - 10000
   * - elapsed
     - 소요 시간 (초)
     - 60
   * - dictionary
     - 사전 파일명
     - dict.tsv
   * - cdate
     - 생성 시간
     - 20191001145628
   * - mdate
     - 수정 시간
     - 20191002145628
   * - serving
     - 모델 서빙 상태
     - on / off
   * - serving_name
     - 서빙 이름
     - root_mnist_v1
   * - state
     - 모델 실행 상태
     - READY, RUNNING, DONE, ERROR

Parameters
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
``mlmodel summary model_name``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - model_name
     - 모델명
     - 
     - mnist_v1
     - 문자열
     - O

Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

"mnist_v1" 모델의 상세 내용을 조회합니다.

``mlmodel summary mnist_v1``

출력 결과

.. list-table::
   :header-rows: 1

   * - name
     - value
   * - id
     - 1
   * - user
     - root
   * - name
     - mnist_v1
   * - filename
     - saved_model.pb
   * - format
     - saved_model
   * - type
     - tf
   * - category
     - deep
   * - algorithm
     - deep
   * - feature
     - feature
   * - label
     - label
   * - parameter
     - {'batch_size': 128, 'epochs': 5, 'continuous': 'True', 'config': 'objectstorage.MINIO_AI_SOURCE:USERS/pjh0347/mnist/angora_mnist_config.json'}
   * - evaluation
     - []
   * - cross_validation
     - {}
   * - grid_info
     - {}
   * - train_cnt
     - 55260
   * - elapsed
     - 569.0207872390747
   * - dictionary
     - dict.tsv
   * - cdate
     - 20200323171102
   * - mdate
     - 20200324100417
   * - serving
     - off
   * - serving_name
     - root_mnist_v1
   * - state
     - DONE

Parameters BNF
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: none

   mlmodel_command : SUMMARY model_name
                   | SUMMARY model_name fields
   model_name : WORD
              |
   fields : field
          | fields COMMA field      
   field : WORD

   WORD = [^,|^ |^\|^(|^)|^\'|\"|^\=]+
   SUMMARY = (?i)list
   COMMA = ","

mlmodel delete
----------------------------------------------------------------------------------------------------

특정 모델을 삭제합니다. 모델 meta정보와 객체저장소의 모델 파일들을 삭제합니다. 성공 시, 모델목록을 보여줍니다. 

Parameters
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

``mlmodel delete model_name``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - model_name
     - 모델명
     - 
     - mnist_v1
     - 문자열
     - O


Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

"mnist_v1" 모델을 삭제합니다.

``mlmodel delete mnist_v1``

출력 결과

.. list-table::
   :header-rows: 1

   * - id
     - user
     - name
     - type
     - category
     - algorithm
     - serving
     - create
     - modified
   * - 1
     - root
     - multi_in_out
     - tf
     - classification
     - deep
     - on
     - 2020/03/24 10:20:57
     - 2020/03/24 10:21:19
   * - 3
     - root
     - tf_clothes
     - tf
     - classification
     - deep
     - on
     - 2020/03/25 07:51:30
     - 2020/03/25 07:53:34  
   * - ...
     - ...
     - ...
     - ...
     - ...
     - ...
     - ...
     - ...
     - ...

Parameters BNF
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: none

   mlmodel_command : DELETE model_name
   model_name : WORD
              |

   WORD = [^,|^ |^\|^(|^)|^\'|\"|^\=]+
   DELETE = (?i)delete


mlmodel import
----------------------------------------------------------------------------------------------------

객체 저장소에 있는 내 계정 소유의 학습 모델 파일을 IRIS Discovery Service가 관리하는 객체저장소에 적재 합니다. 
적재된 모델은 학습, 예측, 평가, 배포 명령어 등에 활용할 수 있습니다.

학습 모델 파일은 tar 아카이브 형태이어야 하며, 아카이브 파일 내 타입별 필수 파일은 다음과 같습니다.

.. list-table::
   :header-rows: 1

   * - 타입
     - 필수 포함 파일
   * - R
     - 학습 모델  파일 (model.rda)
   * - Scikit-Learn
     - 학습 모델  파일 (model.pkl)
   * - Spark
     - 학습 모델  파일 (data.parquet), 학습 모델 메타 파일 (metadata)
   * - TensorFlow
     - 학습 모델  파일 (model.h5 or saved_model.pb)      

Parameters
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

``mlmodel import name=mnist_v1 type=tf category=classification algorithm=deep format=saved_model path=OBJECTSTORAGE.{CONNECTOR_NAME}:{KEY}``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - name
     - 저장할 모델명
     - 
     - sklearn_test
     - 문자열
     - O
   * - type
     - 유형
     - 
     - sklearn
     - 문자열
     - O
   * - category
     - 범주
     - 
     - classification
     - 문자열
     - O
   * - algorithm
     - 알고리즘
     - 
     - LogisticRegression
     - 문자열
     - O
   * - format
     - 모델 포멧
     - 
     - pickle 혹은 h5 혹은 saved_model
     - 문자열
     - O
   * - path
     - 객체 스토리지 내 모델 소스 경로
     - 
     - USERS/root/sklearn_model.tar
     - 문자열
     - O


``CONNECTOR_NAME`` : Conncetor Name입니다. IRIS UI에서 연결정보 생성 후, 연결정보의 ``이름`` 컬럼에서 확인할 수 있는 값입니다.

``KEY`` : OBJECTSTORAGE의 key입니다. bucket은 생략해야 합니다.


Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

모델정보를 아카이브한 파일을 IRIS Discovery Service가 관리하는 객체저장소에 업로드 합니다.

``mlmodel import name=tf_clothes type=tf category=classification algorithm=deep format=saved_model path=OBJECTSTORAGE.MIN_AI:USERS/root/clothes/model.tar``

출력 결과

.. list-table::
   :header-rows: 1

   * - result
   * - ok

Parameters BNF
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: none

   mlmodel_command : IMPORT options
   options : option
           | options option
           |
   option : WORD EQ WORD
          | WORD EQ SQ_TERM_SQ

   WORD = [^,|^ |^\|^(|^)|^\'|\"|^\=]+
   SQ_TERM_SQ = '[a-zA-Z0-9가-힣 _\-\[\]{}()\.:,=]*'
   EQ = \=
   IMPORT = (?i)import


mlmodel export
----------------------------------------------------------------------------------------------------

객체 저장소에 관리되고 있는 학습 모델 디렉토리를 아카이브하여 개인 객체 저장소에 저장하고 download url을 제공합니다.

관리되고 있는 학습모델은 fit명령으로 학습된 모델 혹은 mlmodel import로 적재된 모델을 의미합니다.


Parameters
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

``mlmodel export name=mnist_v1 path=OBJECTSTORAGE.{CONNECTOR_NAME}:{KEY}``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - name
     - 내보낼 모델명
     - 
     - sklearn_test
     - 문자열
     - O
   * - path
     - 객체 스토리지 내 모델 저장 경로
     - 
     - USERS/root/sklearn_test.tar
     - 문자열
     - O


Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

mnist_v1 모델을 개인 객체 저장소에 저장합니다.

``mlmodel export name=mnist_v1 path=OBJECTSTORAGE.MIN_AI:USERS/root/tf_mnist.tar``

출력 결과

.. list-table::
   :header-rows: 1

   * - result
     - download_url
     - expired time (sec)
   * - ok
     - http://IP:PORT/test/ANGORA/AI/DOWNLOAD/root/mnist_v1/tf_mnist.tar...
     - 3600

Parameters BNF
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: none

   mlmodel_command : EXPORT options
   options : option
           | options option
           |
   option : WORD EQ WORD
          | WORD EQ SQ_TERM_SQ

   WORD = [^,|^ |^\|^(|^)|^\'|\"|^\=]+
   SQ_TERM_SQ = '[a-zA-Z0-9가-힣 _\-\[\]{}()\.:,=]*'
   EQ = \=
   EXPORT = (?i)export


mlmodel deploy
----------------------------------------------------------------------------------------------------

관리되고 있는 학습된 모델을 서빙 가능하도록 TersorFlow Serving에 배포합니다. tf 타입만 제공합니다.

모델 배포시, 최초 version은 1이며, 같은 모델명으로 배포 시, version이 1씩 올라갑니다. 
배포가 되면, `serving 명령어 <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/serving.html>`_ 로 version별 모델 예측, 모델 서빙 상태 확인이 가능합니다.


Parameters
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

``mlmodel deploy model_name label='stable'``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - model_name
     - 배포할 모델명
     - 
     - mnist_v1
     - 문자열
     - O
   * - label
     - 배포 모델 설명
     - 
     - unstable
     - 문자열
     - 


Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

학습된 mnist_v1모델을 배포합니다.

``mlmodel deploy mnist_v1 label='unstable'``

출력 결과

- serving_name은 유저명과 모델이름을 합친 문자열입니다. curl로 서빙에 요청할 경우 해당 이름으로 요청해야합니다.

.. list-table::
   :header-rows: 1

   * - reslut
     - latest_version
     - serving_name
   * - on
     - 1
     - root_mnist_v1

mnist_v1모델을 업데이트하고 재배포합니다.

``mlmodel deploy mnist_v1 label='stable'``

출력 결과

- 버전이 1 올라갑니다.

.. list-table::
   :header-rows: 1

   * - reslut
     - latest_version
     - serving_name
   * - on
     - 2
     - root_mnist_v1

`restapi <https://www.tensorflow.org/tfx/serving/api_rest#url_4>`_ 로 배포 모델을 확인합니다.

``!curl -d '{"signature_name": "serving_default", "instances": [[[[0.0], [0.0],..., [0.8196078431372549], [0.8156862745098039], [1.0], [0.8196078431372549],..., [0.0], [0.0], [0.0]]]]}' 
-X POST http://localhost:8501/v1/models/root_mnist_v1/versions/2:predict``

.. code-block:: none

   {
       "predictions": [[1.34083416e-06, 6.62974609e-09, 4.16653876e-08, 6.56875301e-08, 1.06879767e-07, 0.00958874, 7.81568906e-06, 0.354254484, 0.00198251382, 0.634164929]]
   }

Parameters BNF
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: none

   mlmodel_command : DEPLOY model_name options
   options : option
           | options option
           |
   option : WORD EQ WORD
          | WORD EQ SQ_TERM_SQ

   WORD = [^,|^ |^\|^(|^)|^\'|\"|^\=]+
   SQ_TERM_SQ = '[a-zA-Z0-9가-힣 _\-\[\]{}()\.:,=]*'
   EQ = \=
   DEPLOY = (?i)deploy



mlmodel stop
----------------------------------------------------------------------------------------------------

서빙 중인 배포 모델을 더 이상 서빙 하지 않도록 중지합니다. 버전 상관없이 모두 중지합니다.

Parameters
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

``mlmodel stop model_name``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - model_name
     - 중지할 모델명
     - 
     - mnist_v1
     - 문자열
     - O

Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

mnist_v1모델을 중지합니다.

``mlmodel stop mnist_v1``

.. list-table::
   :header-rows: 1

   * - reslut
   * - ok


Parameters BNF
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: none

   mlmodel_command : STOP model_name
   model_name : WORD
              |

   WORD = [^,|^ |^\|^(|^)|^\'|\"|^\=]+
   STOP = (?i)stop

