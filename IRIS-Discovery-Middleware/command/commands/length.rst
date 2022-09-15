length
==========

개요
------

string length를 계산 하는 명령어입니다.

타입
----------------------------------------------------------------------------------------------------
TEXT

설명
------

Arguments으로 받은 column 혹은 string의 length를 row별로 계산을 한다.


* ``* | length column_name``
* ``* | length `column_name```
* ``* | length `column_name` AS my_column_name``
* ``* | length `column_name` AS `my column name```
* ``* | length 'this is a text'``
* ``* | length 'this is a text' AS custom_text``
* ``* | length 'this is a text' AS `custom text```
* ``* | length colA, `col B` as B, 'my text', 'another text' as `my another text```


Example
----------

- 예제 데이터

.. list-table::
   :header-rows: 1

   * - column_1
     - column 2
   * - sample data
     - another data


- 예제 명령어 1

.. code-block:: python

   * | length column_1, column_1 as `alias column 1`, `column 2` as alias_column_2,  `column 2` as `alias column 2 with spaces`, 'my text', 'my text' as `custom text`


결과

.. list-table::
   :header-rows: 1

   * - column_1
     - column 2
     - LENGTH(column_1)
     - alias column 1
     - alias_column_2
     - alias column 2 with space
     - LENGTH(my text)
     - custom text
   * - sample data
     - another data
     - 11
     - 11
     - 12
     - 12
     - 7
     - 7

