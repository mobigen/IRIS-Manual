postagger
====================================================================================================

개요
----------------------------------------------------------------------------------------------------

입력된 컬럼의 문장들을 단어로 쪼개고, 품사를 표기하는 명령어 입니다.

설명
----------------------------------------------------------------------------------------------------

명령어에 입력된 컬럼의 데이터를 단어로 쪼개고, 품사를 표기합니다.
품사를 포기하는 알고리즘은 현재 3가지를 지원하고 있습니다. (mobigen, Mecab, NLTK)

1. mobigen algorithm (Default)
   - 문장을 ``space`` 로 단순히 자르고, 모든 품사는 ``N`` 이 표기합니다.
2. Mecab
   - 한국어 문장을 구문 분석하는 알고리즘으로, ``KoNLPy`` 에서 지원하는 ``Mecab`` 알고리즘을 사용합니다.
3. NLTK
   - 영어 문장을 구문 분석하는 알고리즘으로, ``NLTK`` 라이브러리를 사용합니다.

Parameters
----------------------------------------------------------------------------------------------------

.. code-block:: none

   ... | postagger field (ALGORITHM ...)? (POS ...(, ...)*)?

.. list-table::
   :header-rows: 1
   :widths: 20 60 20

   * - 이름
     - 설명
     - 필수/옵션
   * - field
     - 문장 데이터가 있는 컬럼입니다. 해당 데이터를 분석하여 품사를 표기합니다.
     - 필수
   * - ALOGRITHM ...
     - ALGORITHM 은 예약어 이며, 사용할 수 있는 알고리즘은 현재 3가지 입니다. [mobigen, Mecab, NLTK].
     - 옵션, 기본값 = mobigen
   * - POS ...(, ...)*
     - POS 는 예약어 이며, 체언, 용언, 부사 등 출력에 보여질 태그 값을 선택할 수 있습니다.
     - 옵션, 기본값 = null

품사 태그 종류
""""""""""""""
**품사 태그는 각 알고리즘 마다 의미가 조금씩 다르기 때문에 해당 문서를 확인하여 사용하시기 바랍니다.**

**mobigen 알고리즘은 모든 값을 ``N`` 으로 표기합니다.**

*괄호는 생략해도 무방합니다.*

Mecab 품사
'''''''''''
자세한 내용은 `mecab-ko-dic 품사 태그 설명 <https://docs.google.com/spreadsheets/d/1-9blXKjtjeKZqsf4NzHeYJCrr49-nXeRF6D80udfcwY/edit#gid=589544265>`_ 참고

.. list-table::
   :header-rows: 1

   * - 문자
     - 의미
   * - N(NG|NP|NB|R|P)
     - 체언
   * - V(V|A|X|CP|CN)
     - 용언
   * - M(M|AG|AJ)
     - 수식언
   * - IC
     - 독립언
   * - J(KS|KC|KG|KO|KB|KV|KQ|X|C)
     - 관계언
   * - E(P|F|C|TN|TM)
     - 어미
   * - XPN
     - 접두사
   * - X(SN|SV|SA)
     - 접미사
   * - XR
     - 어근
   * - S(F|E|SSO|SSC|SC|SY)
     - 부호
   * - S(L|H|N)
     - 한글 이외

NLTK 품사
''''''''''
자세한 내용은 `URL <https://www.guru99.com/pos-tagging-chunking-nltk.html>`_ 참고

