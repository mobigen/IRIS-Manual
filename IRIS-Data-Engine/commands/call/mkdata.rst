mkdata
^^^^^^^^^^^^^

json, list 형태로 사용자가 직접 데이터를 생성 합니다.


Use
"""""""""""""
- [O] Jaguar
- [O] Angora


Description
"""""""""""""

- 사용자가 직접 key/value, list 형태로 데이터를 생성하고 그 데이터를 분석에 사용할 수 있는 기능을 제공 합니다.


Caution
"""""""""""""

- key/value로 생성시 모든 레코드는 같은 컬럼 이름을 가지고 있어야 합니다.
- list로 생성되는 모든 레코드는 같은 레코드 개수를 가지고 있어야 합니다.
- 컬럼 타입이 지정되지 않을 경우 알맞은 타입으로 자동 지정 됩니다.

Parameters
"""""""""""""

.. code-block::

    mkdata [<key&value dictionary>(, <key&value dictionary>)*]

.. list-table::

    - * 이름
      * 설명
      * 필수/옵션
      * 기타
    - * key&value dictionary
      * - {‘key’: ‘value’, …} 과 같은 dictionary(json) 형태의 데이터
        - 각 데이터는 comma(,) 로 구분하여 작성
      * 필수
      * 모든 레코드에 같은 동일한 key가 포함되어 있어야 함

.. code-block::

    mkdata [<value_list>(, <value_list>)] [<column list>]

.. list-table::

    - * 이름
      * 설명
      * 필수/옵션
      * 기타
    - * value_list
      * - [‘value1’, ‘value2’, ‘value3’] 과 같은 list 형태의 데이터
        - 각 데이터는 comma(,) 로 구분하여 작성
      * 필수
      *
    - * column list
      * - [‘column_1’, ‘column_2’, ‘column_3’] 과 같은 list 형태의 데이터
        - 각 데이터는 comma(,) 로 구분하여 작성
      * 필수
      *

.. code-block::

    mkdata [<value_list>(, <value_list>)] [<column:type list>]

.. list-table::

    - * 이름
      * 설명
      * 필수/옵션
      * 기타
    - * value_list
      * - [‘value1’, ‘value2’, ‘value3’] 과 같은 list 형태의 데이터
        - 각 데이터는 comma(,) 로 구분하여 작성
      * 필수
      *
    - * column:type list
      * - [‘column_1’:’TYPE’, ‘column_2’:’TYPE’, ‘column_3’:’TYPE’] 과 같은 list 형태의 데이터
        - 각 데이터는 comma(,) 로 구분하여 작성
      * 필수
      * 사용가능한 타입은 TEXT, REAL, BIGINT, INTEGER, DATE, TIMESTAMP

- 타입 종류 설명

.. list-table::

    - * 타입
      * 설명
    - * TEXT
      * 문자열 데이터 타입
    - * REAL
      * 실수형 데이터 타입
    - * INTEGER
      * 4byte 정수형 데이터 타입
    - * BIGINT
      * 8byte 정수형 데이터 타입
    - * DATE
      * 년-월-일 데이터 타입
    - * TIMESTAMP
      * 년-월-일 시:분:초 데이터 타입

Example
"""""""""""""

- Key/Value 형태로 데이터를 생성하는 경우

.. code-block::

    mkdata
        [
            {'A': 12, 'B': 23, 'C': 'abc'},
            {'A': 11, 'B': 25, 'C': 'def'}
        ]

- List 형태로 데이터를 생성하는 경우

.. code-block::

    mkdata
        [
            [12, 23, 'abc'],
            [11, 25, 'def']
        ]
        ['A', 'B', 'C']

- List 형태로 데이터를 생성하고, 타입을 지정하는 경우

.. code-block::

    mkdata
        [
            [12, 23, 'abc'],
            [11, 25.1, 'def']
        ]
        ['A':INTEGER, 'B':REAL, 'C':TEXT]



