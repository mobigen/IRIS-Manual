TYPECAST
===========================

개요
-----------------------------------
선택한 컬럼의 데이터 타입을 원하는 타입으로 변환시켜 반환합니다.

설명
----------------------------------
sql 문의 cast 함수와 같은 역할을 하며, 선택한 컬럼의 타입을 변환시켜 반환합니다.

FIELD_NAME에는 ``''`` 를 사용하거나 사용하지 않을수도 있습니다.

NEW_TYPE에는 따옴표를 사용하지 않습니다.

OPTION에는 ``""`` 를 사용해야 합니다.

``,`` 를 이용하여 연속하여 여러개의 컬럼의 타입 변환이 가능합니다.

Parameters
--------------------------------------

.. code-block::

    ... | typecast ('FIELD_NAME'/ FIELD_NAME) NEW_TYPE "OPTION"
    ... | typecast ('FIELD_NAME'/ FIELD_NAME) TYPE "OPTION", (('FIELD_NAME'/ FIELD_NAME) TYPE "OPTION")*

.. list-table::
    :header-rows: 1

    * - 이름
      - 설명
      - 필수/옵션
    * - FIELD_NAME
      - field의 이름을 의미합니다.
      - 필수
    * - NEW_TYPE
      - 변환할(new) 타입 입니다.
      - 필수
    * - OPTION
      - TIMESTAMP, DATE의 format을 결정할 수 있는 문자열 입니다.
      - 옵션
  
- OPTION을 입력하지 않으면 TIMESTAMP는 "YYYY-MM-DD HH:mm:ss", DATE는 "YYYY-MM-DD"가 기본 옵션입니다.


Examples
-------------------------------
- 예제 데이터 모양 입니다.

.. list-table::
   :header-rows: 1

   * - AAA
     - EEE
     - DDD
   * - 1
     - 2020/07/24
     - 20200724091219
   * - 2
     - 2021/03/18
     - 20210318045402
   * - 3
     - 2021/05/04
     - 20210504114839

- ``INTEGER`` 타입의 AAA 컬럼을 ``REAL`` 타입으로 변환하는 예제입니다.

.. code-block::

   ... | typecast AAA real

.. list-table::
   :header-rows: 1

   * - AAA
     - EEE
     - DDD
   * - 1.0
     - 2020/07/24
     - 20200724091219
   * - 2.0
     - 2021/03/18
     - 20210318045402
   * - 3.0
     - 2021/05/04
     - 20210504114839


- ``STRING`` 타입의 EEE 컬럼을 ``DATE`` 타입으로 변환하는 예제입니다.

.. code-block::

   ... | typecast 'EEE' date "YYYY/MM/DD"

.. list-table::
   :header-rows: 1

   * - AAA
     - EEE
     - DDD
   * - 1
     - 2020-07-24
     - 20200724091219
   * - 2
     - 2021-03-18
     - 20210318045402
   * - 3
     - 2021-05-04
     - 20210504114839


- ``STRING`` 타입의 EEE 컬럼을 ``DATE`` 타입과 ``BIGINT`` 타입의 DDD 컬럼을 ``TIMESTAMP`` 타입으로 변환하는 예제입니다.

.. code-block::

   ... | typecast EEE date "YYYY/MM/DD", 'DDD' TIMESTAMP

.. list-table::
   :header-rows: 1

   * - AAA
     - EEE
     - DDD
   * - 1
     - 2020-07-24
     - 2020-07-24 09:12:19
   * - 2
     - 2021-03-18
     - 2021-03-18 04:54:02
   * - 3
     - 2021-05-04
     - 2021-05-04 11:48:39
