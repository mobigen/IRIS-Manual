.. role:: raw-html-m2r(raw)
   :format: html


img2tsv
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

이미지 파일을 읽어서 TSV 포멧으로 변환 저장하는 명령어

설명
----------------------------------------------------------------------------------------------------

객체 저장소에 저장되어 있는 이미지 파일 또는 폴더를 읽어서 TSV 포멧으로 변환 저장하는 명령어입니다. 
이미지를 Spark 에서 지원하는 spark.read.format("image") API 를 사용하여 원시 이미지 표현으로 로드 후, float형의 벡터로 반환합니다. 
단순히 벡터만 반환할 수도 있고 레이블 컬럼을 추가할수도 있습니다.

Parameters
----------------------------------------------------------------------------------------------------

``img2tsv src=OBJECTSTORAGE.{CONNECTOR_NAME}:{KEY} dst={KEY} column_name=feature label=(label, 3) tag=three``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - src
     - 이미지 파일 or 디렉토리 경로
     - 
     - OBJECTSTORAGE.MIN_AI:mnist/0
     - 문자열
     - O
   * - dst
     - 저장할 TSV 파일 경로 :raw-html-m2r:`<br />` CONNECTOR 정보(``OBJECTSTORAGE.{CONNECTOR_NAME}``)는 ``src`` 파라미터를 참조합니다.
     - 
     - mnist/tsv/0.tsv 
     - 문자열
     - O
   * - column_name
     - 이미지 벡터를 저장할 컬럼명
     - feature
     - feature
     - 문자열
     - 
   * - label
     - 레이블 컬럼 이름, 타입, 값
     - 
     - (label, [1,0,0,0,0,0,0,0,0,0])
     - 튜플
     -    
   * - scale
     - 이미지 벡터를 나눌 값
     - 255.0
     - 255.0
     - 플롯형
     -  
   * - tag
     - 레이블이 의미하는 태그 값 :raw-html-m2r:`<br />` tag값이 있으면, splitter 명령어에서 dictionary를 만들 수 있습니다.
     - 
     - zero
     - 문자열
     -        

``CONNECTOR_NAME`` : Conncetor Name입니다. IRIS UI에서 연결정보 생성 후, 연결정보의 ``이름`` 컬럼에서 확인할 수 있는 값입니다.

``KEY`` : OBJECTSTORAGE의 key입니다. bucket은 생략해야 합니다.

Examples
----------------------------------------------------------------------------------------------------

객체 저장소의 특정 이미지를 TSV로 변환하여 저장합니다.

``img2tsv src=OBJECTSTORAGE.MIN_AI:USERS/pjh0347/mnist/0/35923.png dst=USERS/pjh0347/mnist/tsv/0.tsv column_name=feature label=(label, [1,0,0,0,0,0,0,0,0,0]) tag=zero`` 

- (label, [1,0,0,0,0,0,0,0,0,0]) : 라벨이 저장되는 컬럼 및 데이터입니다. 이 예제는 one-hot 벡터를 표현합니다.

출력 결과
- 변환한 이미지의 개수가 출력됩니다.

.. list-table::
   :header-rows: 1

   * - total
   * - 1


객체 저장소의 이미지 디렉토리를 읽어 TSV로 변환하여 저장합니다.

``img2tsv src=OBJECTSTORAGE.MIN_AI:USERS/pjh0347/mnist/0 dst=USERS/pjh0347/mnist/tsv/0.tsv column_name=feature label=(label, [1,0,0,0,0,0,0,0,0,0]) tag=zero`` 

출력 결과

.. list-table::
   :header-rows: 1

   * - total
   * - 6796

tsv 데이터 예시

.. list-table::
   :header-rows: 1

   * - label
     - tag
     - feature
   * - 0,1,0,0,0,0,0,0,0,0
     - one
     - 0.0,0.0,...,0.14901960784313725,0.8941176470588236,0.11372549019607843,...,0.0,0.0,0.0,0.0,0.0,0.0
   * - ...
     - ...
     - ...


Parameters BNF
----------------------------------------------------------------------------------------------------

.. code-block:: none

   img2tsv_command : SRC EQ WORD DST EQ WORD options
   options : option
            | options option
            |
   option : WORD EQ WORD
          | WORD EQ WORD_WITH_BRACKET
   
   WORD : r'[^ |^\|^\'|\"|^\=]+'
   WORD_WITH_BRACKET : r'\([^\|^\'|\"|^\=]+\)'
   EQ : r'\='
   SRC : r'(?i)src'
   DST : r'(?i)dst'