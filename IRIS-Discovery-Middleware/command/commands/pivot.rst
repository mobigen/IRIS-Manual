.. role:: raw-html-m2r(raw)
   :format: html


pivot
=====

개요
----

테이블을 여러 컬럼들을 축으로 회전 및 각종 통계 정보를 행과 열 별로 구할 수 있습니다.

타입
----------------------------------------------------------------------------------------------------
TEXT, INTEGER, BIGINT, REAL, DATE, TIMESTAMP

설명
----

``SPLITROW``\ , ``SPLITCOL``\ , ``AS`` 의 문구를 지원하며, ``SPLITROW``\ 는 가로축 기반으로 그리고 ``SPLITCOL``\ 은 세로축 기반으로 데이터를 축 기준으로 회전 하거나 aggregation을 할 수 있습니다. ``AS``\ 는 결과 값의 field의 별칭을 줄 수 있습니다.

pivot 의 결과를 sort 하고자 할 때, 옵션 ``SORTROW`` , ``SORTCOL``\ 을 사용할 수 있습니다.

* ``SORTROW``\ ,\ ``SORTCOL`` 의 인자로는 ``asc`` 및 ``desc``\ 를 사용 할 수 있습니다.
* 예) ``SORTCOL asc`` or ``SORTCOL desc``

.. code-block:: none

   # SPLITCOL 인 DATE 컬럼에 대해 desc 로 정렬
   ... | pivot count(LEVEL_INT) SPLITCOL DATE SORTCOL desc

   # SPLITROW 인 HOST 컬럼에 대해 asc 로 정렬
   ... | pivot count(LEVEL_INT) SPLITROW HOST SORTROW asc

   # SPLITROW 인 HOST 와 SPLITCOL 인 DATE 컬럼에 대해 asc, desc 로 정렬
   ... | pivot count(LEVEL_INT) SPLITROW HOST SPLITCOL DATE SORTROW asc SORTCOL desc


Parameters
----------

.. code-block:: python

   ... | pivot FUNCTION (ASLIAS)? (, FUNCTION (ASLIAS)?)* (SPLITROW FIELD_NAME(, FIELD_NAME)*)? (SPLITCOL FIELD_NAME)? (FILTER filter_expr)? (COLSIZE N)? ((SORT order)? | (SORTROW order)? (SORTCOL order)?)

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - FUNCTION
     - ``FUNC(FIELD_NAME)`` 입니다. 지원하는 *\ ``FUNC``\ 의 종류는 아래 표를 참조해주세요. ``FIELD_NAME``\ 은 field 이름을 뜻합니다.\ :raw-html-m2r:`<br />`\ 예 : avg(fieldA), count(fieldB), ...
     - 필수
   * - ASLIAS
     - ``AS FIELD_NAME`` 입니다. ``AS``\ 는 예약어 이며 ``FIELD_NAME``\ 은 field 이름을 뜻합니다.\ :raw-html-m2r:`<br />` 예 : avg(fieldA) as avg_fieldA
     - 옵션
   * - SPLITROW
     - ``SPLITROW``\ 는 예약어이며, 여기에 정의된 field를 그룹핑하여 출력합니다. 각 ``FIELD_NAME``\ 는 ``,`` 으로 구분 됩니다.\ :raw-html-m2r:`<br />`\ 예 : splitrow fieldA, fieldB
     - 옵션
   * - SPLITCOL
     - ``SPLITCOL``\ 은 예약어이며, 여기에 정의된 field를 그룹핑하여 가로축으로 피봇하여 출력합니다. 즉 field의 데이터가 컬럼명이 됩니다.\ :raw-html-m2r:`<br />`\ 예 : splitcol fieldA
     - 옵션
   * - FILTER filter_expr
     - ``FILTER``\ 는 예약어이며 ``filter_expr``\ 은 filter 조건을 뜻합니다.\ :raw-html-m2r:`<br />` 예 : filter fieldA='valueA'
     - 옵션
   * - COLSIZE N
     - ``COLSIZE``\ 는 예약어이며 ``N``\ 은 몇 개의 컬럼을 보여 줄 지에 대한 개수입니다.\ :raw-html-m2r:`<br />`\ 이 때, 컬럼의 개수에 해당하는 것은 ``SPLITCOL``\ 로 지정된 필드의 피벗 결과 컬럼의 개수입니다. ``SPLITROW``\ 의 필드와는 관계가 없습니다.\ :raw-html-m2r:`<br />`\ 예 : colsize 10
     - 옵션
   * - SORT order
     - 삭제될 옵션
     - 옵션
   * - SORTROW order
     - ``SORTROW``\ 는 예약어이며 ``order``\ 은 ``asc/desc``\ 의 값이 들어 갑니다. ``SPLITROW``\ 로 지정된 필드에 대한 Sort 결과를 나타내 줍니다.\ :raw-html-m2r:`<br />`\ 예 : sortrow desc
     - 옵션
   * - SORTCOL order
     - ``SORTCOL``\ 은 예약어이며 ``order``\ 은 ``asc/desc``\ 의 값이 들어 갑니다. ``SPLITCOL``\ 로 지정된 필드의 피벗 결과에 대한 Sort 결과를 나타내 줍니다.\ :raw-html-m2r:`<br />`\ 예 : sortcol desc
     - 옵션
   * - ``order``
     - ``ASC``\ , ``DESC``\ 는 일반적인 정렬을 의미합니다.\ :raw-html-m2r:`<br />`\ 요일 정렬: ``WEEK ASC``\ , ``WEEK DESC``\ :raw-html-m2r:`<br />`\ 달 정렬: ``MONTH ASC``\ , ``MONTH DESC``\ :raw-html-m2r:`<br />`\ 계절 정렬: ``SEASON ASC``\ , ``SEASON DESC``
     - 옵션


