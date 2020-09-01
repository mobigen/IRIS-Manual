weeknumber
==========

개요
----

데이터의 날짜가 1년의 몇 번째 주인지 계산하는 명령어

설명
----

데이터의 날짜가 1년 중 몇 번째 주인지 계산

예) 2020-01-01 -> 1

Parameters
---------

.. code-block:: none

    * | weeknumber 컬럼명 "<date format>"
    * | weeknumber 컬럼명 "<date format>" as 결과컬럼명

.. list-table::
   :header-rows: 1
   
   * - 이름
     - 설명
     - 필수/옵션
   * - 컬럼명
     - 계산하려는 데이터의 컬럼명
     - 필수
   * - date format
     - 데이터의 날짜 포멧을 작성, 아래 날짜 포멧 기준을 참고, 예) YYYYMMDD
     - 필수
   * - as 결과컬럼명
     - 계산된 결과의 컬럼명, (Default: WEEK_NUM_OF_YEAR)
     - 옵션

- 날짜 포멧 기준

.. list-table::
   :header-rows: 1
   
   * - 구분
     - 포멧
   * - 년
     - YYYY
   * - 월
     - MM
   * - 일
     - DD

Examples
--------

- 예제 데이터

.. list-table::
   :header-rows: 1
   
   * - A
     - B
   * - 2020-01-01
     - aa
   * - 2020-03-01
     - bb
   * - 2020-06-01
     - cc

- 예제1

.. code-block:: none

    * | weeknumber A "YYYY-MM-DD"

.. list-table::
   :header-rows: 1

   * - A
     - B
     - WEEK_NUM_OF_YEAR
   * - 2020-01-01
     - aa
     - 1
   * - 2020-03-01
     - bb
     - 9
   * - 2020-06-01
     - cc
     - 23

- 예제2

.. code-block:: none

    * | weeknumber A "YYYY-MM-DD" as `몇 주차?`

.. list-table::
   :header-rows: 1

   * - A
     - B
     - 몇 주차?
   * - 2020-01-01
     - aa
     - 1
   * - 2020-03-01
     - bb
     - 9
   * - 2020-06-01
     - cc
     - 23
