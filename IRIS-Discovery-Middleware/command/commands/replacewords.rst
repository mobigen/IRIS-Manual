replacewords
==========

개요
------
대치어(replace)사전을 통해 단어를 대치합니다.

타입
----------------------------------------------------------------------------------------------------
TEXT

설명
------

인자로는 대치 대상 컬럼(col)과 대치어 사전으로 사용할 데이터 모델(model), 대치어 사전의 old 컬럼의 구분자(delimiter)를 입력받습니다.

대치어 사전의 경우 IRIS Discovery 를 통해 IRIS 의 데이터 모델로 만들어져 있어야 하며, 컬럼명은 old, new로 특정되어야 합니다.
old 컬럽의 데이터는 특정 구분자로 결합된 문자열입니다. 결합된 문자열은 replacewords 의 인자인 delimeter 로 분리되며 분리된 모든 문자열에 대해 new 컬럼의 값으로 대치됩니다.


delimiter 인자는 작은 따옴표(single quote)로 감싸주어야 합니다.

new 컬럼의 데이터는 old 컬럼의 문자에 대한 대치 문자열입니다.

대치어 사전이 적재된 데이터 모델의 예시입니다.  ``delimiter = ','``


.. list-table::
   :header-rows: 1

   * - old
     - new
   * - mobigen,Mobigen
     - 모비젠 
   * - bigdata,Bigdata
     - 빅데이터
   * - MLlib
     - 기계학습 라이브러리

또한 문자열 내부의 서브문자열에 대한 대치는 진행하지 않습니다. 대치 문자열과 동일한 데이터일 경우에만 대치를 진행합니다. 
  - 데이터 : Mobigen -> 대치 가능 (결과값 : 모비젠)
  - 데이터 : Mobigen은 -> 대치 불가능 (결과값 : Mobigen은)


Parameters
--------------------------------------

.. code-block:: python

    ... | replacewords col=COL model=Model_name delimiter=','



.. list-table::
   :header-rows: 1

   * - 이름
     - 설명
     - 필수/옵션
   * - col
     - 대치어 사전을 적용할 컬럼입니다.
     - 필수 
   * - model
     - 대치어 사전의 데이터 모델입니다. 공백 혹은 특수문자를 포함할 경우 작은따옴표(single-quote)로 감싸주어야 합니다.
     - 필수
   * - delimiter
     - model의 old 컬럼에 존재하는 문자열의 구분자이며, 작은따옴표(single-quote)로 감싸주어야 합니다. 해당 인자를 입력하지 않을 경우 old 컬럼을 분리 없이 문자열 그대로 사용합니다.
     - 옵션


Examples
--------

- 예제 데이터

.. list-table::
   :header-rows: 1
   
   * - ID
     - WORD
   * - A
     - mobigen
   * - B
     - Mobigen
   * - C
     - Mobigen은
   * - D
     - 모비젠은
   * - E
     - 빅데이터

-  대치어 사전 모델 명 : my_stopwords

.. list-table::
   :header-rows: 1

   * - old
     - new
   * - mobigen,Mobigen
     - 모비젠 
   * - bigdata,Bigdata
     - 빅데이터


- 예제1

.. code-block:: python

    * | replacewords col=WORD model=my_stopwords delimiter=','

.. list-table::
   :header-rows: 1
   
   * - ID
     - WORD
     - replaced
   * - A
     - mobigen
     - 모비젠
   * - B
     - Mobigen
     - 모비젠
   * - C
     - Mobigen은
     - Mobigen은
   * - D
     - 모비젠은
     - 모비젠은
   * - E
     - bigdata
     - 빅데이터
