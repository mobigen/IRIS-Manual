numbering
==========

개요
----

데이터의 row number 를 붙여주는 명령어

컬럼 타입
----------------------------------------------------------------------------------------------------
TEXT, INTEGER, BIGINT, REAL, DATE, TIMESTAMP

설명
----

데이터의 row number 를 붙여주는 명령어

Parameters
-----------

.. code-block:: python

   * | numbering
   * | numbering 새컬럼명

.. list-table::
   :header-rows: 1
   
   * - 이름
     - 설명
     - 필수/옵션
     - 기본값
   * - 새컬럼명
     - row number 의 컬럼명
     - 옵션
     - ID

Examples
--------

- 예제 데이터

.. list-table::
   :header-rows: 1
   
   * - A
     - B
   * - 2020-01-01
     - aa
   * - 2020-03-01
     - bb
   * - 2020-06-01
     - cc

- 예제1

.. code-block:: python

    * | numbering

.. list-table::
   :header-rows: 1

   * - ID
     - A
     - B
   * - 1
     - 2020-01-01
     - aa
   * - 2
     - 2020-03-01
     - bb
   * - 3
     - 2020-06-01
     - cc

- 예제2

.. code-block:: none

    * | numbering `New Col`

.. list-table::
   :header-rows: 1

   * - New Col
     - A
     - B
   * - 1
     - 2020-01-01
     - aa
   * - 2
     - 2020-03-01
     - bb
   * - 3
     - 2020-06-01
     - cc

