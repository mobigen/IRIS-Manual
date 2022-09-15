job
======================

개요
-------------------------

현재 spark 를 통해 작동하고 있는 작업과 관련하여 명령어로 접근이 가능해진다.

컬럼 타입
----------------------------------------------------------------------------------------------------


설명
--------------------------

pool : 현재 존재하고 있는 풀의 확인과 작업 풀을 설정할 수 있다.

kill : 현재 작동하고 있는 job들을 멈출때 사용할 수 있다.

list : 현재 작동하고 있는 job들을 확인할 수 있다.

Parameters
------------------------------------

.. code-block:: python

   ... | job OPTIONS (ARGUMENTS*)?
   ... | job pool (ARGUMENTS)+
   ... | job kill (ARGUMENTS)+
   ... | job list (ARGUMENTS*)?

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - pool
     - 사용하고 싶은 pool을 선택하거나 확인할 수 있다.
     - 필수
   * - kill
     - 현재 작동하고 있는 job을 중단할 수 있다.
     - 필수
   * - list
     - 현재까지 작동 되어진 모든 job들을 간략하게 확인할 수 있다.
     - 필수
   * - ARGUMENTS
     - 각 필수 옵션들의 추가 명령어이다.
     - 옵션

- pool arguments

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - list
     - 현재 유지하고 있는 pool을 확인할 수 있다.
     - 옵션
   * - set
     - 사용하고 싶은 pool을 설정할 수 있다.
     - 옵션

- kill arguments

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - id
     - 어떤 작업을 멈출지 선택할 수 있다.
     - 필수


- list arguments

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - id
     - 어떤 작업의 상태를 확인할 수 있다.
     - 옵션
   * - detail
     - 작업의 상태를 좀더 detail하게 확인할 수 있다.
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


- 현재 사용 되고 있는 pool을 확인 하는 예제 입니다.

.. code-block:: python

   ... | job pool list

.. list-table::
   :header-rows: 1

   * - pool_list
   * - default
   * - fifo
   * - fair

- 사용할 pool을 설정하는 예제 입니다.

.. code-block:: python

   ... | job pool set = fifo

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - 일반
     - 1
     - 10.123

- 현재 작업하고 있는 job을 멈추는 예제입니다.

.. code-block:: python

   ... | job kill id = 123:5000

.. list-table:: none
   :header-rows: 1

   * - job_kill_status
   * - Success

- memcached에 저장된 키를 불러오는 예제 입니다.

.. code-block:: python

   ... | job list

.. list-table::
   :header-rows: 1

   * - ID
     - status
     - description
     - schedulingPool
     - duration(second)
   * - 1617330707.8633,0.0.0.0:6036
     - SUCCEEDED
     - job kill id = 1617330702.8608,0.0.0.0:6036
     - default
     - 0.06599998474121094
   * - 1617330702.8608,0.0.0.0:6036
     - FAILED
     - "model name = 'ml_house' model_owner = angora \|job pool set=fair\| fit RandomForestRegression FEATURES crim, age, tax LABEL medv maxDepth=3 retrain=True INTO TestModelRandomForestRegression1"
     - fair
     - 0.6180000305175781

.. code-block:: python

   ... | job list id = 1617330702.8608,0.0.0.0:6036

.. list-table::
   :header-rows: 1

   * - ID
     - status
     - description
     - schedulingPool
     - duration(second)
   * - 1617330702.8608,0.0.0.0:6036
     - FAILED
     - "model name = 'ml_house' model_owner = angora \|job pool set=fair\| fit RandomForestRegression FEATURES crim, age, tax LABEL medv maxDepth=3 retrain=True INTO TestModelRandomForestRegression1"
     - fair
     - 0.6180000305175781

.. code-block:: python

   ... | job list detail id = 1617330707.8633,0.0.0.0:6036

.. list-table::
   :header-rows: 1

   * - ID
     - status
     - description
     - submissionTime
     - completionTime
     - schedulingPool
     - name
     - stageIds
     - numTasks
     - duration(second)
   * - 1617330707.8633,0.0.0.0:6036
     - SUCCEEDED
     - 2021-04-02T02:31:48.044GMT
     - 2021-04-02T02:31:48.110GMT
     - job kill id = 1617330702.8608,0.0.0.0:6036
     - default
     - take at /home/ubuntu1/IRIS-Discovery-Service/src/main/python/angora/core/engine.py:396
     - [4]
     - 1
     - 0.06599998474121094
