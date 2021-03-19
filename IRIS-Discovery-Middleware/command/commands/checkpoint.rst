checkpoint
======================

개요
-------------------------

쿼리의 결과를 일정시간동안 저장해 놓을수 있는 명령어

설명
--------------------------

set : 쿼리의 결과가 저장되고 저장된 값을 불러올수 있는 key 를 얻을수 있다.

get : 반환된 key 를 이용하여 쿼리의 결과를 얻을수 있다.

delete : 현재 가지고 있는 key 를 삭제할 수 있다.

list : 현재 가지고 있는 key 를 보여준다.

Parameters
------------------------------------

.. code-block:: python

   ... | checkpoint set (EXPIRE)?
   ... | checkpoint get KEY
   ... | checkpoint delete KEY
   ... | checkpoint list

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - set
     - 키의 값을 설정할 때 사용한다.
     - 옵션
   * - get
     - memcached 에 존재하는 키의 값으로 objectstorage 에 저장해 놓은 df의 결과를 불러올때 사용한다.
     - 옵션
   * - delete
     - memcached 에 존재하는 키를 지울때 사용한다.
     - 옵션
   * - list
     - 현재 memcached 에 존재하는 키의 리스트를 불러온다.
     - 옵션
   * - KEY
     - 저장된 key 를 지정할 수 있다.
     - 필수
   * - EXPIRE
     - expire 할 시간을 지정할 수 있다.(단위: second)
        expire 를 지정하지 않으면 default로 600초가 주어진다.
         - ex:) expire=600
     - 옵션



Example
----------------------------------

- 예제용 데이터 모양 입니다.

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - 일반
     - 1
     - 10.123
   * - 특수
     - 2
     - 20.123
   * - 일반
     - 3
     - 30.123
   * - 특수
     - 4
     - 40.123


- 예제 데이터를 key 를 랜덤하게 저장하고 expire 시간을 주는 예제 입니다.

.. code-block:: python

   ... | checkpoint set expire=3000

.. list-table::
   :header-rows: 1

   * - KEY
   * - ad1042b5-f73d-46ta-8zae-fdb2220ee10e

- check_point_key 로 memcached에 저장된 키의 결과값을 불러오는 예제 입니다.

.. code-block:: python

   ... | checkpoint get key=ad1042b5-f73d-46ta-8zae-fdb2220ee10e

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - 일반
     - 1
     - 10.123
   * - 특수
     - 2
     - 20.123
   * - 일반
     - 3
     - 30.123
   * - 특수
     - 4
     - 40.123

- check_point_key 로 memcached에 저장된 키를 삭제하는 예제 입니다.

.. code-block:: python

   ... | checkpoint delete key=ad1042b5-f73d-46ta-8zae-fdb2220ee10e

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - 일반
     - 1
     - 10.123
   * - 특수
     - 2
     - 20.123
   * - 일반
     - 3
     - 30.123
   * - 특수
     - 4
     - 40.123

- memcached에 저장된 키를 불러오는 예제 입니다.

.. code-block:: python

   ... | checkpoint list

.. list-table::
   :header-rows: 1

   * - KEY
   * - ad1042b5-f73d-46ta-8zae-fdb2220ee10e
   * - d10aa6b5-f94a-4622-8196-c9310f1cc4ea