.. list-table::
   :header-rows: 1

   * - 문자
     - 의미
   * - CC
     - coordinating conjunction
   * - CD
     - cardinal digit
   * - DT
     - determiner
   * - EX
     - existential there
   * - FW
     - foreign word
   * - IN
     - preposition/subordinating conjunction
   * - J(J|JR|JS)
     - adjective
   * - LS
     - list market
   * - MD
     - modal (could, will)
   * - N(N|NS|NP|NPS)
     - noun
   * - PDT
     - predeterminer (all, both, half)
   * - POS
     - possessive ending (parent\ 's)
   * - PRP
     - personal pronoun (hers, herself, him,himself)
   * - PRP$
     - possessive pronoun (her, his, mine, my, our )
   * - RB(R|S)
     - adverb
   * - RP
     - particle (about)
   * - TO
     - infinite marker (to)
   * - UH
     - interjection (goodbye)
   * - V(B|BG|BD|BN|BP|BZ)
     - verb
   * - W(DT|P|RB)
     - wh-determiner(that, what), wh-pronoun(who), wh-adverb(how)

Examples
----------------------------------------------------------------------------------------------------

* 기본 데이터 (한글)

.. list-table::
   :header-rows: 1

   * - TITLE
     - ARTICLE
     - CATEGORY
   * - 아이리스
     - 모비젠의 ‘아이리스(IRIS)’는 기업의 빅데이터 사용 환경에서 빅데이터의 수집부터 분석, 시각화까지의 프로세스를 일원화하는 빅데이터 분석 솔루션이다.
     - 모비젠
   * - IRIS SaaS
     - 클라우드 기반의 빅데이터 플랫폼 ‘IRIS SaaS(Software as a Service)’를 출시
     - 모비젠

* 기본 데이터 (영어)

.. list-table::
   :header-rows: 1

   * - TITLE
     - ARTICLE
     - CATEGORY
   * - IRIS
     - Mobigen's 'IRIS' is a big data analysis solution that unifies the process from collection, analysis, and visualization of big data in a company's big data usage environment.
     - mobigen
   * - IRIS SaaS
     - Mobigen launches cloud-based big data platform ‘IRIS SaaS (Software as a Service)’
     - mobigen

1. ``mobigen`` 알고리즘을 이용한 형태소 태그

.. code-block:: none

   ... | postagger article ALGORITHM mobigen

.. list-table::
   :header-rows: 1

   * - TITLE
     - CATEGORY
     - WORD_BY_MOBIGEN
     - TAG_BY_MOBIGEN
   * - 아이리스
     - 모비젠
     - 모비젠의
     - N
   * - 아이리스
     - 모비젠
     - ‘아이리스(IRIS)’는
     - N
   * - 아이리스
     - 모비젠
     - 기업의
     - N
   * - ...
     - ...
     - ...
     - ...
   * - IRIS SaaS
     - 모비젠
     - 클라우드
     - N
   * - IRIS SaaS
     - 모비젠
     - 기반의
     - N
   * - IRIS SaaS
     - 모비젠
     - 빅데이터
     - N
   * - ...
     - ...
     - ...
     - ...

2. ``mecab`` 알고리즘을 이용한 형태소 태그

.. code-block:: none

   ... | postagger article ALGORITHM mecab

.. list-table::
   :header-rows: 1

   * - TITLE
     - CATEGORY
     - WORD_BY_MECAB
     - TAG_BY_MECAB
   * - 아이리스
     - 모비젠
     - 모비
     - NNG
   * - 아이리스
     - 모비젠
     - 젠
     - NNG
   * - 아이리스
     - 모비젠
     - 의
     - JKG
   * - ...
     - ...
     - ...
     - ...
   * - IRIS SaaS
     - 모비젠
     - 클라우드
     - NNP
   * - IRIS SaaS
     - 모비젠
     - 기반
     - NNG
   * - IRIS SaaS
     - 모비젠
     - 의
     - JKG
   * - ...
     - ...
     - ...
     - ...


3. ``nltk`` 알고리즘을 이용한 형태소 태그

.. code-block:: none

   ... | postagger article ALGORITHM nltk

.. list-table::
   :header-rows: 1

   * - TITLE
     - CATEGORY
     - WORD_BY_NLTK
     - TAG_BY_NLTK
   * - IRIS
     - mobigen
     - Mobigen
     - NNP
   * - IRIS
     - mobigen
     - 's
     - POS
   * - IRIS
     - mobigen
     - 'IRIS
     - NNP
   * - IRIS
     - mobigen
     - '
     - ''
   * - IRIS
     - mobigen
     - is
     - VBZ
   * - ...
     - ...
     - ...
     - ...
   * - IRIS SaaS
     - mobigen
     - Mobigen
     - NN
   * - IRIS SaaS
     - mobigen
     - launches
     - VBZ
   * - IRIS SaaS
     - mobigen
     - cloud-based
     - JJ
   * - IRIS SaaS
     - mobigen
     - big
     - JJ
   * - IRIS SaaS
     - mobigen
     - data
     - NNS
   * - ...
     - ...
     - ...
     - ...

