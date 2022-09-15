.. role:: raw-html-m2r(raw)
   :format: html


pylambda
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

해당 명령어는 Python의 lambda 함수를 실행 합니다.

타입
----------------------------------------------------------------------------------------------------
TEXT, INTEGER, BIGINT, REAL, DATE, TIMESTAMP

설명
----------------------------------------------------------------------------------------------------

검색 결과에 Python lambda 함수를 실행합니다. 입력은 전달 되어진 테이블의 각 열을 의미하며, 해당 열을 가공하여 출력으로 보낼 수 있습니다. ``WITH`` 절에는 출력의 스키마를 입력해야 합니다. 만약 해당 부분이 생략 된다면, 자동으로 출력의 스키마를 계산합니다. 해당 계산 과정이 정확하지 않을 수 있고, 데이터가 많은 경우에는 많은 시간을 소요 할 수 있기 때문에, 스키마를 명시적으로 입력 하는 것이 좋습니다.


Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none 

   ... | pylambda lambda_expr (WITH schema)? (IMPORT built-in-module(, built-in-module)*)?
   ... | pylambda lambda_expr (--use_input_schema)? (IMPORT built-in-module(, built-in-module)*)?

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - lambda_expr
     - Python 의 lambda expression을 의미합니다. 단, ``lambda`` 키워드는 포함하지 않습니다. 또한, 인자는 ``row`` 하나만 들어오게 됩니다.\ :raw-html-m2r:`<br />`\ 예 : row: row[0].replace('2018', '**** ') (O)\ :raw-html-m2r:`<br />`\ 예 : lambda row: row[0].replace('2018', '****') (X)
     - 필수
   * - WITH schema
     - ``WITH`` 는 스키마를 직접 지정하겠음을 지시하는 예약어이며, ``schema``\ 는 schema 표현으로 col_name: * data_type 포맷입니다.\ :raw-html-m2r:`<br />`\ 예 : WITH column1: string, column2: int
     - 옵션
   * - --use_input_schema
     - input 과 output 의 데이터프레임 컬럼수와 순서가 동일하면 input 의 컬럼명을 output 으로 출력하게 하는 옵션
     - 옵션  
   * - IMPORT
     - ``IMPORT`` 는 예약어, python의 built-in 모듈을 import 하기 위해 사용되는 옵션. 예 : import re, datetime
     - 옵션


* data_type

.. list-table::
   :header-rows: 1

   * - 타입
   * - INT
   * - BIGINT
   * - BOOLEAN
   * - FLOAT
   * - DOUBLE
   * - STRING
   * - BINARY
   * - TIMESTAMP
   * - DECIMAL
   * - DECIMAL(precision, scale)
   * - DATE

Examples
----------------------------------------------------------------------------------------------------

- 예제용 데이터 모양 입니다.

.. list-table::
   :header-rows: 1

   * - YEAR
     - LEVEL
     - HOST
   * - 2018id
     - abc
     - 123
   * - 2019ac
     - bcd
     - 456
   * - 2018ca
     - gcs
     - 789
   * - 2019df
     - infoo
     - 101

- 0번째 컬럼의 데이터중 ``2018``\ 이라는 숫자 string 을 ``****``\ 으로 변환

.. code-block:: none 

   ..| pylambda row: row[0].replace('2018', '****')

.. list-table::
   :header-rows: 1

   * - _1
   * - \****id
   * - 2019ac
   * - \****ca
   * - 2019df

.. code-block:: none 

   ..| pylambda row: row[0].replace('2018', '****') WITH output: STRING

.. list-table::
   :header-rows: 1

   * - output
   * - \****id
   * - 2019ac
   * - \****ca
   * - 2019df

- 입력 데이터에 1번째 컬럼의 데이터를 추가

.. code-block:: none 

   ..| pylambda row: row + [row[1]]

.. list-table::
   :header-rows: 1

   * - _1
     - _2
     - _3
     - _4
   * - 2018id
     - abc
     - 123
     - abc
   * - 2019ac
     - bcd
     - 456
     - bcd
   * - 2018ca
     - gcs
     - 789
     - gcs
   * - 2019df
     - infoo
     - 101
     - infoo

.. code-block:: none 

   ..| pylambda row: row + [row[0]] WITH a: string, b: string, c: int, d: string

.. list-table::
   :header-rows: 1

   * - a
     - b
     - c
     - d
   * - 2018id
     - abc
     - 123
     - abc
   * - 2019ac
     - bcd
     - 456
     - bcd
   * - 2018ca
     - gcs
     - 789
     - gcs
   * - 2019df
     - infoo
     - 101
     - infoo

입력 데이터의 1번째 컬럼이 "abc" 를 포함하는지 각 bool 값을 출력

.. code-block:: none 

   ..| pylambda row: "abc" in row[1]

.. list-table::
   :header-rows: 1

   * - _1
   * - true
   * - false
   * - false
   * - false

.. code-block:: none 

   ..| pylambda row: "abc" in row[1] WITH output: boolean

.. list-table::
   :header-rows: 1

   * - output
   * - true
   * - false
   * - false
   * - false

입력 데이터의 LEVEL 필드를 선택하여 필드의 값에서 ``info``\ 라는 값을 를 ``****``\ 으로 변환

.. code-block:: none 

   * | fields YEAR, LEVEL | pylambda row: [r.replace('info', '****') for r in row]

.. list-table::
   :header-rows: 1

   * - _1
     - _2
   * - 2018id
     - abc
   * - 2019ac
     - bcd
   * - 2018ca
     - gcs
   * - 2019df
     - \****o

.. code-block:: none 

   * | fields YEAR, LEVEL | pylambda row: [r.replace('info', '****') for r in row] WITH log_year: string log_level: string

.. list-table::
   :header-rows: 1

   * - log_year
     - log_level
   * - 2018id
     - abc
   * - 2019ac
     - bcd
   * - 2018ca
     - gcs
   * - 2019df
     - \****o

``re`` 모듈을 임포트 하여 정규식 사용하는 예제

.. code-block:: none 

   * | pylambda row: True if re.search('gcs', row[1]) else False import re

.. list-table::
   :header-rows: 1

   * - _1
   * - false
   * - false
   * - true
   * - false

input 과 output 의 데이터프레임 컬럼수와 순서가 동일할 때 input 의 컬럼명을 output 으로 출력하게 하는 옵션

.. code-block:: none 

   * | pylambda x: x --use_input_schema

.. list-table::
   :header-rows: 1

   * - YEAR
     - LEVEL
     - HOST
   * - 2018id
     - abc
     - 123
   * - 2019ac
     - bcd
     - 456
   * - 2018ca
     - gcs
     - 789
   * - 2019df
     - infoo
     - 101
