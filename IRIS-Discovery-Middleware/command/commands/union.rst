
union
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

이 명령어는 현 데이터 모델에 다른 데이터를 union 을 할 때 사용됩니다.

설명
----------------------------------------------------------------------------------------------------

현재 데이터모델에 ``다른 데이터모델/생성 데이터`` 를  union 하는 명령어 입니다.

필수 조건: union 을 할 데이터와 컬럼의 개수가 같아야합니다.

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | union ((ALL)? MODEL_NAME (ORDER BY field(, field)* (ASC | DESC)? )? | sql "select ... from angora")

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - ALL
     - ``union all`` 명령은 ``unionA``, ``unionB`` 데이터를 그대로 합치는 명령어 입니다. ``union`` 명령은 중복된 값을 제거하는 명령어입니다.
     - 옵션
   * - MODEL_NAME
     - union을 할 데이터모델의 이름을 의미합니다. **모델명에 공백이 포함되는 경우 ``' '`` (Single quote)로 감싸줘야 합니다.**
     - 필수
   * - (ORDER BY field(, field)* (ASC / DESC)? )?
     - union 을 한 뒤, 데이터를 sort 하는 명령어 입니다. 지정한 ``field`` 를 기준으로 데이터를 정렬하고, ``ASC`` , ``DESC`` 를 지정 할 수 있습니다. 지정 하지 않으면 ``default = ASC`` 입니다.
     - 옵션
   * - SQL문
     - 현재 데이터 모델을 이용해 sql 문을 통한 새로운 데이터를 생성하고, 생성된 데이터를 union 하여 결과를 도출 합니다.  **다른 옵션과 함께 사용 할 수 없습니다.**
     - 옵션

Examples
----------------------------------------------------------------------------------------------------
* unionA 데이터 모델

.. list-table::
   :header-rows: 1

   * - AAA
     - BBB
   * - 1
     - 2
   * - 3
     - 4
   * - 5
     - 6

* unionB 데이터 모델

.. list-table::
   :header-rows: 1

   * - AAA
     - BBB
   * - 1
     - 2
   * - 5
     - 4
   * - 3
     - 6


1. 현재 데이터모델(``unionA``)과 다른 데이터모델(``unionB``)을 조인하는 예제입니다.

1.1. union all 명령 예시

.. code-block:: none

   ... | union all unionB


.. list-table::
   :header-rows: 1

   * - AAA
     - BBB
   * - 1
     - 2
   * - 3
     - 4
   * - 5
     - 6
   * - 1
     - 2
   * - 5
     - 4
   * - 3
     - 6

1.2. union 명령 예시

.. code-block:: none

   ... | union unionB


.. list-table::
   :header-rows: 1

   * - AAA
     - BBB
   * - 1
     - 2
   * - 3
     - 4
   * - 5
     - 6
   * - 5
     - 4
   * - 3
     - 6

1.3. union ... order by ... ASC 명령 예시

.. code-block:: none

   ... | union unionB order by AAA (ASC)


.. list-table::
   :header-rows: 1

   * - AAA
     - BBB
   * - 1
     - 2
   * - 3
     - 4
   * - 3
     - 6
   * - 5
     - 6
   * - 5
     - 4

1.4. union ... order by ... DESC 명령 예시

.. code-block:: none

   ... | union unionB order by AAA DESC


.. list-table::
   :header-rows: 1

   * - AAA
     - BBB
   * - 5
     - 6
   * - 5
     - 4
   * - 3
     - 4
   * - 3
     - 6
   * - 1
     - 2

2. 현재 데이터모델(``unionA``)과 sql 문을 통해 생성한 데이터를 union 하는 예제입니다.

.. code-block:: none

   ... | union sql "select sum(AAA), sum(BBB) from angora"

.. list-table::
   :header-rows: 1

   * - AAA
     - BBB
   * - 1
     - 2
   * - 3
     - 4
   * - 5
     - 6
   * - 9
     - 12

