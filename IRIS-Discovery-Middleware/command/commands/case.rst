case
======================

개요
-------------------------

조건 처리가 가능한 명령어

설명
--------------------------

"* | case when 조건절(and/or)* then 컬럼명/값 (when ....)* otherwise 컬럼명/값 as 컬럼명"

 > 조건절에는 `and/or` 를 포함할 수 있다.

`when`, `then`, `otherwise`, `as`, `and`, `or` 각각의 키워드는 대소문자의 구분이 없다.

컬럼명에는 원하는 컬럼명을 `\` \`` 혹은 따옴표 없이 넣을 수 있다.

 > 띄어쓰기가 포함된 컬럼명은 `\` \`` 를 통해 넣을 수 있다.

값에는 원하는 문자열을 `' '` 를 통해 넣을 수 있다.

값에는 원하는 숫자를 따옴표 없이 넣을 수 있다.

`as` 뒤의 컬럼명에는 `\` \`` 혹은 따옴표 없이 컬럼명을 설정할 수 있다.

 > 띄어쓰기가 포함된 컬럼명은 `\` \`` 를 통해 넣을 수 있다.

`when-then`, `and/or` 는 중복해서 사용 가능하다.

`then` , `otherwise` 에 null값을 null 또는 'null' 을 이용하여 대입할 수 있다.
 
 > 'null'을 이용하면 문자열로 대입된다.

Parameters
------------------------------------

.. code-block:: none

  조건 = (COLUMN / `COLUMN` / NUMBER / 'STRING'/ and / or)
  값 = (COLUMN / `COLUMN` / NUMBER / 'STRING')
   ... | case when 조건 then 값
   ... | case when 조건 then 값 otherwise 값
   ... | case when 조건 then 값 otherwise 값 as 값

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - 조건
     - 조건을 입력하는 부분입니다. and / or를 포함합니다
     - 필수
   * - 값
     - 값을 입력하는 부분입니다. and / or를 포함하지 않습니다
     - 필수
   * - COLUMN / \`COLUMN\`
     - 원하는 컬럼을 조건 혹은 값에 설정할 수 있습니다.
     - 필수
   * - NUMBER
     - 원하는 숫자를 조건 혹은 값에 설정할 수 있습니다.
     - 필수
   * - 'STRING'
     - 원하는 문자열을 조건 혹은 값에 설정할 수 있습니다.
     - 필수
   * - when
     - Spark SQL의 WHEN에 해당합니다.
     - 필수
   * - then
     - Spark SQL의 THEN에 해당합니다.
     - 필수
   * - otherwise
     - Spark SQL의 OTHERWISE에 해당합니다.
     - 옵션
   * - as
     - Spark SQL의 AS에 해당합니다.
     - 옵션 
   * - and
     - Spark SQL의 AND에 해당합니다.
     - 옵션 
   * - or
     - Spark SQL의 OR에 해당합니다.
     - 옵션 

- 기본적으로 SQL의 문법을 준수합니다.


Example
----------------------------------

- 예제용 데이터 모양 입니다.

.. list-table:: none
   :header-rows: 1

   * - A
     - B
     - C
     - D
   * - 일반
     - 1
     - 10.123
     - ab
   * - 특수
     - 2
     - 20.123
     - ab
   * - 일반
     - 3
     - 30.123
     - ef
   * - 특수
     - 4
     - 40.123
     - gh
   * - 옵션
     - 5
     - 50.123
     - ij

- A 컬럼을 이용하여 조건을 주어 result 컬럼을 생성하여 결과를 출력하는 예제입니다.

.. code-block:: none

   ... | case when A='일반' then '일반입니다.' when A='특수' then '특수입니다.' otherwise '다른타입입니다.'

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
     - result
   * - 일반
     - 1
     - 10.123
     - ab
     - 일반입니다.
   * - 특수
     - 2
     - 20.123
     - ab
     - 특수입니다.
   * - 일반
     - 3
     - 30.123
     - ef
     - 일반입니다.
   * - 특수
     - 4
     - 40.123
     - gh
     - 특수입니다.
   * - 옵션
     - 5
     - 50.123
     - ij
     - 타른타입입니다.

- B 컬럼을 이용하여 조건을 주어 what is this 컬럼을 생성하여 결과를 출력하는 예제입니다.

.. code-block:: none

   ... | case when B>2 then 'three upper' otherwise 'two lower' as `what is this`

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
     - what is this
   * - 일반
     - 1
     - 10.123
     - ab
     - two lower
   * - 특수
     - 2
     - 20.123
     - ab
     - two lower
   * - 일반
     - 3
     - 30.123
     - ef
     - three upper
   * - 특수
     - 4
     - 40.123
     - gh
     - three upper
   * - 옵션
     - 5
     - 50.123
     - ij
     - three upper

- C 컬럼을 이용하여 조건을 주어 result 컬럼을 생성하여 결과를 출력하는 예제입니다.

.. code-block:: none

   ... | when C*3+4 > 90 then 'TRUE' otherwise 'FALSE'

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
     - result
   * - 일반
     - 1
     - 10.123
     - ab
     - FALSE
   * - 특수
     - 2
     - 20.123
     - ab
     - FALSE
   * - 일반
     - 3
     - 30.123
     - ef
     - TRUE
   * - 특수
     - 4
     - 40.123
     - gh
     - TRUE
   * - 옵션
     - 5
     - 50.123
     - ij
     - TRUE

- A 컬럼과 B 컬럼 C 컬럼을 이용하여 조건을 주어 test result 컬럼을 생성하여 결과를 출력하는 예제입니다.

.. code-block:: none

   ... | when `A`='일반' and `B`>2 then 'A,B result' when `C`=10.123 then 'C result' otherwise 0 as `test result`

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
     - test result
   * - 일반
     - 1
     - 10.123
     - ab
     - C result
   * - 특수
     - 2
     - 20.123
     - ab
     - 0
   * - 일반
     - 3
     - 30.123
     - ef
     - A and B result
   * - 특수
     - 4
     - 40.123
     - gh
     - 0
   * - 옵션
     - 5
     - 50.123
     - ij
     - 0

.. code-block:: none

   ... | when B<=5 then null

.. list-table::
   :header-rows: 1

   * - A
     - B
     - C
     - D
     - result
   * - 일반
     - 1
     - 10.123
     - ab
     - null
   * - 특수
     - 2
     - 20.123
     - ab
     - null
   * - 일반
     - 3
     - 30.123
     - ef
     - null
   * - 특수
     - 4
     - 40.123
     - gh
     - null
   * - 옵션
     - 5
     - 50.123
     - ij
     - null
