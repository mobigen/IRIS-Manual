checkpoint
======================

개요
-------------------------

쿼리의 결과를 일정시간동안 저장해 놓을수 있는 명령어

설명
--------------------------

쿼리의 결과로 나온 데이터를 Studio 에서 여러 번 사용할 때 유용한 명령어입니다.

쿼리의 결과는 IRIS 내의 objectstorage 에 저장되고, 할당받은 key 로 저장된 결과를 불러올 수 있습니다.
이 key 를 통해 expire 옵션으로 설정한 시간동안 쿼리 결과를 계속 불러올 수 있습니다.

- set
    - 쿼리의 결과가 저장되고 저장된 값을 불러올수 있는 key 를 얻을수 있습니다. 
    - 설정한 expire 시간동안만 key 가 유효합니다. expire를 지정하지 않으면 600초가 자동지정됩니다.
    - 예 1) ``checkpoint set``  은 600초 동안 쿼리 결과가 저장되고 할당받은 key 가 유효합니다.
    - 예 2) ``checkpoint set expire=1000`` 은 1000 초 동안 쿼리 결과가 저장되고 할당받은 key 가 유효합니다.  
- get : 할당받은 key 를 이용하여 일정시간동안(EXPIRE 설정시간) 쿼리의 결과를 얻을수 있습니다.
- delete : 현재 가지고 있는 key 와 쿼리 결과를 함께 삭제할 수 있습니다.
- list : 현재 memcached 에 저장된 key list 를 보여줍니다.


Parameters
------------------------------------

.. code-block:: python

   ... | checkpoint set
   ... | checkpoint set expire=...
   ... | checkpoint get key=...
   ... | checkpoint delete key=...
   ... | checkpoint list


.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - set
     - 쿼리 결과를 저장하고, 결과를 불러올 수 있는 key 를 할당받습니다.
     - 옵션
   * - get
     - memcached 에 존재하는 키의 값으로 objectstorage 에 저장해 놓은 쿼리 결과를 불러올때 사용합니다.
     - 옵션
   * - delete
     - memcached 에 존재하는 키를 지울때 사용합니다.
     - 옵션
   * - list
     - 현재 memcached 에 존재하는 키의 리스트를 불러옵니다.
     - 옵션
   * - expire
     - expire 할 시간을 지정할 수 있습니다.(단위: second)
        expire 를 지정하지 않으면 default  600초.
        600 <= expire 시간(초) < 2592000
         - ex:) expire=600
     - 옵션



Example
----------------------------------

- 예제용 데이터는 어떤 쿼리의 결과입니다.

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


- ``checkpoint set`` 을 이용하여 쿼리 결과 데이터를 3000 초 동안 저장하고, 불러올 수 있는 key 를 얻을 수 있습니다.

.. code-block:: python

   ... | checkpoint set expire=3000


.. list-table::
   :header-rows: 1

   * - KEY
   * - ad1042b5-f73d-46ta-8zae-fdb2220ee10e


- ``checkpoint get`` 을 이용하여 할당받은 key 에 해당하는 쿼리 결과를 가져옵니다.

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


- ``checkpoint delete`` 를 이용하여 키를 삭제합니다. delete 의 실행결과는 해당 key와 같이 삭제되는 쿼리 결과를 출력합니다.

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

- ``checkpoint list``  는 저장되어 있는 모든 key를 보여줍니다. 다른 사용자가 할당받은 key 를 포함하여 memcached 에 저장된 key가 다 출력됩니다.

.. code-block:: python

   ... | checkpoint list

.. list-table::
   :header-rows: 1

   * - KEY
   * - ad1042b5-f73d-46ta-8zae-fdb2220ee10e
   * - d10aa6b5-f94a-4622-8196-c9310f1cc4ea
