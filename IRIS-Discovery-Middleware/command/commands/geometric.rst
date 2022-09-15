geometric
============

개요
----

``데이터의 계산값`` / ``필터와의 관계`` 와 같은 연산을 하는 명령어

타입
----------------------------------------------------------------------------------------------------
TEXT

설명
----

``데이터의 계산값`` / ``필터와의 관계`` 와 같은 계산 결과를 리턴

input: geometry/geojson 데이터

output: 분석 결과 데이터

Parameters
-----------

.. code-block:: bash

    * | georelation (intype=<geometry/geojson type>)?
                    (outtype=<geometry/geojson type>)?
                    (op=<value>)?
                    (op_option=<value>)?
                    (geocol=<geometry/geojson column>(, <geometry/geojson column>)* | x_col=<longitude column> y_col=<latitude column>)
                    (filter=<WKT>(, <WKT>)*)?

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
   * - outtype
     - output 의 타입, 결과가 geometry 인 경우 해당하는 옵션 |br| 아래 outtype 테이블 참고
     - TEXT
     - geojson
     - 옵션
   * - op
     - 필터 함수 설정 |br| 아래 operation 테이블 참고
     - TEXT
     - 
     - 필수
   * - op_option
     - 필터 함수의 파라미터가 들어가는 경우 사용하는 옵션
     - TEXT
     - 
     - 옵션
   * - geocol
     - geometry 타입 인 컬럼명 선택 |br| 여러 컬럼은 콤마(``,``)로 구분 |br| (x_col/y_col) 대신 사용
     - TEXT
     - 
     - 필수
   * - x_col/y_col
     - 경도/위도 컬럼명 선택 |br| (geometry) 대신 사용
     - TEXT
     - 
     - 필수
   * - filter
     - 필터 조건, WKT 형식의 string, 여러 조건은 콤마(``,``)로 구분
     - TEXT(WKT)
     - 
     - 옵션

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

- outtype 테이블

  - op 로 centroid, intersection 과 같은 geometry 형태의 데이터가 output 으로 나오는 경우에 사용하는 옵션

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

- operation 테이블

  - 아래의 표현을 위해서 다음을 정의
  - 필터 폴리곤: ``A``
  - 데이터: ``B``

.. list-table::
   :header-rows: 1
   :widths: 10 40 10 10 20
   
   * - 필터 함수
     - 설명
     - filter |br| 필요 여부
     - op_option |br| 필요 여부
     - 반환 타입
   * - Area
     - B 의 면적 리턴
     - X
     - X
     - REAL
   * - Boundary
     - B 의 닫힌 바운더리 결과를 리턴
     - X
     - X
     - geometry
   * - Buffer
     - B 와 입력된 숫자 만큼 떨어진 모든 포인트를 표현하는 geometry 리턴
     - X
     - O
     - geometry
   * - ConvexHull
     - B 이 convex hull 을 리턴
     - X
     - X
     - geometry
   * - Centroid
     - B 의 중심점 리턴
     - X
     - X
     - geometry
   * - Distance
     - A 와 B 의 최단 거리 리턴
     - O
     - X
     - geometry
   * - Envelope
     - B 를 감싸는 바운더리 리턴
     - X
     - X
     - geometry
   * - ExteriorRing
     - B 의 polygon 바깥쪽의 ring을 Linestring 형태로 반환 |br| Polygon 이 아니면 null
     - X
     - X
     - geometry
   * - GeometryN
     - B 에서 구성요소중 N 번째 geometry 값 반환
     - X
     - O
     - geometry
   * - GeometryType
     - B 의 geometry 타입을 리턴 |br| ex) ST_Polygon, ST_Linestring
     - X
     - X
     - TEXT
   * - Intersection
     - A 와 B 가 공유하는 부분을 리턴
     - O
     - X
     - geometry
   * - IsClosed
     - B 가 Linestring 일 때, 시작과 끝이 같은면 True (닫혀있는지)
     - X
     - X
     - BOOL
   * - IsRing
     - B 가 Linestring 일 때, IsClosed, IsSimple 값이 둘다 해당하면 True
     - X
     - X
     - BOOL
   * - IsSimple
     - B 의 자체 교차점이 1개만 있는지 확인
     - X
     - X
     - BOOL
   * - IsValid
     - B 가 geometry 형태인지 확인
     - X
     - X
     - BOOL
   * - Length
     - B 의 길이 or 주변부 길이 리턴
     - X
     - X
     - REAL
   * - NPoints
     - B 의 포인트들의 개수를 리턴
     - X
     - X
     - INTEGER
   * - NumInteriorRings
     - B 가 Polygon 일 때, 내부 ring 형태의 개수 반환
     - X
     - X
     - INTEGER
   * - PrecisionReduce
     - B 의 좌표의 소수점 자릿수를 주어진 옵션에 맞춰 수정해 줍니다.
     - X
     - O
     - geometry
   * - SimplifyPreserveTopology
     - B 를 지리학적인 관계는 유지를 하면서 옵션 값을 이용해 단순화 하는 것
     - X
     - O
     - geometry


Examples
--------

- 인풋 데이터는 항상 ``(geometry/geojson 형식의 StringType 컬럼)`` 또는 ``(경위도 데이터)`` 를 포함한다.

- 필터 조건은 WKT 형태로 작성한다.

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
   
- 예제1, 필터와 겹치(intersection)는 부분의 폴리곤을 반환

.. code-block:: bash

   * | geometric op = intersection filter = POLYGON(( 0 0, 0 3, 3 3, 3 0, 0 0)) intype=WKT outtype=WKT

.. list-table::
   :header-rows: 1
   
   * - A
     - B
     - RESULT_ANALYSIS
   * - OO구
     - POLYGON(( 1 1, 1 2, 2 2, 2 1, 1 1))
     - POLYGON(...) (겹치는 부분이 있는 경우)
   * - XX구
     - POLYGON(( 4 4, 4 5, 5 5, 5 4, 4 4))
     - GEOMETRYCOLLECTION EMPTY (겹치는 부분이 없는 경우)
   * - AA구
     - POLYGON(( 5 1, 5 2, 6 2, 6 1, 5 1))
     - GEOMETRYCOLLECTION EMPTY (겹치는 부분이 없는 경우)

- 예제2, 넓이 반환

.. code-block:: bash

   * | geometric op = area intype=WKT

.. list-table::
   :header-rows: 1
   
   * - A
     - B
     - RESULT_ANALYSIS
   * - OO구
     - POLYGON(( 1 1, 1 2, 2 2, 2 1, 1 1))
     - 1
   * - XX구
     - POLYGON(( 4 4, 4 5, 5 5, 5 4, 4 4))
     - 1
   * - AA구
     - POLYGON(( 5 1, 5 2, 6 2, 6 1, 5 1))
     - 1

.. |br| raw:: html

  <br/>