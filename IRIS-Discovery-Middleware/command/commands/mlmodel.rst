mlmodel
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

| ML 모델을 관리하는 명령어입니다.
| 현재(2021-04월) 는 IRIS 에서 fit 명령어로 학습한 후 저장한 spark type 의 ML 모델과 
| IRIS 외부에서 tensorFlow 로 만든 tf type의 ML 모델을 대상으로 합니다. 

설명
----------------------------------------------------------------------------------------------------

IRIS Discovery 의 ML 모델 저장소에 저장된 학습 모델에 대해 목록 조회, 상세 조회, 삭제, 적재, 반출, 배포, 중지 기능을 제공합니다.

- operation 종류
    - list     ( spark 모델 / tf 모델 )
    - summary  ( spark 모델 / tf 모델 )
    - delete   ( spark 모델 / tf 모델 )
    - import  ( only tf 모델 )
    - export  ( only tf 모델 )
    - deploy  ( only tf 모델 )
    - stop    ( only tf 모델 )

- dsl-object API 이용시 json 구성 (각 operation 에 대한 파라미터는 아래에서 확인)

.. code-block:: json

   {
     "command": "mlmodel",
     "params":{
       "operation": <string>,  # "list|summary|delete|import|export|deploy|stop",
       "name": <string>,  # "학습모델명"
       "version": <int>,  # 2
       "user": <string>,  # "유저명"
       ...
     }
   }


mlmodel list
----------------------------------------------------------------------------------------------------

- IRIS Discovery 의 ML 모델 저장소에서 관리중인 전체 모델 목록을 조회합니다.(spark 모델과 tf 모델 모두 사용 가능)

Parameters
''''''''''

.. code-block:: python

   mlmodel list

- 아래 내용을 확인 할 수 있습니다.

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
   * - version
     - 버전
     - 1
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

Example
'''''''

.. code-block:: python

   mlmodel list

- 출력 결과

.. list-table::
   :header-rows: 1

   * - id
     - user
     - name
     - version
     - type
     - category
     - algorithm
     - serving
     - create
     - modified
   * - 1
     - root
     - mnist_v1
     - 1
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
     - ...


mlmodel summary
----------------------------------------------------------------------------------------------------

- IRIS Discovery 의 ML 모델 저장소에 저장된 특정 모델의 상세 내용을 조회합니다.(spark 모델과 tf 모델 모두 사용 가능)


Parameters
''''''''''
.. code-block:: python

   mlmodel summary (user=<user>)? name=<model_name> (version=<version>)?

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예제
     - 타입
     - 필수
   * - user
     - 모델 소유주 명
     - API를 요청하는 user
     - demo
     - string
     - False
   * - name
     - 모델명
     -
     - mnist_v1
     - string
     - True
   * - version
     - 모델의 버전
     - last version of model
     - 1
     - int
     - False

- 아래 내용을 확인 할 수 있습니다.

.. list-table::
   :header-rows: 1
   :widths: 20 20 60

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
   * - version
     - 버전
     - 1
   * - filename
     - 파일명
     - model.h5
   * - format
     - 포멧
     - h5 또는 saved_model
   * - type
     - 유형
     - spark 또는 tf
   * - category
     - 범주
     - classification, regression 등
   * - algorithm
     - 알고리즘
     - deep 또는 RandomForestRegression ,,,
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

Example
'''''''

.. code-block:: python

   mlmodel summary name=mnist_v1

- 출력 결과

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
   * - version
     - 1
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

mlmodel delete
----------------------------------------------------------------------------------------------------

- IRIS Discovery의 ML 모델 저장소에서 특정 모델을 삭제합니다. 모델 meta정보와 객체저장소의 모델 파일들을 삭제합니다. 성공 시, ML 모델 저장소에서 관리 중인 모델목록을 보여줍니다.( spark 모델과 tf 모델 모두 사용 가능 )

Parameters
''''''''''
.. code-block:: python

   mlmodel delete (user=<user>)? name=<model_name> version=<version>

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예제
     - 타입
     - 필수
   * - user
     - 모델 소유주 명
     - API를 요청하는 user
     - demo
     - string
     - False
   * - name
     - 모델명
     -
     - mnist_v1
     - string
     - True
   * - version
     - 모델의 버전
     -
     - 1
     - int
     - True


Examples
''''''''

"mnist_v1" 모델을 삭제합니다.

.. code-block:: python

   mlmodel delete name=mnist_v1 version=2

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

mlmodel import
----------------------------------------------------------------------------------------------------

객체 저장소에 있는 사용자의 계정 소유의 학습 모델 파일을 IRIS Discovery Service가 관리하는 ML 모델 저장소에 적재 합니다.(tf 모델만 사용 가능)
적재된 모델은 학습, 예측, 평가, 배포 명령어 등에 활용할 수 있습니다.

학습 모델 파일은 tar 아카이브 형태이어야 하며, 아카이브 파일 내 타입별 필수 파일은 다음과 같습니다.

Parameters
''''''''''

.. code-block:: python

   .. | mlmodel import name=mnist_v1 analysis_tool=tf kind=classification algorithm=deep format=saved_model connector_id={CONNECTOR_ID} path={KEY}


.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예제
     - 타입
     - 필수
   * - name
     - 저장할 모델명
     -
     - mnist_v1
     - 문자열
     - True
   * - analysis_tool
     - 구현 tool
     -
     - tf
     - 문자열
     - True
   * - kind
     - 기계학습 종류
     -
     - classification
     - 문자열
     - True
   * - algorithm
     - 알고리즘
     -
     - deep
     - 문자열
     - True
   * - format
     - 모델 포멧
     -
     - h5 또는 saved_model
     - 문자열
     - True
   * - connector_id
     - 객체 스토리지 연결정보 아이디
     -
     - 255
     - 문자열
     - True
   * - path
     - 객체 스토리지 내 모델 소스 경로, bucket은 생략해야 합니다.
     -
     - USERS/root/model.tar
     - 문자열
     - True

