merge
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

이 명령어는 다른 데이터 모델과 join을 할 때 사용됩니다.

컬럼 타입
----------------------------------------------------------------------------------------------------
TEXT, INTEGER, BIGINT, REAL, DATE, TIMESTAMP

설명
----------------------------------------------------------------------------------------------------

이 명령어는 다른 데이터 모델과 join을 할 때 사용됩니다.

여러 조인 타입을 지정 할 수 있으며, 조인 조건을 입력하여야 매끄러운 동작이 가능합니다. 

후속 데이터 모델의 시점을 설정하여 조인할 수 있습니다.

후속 데이터 모델에 원하는 컬럼명의 값을 추출하여 조인할 수 있습니다.

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | merge (MERGE_TYPE)? (MODEL_NAME (start_date)? (end_date)? (EXPR)*) (AS ALIAS)? (ON)*

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - MERGE_TYPE
     - 조인 타입을 지정합니다. 기본값은 ``CROSS`` 조인 입니다.
     - 옵션
   * - start_date
     - 시작 시점을 지정합니다. 
     - 옵션
   * - end_date
     - 종료 시점을 지정합니다.
     - 옵션
   * - MODEL_NAME
     - 조인을 할 데이터모델의 이름을 의미합니다. 모델명에 공백이 포함되는 경우 ``` ``` (Back quote)로 감싸줘야 합니다.
     - 필수
   * - EXPR
     - 후속 데이터 모델에 원하는 컬럼명의 값을 한정할 수 있는 조건을 사용할 수 있습니다.
     - 옵션
   * - AS ALIAS
     - 조인을 할 데이터모델의 이름을 치환합니다.
     - 옵션
   * - ON
     - 조인 조건을 의미합니다. SQL JOIN QUERY 의 ON 절과 유사합니다. ``CROSS`` 조인의 경우 생략할 수 있습니다. ``AND`` 를 이용하여 여러 조건을 사용할 수 있습니다.
     - 옵션

Merge Type
''''''''''

.. list-table::
   :header-rows: 1

   * - 조인 타입
     - 설명
   * - INNER
     - 조인 조건에 일치하는 데이터만 리턴
   * - CROSS
     - Cartesian Product 조인
   * - OUTER
     - 왼쪽 or 오른쪽 데이터에 매치되는 모든 데이터 리턴
   * - FULL
     - outer 조인
   * - FULL_OUTER
     - outer 조인
   * - LEFT
     - 왼쪽 데이터를 기준으로 조인, 오른쪽 데이터에 매치되는 것이 없으면 NULL 로 대체
   * - LEFT_OUTER
     - 왼쪽 데이터를 기준으로 조인, 오른쪽 데이터에 매치되는 것이 없으면 NULL 로 대체
   * - RIGHT
     - 오른쪽 데이터를 기준으로 조인, 왼쪽 데이터에 매치되는 것이 없으면 NULL 로 대체
   * - RIGHT_OUTER
     - 오른쪽 데이터를 기준으로 조인, 왼쪽 데이터에 매치되는 것이 없으면 NULL 로 대체
   * - LEFT_SEMI
     - 왼쪽 데이터를 기준으로 매치 가능 한 데이터 출력
   * - LEFT_ANTI
     - 왼쪽 데이터를 기준으로 매치 되지 않은 데이터 출력

Examples
----------------------------------------------------------------------------------------------------
- 현재 데이터 모델(z_L)

.. list-table::
   :header-rows: 1

   * - A
     - B
   * - 1
     - a
   * - 2
     - b
   * - 3
     - c

- 다른 데이터 모델(z_R)

.. list-table::
   :header-rows: 1

   * - A
     - B
   * - 1
     - f
   * - 4
     - g
   * - 5
     - f
   * - 2
     - g
   * - 5
     - f
   * - 3
     - z
   * - 4
     - h
   * - 1
     - i
   * - 3
     - f
   * - 5
     - f
   * - 5
     - c
   * - 1
     - c
   * - 12
     - c
   * - 2
     - c
   * - 3
     - c              

- ``CROSS`` 조인(조건 없음)

.. code-block:: none

   ... | merge (z_R end_date=20200908101210 A=1 B='f')

.. list-table::
   :header-rows: 1

   * - A
     - B
     - z_R_A
     - z_R_B
   * - 1
     - a
     - 1
     - f        
   * - 2
     - b
     - 1
     - f
   * - 3
     - c
     - 1
     - f  

.. code-block:: none

   ... | merge (z_R start_date=20200907101210 end_date=20200908101210 A=1 B='f')

.. list-table::
   :header-rows: 1

   * - A
     - B
     - z_R_A
     - z_R_B
   * - 1
     - a
     - 1
     - f        
   * - 2
     - b
     - 1
     - f
   * - 3
     - c
     - 1
     - f      
                                              

.. code-block:: none

   ... | merge (z_R start_date=20200907101210 end_date=20200908101210 B='f') AS Q on A = Q.A

.. list-table::
   :header-rows: 1

   * - A
     - B
     - Q_A
     - Q_B
   * - 1
     - a
     - 1
     - f
   * - 3
     - c
     - 3
     - f

- ``INNER`` 조인(동등 조인)

.. code-block:: none

   ... | merge inner (z_R start_date=20200907101210 end_date=20200908101210) as IN on A = IN.A
   ... | merge inner (z_R start_date=20200907101210 end_date=20200908101210) as IN on z_L.A = IN.A

.. list-table::
   :header-rows: 1

   * - A
     - B
     - Q_A
     - Q_B
   * - 1
     - a
     - 1
     - f
   * - 1
     - a
     - 1
     - i
   * - 1
     - a
     - 1
     - c
   * - 3
     - c
     - 3
     - z
   * - 3
     - c
     - 3
     - f
   * - 3
     - c
     - 3
     - c
   * - 2
     - b
     - 2
     - g
   * - 2
     - b
     - 2
     - c

.. code-block:: none

   ... | merge inner (z_R start_date=20200907101210 end_date=20200908101210) as IN on A = IN.A and B = IN.B
   ... | merge inner (z_R start_date=20200907101210 end_date=20200908101210) as IN on z_L.A = IN.A and z_L.B = IN.B

.. list-table::
   :header-rows: 1

   * - A
     - B
     - IN_A
     - IN_B
   * - 3
     - c
     - 3
     - c
                    

- 데이터모델에 공백이 있는 경우( ``modelA`` 와 ``space test`` 데이터모델)

.. code-block:: none

   ... | merge inner ('space test' start_date=20200907101210 end_date=20200908101210) on ID = space test.ID

- 데이터 모델명 치환 ( ``AS`` 문법 사용, 현 데이터 모델은 치환불가)

.. code-block:: none

   ... | merge inner ('space test' start_date=20200907101210 end_date=20200908101210) as B on ID = B.ID
