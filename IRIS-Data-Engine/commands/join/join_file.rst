join_file
^^^^^^^^^^^^^

file에서 검색된 데이터를 기존 데이터와 join작업 진행

Description
"""""""""""""

- 연결정보와 file 경로를 이용하여 특정 file 의 데이터를 합칠경우 사용합니다.
- jaguar를 통해 검색된 데이터를 기존 데이터와 특정 컬럼을 기준으로 합치는 작업을 진행 합니다.

Caution
"""""""""""""

- right에 놓이는 데이터의 경우 join후 컬럼의 이름은 **{alias_table_name}_{기존 컬럼 이름}** 형태를 사용합니다.
    - 따라서 leftdp 놓이는 테이블 컬럼 이름이 join후 생성되는 컬럼 이름과 겹치면 안됩니다.

Parameters
"""""""""""""

- 연결정보의 Name/Owner를 이용하는 방식

.. code-block::

    ...
    | join_file
        connector_name = '{name}'
        connector_owner = '{owner}'
        path = '{path}'
        alias_table_name = '{alias_table_name}'
        join_type = '{join_type}'
        join_query = '{join_query}'

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


- 연결정보의 ID를 이용하는 방식

.. code-block::

    ...
    | join_file
        connector_id = '{id}'
        path = '{path}'
        alias_table_name = '{alias_table_name}'
        join_type = '{join_type}'
        join_query = '{join_query}'

.. list-table::

    - * 이름
      * 설명
      * 필수/옵션
      * 기타
    - * connector_id
      * 연결정보의 id
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


Example
"""""""""""""

- 연결정보의 Name/Owner를 이용하는 방식

.. code-block::

    mkdata [
            {"c1": "key2", "c2": "left-2", "c3": 2},
            {"c1": "key3", "c2": "left-3", "c3": 3}
        ]
    | join_file
        connector_owner = 'root'
        connector_name = 'file-test'
        alias_table_name = 'r'
        join_type = 'inner'
        join_query = 'c1 = r.c1'

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
      * right-2
      * 2

- 연결정보의 ID를 이용하는 방식

.. code-block::

    mkdata [
            {"c1": "key2", "c2": "left-2", "c3": 2},
            {"c1": "key3", "c2": "left-3", "c3": 3}
        ]
    | join_file
        connector_id = 'id'
        alias_table_name = 'r'
        join_type = 'inner'
        join_query = 'c1 = r.c1'

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
      * right-2
      * 2