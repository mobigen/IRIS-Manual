.. _test_docs:
데이터 처리 DSL
-------------

데이터 처리를 위한 DSL 종류에 대해 설명 합니다.

**필터**

- 특정 조건에 따라 레코드를 제외하거나, 컬럼을 생성합니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * `fields <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/fields>`_
      * 특정 컬럼을 조회
      * (유) rename
    - * `where <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/where>`_
      * 조건에 맞는 레코드 조회
      *
    - * `case <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/case>`_
      * 조건에 따른 컬럼을 생성
      *

**숫자 연산**

- 인자로 숫자 타입의 데이터를 사용하고, 그 결과로 숫자 타이의 데이터를 반환 하는 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * `abs <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/abs>`_
      * 절대값 계산
      *
    - * ceil
      * 소수점 자리의 숫자를 무조건 올림
      * (유)floor, (유)round
    - * floor
      * 소수점 자리의 숫자를 무조건 내림
      * (유)ceil, (유)round
    - * `round <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/round>`_
      * 소수점 자리의 숫자를 반올림
      * (유)ceil, (유)floor
    - * `calculate <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/calculate>`_
      * 행/열 간의 수식 계산을 진행
      *

**문자 연산**

- 인자로 문자 타입의 데이터를 사용하는 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * `concat <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/concat>`_
      * 선택한 컬럼을 이어 붙임
      * (반) explode
    - * `fillna <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/fillna>`_
      * 특정한 필드의 결측치 처리에 사용
      *
    - * `substr <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/substr>`_
      * 특정한 필드나 문자열을 잘라낸다
      *
    - * `replace <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/replace>`_
      * 선택한 컬럼의 데이터의 문자를 변환
      *
    - * `length <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/length>`_
      * string length를 계산
      *
    - * `regex <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/regex>`_
      * 대상 컬럼의 값에서 지정한 정규식과 일치하는 결과를 추출
      *
    - * `split <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/split>`_
      * 선택한 컬럼을 구분자를 통해 분리
      * (반) concat

**날자 연산**

- 날자, 시간을 처리하기 위한 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * `weekend <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/weekend>`_
      * 주말/주중 데이터를 필터링
      *
    - * `time2sec <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/time2sec>`_
      * 시:분:초 로 이루어진 데이터를 초 단위 숫자로 변경
      *
    - * `timediff <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/timediff>`_
      * 선택한 컬럼 끼리 혹은 원하는 데이터와 컬럼 간의 시간 차이 계산
      *
    - * `weekdates <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/weekdates>`_
      * 해당 주차의 시작일과 종료일을 판별
      *
    - * `weeknumber <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/weeknumber>`_
      * 데이터의 날짜가 1년의 몇 번째 주인지 계산
      *

**그룹/통계/집계**

- 데이터를 그룹화여 처리하기 위한 명령어 입니다.
- SQL의 aggregate 와 관련된 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * `distinct <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/distinct>`_
      * 선택한 컬럼에 대한 중복값 없는 데이터를 반환
      * (유) value_counts
    - * `value_counts <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/value_counts>`_
      * 특정 필드에 대한 고유값과 해당 값 계산
      * (유)count
    - * `count <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/count>`_
      * 입력의 래코드 건수를 계산
      * (유) value_counts
    - * `stats <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/stats>`_
      * 각종 통계 데이터를 계산
      *
    - * `timeline <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/timeline>`_
      * 시간 범위에 따라 group by된 카운트의 숫자 출력
      *
    - * `pivot <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/pivot>`_
      * 테이블을 여러 컬럼들을 축으로 회전 및 각종 통계 정보를 행과 열 별로 반환
      * (반) unpivot
    - * `unpivot <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/unpivot>`_
      * 가로로 출력되는 열(COLUMN) 데이터를 세로로 돌려 행(ROW)으로 반환
      * (반) pivot
    - * `explode <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/explode>`_
      * 선택한 컬럼을 구분자를 통해서 분리하여 새로운 레코드로 만드는 명령어
      * (반) concat
    - * `summary <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/summary>`_
      * DF 의 각 컬럼에 대한 집계 정보 출력
      *

**정렬**

- 특정 컬럼 혹은 레코드를 기준으로 오름차순, 내림차순 정렬을 위한 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * `sort <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/sort.html>`_
      * 검색 결과를 지정된 필드를 기준으로 정렬
      * (유) ysort
    - * `ysort <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/ysort>`_
      * 검색 결과로 생성된 필드의 순서를 지정된 기준으로 정렬
      * (유) sort

**schema 변경**

- 데이터의 값에는 영향을 주지 않고, 컬럼 이름 혹은 타입을 변경하기 위한 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * `rename <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/rename>`_
      * field를 다른 명칭으로 변경
      * (유) fields
    - * `typecase <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/typecast.html>`_
      * 선택한 컬럼의 데이터 타입을 원하는 타입으로 변환
      *


**limit**

- 데이터의 일부만 반환하기 위한 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * `head <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/head>`_
      * 상위 records를 원하는 갯수 만큼 검색 결과에 출력
      * (유) top
    - * `top <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/top>`_
      * 검색 결과를 지정된 필드를 기준으로 정렬 후 상위 값을 출력
      * (유) head
    - * `sampling <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/sampling>`_
      * 일부 데이터를 특정한 방식으로 샘플 출력
      *


**지도**

- 지도 데이터 처리를 위한 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * `geoconverter <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/geoconverter>`_
      * gemotery / geojson 형태를 서로 변환
      *
    - * `geoflip <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/geoflip>`_
      * 공간 데이터의 경도/위도 <-> 위도/경도 의 순서를 변환
      *
    - * `georelation <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/georelation>`_
      * geometry 타입의 데이터를 필터 조건을 이용해 필터링
      *


**기타**

- 위에 해당하지 않는 특수 작업을 사용되는 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * `lag <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/lag>`_
      * 현재 행 이전의 행의 값을 가져와 출력
      * (유) lead
    - * `lead <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/lead>`_
      * 현재 행의 다음 행의 값을 가져와 출력
      * (유) lag
    - * `numbering <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/numbering.html>`_
      * 데이터의 row number 를 붙여 출력
      *
    - * `interpolation <http://docs.iris.tools/manual/IRIS-Manual/IRIS-Discovery-Middleware/command/commands/interpolation>`_
      * 시계열 데이터를 이용하여 집계를 할 때, 설정한 간격 사이의 시계열 데이터가 없으면 해당 데이터를 추가(보간)
      * (유) pivot