.. role:: raw-html-m2r(raw)
   :format: html


split
=====

개요
-----

선택한 컬럼을 구분자를 통해서 분리하여 새로운 레코드로 만드는 명령어

타입
----------------------------------------------------------------------------------------------------
TEXT

설명
-----

선택한 컬럼의 데이터를 구분자를 이용해 분리하고, 새로운 레코드로 만듭니다. (원래 데이터는 삭제)
구분자는 사용자가 지정한 문자이며, 각 컬럼에 대한 구분자를 지정할 수도 있습니다.

예)

원본 데이터

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - a,b,c,d
     - 1,2,3,4
     - 가,나,다,라

split 후 데이터

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - a
     - 1
     - 가
   * - b
     - 2
     - 나
   * - c
     - 3
     - 다
   * - d
     - 4
     - 라

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | split [DELIMITER|LIST] ([col|idx]=[field_name|field_idx|LIST])?

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - DELIMITER
     - 구분자로 사용할 문자. 한 문자를 입력할 수 있으며, single-quote 를 사용해 구분자를 입력 할 수도 있습니다. 예) 단순 문자: ``,``, single-quote: ``','`` 
     -
   * - LIST
     - LIST 형태로 작성할 수 있는 파라미터 입니다. 예) 구분자를 리스트 형태로 입력시: ``[',', '|', '%']``, 필드명 입력시: ``[필드1, 필드2]``
     -
   * - col
     - 필드 이름을 사용합니다.
     - 옵션
   * - idx
     - 필드 인덱스를 사용합니다. 인덱스는 0 에서 시작합니다.
     - 옵션
   * - field_name
     - 반올림 할 필드 이름입니다. 예) 리스트 사용: [field1, field2,...], 단수 필드명 사용: field1
     - 옵션
   * - field_idx
     - 반올림 할 필드의 인덱스 입니다. 예) 리스트 사용: [field_index1, field_index1,...], 단수 필드 인덱스 사용: field_index1
     - 옵션

Examples
---------
예제 데이터

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - a,b,c,d
     - 1,2,3,4
     - 가,나,다,라
   * - e,b,g
     - 17,51,,45
     - 가,,나,미
   * - h,a,j,k
     - 6,33,44
     - 모,비,젠

1. 모든 선택 필드를 하나의 구분자로 처리 하는 방법

.. code-block:: none

   ... | split , col=[A, B, C]
   ... | split , idx=[0, 1, 2]

1.1. 결과

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
   * - a
     - 1
     - 가
   * - b
     - 2
     - 나
   * - c
     - 3
     - 다
   * - d
     - 4
     - 라
   * - ...
     - ...
     - ...

2. 하나의 필드만 처리하는 방법

.. code-block:: none

   ... | split , col=[A]
   ... | split , col=A
   ... | split , idx=[0]
   ... | split , idx=0

2.1. 결과

.. list-table::
   :header-rows: 1

   * - A
   * - a
   * - b
   * - c
   * - d
   * - ...

3. 각 필드 마다 다른 구분자 사용하는 방법

.. code-block:: none

   ... | split [',', ','] col=[A,B]
   ... | split [',', ','] idx=[0,1]

.. list-table::
   :header-rows: 1

   * - A
     - B
   * - a
     - 1
   * - b
     - 2
   * - c
     - 3
   * - d
     - 4
   * - ...
     - ...

