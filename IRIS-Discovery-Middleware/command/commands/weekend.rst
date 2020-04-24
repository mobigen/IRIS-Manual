
weekend
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

이 명령어는 주말/주중 데이터를 필터링하는데 사용합니다.

설명
----------------------------------------------------------------------------------------------------

데이터모델에서 설정한 시간 필드를 기준으로 주말/주중에 해당하는 행(Row)만 골라 검색합니다. 반드시 시간 필드가 존재하는 데이터모델이어야 합니다.

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | weekend (TRUE|FALSE) (date field (FORMAT dateFormat)?)?

.. list-table::
   :header-rows: 1
   :widths: 20 60 20

   * - 이름
     - 설명
     - 필수/옵션
   * - ``TRUE``
     - 데이터 중 주말(weekend, 토 ~ 일) 데이터만 선택합니다.
     - 필수
   * - ``FALSE``
     - 데이터 중 주중(weekday, 월 ~ 금) 데이터만 선택합니다.
     - 필수
   * - date field
     - 사용자가 선택한 시간 컬럼으로, 날짜 데이터가 string 형식으로 존재하는 컬럼의 컬럼명을 입력합니다.
       :raw-html-m2r: `<br/>` 예) timefield = [['20200101010101'], ['20200101010105'], ...] 과 같은 데이터의 컬럼명인 ``timefield``
     - 옵션
   * - FORMAT dateFormat
     - FORMAT 은 예약어입니다.
       :raw-html-m2r: `<br/>` dateFormat 부분에는 아래의 시간 포멧 규칙에 따라 데이터의 모양에 해당하는 포멧을 입력해줍니다.
       :raw-html-m2r: `<br/>` 예) '20200101010101' 과 같은 데이터는 ``YYYYMMDDHHmmss``, '2020/01/01 01:01:01' 과 >같은 데이터는 ``YYYY/MM/DD HH:mm:ss`` 로 사용합니다.
       :raw-html-m2r: `<br/>` (기본값 = ``YYYYMMDDHHmmss``)
     - 옵션

시간 포멧 규칙
"""""""""""""""""""

.. list-table::
   :header-rows: 1

   * - 문자
     - 의미
   * - YYYY
     - ``년``
   * - MM
     - ``월``
   * - DD
     - ``일``
   * - HH
     - ``시간``
   * - mm
     - ``분``
   * - ss
     - ``초``


Parameters BNF
----------------------------------------------------------------------------------------------------

.. code-block:: none

   func : BOOL
        | BOOL tokens
        | BOOL tokens date_format

   date_format : FORMAT TOKEN

   tokens : TOKEN
          | tokens TOKEN

   TOKEN : [^ ]+
   BOOL : True | False
   FORMAT : FORMAT

Examples
----------------------------------------------------------------------------------------------------
* 기본 데이터

  * 2020/01/03 = 금요일, 2020/01/04 = 토요일

.. list-table::
   :header-rows: 1

   * - DATETIME
     - HOST
     - PROGRAM
   * - 20200103010101
     - gcs1
     - a
   * - 20200104010101
     - gcs3
     - b

1. 데이터 모델의 시간 필드 기준으로 사용할 때 ( 새 모델 생성시 ``시간`` 으로 선택한 필드)

   * DATETIME 컬럼이 ``시간`` 필드로 선택되어 있는 상황.

* 예제 데이터

.. list-table::
   :header-rows: 1

   * - DATETIME (type = TIMESTAMP, 시간 컬럼)
     - HOST
     - PROGRAM
   * - datetime.datetime(2020,1,3,1,1,1)
     - gcs1
     - a
   * - datetime.datetime(2020,1,4,1,1,1)
     - gcs3
     - b

* 주말에 해당하는 데이터만 선택하는 예제입니다.

.. code-block:: none

   ... | weekend True

.. list-table::
   :header-rows: 1

   * - DATETIME
     - HOST
     - PROGRAM
   * - 20200104010101
     - gcs3
     - b

* 주중에 해당하는 데이터만 선택하는 예제입니다.

.. code-block:: none

   ... | weekend False

.. list-table::
   :header-rows: 1

   * - DATETIME
     - HOST
     - PROGRAM
   * - 20200103010101
     - gcs1
     - a

2. 사용자가 원하는 시간 관련 컬럼을 기준으로 사용할 때

2-1. DATETIME 의 type 이 TIMESTAMP 일 때

* 예제 데이터

.. list-table::
   :header-rows: 1

   * - DATETIME (type = TIMESTAMP, 일반 컬럼)
     - HOST
     - PROGRAM
   * - datetime.datetime(2020,1,3,1,1,1)
     - gcs1
     - a
   * - datetime.datetime(2020,1,4,1,1,1)
     - gcs3
     - b

* 주말에 해당하는 데이터만 선택하는 예제입니다.

.. code-block:: none

   ... | weekend True DATETIME

.. list-table::
   :header-rows: 1

   * - DATETIME
     - HOST
     - PROGRAM
   * - datetime.datetime(2020,1,4,1,1,1)
     - gcs3
     - b

* 주중에 해당하는 데이터만 선택하는 예제입니다.

.. code-block:: none

   ... | weekend False DATETIME

.. list-table::
   :header-rows: 1

   * - DATETIME
     - HOST
     - PROGRAM
   * - datetime.datetime(2020,1,3,1,1,1)
     - gcs1
     - a

2-2. DATETIME 의 type 이 TEXT 일 때

* 예제 데이터

.. list-table::
   :header-rows: 1

   * - DATETIME (type = TIMESTAMP, 일반 컬럼)
     - HOST
     - PROGRAM
   * - 20200103010101
     - gcs1
     - a
   * - 20200104010101
     - gcs3
     - b

* 주말에 해당하는 데이터만 선택하는 예제입니다.

.. code-block:: none

   ... | weekend True DATETIME FORMAT YYYYMMDDHHmmss

.. list-table::
   :header-rows: 1

   * - DATETIME
     - HOST
     - PROGRAM
   * - 20200104010101
     - gcs3
     - b

* 주중에 해당하는 데이터만 선택하는 예제입니다.

.. code-block:: none

   ... | weekend False DATETIME FORMAT YYYYMMDDHHmmss

.. list-table::
   :header-rows: 1

   * - DATETIME
     - HOST
     - PROGRAM
   * - 20200103010101
     - gcs1
     - a

