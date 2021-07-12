lag
=========

개요
----

현재 행 이전의 행의 값을 가져오는 명령어

설명
----

현재 행 이전의 행의 값을 가져오는 명령어

현재 데이터와 이전 데이터의 값을 비교 할 때 사용되면 유용한 명령어

예를들어, 선택한 컬럼의 데이터가 1,2,3,4 순서로 있을 때, 해당 데이터의 이전값 즉 null,1,2,3 의 값을 가져오는 동작을 실행

Parameters
---------

.. code-block:: none

    * | lag {column:str} BY {orderby:str} (COUNT {count:int}) (DEFAULT {default:str}) (PARTITION {partition:str}) (AS {alias:str})?

.. list-table::
   :header-rows: 1

   * - 이름
     - Type
     - 설명
     - 필수
   * - column
     - string
     - 해당 컬럼의 값의 이전행을 가져오기 위한 컬럼
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
     - 결과 데이터를 저장할 컬럼명, (Default: PREVIOUS_ROW_OF_값컬럼명)
     - 옵션
Examples
--------

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

    * | lag B BY A

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - PREVIOUS_ROW_OF_B
   * - 2020-01-01
     - 45
     - 1
     - 
   * - 2020-01-02
     - 53
     - 1
     - 45
   * - 2020-01-03
     - 23
     - 2
     - 53
   * - 2020-01-04
     - 1
     - 2
     - 23

- 예제2
.. code-block:: none

    * | lag B BY A AS 결과컬럼

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - PREVIOUS_ROW_OF_B
   * - 2020-01-01
     - 45
     - 1
     - 
   * - 2020-01-02
     - 53
     - 1
     - 45
   * - 2020-01-03
     - 23
     - 2
     - 53
   * - 2020-01-04
     - 1
     - 2
     - 23

- 예제3
.. code-block:: none

    * | lag B BY A COUNT 2

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - PREVIOUS_ROW_OF_B
   * - 2020-01-01
     - 45
     - 1
     -
   * - 2020-01-02
     - 53
     - 1
     -
   * - 2020-01-03
     - 23
     - 2
     - 45
   * - 2020-01-04
     - 1
     - 2
     - 53


- 예제4
.. code-block:: none

    * | lag B BY A DEFAULT 0

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - PREVIOUS_ROW_OF_B
   * - 2020-01-01
     - 45
     - 1
     - 0
   * - 2020-01-02
     - 53
     - 1
     - 45
   * - 2020-01-03
     - 23
     - 2
     - 53
   * - 2020-01-04
     - 1
     - 2
     - 23


- 예제5
.. code-block:: none

    * | lag B BY A PARTITION C

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - PREVIOUS_ROW_OF_B
   * - 2020-01-01
     - 45
     - 1
     -
   * - 2020-01-02
     - 53
     - 1
     - 45
   * - 2020-01-03
     - 23
     - 2
     -
   * - 2020-01-04
     - 1
     - 2
     - 23