- type 별 필수 포함 파일 명

.. list-table::
   :header-rows: 1

   * - 타입
     - 필수 포함 파일
   * - Spark
     - 학습 모델  파일 (data.parquet), 학습 모델 메타 파일 (metadata)
   * - TensorFlow
     - 학습 모델  파일 (model.h5 or saved_model.pb)

Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

- 모델정보를 아카이브한 tar 파일을 IRIS Discovery Service가 관리하는 객체저장소에 업로드 합니다.

.. code-block:: python

   mlmodel import name=tf_clothes analysis_tool=tf kind=classification algorithm=deep format=saved_model connector_id=aqef32-asdf23-sadf path=USERS/root/clothes/model.tar
   

- 출력 결과

.. list-table::
   :header-rows: 1

   * - result
   * - ok

mlmodel export
----------------------------------------------------------------------------------------------------

먼저 IRIS Discovery 의 ML 모델 저장소에서 관리되고 있는 학습 모델을 파라미터로 입력한 연결정보의 개인 객체 저장소에 저장한 후, download url을 제공합니다.(tf 모델만 사용 가능)


Parameters
''''''''''

.. code-block:: python

   mlmodel export (user=<user>)? name=<model_name> (version=<version>)? connector_id={CONNECTOR_ID} path={KEY}

.. list-table::
   :header-rows: 1
   
   * - 이름
     - 설명
     - 기본값
     - 예제
     - 타입
     - 필수
   * - user
     - 모델 소유주 명
     - API를 요청하는 user
     - demo
     - string
     - False
   * - name
     - 모델명
     -
     - mnist_v1
     - string
     - True
   * - version
     - 모델의 버전
     - last version of model
     - 1
     - int
     - False
   * - connector_id
     - 객체 스토리지 연결정보 아이디
     -
     - 255
     - 문자열
     - True
   * - path
     - 객체 스토리지 내 모델 저장 경로, bucket은 생략해야 합니다.
     -
     - USERS/root/model.tar
     - 문자열
     - True


Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

- mnist_v1 모델을 개인 객체 저장소에 저장하고, 다운로드 받을 수 있는 url 을 통해 로컬PC 로 다운로드합니다.

.. code-block:: python

   mlmodel export user=demo name=mnist_v1 connector_id=179 path=USERS/ROOT/mnist_v1_export.tar

- 출력 결과

.. list-table::
   :header-rows: 1

   * - result
     - download_url
     - expired time (sec)
   * - ok
     - http://b-iris.mobigen.com/hdfs-browser/minio/download?path=%2FROOT%2F%2Fmnist_v1_export.tar
     - 3600

mlmodel deploy
----------------------------------------------------------------------------------------------------

mlmodel import 를 통해 IRIS Discovery 의 ML 모델 저장소에서 관리되고 있는 ML 모델(tf type) 을 서빙 가능하도록 TersorFlow Serving에 배포합니다. (tf 모델만 사용 가능)

모델 배포시 버전을 선택하지 않으면 자동으로 마지막 버전을 배포합니다.
배포가 되면, `serving 명령어 <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/serving.html>`_ 로 version별 모델 예측, 모델 서빙 상태 확인이 가능합니다.


Parameters
''''''''''

.. code-block:: python

   mlmodel deploy (user=<user>)? name=<model_name> (version=<version>)? label='stable'

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예제
     - 타입
     - 필수
   * - user
     - 모델 소유주 명
     - API를 요청하는 user
     - demo
     - string
     - False
   * - name
     - 모델명
     -
     - mnist_v1
     - string
     - True
   * - version
     - 모델의 버전
     - last version of model
     - 1
     - int
     - False
   * - label
     - 배포 모델의 설명
     -
     - unstable 또는 stable 등
     - 문자열
     - False


Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

- 학습된 mnist_v1모델을 서빙 배포합니다.

.. code-block:: python

   mlmodel deploy name=mnist_v1 label='unstable'

출력 결과

- serving_name은 유저명과 모델이름을 합친 문자열입니다. curl로 서빙에 요청할 경우 해당 이름으로 요청해야합니다.

.. list-table::
   :header-rows: 1

   * - result
     - latest_version
     - serving_name
   * - on
     - 1
     - root_mnist_v1

mnist_v1모델을 업데이트하고 재배포합니다.

.. code-block:: python

   mlmodel deploy name=mnist_v1 label='stable'

출력 결과

- 버전이 1 올라갑니다.

.. list-table::
   :header-rows: 1

   * - result
     - latest_version
     - serving_name
   * - on
     - 2
     - root_mnist_v1



mlmodel stop
----------------------------------------------------------------------------------------------------

서빙 중인 배포 모델을 더 이상 서빙 하지 않도록 중지합니다. 버전을 선택하지 않으면 마지막 버전을 중지합니다.(tf 모델만 사용 가능)

Parameters
''''''''''

.. code-block:: python

   mlmodel stop (user=<user>)? name=<model_name> (version=<version>)?

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예제
     - 타입
     - 필수
   * - user
     - 모델 소유주 명
     - API를 요청하는 user
     - demo
     - string
     - False
   * - name
     - 모델명
     -
     - mnist_v1
     - string
     - True
   * - version
     - 모델의 버전
     - last version of model
     - 1
     - int
     - False

Examples
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

- mnist_v1모델을 중지합니다.

.. code-block:: python

   mlmodel stop mnist_v1

- 결과

.. list-table::
   :header-rows: 1

   * - result
   * - off

