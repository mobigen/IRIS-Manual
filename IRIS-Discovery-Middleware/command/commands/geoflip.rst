geoflip
============

개요
----

공간 데이터의 경도/위도 <-> 위도/경도 의 순서를 변환 하는 명령어

타입
----------------------------------------------------------------------------------------------------
TEXT

설명
----

공간 데이터의 경도/위도 <-> 위도/경도 의 순서를 변환 하는 명령어

input: geometry/geojson 데이터

output: 경도/위도 <-> 위도/경도 의 순서가 바뀐 데이터

Parameters
-----------

.. code-block:: bash

    * | geoflip intype=<geometry/geojson type>
                geocol=<geometry/geojson column>

.. list-table::
   :header-rows: 1
   :widths: 10 60 10 10 10

   * - 이름
     - 설명
     - 포멧
     - 기본값
     - 필수/옵션
   * - intype
     - input 으로 들어오는 geocol 의 타입 |br| 아래 intype 테이블 참고
     - TEXT
     - geojson
     - 옵션
   * - geocol
     - geometry 타입 인 컬럼명 선택
     - TEXT
     - 
     - 필수

- intype 테이블

.. list-table::
   :header-rows: 1
   :widths: 20 80
   
   * - 필터 함수
     - 설명
   * - GEOJSON
     - geocol 로 설정하는 데이터의 포멧이 geojson 형태인 경우
   * - WKT
     - geocol 로 설정하는 데이터의 포멧이 WKT 형태인 경우
   * - WKB
     - geocol 로 설정하는 데이터의 포멧이 WKB 형태인 경우
   * - LONGLAT
     - x_col(경도), y_col(위도) 로 설정하는 데이터 일 경우
   * - GEOMETRY
     - geojson 의 ``geometry`` 의 데이터 형태인 경우 |br| ex) ``{"type": "Polygon", "coordinates": [[....]]}``

Examples
--------

- 인풋 데이터는 항상 ``(geometry/geojson 형식의 StringType 컬럼)`` 를 포함한다.

- 예제 데이터

.. list-table::
   :header-rows: 1
   
   * - A
     - B
   * - OO구
     - POLYGON(( 1 1, 1 2, 2 2, 2 1, 1 1))
   * - XX구
     - POLYGON(( 4 4, 4 5, 5 5, 5 4, 4 4))
   * - AA구
     - POLYGON(( 5 1, 5 2, 6 2, 6 1, 5 1))
   
- 예제1, WKT 의 x, y 좌표 순서를 y, x 순서로 변경

.. code-block:: bash

   * | geoflip intype=WKT geocol=B

.. list-table::
   :header-rows: 1
   
   * - A
     - B
   * - OO구
     - POLYGON(( 1 1, 2 1, 2 2, 1 2, 1 1))
   * - XX구
     - POLYGON(( 4 4, 5 4, 5 5, 4 5, 4 4))
   * - AA구
     - POLYGON(( 1 5, 2 5, 2 6, 1 6, 1 5))

.. |br| raw:: html

  <br/>