file
^^^^^^^^^^^^^

연결정보를 이용하여 File의 데이터를 가져옵니다.

Use
"""""""""""""
- [O] Jaguar
- [X] Angora


Description
"""""""""""""

- 연결정보의 Name/Owner 혹은 ID 값을 이용하여 데이터를 호출합니다.
    - 현재 웹을 통해 ID값을 확인하는 인터페이스는 따로 제공하고 있지 않습니다.

Caution
"""""""""""""

- 연결정보의 Name/Owner를 이용할 경우
    - 등록된 연결정보의 Name/Owner가 변경될 경우 연결 대상을 잃어 버릴수 있음
    - 단, 연결졍보를 삭제후 같은 Name/Owner가 생성할 경우 그대로 사용 가능
- 연결정보의 ID를 이용할 경우
    - 등록된 연결정보의 Name/Owner가 변경되더라도 문제 없이 사용 가능
    - 단, 연결정보를 삭제후 같은 Name/Owner가 생성하더라도 연결 대상을 잃어 버림

Parameters
"""""""""""""

.. code-block::

    file connector_name='{name}' connector_owner='{owner}' path='{file_path}'

.. code-block::

    file connector_id='{name}' path='{file_path}'

.. list-table::

    - * 이름
      * 설명
      * 필수/옵션
      * 기타
    - * connector_name
      * 연결정보의 이름
      * 옵션
      * connector_name을 사용할 경우 connector_owner를 같이 입력해 주어야 함
    - * connecotr_owner
      * 연결정보의 생성자
      * connector_name 사용시 필수
      *
    - * connector_id
      * 연결정보의 아이디
      * 옵션
      *
    - * path
      * 파일의 경로
      * 필수
      *

Example
"""""""""""""

- mariadb를 대상으로 연결정보의 Name/Owner 와 ID를 이용하여 데이터를 조회하는 예시

.. code-block::

    file
        connector_name = 'minio-test'
        connector_owner = 'root'
        path = '/corona19.csv'


.. code-block::

    file
        connector_id = '62c2bfb6-2acd-4b2d-a7e4-fb57f42f1f29'
        path = '/corona19.csv'

