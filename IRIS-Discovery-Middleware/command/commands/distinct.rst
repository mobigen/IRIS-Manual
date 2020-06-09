distinct
========

개요
-----

선택한 컬럼에 대한 중복값 없는 데이터를 반환합니다.

설명
----------------------------------------------------------------------------------------------------

sql 문의 distinct 함수와 같은 역할을 하며, 선택한 컬럼의 중복값을 제거한 데이터를 반환합니다.

여러 컬럼을 선택하면, 선택한 컬럼의 그룹단위로 중복값을 제거 합니다.

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | distinct (FIELD_NAME (, FIELD_NAME)* | *)

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - FIELD_NAME
     - field의 이름을 의미합니다.
     - 필수
   * - *
     - ``*``\ , wildcard를 의미합니다.
     - 옵션

Examples
----------------------------------------------------------------------------------------------------

- 예제데이터
 
.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - Iron
     - 40
     - America
   * - Captain
     - 80
     - America
   * - Park
     - 30
     - Korea
   * - Lee
     - 20
     - Korea

- ``C`` 필드의 데이터 중복 제거 예제

.. code-block:: none

   ... | distinct C

.. list-table::
   :header-rows: 1

   * - C
   * - America
   * - Korea

- ``A, C`` 필드의 데이터 중복 제거 예제

.. code-block:: none

   ... | distinct A, C

.. list-table::
   :header-rows: 1

   * - A
     - C
   * - Iron
     - America
   * - Captain
     - America
   * - Park
     - Korea
   * - Lee
     - Korea

- 모든 필드의 데이터 중복 제거 예제

.. code-block:: none

   ... | distinct *

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - Iron
     - 40
     - America
   * - Captain
     - 80
     - America
   * - Park
     - 30
     - Korea
   * - Lee
     - 20
     - Korea
