concat
======

개요
----

선택한 컬럼을 concatenation 하는 명령어 입니다

설명
----

``,`` 를 이용해서 필드명을 구분 하여 concatenation 을 하고자 하는 필드들을 지정 할 수 있습니다.
필드 명이 복잡한 경우 single quote(``'``) 를 이용해서 감싸주어 사용 할 수 있습니다.
문자 자체를 이용하고 싶을 때는 double quote(``"``) 를 이용해서 감싸주어 사용 할 수 있습니다.

Examples
----------------------------------------------------------------------------------------------------

- 예제용 데이터 모양 입니다.

.. list-table::
   :header-rows: 1

   * - 한글
     - 알파벳
     - 숫자
     - 혼, 합-문자
   * - 가
     - a
     - 1
     - 가a1
   * - 나
     - b
     - 2
     - 나b2
   * - 다
     - c
     - 3
     - 다c3

- 필드 끼리 concatenation 하는 예제입니다.

.. code-block:: none

   ... | concat 한글, 알파벳, 숫자

.. list-table::
   :header-rows: 1

   * - 한글
     - 알파벳
     - 숫자
     - 혼, 합-문자
     - concated
   * - 가
     - a
     - 1
     - 가a1
     - 가a1
   * - 나
     - b
     - 2
     - 나b2
     - 나b2
   * - 다
     - c
     - 3
     - 다c3
     - 다c3

- 문자를 이용해서 각 필드를 연결 시키는 예제입니다. (double quote(``"``) 를 이용합니다.)

.. code-block:: none

   ... | concat 한글, "-", 알파벳, "-", 숫자

.. list-table::
   :header-rows: 1

   * - 한글
     - 알파벳
     - 숫자
     - 혼, 합-문자
     - concated
   * - 가
     - a
     - 1
     - 가a1
     - 가-a-1
   * - 나
     - b
     - 2
     - 나b2
     - 나-b-2
   * - 다
     - c
     - 3
     - 다c3
     - 다-c-3

- ``,`` 가 포함된 필드명 선택 및 복잡한 필드명을 선택하는 예제입니다. (single quote(``'``) 를 이용합니다.)

.. code-block:: none

   ... | concat 한글, ",", '혼, 합-문자', "-", 숫자

.. list-table::
   :header-rows: 1

   * - 한글
     - 알파벳
     - 숫자
     - 혼, 합-문자
     - concated
   * - 가
     - a
     - 1
     - 가a1
     - 가,가a1-1
   * - 나
     - b
     - 2
     - 나b2
     - 나,나b2-2
   * - 다
     - c
     - 3
     - 다c3
     - 다,다c3-3


Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | concat ('|")?(FIELD_NAME|STRING)('|")? (, ('|")?(FIELD_NAME|STRING)('|")?)*

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - FIELD_NAME
     - field의 이름을 의미합니다.
     - 필수
   * - STRING
     - concatenation 에 사용할 문자열 입니다.
     - 필수
   * - ``'`` or ``"``
     - single quote는 필드명을 묶어 줄 때 사용합니다.\ :raw-html-m2r:`<br />`\ double quote는 문자 자체를 concatenation 할 때 사용합니다.
     - 옵션


Parameters BNF
----------------------------------------------------------------------------------------------------

.. code-block:: none

   fields : token
          | fields COMMA token

   token : terms
         | DQ_TERM
         | SQ_TERM

   terms : TERM
         | terms TERM

   t_SQ_TERM = '[^']+'
   t_DQ_TERM = "[^"]+"
   COMMA : ,
   TOKEN : [^ |^,|^"|^']+
