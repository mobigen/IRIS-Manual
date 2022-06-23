join_mkdata
^^^^^^^^^^^^^

right dataframe 을 생성 하여 기존 데이터 와 join 작업 진행

Description
"""""""""""""

- 임시로 생성한 데이터를 합칠 경우 사용 합니다.
- jaguar를 통해 생성된 데이터를 기존 데이터와 특정 컬럼을 기준으로 합치는 작업을 진행 합니다.

Caution
"""""""""""""

- right에 놓이는 데이터의 경우 join후 컬럼의 이름은 **{alias_table_name}_{기존 컬럼 이름}** 형태를 사용합니다.
    - 따라서 leftdp 놓이는 테이블 컬럼 이름이 join후 생성되는 컬럼 이름과 겹치면 안됩니다.

Parameters
"""""""""""""

.. code-block::

    ...
    | join_mkdata
        alias_table_name='tt'
        join_type='inner'
        join_query='A = tt.A'
        mkdata='[[12, 23, 'abc'], [11, 25, 'def']] ['A', 'B', 'C']'


.. list-table::

    - * 이름
      * 설명
      * 필수/옵션
      * 기타
    - * connector_name
      * 연결정보의 이름
      * 필수
      * connector_name을 사용할 경우 connector_owner를 같이 입력해 주어야 함
    - * connector_owner
      * 연결정보의 생성자
      * 필수
      *
    - * path
      * right 데이터의 경로 및 파일명
      * 필수
      * (현) csv, json 지원
    - * alias_table_name
      * right 데이터의 임시 이름
      * 필수
      *
    - * join_type
      * 조인 형태
      * 필수
      * 종류: left, right, inner
    - * join_query
      * 데이터의 조인 조건
      * 필수
      * query의 on절에 해당



- mkdata 를 생성하는 방식
    - 기본 사용

    .. code-block::

        mkdata='[[12, 23, 'abc'], [11, 25, 'def']] ['A', 'B', 'C']'

    - 컬럼에 타입을 설정

    .. code-block::

        mkdata='[[12, 23.2, 'abc'], [11, 25.1, 'def']] ['A':INTEGER, 'B':REAL, 'C':TEXT]'

    - diction 을 사용

    .. code-block::

        mkdata='[{'A': 12, 'B': 23, 'C': 'abc'}, {'A': 11, 'B': 25, 'C': 'def'}]'



Example
"""""""""""""

.. code-block::

    mkdata [
            {"c1": "key2", "c2": "left-2", "c3": 2},
            {"c1": "key3", "c2": "left-3", "c3": 3}
        ]
    | join_mkdata
        alias_table_name = 'r'
        join_type = 'inner'
        join_query = 'c1 = r.c1'
        mkdata='[['key2', 23, 'abc'], [key3, 25, 'def']] ['c1', 'c2', 'c3']'


.. list-table::

    - * c1
      * c2
      * c3
      * r_c1
      * r_c2
      * r_c3
    - * key2
      * left-2
      * 2
      * key2
      * 23
      * abc
    - * key3
      * left-3
      * 3
      * key3
      * 25
      * def