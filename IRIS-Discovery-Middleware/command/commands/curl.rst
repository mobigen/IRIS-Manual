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

.. code-block::

   ... | curl2 (op)? (-X (GET/POST))? (-H Header)? (-d data)? url

.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - op
     - 사이트 형식에 따라 요청 결과가 다르기 때문에 해당 사이트라는 것을 명시해주는 값, (2block 사이트를 지원하기 위해 만든 옵션이므로 일반 사용자는 무시)
     - 옵션
   * - -X
     - 요청 method 를 선택하는 옵션, 현재 지원되는 것은 GET, POST (Default: GET)
     - 옵션
   * - -H
     - 요청 header 를 작성하는 옵션
     - 옵션
   * - -d
     - 요청 파라미터를 설정하는 옵션
     - 옵션
   * - url
     - 요청할 url 을 입력하는 부분
     - 옵션

Example
----------

- 예제 명령어

https://raw.githubusercontent.com/jooeungen/coronaboard_kr/master/kr_daily.csv 사이트에 있는 csv 파일 데이터를 가져오는 명령어

.. code-block::

    * | curl2 -X GET https://raw.githubusercontent.com/jooeungen/coronaboard_kr/master/kr_daily.csv 

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
