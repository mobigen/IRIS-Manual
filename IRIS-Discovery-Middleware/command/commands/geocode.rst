geocode
============

개요
----

주소, 우편번호, 위경도 등의 데이터 컬럼에 해당하는 geometry 데이터를 찾아주는 명령어

설명
----

주소, 우편번호, 위경도 등의 데이터 컬럼에 해당하는 geometry 데이터를 찾아주는 명령어

생성된 데이터는 경/위도 순서이다. |br|
데이터를 위/경도 순서로 바꾸고 싶다면 ``geoflip`` 명령어를 사용한다.

Parameters
-----------

.. code-block:: bash

    * | geocode codetype=address country=<country code> SI=column GU=column DONG=column
    * | geocode codetype=code country=<country code> ZIPCODE=column
    * | geocode codetype=ip IPCOL=column
    * | geocode codetype=LONGLAT COORD=column

.. list-table::
   :header-rows: 1
   :widths: 10 60 10 10 10

   * - 이름
     - 설명
     - 포멧
     - 기본값
     - 필수/옵션
   * - country
     - 나라 코드 |br| *현재 한국 데이터만 처리가능하여 동작하지 않습니다.*
     - TEXT
     - KR
     - 옵션
   * - codetype
     - address(주소), code(우편번호), 등의 타입을 선택
     - TEXT
     - 
     - 옵션
   * - option of type
     - 타입별로 사용되는 옵션이 다름. 각 타입별로 필요한 데이터가 들어있는 컬럼명을 지정
     - TEXT
     - 
     - 부분필수

각 타입별 옵션 정보
""""""""""""""""""""""""""""""""""

- address

.. list-table::
   :header-rows: 1

   * - 옵션 키
     - 설명
   * - SI
     - 시도
   * - GU
     - 시군구
   * - DONG
     - 읍면동
   * - DORO
     - 도로명

- code

.. list-table::
   :header-rows: 1

   * - 옵션 키
     - 설명
   * - ZIPCODE
     - 우편번호

- LONGLAT

.. list-table::
   :header-rows: 1

   * - 옵션 키
     - 설명
   * - COORD
     - 좌표정보가 있는 컬럼, 해당 좌표가 포함된 주소를 가져옴


Examples
--------

예제 1 - 주소
"""""""""""""""""

- 예제 데이터

.. list-table::
   :header-rows: 1
   
   * - A
     - B
     - C
   * - OO시
     - OO구
     - OO동
   * - XX시
     - XX구
     - XX동
   * - AA시
     - AA구
     - AA동
   
1. 주소에 해당하는 geometry

.. code-block:: bash

   * | geocode codetype=address country=KR SI=A GU=B DONG=C

.. list-table::
   :header-rows: 1
   
   * - A
     - B
     - C
     - GEOMETRY
   * - OO시
     - OO구
     - OO동
     - POLYGON(( 1 1, 1 2, 2 2, 2 1, 1 1))
   * - XX시
     - XX구
     - XX동
     - POLYGON(( 4 4, 4 5, 5 5, 5 4, 4 4))
   * - AA시
     - AA구
     - AA동
     - POLYGON(( 5 1, 5 2, 6 2, 6 1, 1 1))

예제 2 - 우편번호
"""""""""""""""""""""

- 예제 데이터

.. list-table::
   :header-rows: 1
   
   * - A
   * - 05854
   * - 05432
   * - 03420

1. 우편번호에 해당하는 geometry

.. code-block:: bash

   * | geocode codetype=code country=KR ZIPCODE=A

.. list-table::
   :header-rows: 1
   
   * - A
     - GEOMETRY
   * - 05854
     - POLYGON(( 1 1, 1 2, 2 2, 2 1, 1 1))
   * - 05432
     - POLYGON(( 4 4, 4 5, 5 5, 5 4, 4 4))
   * - 03420
     - POLYGON(( 5 1, 5 2, 6 2, 6 1, 1 1))


.. |br| raw:: html

  <br/>
