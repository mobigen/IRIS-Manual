mkdata
==========

개요
----

DSL 에 작성한 데이터를 이용하여 DataFrame 을 만드는 명령어

타입
----------------------------------------------------------------------------------------------------
TEXT, INTEGER, REAL

설명
----

DSL 에 작성한 데이터를 이용하여 DataFrame 을 만드는 명령어
input 으로 들어온 DataFrame 은 무시한다. ( `... | mkdata args` 을 사용하면 `...` 부분의 데이터는 사용하지 않는다.)

Parameters
-----------

.. code-block:: python

   * | mkdata [<key&value dictionary>(, <key&value dictionary>)*]
   * | mkdata [<value_list>(, <value_list>)] [<column list>]

.. list-table::
   :header-rows: 1
   
   * - 이름
     - 설명
     - 필수/옵션
     - 기본값
   * - key&value dictionary
     - `{'key': 'value', ...}` 과 같은 dictionary(json) 형태의 데이터 |br| 각 데이터는 comma(,) 로 구분하여 작성
     - 필수
     -
   * - value_list
     - `['value1', 'value2', 'value3']` 과 같은 list 형태의 데이터 |br| 각 데이터는 comma(,) 로 구분하여 작성
     - 옵션
     -
   * - column list
     - `['column_1', 'column_2', 'column_3']` 과 같은 list 형태의 데이터 |br| type 을 지정하고 싶은 경우 `컬럼명 : 타입` 형식으로 작성하면 된다. |br| 각 데이터는 comma(,) 로 구분하여 작성
     - 옵션
     -

- 타입 종류

.. list-table::
   :header-rows: 1

   * - 타입
     - 설명
   * - TEXT
     - 문자열 데이터 타입
   * - REAL
     - 실수(floating) 데이터 타입
   * - BIGINT
     - long 데이터 타입
   * - INTEGER
     - int 데이터 타입
   * - DATE
     - `년-월-일` 데이터 타입
   * - TIMESTAMP
     - `년-월-일 시:분:초` 데이터 타입


Examples
--------

- 예제1

.. code-block:: python

    * | mkdata [{'A': 12, 'B': 23, 'C': 'abc'}, {'A': 11, 'B': 25, 'C': 'def'}]

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - 12
     - 23
     - abc
   * - 11
     - 25
     - def

- 예제2

.. code-block:: python

    * | mkdata [[12, 23, 'abc'], [11, 25, 'def']] ['A', 'B', 'C']

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - 12
     - 23
     - abc
   * - 11
     - 25
     - def

- 예제3

.. code-block:: python

    * | mkdata [[12, 23, 'abc'], [11, 25.1, 'def']] ['A':INTEGER, 'B':REAL, 'C':TEXT]

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - 12
     - 23.0
     - abc
   * - 11
     - 25.1
     - def


.. |br| raw:: html

  <br/>