\ ``FUNC``\ 의 종류

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 지원 타입
   * - ``avg()``
     - 평균 값을 구합니다.
     - ``TEXT``\ , ``BINARY``\ , ``BOOLEAN`` 불가능
   * - ``count()``
     - 카운트를 구합니다.
     - 모든Type 가능
   * - ``first()``
     - 첫 번째 값을 구합니다.
     - 모든Type 가능
   * - ``last()``
     - 마지막 값을 구합니다.
     - 모든Type 가능
   * - ``max()``
     - 제일 큰 값을 구합니다.
     - ``TEXT``\ , ``BINARY``\ , ``BOOLEAN`` 불가능
   * - ``min()``
     - 제일 작은 값을 구합니다.
     - ``TEXT``\ , ``BINARY``\ , ``BOOLEAN`` 불가능
   * - ``median()``
     - 중간 값을 구합니다.
     - ``TEXT``\ , ``BINARY``\ , ``BOOLEAN`` 불가능
   * - ``sum()``
     - 전체 값을 구합니다.
     - ``TEXT``\ , ``BINARY``\ , ``BOOLEAN`` 불가능
   * - ``stddev()``
     - 표준편차 값을 구합니다.
     - ``TEXT``\ , ``BINARY``\ , ``BOOLEAN`` 불가능
   * - ``stddev_samp()``
     - 표준편차 값을 구합니다.
     - ``TEXT``\ , ``BINARY``\ , ``BOOLEAN`` 불가능
   * - ``stddev_pop()``
     - 모표준편차 값을 구합니다.
     - ``TEXT``\ , ``BINARY``\ , ``BOOLEAN`` 불가능
   * - ``variance()``
     - 표본분산 값을 구합니다.
     - ``TEXT``\ , ``BINARY``\ , ``BOOLEAN`` 불가능
   * - ``var_samp()``
     - 표본분산 값을 구합니다.
     - ``TEXT``\ , ``BINARY``\ , ``BOOLEAN`` 불가능
   * - ``var_pop()``
     - 모분산 값을 구합니다.
     - ``TEXT``\ , ``BINARY``\ , ``BOOLEAN`` 불가능
   * - ``countDistinct()``
     - 유니크한 값의 갯수를 구합니다.
     - 모든Type 가능


요일 정렬

아래 이름이나 별명에 대해 우선적으로 정렬합니다.

.. list-table::
   :header-rows: 1

   * - 이름
     - 별명
     - 설명
   * - Monday
     - MON
     - 월요일
   * - Tuesday
     - TUE
     - 화요일
   * - Wednesday
     - WED
     - 수요일
   * - Thursday
     - THU
     - 목요일
   * - Friday
     - FRI
     - 금요일
   * - Saturday
     - SAT
     - 토요일
   * - Sunday
     - SUN
     - 일요일


달 정렬

아래 이름이나 별명에 대해 우선적으로 정렬합니다.

.. list-table::
   :header-rows: 1

   * - 이름
     - 별명
     - 설명
   * - January
     - JAN
     - 1월
   * - February
     - FEB
     - 2월
   * - March
     - MAR
     - 3월
   * - April
     - APR
     - 4월
   * - May
     - 
     - 5월
   * - June
     - 
     - 6월
   * - July
     - 
     - 7월
   * - August
     - AUG
     - 8월
   * - September
     - SEPT
     - 9월
   * - October
     - OCT
     - 10월
   * - November
     - NOV
     - 11월
   * - December
     - DEC
     - 12월


계절 정렬

아래 이름에 대해 우선적으로 정렬합니다.

.. list-table::
   :header-rows: 1

   * - 이름
     - 의미
   * - spring
     - 봄
   * - summer
     - 여름
   * - fall, autumn
     - 가을
   * - winter
     - 겨울


Examples
----------------------------------------------------------------------------------------------------

- 예제 데이터

