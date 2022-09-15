summary
==========

개요
------
조회한 DataFrame에 대한 summary 정보를 구해줍니다.

타입
----------------------------------------------------------------------------------------------------


설명
------

인자로는 아무것도 받지 않으며, summary 연산이 가능한 컬럼에 대해서만 count, mean, stddev, min, 25%, 50%, 75%, max 값을 구해줍니다.

수치형 자료인 경우 모든 값을 구할 수 있으며, 문자열일 경우 count값 만 구해집니다.

특정 컬럼이 불리언인 경우에는 해당 컬럼을 반환하지 않습니다.

Parameters
--------------------------------------

.. code-block:: none

    ... | summary

Examples
--------

- 예제 데이터

.. list-table::
   :header-rows: 1
   
   * - INTEGERS
     - STRING
     - STR_START_DATE_OF_WEEK
     - BOOL
   * - 1823
     - hello
     - 2020-01-06
     - FALSE
   * - 2073
     - world
     - 2020-01-06
     - TRUE
   * - 2020
     - mobigen
     - 2020-01-02
     - TRUE

- 예제1

.. code-block:: none

    * | summary

.. list-table::
   :header-rows: 1
   
   * - summary
     - INTEGERS
     - STRING
     - STR_START_DATE_OF_WEEK
   * - count
     - 3
     - 3
     - 3
   * - mean
     - 1972.0
     - None
     - None
   * - stddev
     - 131.73
     - None
     - None
   * - min
     - 1823.0
     - None
     - None
   * - 25%
     - 1823.0
     - None
     - None
   * - 50%
     - 2020.0
     - None
     - None
   * - 75%
     - 2073.0
     - None
     - None
   * - max
     - 2073.0
     - None
     - None

