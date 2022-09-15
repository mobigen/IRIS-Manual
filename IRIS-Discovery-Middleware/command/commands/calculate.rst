calculate
=========

개요
----

행 or 열 간의 간단한 수식 계산을 합니다.

컬럼 타입
----------------------------------------------------------------------------------------------------
INTEGER, BIGINT, REAL

설명
----

calculate 명령어를 사용하면, 간단한 사칙연산이나 행/열 간 계산을 진행 할 수 있습니다.

사용 방법은 2가지로 구분되어 있습니다.

첫번째, 계산기에서 사용하는 ``+``, ``-``, ``*``, ``/`` 문자를 사용하여 ``열`` 간 계산을 할 수 있습니다. 현재 지원하는 문자는 [ ``+``, ``-``, ``*``, ``/``, ``(``, ``)`` ] 입니다.
두번째, ``add``, ``avg`` 와 같은 함수를 사용해 ``행/열`` 간 계산을 할 수 있습니다. 현재 지원하는 함수는 [ ``add``, ``avg`` ] 입니다.

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | calculate (OPERATION DIRECTION (field_A(, field_B)?)? | arithmetic operation) (AS ALIAS_NAME)? (FLAG)?

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - OPERATION
     - Aggregation 함수가 들어가는 곳으로, add, avg 와 같은 약어로 들어갑니다. (현재, ``add``, ``avg`` 만 지원됩니다.)
     - 필수
   * - DIRECTION
     - 연산의 방향을 정하는 부분입니다. ``ALL``, ``ROW``, ``COL`` 이 있습니다.
     - 필수
   * - field_A ...
     - 원하는 컬럼을 지정하여 해당 컬럼에 대한 값을 도출 할 때 사용합니다.
     - 옵션
   * - arithmetic operation
     - 특수 문자를 이용한 연산식 을 사용하는 부분입니다. 현재 (``+``, ``-``, ``*``, ``/``, ``(``, ``)``) 문자를 인식 할 수 있습니다.
     - 필수
   * - AS ALIAS_NAME
     - ``AS`` 문자를 이용해 substr 결과를 나타낼 필드의 이름을 지정합니다. space가 포함된 문자는 single-quote(``'``)로 감싸야 합니다. (Default = calculated)
     - 옵션
   * - FLAG
     - 계산 결과 컬럼의 위치를 지정하는 옵션입니다. ``LEFT``, ``RIGHT`` 가 있으며, 각각 ``1번컬럼``, ``마지막컬럼`` 을 의미합니다. Default: RIGHT
     - 옵션

Examples
----------------------------------------------------------------------------------------------------

- 예제용 데이터 모양 입니다.

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - 1
     - 2
     - 3
     - 4
   * - 5
     - 6
     - 7
     - 8
   * - 9
     - 10
     - 11
     - 12

1. 사칙연산 예제입니다. (열 과 열 사이만 계산이 가능합니다.)

1-1. 예제 1

.. code-block:: none

   ... | calculate A * B + C - D

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
     - calculated
   * - 1
     - 2
     - 3
     - 4
     - 1
   * - 5
     - 6
     - 7
     - 8
     - 29
   * - 9
     - 10
     - 11
     - 12
     - 89

1-2. 예제 2

.. code-block:: none

   ... | calculate A * (B + C) - D

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
     - calculated
   * - 1
     - 2
     - 3
     - 4
     - 1
   * - 5
     - 6
     - 7
     - 8
     - 57
   * - 9
     - 10
     - 11
     - 12
     - 177

1-3. 예제 3

.. code-block:: none

   ... | calculate A + null

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
     - calculated
   * - 1
     - 2
     - 3
     - 4
     - 1
   * - 5
     - 6
     - 7
     - 8
     - 5
   * - 9
     - 10
     - 11
     - 12
     - 9


2. 연산자 함수를 사용해 계산하는 예제입니다. 행/열 간의 계산이 가능하며, Numeric 컬럼만 지원합니다. ( 현재, ``ADD``, ``AVG`` 함수만 지원합니다.)

2-1. 예제 1. 행, 열 모두 계산합니다.

.. code-block:: none

   ... | calculate add all

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
     - calculated
   * - 1
     - 2
     - 3
     - 4
     - 10
   * - 5
     - 6
     - 7
     - 8
     - 26
   * - 9
     - 10
     - 11
     - 12
     - 42
   * - 15
     - 18
     - 21
     - 24
     - 78

2-2. 예제 2. 각 열의 합계를 계산합니다.

.. code-block:: none

   ... | calculate add col

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - 1
     - 2
     - 3
     - 4
   * - 5
     - 6
     - 7
     - 8
   * - 9
     - 10
     - 11
     - 12
   * - 15
     - 18
     - 21
     - 24

2-3. 예제 3. 각 행의 합계를 계산합니다.

.. code-block:: none

   ... | calculate add row

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
     - calculated
   * - 1
     - 2
     - 3
     - 4
     - 10
   * - 5
     - 6
     - 7
     - 8
     - 26
   * - 9
     - 10
     - 11
     - 12
     - 42

2-4. 예제 4. 선택한 행, 열간의 계산을 합니다.

.. code-block:: none

   ... | calculate add all A B

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
     - calculated
   * - 1
     - 2
     - 3
     - 4
     - 3
   * - 5
     - 6
     - 7
     - 8
     - 11
   * - 9
     - 10
     - 11
     - 12
     - 19
   * - 15
     - 18
     - 
     - 
     - 33