.. list-table::
   :header-rows: 1

   * - sepal_length
     - sepal_width
     - speceis
   * - 5.1
     - 3.5
     - Iris-setosa
   * - 4.9
     - 3.0
     - Iris-setosa
   * - 4.7
     - 3.2
     - Iris-setosa
   * - 3.7
     - 4.7
     - Iris-setosa
   * - 5.8
     - 8.2
     - Iris-setosa
   * - 7.3
     - 2.6
     - Iris-setosa
   * - 7.4
     - 5.4
     - Iris-setosa
   * - 6.5
     - 7.8
     - setosa
   * - 6.2
     - 4.7
     - setosa
   * - 5.9
     - 12.5
     - setosa
   * - 4.3
     - 5.2
     - setosa
   * - 5.7
     - 7.3
     - setosa
   * - 5.2
     - 3.8
     - setosa
   * - 2.5
     - 7.1
     - setosa


* count, avg, stddev, min, max, median, sum  통계 &  SPLITROW Species
    * ``Species``  는 3개 종이므로 SPLITROW Species 는 3개의 행으로 split 되어 결과가 나옵니다.
    * ``Species``  이름으로 그룹핑 된 결과 에서  갯수, ``sepal_width`` 필드의 평균, 표준편차, 최소값, 최대값, 중간값, 합계를 구합니다.

* SORTROW 
    * ``SPLITROW Species SORTROW desc`` 는  Species 가 행으로 split 된 결과를 내림차순으로 표시합니다.

.. code-block:: python

   *  | pivot count(*) as 개수,  
              avg(sepal_width) as 평균_sepal_width,  
              stddev(sepal_width) as 표준편차_sepal_width,
              min(sepal_width) as 최소값_sepal_width, 
              max(sepal_width) as 최대값_sepal_width,
              median(sepal_width) as 중간값_epal_width,  
              sum(sepal_width) as 합계_sepal_width
        SPLITROW Species SORTROW desc


.. list-table::
   :header-rows: 1

   * - species
     - 개수
     - 평균_sepal_width
     - 표본표준편차_sepal_width
     - 모표준편차_sepal_width
     - 최소값_sepal_width
     - 최대값_sepal_width
     - 중간값_epal_width
     - 합계_sepal_width
     - 분산_sepal_width
   * - Iris-setosa
     - 7
     - 4.371428571428572
     - 1.9567952419830796
     - 1.8116403661672287
     - 2.6
     - 8.2
     - 3.5
     - 30.6
     - 3.829047619047619
   * - setosa
     - 7
     - 6.914285714285714
     - 2.8783262332060113
     - 2.6648122804047416
     - 3.8
     - 12.5
     - 7.1
     - 48.4
     - 8.284761904761906


* count, avg  통계 &  SPLICOL Species & SORTCOL
    * SPLITCOL Species 는  ``Species결과_함수명(alias)`` 가 컬럼으로 생성되어 보여집니다.


.. code-block:: python

    *  | pivot count(*) as 개수, avg(sepal_width) as 평균_sepal_width SPLITCOL Species SORTCOL desc

.. list-table::
   :header-rows: 1

   * - Iris-setosa_평균_sepal_width
     - Iris-setosa_개수
     - setosa_평균_sepal_width
     - setosa_개수
   * - 6.914285714285714
     - 7
     - 4.371428571428572
     - 7


* countDistinct 

.. code-block:: python

    *  | pivot countDistinct(Species) 

.. list-table::
   :header-rows: 1

   * - countDistinct
   * - 2

- 예제 데이터 2

.. list-table::
   :header-rows: 1

   * - DATETIME
     - HOST
   * - "2020-07-03 12:14:00"
     - gcs1
   * - "2020-07-03 12:24:00"
     - gcs1
   * - "2020-07-05 12:34:00"
     - gcs1
   * - "2020-07-03 11:34:00"
     - gcs1
   * - "2020-07-04 04:34:00"
     - gcs1
   * - "2020-07-03 04:34:00"
     - gcs2
   * - "2020-07-04 02:34:00"
     - gcs2
   * - "2020-07-03 01:34:00"
     - gcs2
   * - "2020-07-04 05:34:00"
     - gcs2
   * - "2020-07-05 03:34:00"
     - gcs2
   * - "2020-07-04 12:13:00"
     - gcs2
   * - "2020-07-03 12:14:00"
     - gcs2

* HOST 별로 10시간 단위로 로그 COUNT 를 구합니다. ``SPLITROW 필드,필드 SORTROW asc/desc``

.. code-block:: python

    * | pivot count(*) as CNT SPLITROW date_group(DATETIME, 10H) as TIME, HOST SORTROW asc

.. list-table::
   :header-rows: 1

   * - TIME
     - HOST
     - CNT
   * - 2020-07-03 00:00:00
     - gcs2
     - 2
   * - 2020-07-03 10:00:00
     - gcs1
     - 3
   * - 2020-07-03 10:00:00
     - gcs2
     - 1
   * - 2020-07-04 00:00:00
     - gcs1
     - 1
   * - 2020-07-04 00:00:00
     - gcs2
     - 2
   * - 2020-07-04 10:00:00
     - gcs2
     - 1
   * - 2020-07-05 00:00:00
     - gcs2
     - 1
   * - 2020-07-05 10:00:00
     - gcs1
     - 1
     
