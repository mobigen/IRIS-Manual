value_counts
==========

개요
------
특정 필드에 대한 고유값과 해당 값의 count를 리턴합니다.

컬럼 타입
----------------------------------------------------------------------------------------------------
TEXT, INTEGER, BIGINT, REAL, DATE, TIMESTAMP

설명
------

인자로는 하나의 컬럼만 가능하며, 선택된 컬럼의 모든 값들에 대한 고유 값(distinct value)과 해당 값들의 갯수를 반환합니다.

pandas의 value_counts 메서드와 동일하게 동작합니다.

Parameters
--------------------------------------

.. code-block:: none

    ... | value_counts FIELD

Examples
--------

- 예제 데이터

.. list-table::
   :header-rows: 1
   
   * - A
     - B
     - C
     - Bool
   * - CL_A
     - 10
     - 32
     - True
   * - CL_A
     - 5
     - 30
     - True
   * - CL_B
     - 3
     - 12
     - False

- 예제1

.. code-block:: none

    * | value_counts A

.. list-table::
   :header-rows: 1
   
   * - A
     - count
   * - CL_A
     - 2
   * - CL_B
     - 1

- 예제2

.. code-block:: none

    * | value_counts Bool

.. list-table::
   :header-rows: 1
   
   * - Bool
     - count
   * - true
     - 2
   * - false
     - 1

