
join
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

이 명령어는 다른 데이터 모델과 join을 할 때 사용됩니다.

타입
----------------------------------------------------------------------------------------------------
TEXT, INTEGER, BIGINT, REAL, DATE, TIMESTAMP

설명
----------------------------------------------------------------------------------------------------

이 명령어는 다른 데이터 모델과 join을 할 때 사용됩니다.

여러 조인 타입을 지정 할 수 있으며, 조인 조건을 입력하여야 매끄러운 동작이 가능합니다. 

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | join (JOIN_TYPE)? MODEL_NAME (AS ALIAS)? (ON)?

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - JOIN_TYPE
     - 조인 타입을 지정합니다. 기본값은 ``CROSS`` 조인 입니다.
     - 옵션
   * - MODEL_NAME
     - 조인을 할 데이터모델의 이름을 의미합니다. **모델명에 공백이 포함되는 경우 ``' '``\ (Single quote)로 감싸줘야 합니다.**
     - 필수
   * - AS ALIAS
     - 조인을 할 데이터모델의 이름을 치환합니다.
     - 옵션
   * - ON
     - 조인 조건을 의미합니다. SQL JOIN QUERY 의 ON 절과 유사합니다. ``CROSS`` 조인의 경우 생략할 수 있습니다.
     - 옵션

Join Type
''''''''''

.. list-table::
   :header-rows: 1

   * - 조인 타입
     - 설명
   * - INNER
     - 조인 조건에 일치하는 데이터만 리턴
   * - CROSS
     - Cartesian Product 조인
   * - OUTER
     - 왼쪽 or 오른쪽 데이터에 매치되는 모든 데이터 리턴
   * - FULL
     - outer 조인
   * - FULL_OUTER
     - outer 조인
   * - LEFT
     - 왼쪽 데이터를 기준으로 조인, 오른쪽 데이터에 매치되는 것이 없으면 NULL 로 대체
   * - LEFT_OUTER
     - 왼쪽 데이터를 기준으로 조인, 오른쪽 데이터에 매치되는 것이 없으면 NULL 로 대체
   * - RIGHT
     - 오른쪽 데이터를 기준으로 조인, 왼쪽 데이터에 매치되는 것이 없으면 NULL 로 대체
   * - RIGHT_OUTER
     - 오른쪽 데이터를 기준으로 조인, 왼쪽 데이터에 매치되는 것이 없으면 NULL 로 대체
   * - LEFT_SEMI
     - 왼쪽 데이터를 기준으로 매치 가능 한 데이터 출력
   * - LEFT_ANTI
     - 왼쪽 데이터를 기준으로 매치 되지 않은 데이터 출력

Examples
----------------------------------------------------------------------------------------------------
- 현재 데이터 모델(left_model)

.. list-table::
   :header-rows: 1

   * - A
     - B
   * - 1
     - A1
   * - 2
     - A2
   * - 3
     - A3

- 다른 데이터 모델(right_model)

.. list-table::
   :header-rows: 1

   * - A
     - B
   * - 3
     - A3
   * - 5
     - A5
   * - 6
     - A6

- ``CROSS`` 조인(조건 없음)

.. code-block:: none

   ... | join right_model

.. list-table::
   :header-rows: 1

   * - A
     - B
     - right_model_A
     - right_model_B
   * - 1
     - A1
     - 3
     - A3
   * - 1
     - A1
     - 5
     - A5
   * - 1
     - A1
     - 6
     - A6
   * - 2
     - A2
     - 3
     - A3
   * - 2
     - A2
     - 5
     - A5
   * - 2
     - A2
     - 6
     - A6
   * - 3
     - A3
     - 3
     - A3
   * - 3
     - A3
     - 5
     - A5
   * - 3
     - A3
     - 6
     - A6

- ``INNER`` 조인(동등 조인)

.. code-block:: none

   ... | join inner right_model A = right_model.A
   ... | join inner right_model left_model.A = right_model.A

.. list-table::
   :header-rows: 1

   * - A
     - B
     - right_model_A
     - right_model_B
   * - 3
     - A3
     - 3
     - A3

- 데이터모델에 공백이 있는 경우( ``modelA`` 와 ``space test`` 데이터모델)

.. code-block:: none

   ... | join inner 'space test' ID = space test.ID

- 데이터 모델명 치환 ( ``AS`` 문법 사용, 현 데이터 모델은 치환불가)

.. code-block:: none

   ... | join inner 'space test' as B ID = B.ID
