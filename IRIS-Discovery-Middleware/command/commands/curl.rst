curl
==========

개요
------

Linux 의 curl 명령어와 유사한 동작을 하는 명령어입니다.

curl 을 통해 가져온 데이터를 DataFrame 으로 만들어 spark 을 통해 처리 가능하도록 합니다.
    
설명
------

Linux 의 curl 과 유사하게 동작하는 명령어로, 현재 지원되는 옵션은 세 가지 입니다.

지원되는 옵션

- ``-X``: GET | POST - 요청 메소드
- ``-H``: 요청 헤더
- ``-d``: 요청 파라미터

Parameters
------------

.. code-block:: none

   ... | curl (op)? (-X (GET/POST))? (-H Header)? (-d data)? url

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
     - Defualt
   * - op
     - curl request 에 대한 response 의 형태가 다르기 때문에 어떤 데이터 인지 지정하는 옵션 |br| (csv/json/xml/2b/2block) 사용가능. |br| 아래 테이블 참조.
     - 옵션
     - csv
   * - -X
     - 요청 method 를 선택하는 옵션, 현재 지원되는 것은 GET, POST
     - 옵션
     - GET
   * - -H
     - 요청 header 를 작성하는 옵션
     - 옵션
     - 
   * - -d
     - 요청 파라미터를 설정하는 옵션
     - 옵션
     -
   * - url
     - 요청할 url 을 입력하는 부분
     - 필수
     - 

- op 옵션의 값

.. list-table::
   :header-rows: 1

   * - 값
     - 설명
     - 비고
   * - csv
     - response 의 형태가 csv 형태, comma( ``,`` ) 를 이용해 데이터를 분리하여 n X n 테이블 생성
     - 
   * - json
     - response 의 형태가 json 형태, json string 데이터를 분해하지 않고 1 X 1 테이블 생성
     - 
   * - xml
     - response 의 형태가 xml 형태, xml string 데이터를 분해하지 않고 1 X 1 테이블 생성
     - 
   * - 2b/2block
     - response 의 형태가 2block 사이트의 데이터 반환 형태
     - 2block 사이트를 지원하기 위해 만든 옵션이므로 일반 사용자는 무시

Example
----------

- 예제 명령어 1

https://raw.githubusercontent.com/jooeungen/coronaboard_kr/master/kr_daily.csv 사이트에 있는 csv 파일 데이터를 가져오는 명령어

.. code-block:: none

    * | curl -X GET https://raw.githubusercontent.com/jooeungen/coronaboard_kr/master/kr_daily.csv 

결과

.. list-table::
   :header-rows: 1

   * - date
     - confirmed
     - ...
   * - 20200121
     - 1
     - ...
   * - 20200122
     - 3
     - ...
   * - 20200123
     - 4
     - ...
   * - ...
     - ...
     - ...

- 예제 명령어 2

https://www.w3schools.com/xml/plant_catalog.xml 사이트에 있는 xml 파일 데이터를 가져오는 명령어

.. code-block:: none

    * | curl xml -X GET https://www.w3schools.com/xml/plant_catalog.xml

결과

.. list-table::
   :header-rows: 1

   * - _raw
   * - |pre_start| <?xml version="1.0" encoding="UTF-8"?> |br| <CATALOG> |br|   <PLANT> |br|     <COMMON>Bloodroot</COMMON> |br|     <BOTANICAL>Sanguinaria canadensis</BOTANICAL> |br|     <ZONE>4</ZONE> |br|     <LIGHT>Mostly Shady</LIGHT> |br|     <PRICE>$2.44</PRICE> |br|     <AVAILABILITY>031599</AVAILABILITY> |br|   </PLANT> |br|   ... |br| <CATALOG/> |pre_end|

.. |br| raw:: html

  <br/>

.. |pre_start| raw:: html

  <pre>

.. |pre_end| raw:: html

  <pre/>
