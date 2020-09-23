georelation
============

개요
----

geometry 타입의 데이터를 필터 조건을 이용해 필터링 하는 명령어

설명
----

geometry 타입의 필터 조건을 이용해, 해당 필터에 적용되는 데이터만 보여주도록 필터링 하는 명령어

input: geometry/geojson 데이터

output: 필터링된 geometry/geojson 데이터

Parameters
-----------

.. code-block::

    * | georelation (intype=<geometry/geojson type>)?
                    (op=<value>)?
                    (geocol=<geometry/geojson column>(, <geometry/geojson column>)* | x_col=<longitude column> y_col=<latitude column>)
                    filter=<WKT>(, <WKT>)*
                    (circle_radius=<value>)?

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
   * - op
     - 필터 함수 설정 |br| 아래 operation 테이블 참고
     - TEXT
     - 
     - 필수
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
     - 필수
   * - circle_radius
     - 원형의 필터 조건을 사용 할 때 옵션 |br| 필터에는 Point 한 개, 본 옵션에는 원의 반지름을 표기(단위: m(미터)) |br| 예) point(127.123 35.234), 0.01 -> point 를 중심으로 0.01 반지름인 원
     - TEXT(WKT)
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

- operation 테이블

  - 아래의 표현을 위해서 다음을 정의
  - 필터 폴리곤: ``A``
  - 데이터: ``B``

.. list-table::
   :header-rows: 1
   :widths: 20 80
   
   * - 필터 함수
     - 설명
   * - Contains
     - A 가 B 를 완벽히 포함하는 경우
   * - Crosses
     - A 와 B 가 횡단 하는 경우
   * - Equals
     - A 와 B 가 동일한 경우
   * - Intersects
     - A 와 B 가 공유하는 부분이 있는 경우
   * - Overlaps
     - A 와 B 가 부분적으로 겹치는 영역이 존재하며, 완전히 포함되지는 않을 경우
   * - Touches
     - A 와 B 가 한점에서 만나는 경우, 포함 관계가 아닌 것
   * - Within
     - A 가 B 에 완벽히 포함되는 경우


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
   
- 예제1, 필터와 겹치(intersects)는 부분이 있는 폴리곤 필터링

.. code-block::

   * | georelation intype=WKT op = intersects geocol=B filter = POLYGON(( 0 0, 0 3, 5 5, 4 0, 0 0))

.. list-table::
   :header-rows: 1
   
   * - A
     - B
   * - OO구
     - POLYGON(( 1 1, 1 2, 2 2, 2 1, 1 1))
   * - XX구
     - POLYGON(( 4 4, 4 5, 5 5, 5 4, 4 4))

- 예제2, 필터에 포함(contains)되는 폴리곤 필터링

.. code-block::

   * | georelation intype=WKT op = contains geocol=B filter = POLYGON(( 0 0, 0 3, 3 3, 3 0, 0 0))

.. list-table::
   :header-rows: 1
   
   * - A
     - B
   * - OO구
     - POLYGON(( 1 1, 1 2, 2 2, 2 1, 1 1))

.. |br| raw:: html

  <br/>