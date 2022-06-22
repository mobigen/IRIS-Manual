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
    - * fields
      * 특정 컬럼을 조회
      *
    - * where
      * 조건에 맞는 레코드 조회
      *
    - * case
      * 조건에 따른 컬럼을 생성
      *

**숫자 연산**

- 인자로 숫자 타입의 데이터를 사용하고, 그 결과로 숫자 타이의 데이터를 반환 하는 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * abs
      * 절대값 계산
      *
    - * ceil
      * 소수점 자리의 숫자를 무조건 올림
      * (유)floor, (유)round
    - * floor
      * 소수점 자리의 숫자를 무조건 내림
      * (유)ceil, (유)round
    - * round
      * 소수점 자리의 숫자를 반올림
      * (유)ceil, (유)floor
    - * calculate
      * 행/열 간의 수식 계산을 진행
      *

**문자 연산**

- 인자로 문자 타입의 데이터를 사용하는 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * concat
      *
      *
    - * fillna
      *
      *
    - * substr
      *
      *
    - * replace
      *
      *
    - * length
      *
      *
    - * regex
      *
      *
    - * split
      *
      *

**날자 연산**

- 날자, 시간을 처리하기 위한 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * weekend
      *
      *
    - * time2sec
      *
      *
    - * timediff
      *
      *
    - * weekdates
      *
      *
    - * weeknumber
      *
      *

**그룹/통계/집계**

- 데이터를 그룹화여 처리하기 위한 명령어 입니다.
- SQL의 aggregate 와 관련된 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * distenct
      *
      * (유) value_counts
    - * value_counts
      *
      * count
    - * count
      *
      *
    - * stats
      *
      *
    - * timeline
      *
      *
    - * pivot
      *
      * (반) unpivot
    - * unpivot
      *
      * (반) pivot
    - * explode
      *
      *
    - * summary
      *
      *

**정렬**

- 특정 컬럼 혹은 레코드를 기준으로 오름차순, 내림차순 정렬을 위한 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * sort
      *
      *
    - * ysort
      *
      *

**schema 변경**

- 데이터의 값에는 영향을 주지 않고, 컬럼 이름 혹은 타입을 변경하기 위한 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * rename
      *
      *
    - * typecase
      *
      *


**limit**

- 데이터의 일부만 반환하기 위한 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * head
      *
      *
    - * top
      *
      *
    - * sampling
      *
      *


**지도**

- 지도 데이터 처리를 위한 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * geoconverter
      *
      *
    - * geoflip
      *
      *
    - * georelation
      *
      *


**기타**

- 위에 해당하지 않는 특수 작업을 사용되는 명령어 입니다.

.. list-table::

    - * DSL 종류
      * 설명
      * 관련 DSL
    - * lag
      *
      *
    - * lead
      *
      *
    - * numbering
      *
      *
    - * interpolation
      *
      *