weekdates
==========

개요
----

정수를 이용해 해당 주차의 시작일과 종료일을 찾는 명령어

타입
----------------------------------------------------------------------------------------------------
DATE, TIMESTAMP

설명
----

1~52 사이의 정수를 이용해서 해당하는 주차의 시작일과 종료이를 찾는 명령어

주의사항: 항상 년도와 몇 주차인지 에 대한 데이터가 있어야 계산이 가능

예) 2020-2 -> 2020-01-06 ~ 2020-01-12

Parameters
---------

.. code-block:: none

    * | weekdates 컬럼명 "<date format>"

.. list-table::
   :header-rows: 1
   
   * - 이름
     - 설명
     - 필수/옵션
   * - 컬럼명
     - 주차 of 년 의 정수가 있는 컬럼
     - 필수
   * - date format
     - 데이터의 날짜 포멧을 작성, 아래 날짜 포멧 기준을 참고, 예) YYYY.WW
     - 필수

- 날짜 포멧 기준

.. list-table::
   :header-rows: 1
   
   * - 구분
     - 포멧
   * - 년
     - YYYY
   * - 주차
     - WW

Examples
--------

- 예제 데이터

.. list-table::
   :header-rows: 1
   
   * - A
     - B
   * - 2020-2
     - aa
   * - 2020-23
     - bb
   * - 2020-34
     - cc

- 예제1

.. code-block:: none

    * | weekdates A "YYYY-WW"

.. list-table::
   :header-rows: 1

   * - A
     - B
     - START_DATE_OF_WEEK
     - END_DATE_OF_WEEK
   * - 2020-2
     - aa
     - 2020-01-06
     - 2020-01-12
   * - 2020-23
     - bb
     - 2020-06-01
     - 2020-06-07
   * - 2020-34
     - cc
     - 2020-08-17
     - 2020-08-23
