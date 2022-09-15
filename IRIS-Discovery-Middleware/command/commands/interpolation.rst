interpolation
=========

개요
----

| 시계열 데이터를 동일한 시간 interval(간격)을 설정하여 집계를 할 때,
| interval 당 해당하는 데이터가 없는 경우 데이터를 임의로 interpolation(보간) 하여 집계하는 명령어

타입
----------------------------------------------------------------------------------------------------
TEXT, INTEGER, BIGINT, REAL, DATE, TIMESTAMP

설명
----

| 시계열 데이터를 동일한 시간 interval(간격) 으로 집계 하는 명령어
| 해당 interval 에 데이터가 없으면 임의로 보간 한다.

Parameters
---------

.. code-block:: none

    * | interpolation agg_func(, agg_func) index=<time col> interval=<value> fillna=<value> (options)?

.. list-table::
   :header-rows: 1
   
   * - 이름
     - 설명
     - 필수/옵션
   * - agg_func
     - 집계함수 |br| comma(,) 를 이용해 구분가능
     - 필수
   * - index
     - 시간 컬럼명, 해당 값을 이용해 데이터를 보간 한다.
     - 필수
   * - interval
     - 시간의 간격, <숫자><문자> 로 이루어진 값을 사용 |br| 문자: y(year), m(month), d(day), H(hour), M(minute), S(second) |br| 예) 10초간격=10S, 10분간격=10M
     - 필수
   * - fillna
     - 보간을 한 데이터의 집계 결과는 항상 Null 이기 때문에 해당 값을 치환하는 값을 입력 |br| 숫자 입력시: 값 그대로 사용 |br| ffill/bfill : 전위/후위 값 사용
     - 필수
   * - options
     - 나머지 옵션들, 아래 테이블 참조
     - 옵션

- 옵션 테이블

.. list-table::
   :header-rows: 1
   
   * - 이름
     - 설명
     - 필수/옵션
   * - index_format
     - 시간 컬럼이 string 인 경우 해당 데이터의 포멧을 입력 |br| default: 'YYYY-MM-DD HH:mm:ss' |br| 예) 20201012135023 -> YYYYMMDDHHmmss
     - 옵션
   * - by
     - 그룹핑 컬럼명 설정 가능, comma(,) 로 구분 가능
     - 옵션
   * - start_date
     - 보간의 시작 시간, 시작시간은 포함 |br| 우선순위: start_date 로 설정한 시간 > 검색시 사용한 시작 시간 > 각 그룹의 데이터의 가장 작은 시간 |br| 예) start_date=20201020124000
     - 옵션
   * - end_date
     - 보간의 끝 시간, 끝 시간 미만까지 처리 |br| 우선순위: end_date 로 설정한 시간 > 검색시 사용한 끝 시간 > 각 그룹의 데이터의 가장 큰 시간 |br| 예) end_date=20201120124000
     - 옵션

Examples
--------

- 예제 데이터

.. list-table::
   :header-rows: 1

   * - DATETIME1
     - DATETIME2
     - HOST
     - PROGRAM
     - LEVEL_INT
   * - 2020-10-13 17:10:32
     - 20201013171032
     - ans41
     - CROND
     - 7
   * - 2020-10-13 17:10:40
     - 20201013171040
     - ans41
     - CROND
     - 4
   * - 2020-10-13 17:31:02
     - 20201013173102
     - ans41
     - kubelet
     - 2
   * - 2020-10-13 17:44:20
     - 20201013174420
     - ans41
     - kubelet
     - 7
   * - 2020-10-13 17:52:40
     - 20201013175240
     - ans41
     - kubelet
     - 3
   * - 2020-10-13 17:14:00
     - 20201013171400
     - gcs1
     - kubelet
     - 7
   * - 2020-10-13 17:22:02
     - 20201013172202
     - gcs1
     - kubelet
     - 2
   * - 2020-10-13 17:22:10
     - 20201013172210
     - gcs1
     - CROND
     - 3
   * - 2020-10-13 17:26:23
     - 20201013172623
     - gcs1
     - CROND
     - 1
   * - 2020-10-13 17:40:00
     - 20201013174000
     - gcs1
     - CROND
     - 6   

- 예제1) 10분 단위 간격, 시작:20201013171000, 끝:20201013180000

.. code-block:: python

    * | interpolation count(*)
                      by=HOST
                      index=DATETIME1
                      interval=600S
                      fillna=0
                      start_date='20201013171000'
                      end_date='20201013180000'

.. list-table::
   :header-rows: 1

   * - DATETIME1
     - HOST
     - count(*)
   * - 2020-10-13 17:10:00
     - gcs1
     - 1
   * - 2020-10-13 17:20:00
     - gcs1
     - 3
   * - 2020-10-13 17:30:00
     - gcs1
     - 0
   * - 2020-10-13 17:40:00
     - gcs1
     - 1
   * - 2020-10-13 17:50:00
     - gcs1
     - 0
   * - 2020-10-13 17:10:00
     - ans41
     - 2
   * - 2020-10-13 17:20:00
     - ans41
     - 0
   * - 2020-10-13 17:30:00
     - ans41
     - 1
   * - 2020-10-13 17:40:00
     - ans41
     - 1
   * - 2020-10-13 17:50:00
     - ans41
     - 1

- 예제2

.. code-block:: python

    * | interpolation count(*) as CNT, max(LEVEL_INT) as MLI
                      by=HOST
                      index=DATETIME2
                      index_format='YYYYMMDDHHmmss'
                      interval=10M
                      fillna=0
                      start_date='20201013171000'
                      end_date='20201013180000'

.. list-table::
   :header-rows: 1

   * - DATETIME2
     - HOST
     - CNT
     - MLI
   * - 2020-10-13 17:10:00
     - gcs1
     - 1
     - 7.0
   * - 2020-10-13 17:20:00
     - gcs1
     - 3
     - 3.0
   * - 2020-10-13 17:30:00
     - gcs1
     - 0
     - 0
   * - 2020-10-13 17:40:00
     - gcs1
     - 1
     - 6.0
   * - 2020-10-13 17:50:00
     - gcs1
     - 0
     - 0
   * - 2020-10-13 17:10:00
     - ans41
     - 2
     - 7.0
   * - 2020-10-13 17:20:00
     - ans41
     - 0
     - 0
   * - 2020-10-13 17:30:00
     - ans41
     - 1
     - 2.0
   * - 2020-10-13 17:40:00
     - ans41
     - 1
     - 7.0
   * - 2020-10-13 17:50:00
     - ans41
     - 1
     - 3.0

.. |br| raw:: html

  <br/>
