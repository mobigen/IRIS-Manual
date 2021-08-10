lead
=========

개요
----

현재 행의 다음 행의 값을 가져오는 명령어

설명
----

현재 행의 다음 행의 값을 가져오는 명령어

현재 데이터와 다음 데이터의 값을 비교 할 때 사용되면 유용한 명령어

예를들어, 선택한 컬럼의 데이터가 1,2,3,4 순서로 있을 때, 해당 데이터의 다음값 즉 1,2,3,null 의 값을 가져오는 동작을 실행

Parameters
---------

.. code-block:: none

    * | lead {column:str} BY {orderby:str} (COUNT {count:int}) (DEFAULT {default:str}) (PARTITION {partition:str}) (AS {alias:str})?

.. list-table::
   :header-rows: 1

   * - 이름
     - Type
     - 설명
     - 필수
   * - column
     - string
     - 해당 컬럼의 값의 다음행을 가져오기 위한 컬럼
     - 필수
   * - orderby
     - string
     - 해당 컬럼 기준으로 sort 적용
     - 필수
   * - count
     - int
     - row offset 설정 값
     - 옵션
   * - default
     - string
     - 빈 칸에 들어갈 default 값
     - 옵션
   * - partition
     - string
     - 해당 column기준 같은 row들을 grouping 한다. 나중에 각 group에서 가능을 따로 적용 한다.
     - 옵션
   * - alias
     - string
     - 결과 데이터를 저장할 컬럼명, (Default: NEXT_ROW_OF_값컬럼명)
     - 옵션

Examples
--------

- 예제 데이터

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - 2020-01-01
     - 45
     - 1
   * - 2020-01-02
     - 53
     - 1
   * - 2020-01-03
     - 23
     - 2
   * - 2020-01-04
     - 1
     - 2


- 예제1
.. code-block:: none

    * | lead B BY A

.. list-table::
   :header-rows: 1

   * - A
     - B
     - NEXT_ROW_OF_B
   * - 2020-01-01
     - 45
     - 53
   * - 2020-01-02
     - 53
     - 23
   * - 2020-01-03
     - 23
     - 1
   * - 2020-01-04
     - 1
     -

- 예제2
.. code-block:: none

    * | lead B BY A AS 결과컬럼

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - NEXT_ROW_OF_B
   * - 2020-01-01
     - 45
     - 1
     - 53
   * - 2020-01-02
     - 53
     - 1
     - 23
   * - 2020-01-03
     - 23
     - 2
     - 1
   * - 2020-01-04
     - 1
     - 2
     -


- 예제3
.. code-block:: none

    * | lead B BY A count 2

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - NEXT_ROW_OF_B
   * - 2020-01-01
     - 45
     - 1
     - 53
   * - 2020-01-02
     - 53
     - 1
     - 23
   * - 2020-01-03
     - 23
     - 2
     -
   * - 2020-01-04
     - 1
     - 2
     -


- 예제4
.. code-block:: none

    * | lead B BY A default 0

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - NEXT_ROW_OF_B
   * - 2020-01-01
     - 45
     - 1
     - 53
   * - 2020-01-02
     - 53
     - 1
     - 23
   * - 2020-01-03
     - 23
     - 2
     - 1
   * - 2020-01-04
     - 1
     - 2
     - 0



- 예제5
.. code-block:: none

    * | lead B BY A partition C

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - NEXT_ROW_OF_B
   * - 2020-01-01
     - 45
     - 1
     - 53
   * - 2020-01-02
     - 53
     - 1
     - 0
   * - 2020-01-03
     - 23
     - 2
     - 1
   * - 2020-01-04
     - 1
     - 2
     - 0