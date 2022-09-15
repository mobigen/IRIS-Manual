spath
==========

개요
------

json/xml 과 같은 구조화된 데이터의 key 를 이용해서 해당하는 데이터를 추출하는 명령어


컬럼 타입
----------------------------------------------------------------------------------------------------
TEXT

설명
------

| 구조화된 데이터의 key path 를 fullpath(처음부터 순차적)로 지정하여 해당하는 값을 가져옵니다.
| astarisk (``*``) 를 사용하면 해당 위치의 모든 key 값을 찾습니다.
| index 는 ``key{0}`` 를 이용하여 표기 할 수 있습니다. 숫자만 가능합니다. (``key{}`` 와 ``key`` 는 동일합니다.)
| xml 에서 attribute 의 값을 가져오기 위해서는 ``key{@name}`` 과 같이 ``@`` 를 사용합니다.

ex) 아래 데이터에서 COMMON 데이터 가져오기 -> ``CATALOG.PLANT.COMMON``

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?> 
   <CATALOG> 
     <PLANT> 
       <COMMON>Bloodroot</COMMON> 
       <BOTANICAL>Sanguinaria canadensis</BOTANICAL> 
       <ZONE>4</ZONE> 
       <LIGHT>Mostly Shady</LIGHT> 
       <PRICE>$2.44</PRICE> 
       <AVAILABILITY>031599</AVAILABILITY> 
     </PLANT> 
     ...
   <CATALOG/>

Parameters
------------

.. code-block:: python

   ... | spath (input=<column name>)? (format=<json/xml>)? path=<full path>

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
     - Defualt
   * - input
     - json/xml 데이터가 들어있는 컬럼명
     - 옵션
     - _raw
   * - format
     - 데이터의 포멧
     - 옵션
     - json
   * - path
     - 구조화 데이터의 key path. 항상 full path 로 기록해야한다.
     - 필수
     - 

Example
----------

- 예제 - json

.. list-table::
   :header-rows: 1

   * - _raw
   * - |pre_start| { |br|   "glossary": { |br|     "title": "example glossary", |br|     "GlossDiv": { |br|       "title": "S", |br|       "GlossList": { |br|         "GlossEntry": { |br|           "ID": "SGML", |br|           "SortAs": "SGML", |br|           "GlossTerm": "Standard Generalized Markup Language", |br|           "Acronym": "SGML", |br|           "Abbrev": "ISO 8879:1986", |br|           "GlossDef": { |br|             "para": "A meta-markup language, used to create markup languages such as DocBook.", |br|             "GlossSeeAlso": ["GML", "XML"] |br|           }, |br|           "GlossSee": "markup" |br|         } |br|       } |br|     } |br|   } |br| } |pre_end|

.. code-block:: python

   * | spath path = glossary.GlossDiv.GlossList.GlossEntry.*

결과

.. list-table::
   :header-rows: 1

   * - ID
     - SortAs
     - GlossTerm
     - Acronym
     - Abbrev
     - GlossDef
     - GlossSee
   * - SGML
     - SGML
     - Standard Generalized Markup Language
     - SGML
     - ISO 8879:1986
     - {"para": "A meta-markup language, used to create markup languages such as DocBook.", "GlossSeeAlso": ["GML", "XML"]}
     - markup

- 예제 - xml

.. list-table::
   :header-rows: 1

   * - _raw
   * - |pre_start| <?xml version="1.0" encoding="UTF-8"?> |br| <CATALOG> |br|   <PLANT> |br|     <COMMON>Bloodroot</COMMON> |br|     <BOTANICAL>Sanguinaria canadensis</BOTANICAL> |br|     <ZONE>4</ZONE> |br|     <LIGHT>Mostly Shady</LIGHT> |br|     <PRICE>$2.44</PRICE> |br|     <AVAILABILITY>031599</AVAILABILITY> |br|   </PLANT> |br|   ... |br| <CATALOG/> |pre_end|

.. code-block:: python

   * | spath path = CATALOG.PLANT.*

결과

.. list-table::
   :header-rows: 1

   * - COMMON
     - BOTANICAL
     - ZONE
     - LIGHT
     - PRICE
     - AVAILABILITY
   * - Bloodroot
     - Sanguinaria canadensis
     - 4
     - Mostly Shady
     - $2.44
     - 031599

.. |br| raw:: html

  <br/>

.. |pre_start| raw:: html

  <pre>

.. |pre_end| raw:: html

  <pre/>
