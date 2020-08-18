
WHERE
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

데이터를 일정 조건에 따라 filter 합니다.

설명
----------------------------------------------------------------------------------------------------

데이터를 일정 조건에 따라 filter 합니다. 여기서 filter문은 SQL의 WHERE 절과 동일합니다.

Parameters
----------------------------------------------------------------------------------------------------

.. code-block::

   ... | where FILTER_CONDITION

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - FILTER_CONDITION
     - SQL문에서 WHERE 절에 조건문과 동일 합니다.
     - 필수

Examples
-------------------------------
- 예제 데이터 모양 입니다.

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - ClassA
     - a
     - 90
     - 95
   * - ClassB
     - b
     - 80
     - 90
   * - ClassB
     - c
     - 70
     - 100
   * - ClassC
     - d
     - 100
     - 50
   * - ClassC
     - e
     - 95
     - 70
   * - ClassC
     - f
     - 60
     - 95

`D` 필드의 값 중 95인 값을 출력합니다.

.. code-block::

   ... | where D = 95 

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - ClassA
     - a
     - 90
     - 95
   * - ClassC
     - f
     - 60
     - 95

`A` 필드의 값 중 ClassA와 ClassC를 포함하는 값을 출력합니다.

.. code-block::

   ... | A in ('ClassA', 'ClassC')

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - ClassA
     - a
     - 90
     - 95
   * - ClassC
     - d
     - 100
     - 50
   * - ClassC
     - e
     - 95
     - 70
   * - ClassC
     - f
     - 60
     - 95

`A` 필드의 값 중 A 또는 C 로 끝나는 문자열을 포함하는 값을 출력합니다.

.. code-block::

   ... | A like '%A' or A like '%C'

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - ClassA
     - a
     - 90
     - 95
   * - ClassC
     - d
     - 100
     - 50
   * - ClassC
     - e
     - 95
     - 70
   * - ClassC
     - f
     - 60
     - 95
