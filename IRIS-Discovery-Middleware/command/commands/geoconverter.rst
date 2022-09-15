geoconverter
============

개요
----

gemotery / geojson 형태를 서로 바꿔주는 명령어

컬럼 타입
----------------------------------------------------------------------------------------------------
TEXT

설명
----

gemotery 와 property 정보들을 geojson 형태로 만들어 주는 명령어

geojson 데이터를 properties 컬럼 들과 geometry 컬럼으로 바꿔주는 명령어

Parameters
-----------

.. code-block:: bash

    * | geoconverter (intype=<geometry type>)?
                     (outtype=goejson)?
                     geocol=<geometry column>
                     (properties=(<field name>(, <field name>)*|*))?
    * | geoconverter (intype=geojson)?
                     (outtype=<geometry type>)?
                     geocol=<geojson column>

.. list-table::
   :header-rows: 1
   :widths: 10 60 10 10 10

   * - 이름
     - 설명
     - 포멧
     - 기본값
     - 필수/옵션
   * - intype
     - 인풋 데이터의 타입, geometry/geojson, 예) WKT, geojson
     - TEXT
     - geojson
     - 옵션
   * - outtype
     - 결과 리턴 타입, geometry/geojson, 예) WKT, geojson
     - TEXT
     - geojson
     - 옵션
   * - geocol
     - geometry/geojson 컬럼 이름을 선택
     - TEXT
     - 
     - 필수
   * - properties
     - geometry -> geojson 으로 변환시에 사용하는 옵션 |br| 지리정보 가 들어있는 컬럼명, 여러 컬럼 지정가능 |br| star(`*`) 문자를 사용하여 geocol 외의 모든 컬럼을 properties 로 지정 가능
     - TEXT
     - None
     - 옵션
   * - keep
     - 원 데이터 컬럼을 유지한 채 컬럼의 포멧 변경
     - BOOL
     - false
     - 옵션


- intype/outtype 테이블
.. list-table::
   :header-rows: 1

   * - 필터 함수
     - 설명
   * - WKT
     - geocol로 설정하는 데이터의 포멧이 WKT형태인 경우
   * - GEOJSON
     - geocol로 설정하는 데이터의 포멧이 geojson인 경우
   * - GEOMETRY
     - geocol로 설정하는 데이터의 포멧이 geojson 내부에 존재하는 geometry 형태인 경우(geojson과의 변환만 가능)
   

Studio 지도객체의 도형유형에서 사용하기 위해서는 geometry 형식으로 변경하여야 한다.


Examples
--------

- 인풋 데이터는 항상 ``(geometry/geojson 형식의 StringType 컬럼)`` 또는 ``(경위도 데이터)`` 를 포함한다.


예제 1
""""""""

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
   
1. WKT -> geojson 변환

.. code-block:: bash

   * | geoconverter intype=WKT outtype=geojson geocol=B

.. list-table::
   :header-rows: 1
   
   * - GEOJSON
   * - {"type": "Feature", "properties": {}, "geometry": {"type": "Polygon", "coordinates": [[[1, 1], [1, 2], [2, 2], [2, 1], [1, 1]]]}}
   * - {"type": "Feature", "properties": {}, "geometry": {"type": "Polygon", "coordinates": [[[4, 4], [4, 5], [5, 5], [5, 4], [4, 4]]]}}
   * - {"type": "Feature", "properties": {}, "geometry": {"type": "Polygon", "coordinates": [[[5, 1], [5, 2], [6, 2], [6, 1], [5, 1]]]}}


2. WKT -> geojson 변환, properties 설정

.. code-block:: bash

   * | geoconverter intype=WKT outtype=geojson geocol=B properties=A

.. list-table::
   :header-rows: 1
   
   * - GEOJSON
   * - {"type": "Feature", "properties": {"A": "OO구"}, "geometry": {"type": "Polygon", "coordinates": [[[1, 1], [1, 2], [2, 2], [2, 1], [1, 1]]]}}
   * - {"type": "Feature", "properties": {"A": "XX구"}, "geometry": {"type": "Polygon", "coordinates": [[[4, 4], [4, 5], [5, 5], [5, 4], [4, 4]]]}}
   * - {"type": "Feature", "properties": {"A": "AA구"}, "geometry": {"type": "Polygon", "coordinates": [[[5, 1], [5, 2], [6, 2], [6, 1], [5, 1]]]}}


예제 2
""""""""

- 예제 데이터

.. list-table::
   :header-rows: 1
   
   * - GEOJSON
   * - {"type": "Feature", "properties": {"A": "OO구"}, "geometry": {"type": "Polygon", "coordinates": [[[1, 1], [1, 2], [2, 2], [2, 1], [1, 1]]]}}
   * - {"type": "Feature", "properties": {"A": "XX구"}, "geometry": {"type": "Polygon", "coordinates": [[[4, 4], [4, 5], [5, 5], [5, 4], [4, 4]]]}}
   * - {"type": "Feature", "properties": {"A": "AA구"}, "geometry": {"type": "Polygon", "coordinates": [[[5, 1], [5, 2], [6, 2], [6, 1], [5, 1]]]}}

1. geojson -> WKT 변환

.. code-block:: bash

   * | geoconverter intype=WKT outtype=geojson geocol=B

.. list-table::
   :header-rows: 1
   
   * - WKT
   * - POLYGON(( 1 1, 1 2, 2 2, 2 1, 1 1))
   * - POLYGON(( 4 4, 4 5, 5 5, 5 4, 4 4))
   * - POLYGON(( 5 1, 5 2, 6 2, 6 1, 5 1))


.. |br| raw:: html

  <br/>
