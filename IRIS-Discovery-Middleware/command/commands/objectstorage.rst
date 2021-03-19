objectstorage
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

objectstorage 읽기 및 쓰기를 하는 명령어 입니다.

설명
----------------------------------------------------------------------------------------------------

해당 명령어를 통해 objectstorage 로 데이터를 읽기 및 쓰기를 할 수 있습니다.

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: python

   ... | objectstorage SOURCE [WRITE|READ] (OPTION(key 'value' (, key 'value')*))?

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - ``SOURCE``
     - 데이터의 타입을 나타내며 ``csv`` , ``json`` , ``parquet`` 와 ``excel`` 등이 될 수 있습니다.
     - 필수
   * - [WRITE / READ]
     - ``WRITE`` 와 ``READ``\ 는 키워드 이며, 읽을 것인지 쓸 것인지를 나타냅니다.
     - 필수
   * - ``(OPTION(key 'value' (, key 'value')*))?``
     - ``OPTION`` 은 키워드 이며, ``KEY`` 와 ``VALUE`` 는 그에 해당하는 옵션이 될 수 있습니다.
       해당 옵션들은 Spark의 read 및 write 옵션과 동일합니다. 또한, ``outputNum`` 옵션으로 output의 file 갯수를 조절 할 수 있습니다.
       **option 중 connector_id 는 필수로 작성을 해야합니다.**
     - 옵션


``OPTION`` 의 종류

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 기본 값
   * - header
     - uses the first line as names of columns
     - True
   * - inferSchema
     - infers the input schema automatically from data. It requires one extra pass over the data
     - False
   * - ignoreLeadingWhiteSpace
     - a flag indicating whether or not leading whitespaces from values being read should be skipped.
     - True
   * - ignoreTrailingWhiteSpace
     - a flag indicating whether or not trailing whitespaces from values being read should be skipped
     - True
   * - multiLine
     - parse one record, which may span multiple lines
     - False
   * - sep
     - sets a single character as a separator for each field and value
     - ','

Examples
----------------------------------------------------------------------------------------------------

* 아래 예제는 앞서 처리된 값을 ``/tmp/path.csv`` 라는 파일로 write하는 예제 입니다.

.. code-block:: python

   ... | objectstorage csv write /tmp/path option(connector_id '177')


* ``/tmp/path/test.csv`` 를 read 하는 예제입니다.

.. code-block:: python

   ... | objectstorage csv read /tmp/path/test.csv option(connector_id '177')
