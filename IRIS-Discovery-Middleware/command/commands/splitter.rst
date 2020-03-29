.. role:: raw-html-m2r(raw)
   :format: html


splitter
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

레코드형 파일(들)을 train/test 로 분리 저장하는 명령어

설명
----------------------------------------------------------------------------------------------------

객체 저장소에 저장되어 있는 레코드형 파일(들)을 train/test 로 분리 저장하는 명령어입니다. 
파일에 학습 레이블 컬럼과 태그 컬럼이 있으면 사전 데이터를 추가로 생성할 수 있습니다. 
출력 결과로 train으로 분리된 레코드 수와 test로 분리된 레코드 수를 출력합니다.

.. list-table::
   :header-rows: 1

   * - train
     - test
   * - 800
     - 200

Parameters
----------------------------------------------------------------------------------------------------

``splitter src=OBJECTSTORAGE.{CONNECTOR_NAME}:{KEY} train=({KEY}, 0.8) test=({KEY}, 0.2)``

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본값
     - 예시값
     - 타입
     - 필수
   * - src
     - 입력 파일 or 디렉토리 경로
     - 
     - OBJECTSTORAGE.MIN_AI:mnist/tsv
     - 문자열
     - O
   * - train
     - 학습 데이터 저장 경로와 비율 :raw-html-m2r:`<br />` CONNECTOR 정보(``OBJECTSTORAGE.{CONNECTOR_NAME}``)는 ``src`` 파라미터를 참조합니다.
     - 
     - (mnist/train.tsv, 0.8)
     - 문자열
     - O
   * - test
     - 테스트 데이터 저장 경로와 비율
     - 
     - (mnist/test.tsv, 0.2)
     - 문자열
     - O
   * - dictionary
     - 사전 데이터 저장 경로, 학습 레이블 컬럼명, 태그 컬럼명
     - 
     - (mnist/dict.tsv, label, tag)
     - 튜플
     -    
   * - mode
     - 학습, 테스트, 사전 데이터 저장 모드 (overwrite, append)
     - overwrite
     - append
     - 문자열
     -  
   * - delimiter
     - src 파라미터의 구분자
     - \t
     - ,
     - 문자열
     -        

``CONNECTOR_NAME`` : Conncetor Name입니다. IRIS UI에서 연결정보 생성 후, 연결정보의 ``이름`` 컬럼에서 확인할 수 있는 값입니다.

``KEY`` : OBJECTSTORAGE의 key입니다. bucket은 생략해야 합니다.

Examples
----------------------------------------------------------------------------------------------------

객체 저장소에 저장되어 있는 레코드형 파일들을 80:20으로 학습 파일과 테스트 파일로 분리 저장합니다.

``splitter src=OBJECTSTORAGE.MIN_AI:USERS/pjh0347/mnist/tsv train=(USERS/pjh0347/mnist/train.tsv, 0.8) test=(USERS/pjh0347/mnist/test.tsv, 0.2)``

출력 결과
- train으로 분리된 레코드 수와 test로 분리된 레코드 수를 출력

.. list-table::
   :header-rows: 1

   * - total
     - test
   * - 54540
     - 13420

학습 레이블 컬럼명이 label이고 태그 컬럼명이 tag인 레코드형 파일(0.tsv)을 train/test 로 분리하여 덮어씁니다.

``splitter src=OBJECTSTORAGE.MIN_AI:USERS/pjh0347/mnist/tsv/0.tsv train=(USERS/pjh0347/mnist_demo/train.tsv, 0.8) test=(USERS/pjh0347/mnist_demo/test.tsv, 0.2) dictionary=(USERS/pjh0347/mnist_demo/dict.tsv, label, tag) mode=overwrite``

.. list-table::
   :header-rows: 1

   * - total
     - test
   * - 5000
     - 1000

학습 레이블 컬럼명이 label이고 태그 컬럼명이 tag인 레코드형 파일(1.tsv)을 train/test 로 분리하여 기존 파일에 추가합니다.

``splitter src=OBJECTSTORAGE.MIN_AI:USERS/pjh0347/mnist/tsv/1.tsv train=(USERS/pjh0347/mnist_demo/train.tsv, 0.8) test=(USERS/pjh0347/mnist_demo/test.tsv, 0.2) dictionary=(USERS/pjh0347/mnist_demo/dict.tsv, label, tag) mode=overwrite``

.. list-table::
   :header-rows: 1

   * - total
     - test
   * - 5000
     - 1000

tsv 데이터 예시

- train.tsv or test.tsv

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

- dictionary

.. list-table::
   :header-rows: 1

   * - label
     - tag
   * - 1,0,0,0,0,0,0,0,0,0
     - zero    
   * - 0,1,0,0,0,0,0,0,0,0
     - one
   * - 0,0,1,0,0,0,0,0,0,0
     - two
   * - ...
     - ...
   * - 0,0,0,0,0,0,0,0,0,1
     - nine
     

Parameters BNF
----------------------------------------------------------------------------------------------------

.. code-block:: none

   splitter_command : SRC EQ WORD TRAIN EQ WORD_WITH_BRACKET TEST EQ WORD_WITH_BRACKET options
   options : option
            | options option
            |
   option : WORD EQ WORD
          | WORD EQ WORD_WITH_BRACKET
   
   WORD : r'[^ |^\|^\'|\"|^\=]+'
   WORD_WITH_BRACKET : r'\([^\|^\'|\"|^\=]+\)'
   EQ : r'\='
   SRC : r'(?i)src'
   TRAIN : r'(?i)train'
   TEST : r'(?i)test'