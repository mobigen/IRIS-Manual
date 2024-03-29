
ysort
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

이 명령어는 검색 결과로 생성된 필드의 순서를 지정된 기준으로 정렬합니다.

타입
----------------------------------------------------------------------------------------------------
TEXT, INTEGER, BIGINT, REAL, DATE, TIMESTAMP

설명
----------------------------------------------------------------------------------------------------

검색 결과를 지정된 기준으로 필드들의 순서를 정렬합니다. ``asc``\ 는 오름차순을, ``desc``\ 는 내림차순을 의미합니다. 이후 지정한 ``ignore``\ 는 정렬 기준에서 생략되고, 지정한 순서대로 맨 앞 필드로 선택됩니다.

Examples
----------------------------------------------------------------------------------------------------


* 예시 데이터.

.. code-block:: none

   [A, B, C, D, E, F, G, H] - 필드명

   [0, 9, 6, 3, 1, 8, 7, 4]
   [0, 8, 4, 3, 1, 7, 8, 4]


* ``asc`` 정렬.

.. code-block:: none

   ..| ysort asc

결과

.. code-block:: none

   [A, E, D, H, C, F, G, B] - 필드명

   [0, 1, 3, 4, 6, 8, 7, 9]
   [0, 1, 3, 4, 4, 7, 8, 8]


* ``desc`` 정렬

.. code-block:: none

   ..| ysort desc

결과

.. code-block:: none

   [A, E, D, H, C, F, G, B] - 필드명

   [0, 1, 3, 4, 6, 8, 7, 9]
   [0, 1, 3, 4, 4, 7, 8, 8]


* ``xaxis`` 지정.

.. code-block:: none

   ..| ysort asc B, C

결과

.. code-block:: none

   [B, C, A, E, D, H, F, G] - 필드명

   [9, 6, 0, 1, 3, 4, 8, 7]
   [8, 4, 0, 1, 3, 4, 7, 8]

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | ysort order xaxis

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - order
     - sorting 방법을 의미합니다. ``asc``\ , ``desc`` 를 사용 할 수 있습니다.
     - 필수
   * - xaxis
     - sorting에 포함하지 않을 필드를 지정합니다. 지정한 순서대로 생성된 데이터의 앞 선 필드가 되고, 그 이후 정렬된 필드들이 옵니다.
     - 옵션



* 추가정보 : **현재는 정렬 시 해당 필드 값의 평균을 기준으로 하고 있습니다.**

Parameters BNF
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ysort_command : order xaxis

   order : TERM

   xaxis : xaxis TERM
           | TERM

   TERM = ([^\s=])+
   COMMA = ,
