fillna
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

이 명령어는 특정한 필드의 결측치 처리에 사용합니다.

타입
----------------------------------------------------------------------------------------------------
TEXT, INTEGER, BIGINT, REAL, DATE, TIMESTAMP

설명
----------------------------------------------------------------------------------------------------

* 해당 필드의 결측치를 다른 값으로 채우거나 결측치가 존재하는 행(ROW)을 제거하고자 할 때 사용합니다.
* 결측치를 채우는 경우 대상 필드와 대체할 값의 유형이 일치해야합니다(예: NUMBER TYPE 필드의 경우 숫자 값, STRING TYPE 필드의 경우 문자열 값).
* 유형이 일치하지 않으면 해당 명령은 **무시**\ 됩니다.
* value값에 띄어쓰기가 들어간 문자열을 사용하고 싶을 시 `' '` 을 이용합니다.
* TEXT column 에서만 사용되는 ' '(empty string) 과 ``null`` 은 다른 값 입니다.

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | fillna FIELD (VALUE)?
   ... | fillna FIELD (VALUE)?, (FIELD (VALUE)?)*

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - ``FIELD``
     - 대상인 필드를 의미합니다.
     - 필수
   * - ``VALUE``
     - 결측치를 대체하는 값입니다. 생략하면 결측치가 있는 행을 제거합니다. bool 값을 줄 수 없습니다.
     - 옵션

Example
----------

- 예제용 데이터 모양 입니다.

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - ClassA
     - a
     - 90
     - 10
   * - ClassB
     - ``null``
     - 80
     - 20
   * - ClassB
     - ``null``
     - 70
     - 30
   * - ClassC
     - d
     - ``null``
     - 40
   * - ``null``
     - e
     - 50
     - 100
   * - ClassC
     - f
     - 40
     - ``null``

- D 필드의 결측치를 10으로 대체하는 예제입니다.

.. code-block:: none

   ... | fillna D 10

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - ClassA
     - a
     - 90
     - 10
   * - ClassB
     - ``null``
     - 80
     - 20
   * - ClassB
     - ``null``
     - 70
     - 30
   * - ClassC
     - d
     - ``null``
     - 40
   * - ``null``
     - e
     - 50
     - 100
   * - ClassC
     - f
     - 40
     - 10

- B 필드의 결측치를 test data로 대체하는 예제입니다.

.. code-block:: none

   ... | fillna B 'test data'

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - ClassA
     - a
     - 90
     - 10
   * - ClassB
     - test data
     - 80
     - 20
   * - ClassB
     - test data
     - 70
     - 30
   * - ClassC
     - d
     - ``null``
     - 40
   * - ``null``
     - e
     - 50
     - 100
   * - ClassC
     - f
     - 40
     - ``null``

- A, B, C 필드 중 결측치가 있는 행을 제거하는 예제입니다.

.. code-block:: none

   ... | fillna A, B, C

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - ClassA
     - a
     - 90
     - 10
   * - ClassC
     - f
     - 40
     - ``null``

- B, D 필드의 결측치를 test data와 120으로 대체하는 예제입니다.

.. code-block:: none

   ... | fillna B 'test data', D 120

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - ClassA
     - a
     - 90
     - 10
   * - ClassB
     - test data
     - 80
     - 20
   * - ClassB
     - test data
     - 70
     - 30
   * - ClassC
     - d
     - ``null``
     - 40
   * - ``null``
     - e
     - 50
     - 100
   * - ClassC
     - f
     - 40
     - 120
