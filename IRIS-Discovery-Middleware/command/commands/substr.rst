
substr
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

이 명령어는 특정한 필드나 문자열을 SUBSTR 하고자 할 때 사용됩니다.

설명
----------------------------------------------------------------------------------------------------

``FIELD`` 에 해당하는 필드를 시작위치로 부터 원하는 길이로 SUBSTR 할 수 있습니다.  

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | substr FIELD START_POSITION (LENGTH)? (AS ALIAS_NAME)?

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - FIELD
     - SUBSTR 대상인 필드를 의미합니다.
     - 필수
   * - START_POSITION
     - SUBSTR 시작 위치를 의미합니다.(정수)
     - 필수
   * - LENGTH
     - ``START_POSITION`` 부터 SUBSTR 할 길이를 의미합니다. 생략이 가능합니다. (0이상의 정수)
     - 옵션
   * - AS ALIAS_NAME
     - ``AS`` 문자를 이용해 substr 결과를 나타낼 필드의 이름을 지정합니다. space가 포함된 문자는 single-quote(``'``)로 감싸야 합니다. (Default = SUBSTRED)
     - 옵션 

Examples
----------------------------------------------------------------------------------------------------

- 예제 데이터

.. list-table::
   :header-rows: 1

   * - A
     - B
   * - 123
     - IRON MAN
   * - 2345
     - CAPTAIN AMERICA

- ``B`` 필드의 값을 2번째 문자부터 5개 문자를 SUBSTR

.. code-block:: none

   ... | substr B 2 5

.. list-table::
   :header-rows: 1

   * - A
     - B
     - SUBSTRED
   * - 123
     - IRON MAN
     - RON M
   * - 2345
     - CAPTAIN AMERICA
     - APTAI

- ``B`` 필드의 값을 3번째 부터 끝까지 SUBSTR

.. code-block:: none

   ... | substr B 3

.. list-table::
   :header-rows: 1

   * - A
     - B
     - SUBSTRED
   * - 123
     - IRON MAN
     - ON MAN
   * - 2345
     - CAPTAIN AMERICA
     - PTAIN AMERICA

- substr 결과 필드의 이름에 별칭을 지정하는 예제

.. code-block:: none

   ... | substr B 3 as '별칭 지정'

.. list-table::
   :header-rows: 1

   * - A
     - B
     - 별칭 지정
   * - 123
     - IRON MAN
     - ON MAN
   * - 2345
     - CAPTAIN AMERICA
     - PTAIN AMERICA

- ``B`` 필드의 값을 -5번째 문자부터 끝까지 SUBSTR

.. code-block:: none

   ... | substr B -5

.. list-table::
   :header-rows: 1

   * - A
     - B
     - SUBSTRED
   * - 123
     - IRON MAN
     - N MAN
   * - 2345
     - CAPTAIN AMERICA
     - ERICA

- ``B`` 필드의 값을 -5번째 문자부터 4개 문자를 SUBSTR

.. code-block:: none

   ... | substr B -5 4

.. list-table::
   :header-rows: 1

   * - A
     - B
     - SUBSTRED
   * - 123
     - IRON MAN
     - N MA
   * - 2345
     - CAPTAIN AMERICA
     - ERIC
