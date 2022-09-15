replace
========

개요
----

선택한 컬럼의 데이터의 글자를 변환하는 명령어 입니다.

타입
----------------------------------------------------------------------------------------------------
TEXT

설명
----

* python 의 replace 명령어와 같이, old 문자열을 new 문자열로 치환해주는 명령어입니다.
* TEXT column 에서만 사용되는 ' '(empty string) 과 ``null`` 은 다른 값 입니다.

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | replace FIELD_NAME OLD NEW

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - FIELD_NAME
     - field의 이름을 의미합니다.
     - 필수
   * - OLD
     - 대체될(old) 문자열 입니다.
     - 필수
   * - NEW
     - 대체하려는(new) 문자열 입니다.
     - 필수


Examples
----------------------------------------------------------------------------------------------------

- 예제용 데이터 모양 입니다.

.. list-table::
   :header-rows: 1

   * - A
     - B
   * - abc:abc:abc
     - a
   * - abc:abc-abc
     - b
   * - abc-abc:abc
     - c

- A 컬럼의 ``-`` 문자를 ``:`` 문자로 치환하는 예제입니다.

.. code-block:: none

   ... | replace A "-" ":"

.. list-table::
   :header-rows: 1

   * - A
     - B
   * - abc:abc:abc
     - a
   * - abc:abc:abc
     - b
   * - abc:abc:abc
     - c

- A 컬럼의 ``abc`` 문자를 ``xyz`` 문자로 치환하는 예제입니다.

.. code-block:: none

   ... | replace A "abc" "xyz"

.. list-table::
   :header-rows: 1

   * - A
     - B
   * - xyz:xyz:xyz
     - a
   * - xyz:xyz-xyz
     - b
   * - xyz-xyz:xyz
     - c
