Note
=====

- DSL 에서 사용되는 규칙

- 컬럼명

  1. 한 단어로 이루어진 문자열인 경우
    - 그대로 사용
    - 예) host, level, 시간, 문장
  2. 여러 단어 or 특수문자 or 숫자 등이 포함되어 복잡한 경우
    - 백쿼터(back-quote : \`) 를 사용하여 표현
    - 예) \`test column\`, \`%2컬럼명 abc\`

- 문자열

  - 문자열 표현은 따옴표(single-quote : ') 를 사용하여 표현
  - 예) '문자열입니다.'


Command References
====================

.. list-table::
   :widths: 15 60 15
   :header-rows: 1

   * - Command
     - Description
     - Category
   * - `abs <commands/abs>`_
     - integer, bigint, real 등 숫자 컬럼의 절댓값을 구하는 명령어 입니다.
     -
   * - `adv <commands/adv>`_
     - 테이블의 각종 통계 정보를 구하거나 pivoting 할 수 있습니다. 고급시각화 화면에 사용되는 명령어 모음입니다.
     -
   * - `anomalies <commands/anomalies>`_
     - 주어진 input 데이터에서 일반적인 범위를 벗어난 비정상적인 값을 찾아내는 기능입니다.
     -
   * - `bimatrix <commands/bimatrix>`_
     - bimatrix에 쓰기를 하는 명령어 입니다.
     -
   * - `calculate <commands/calculate>`_
     - 행 or 열 간의 간단한 수식 계산을 합니다.
     -
   * - `case <commands/case>`_
     - 조건 처리가 가능한 명령어 입니다.
     -
   * - `checkpoint <commands/checkpoint>`_
     - 쿼리의 결과를 일정시간동안 저장해 놓을수 있는 명령어
     -
   * - `concat <commands/concat>`_
     - 선택한 컬럼을 concatenation 하는 명령어 입니다.
     -
   * - `corr <commands/corr>`_
     - input 으로 받은 컬럼들 간의 상관계수를 구하는 명령어입니다.
     -
   * - `count <commands/count>`_
     - 입력의 래코드 건수를 계산합니다.
     -
   * - `curl <commands/curl>`_
     - curl 을 통해 가져온 데이터를 DataFrame 으로 만들어 spark 을 통해 처리 가능하도록 합니다.
     -
   * - `distinct <commands/distinct>`_
     - 선택한 컬럼에 대한 중복값 없는 데이터를 반환합니다.
     -
   * - `eval <commands/eval>`_
     - 학습된 모델을 이용해 예측 결과를 반환하는 명령어 입니다.
     -
   * - `explode <commands/explode>`_
     - 선택한 컬럼의 데이터를 구분자를 이용해 분리하고, 새로운 레코드로 만듭니다. (원래 데이터는 삭제)
     -
   * - `fields <commands/fields>`_
     - 검색 결과가 출력 될 field를 설정합니다.
     -
   * - `fillna <commands/fillna>`_
     - 이 명령어는 특정한 필드의 결측치 처리에 사용합니다.
     -
   * - `fit <commands/fit>`_
     - 이 명령어는 해당 데이터를 선택한 알고리즘으로 기계학습 모델을 만들어줍니다.
     -
   * - `fit_predict <commands/fit_predict>`_
     - 이 명령어는 해당 데이터를 선택한 알고리즘으로 기계학습 모델에 TEST set을 넣은 값을 출력합니다.
     -
   * - `forecasts <commands/forecasts>`_
     - 주어진 데이터의 미래 시점 데이터를 예측합니다.
     -
   * - `geocode <commands/geocode>`_
     - 주소, 우편번호, 위경도 등의 데이터 컬럼에 해당하는 geometry 데이터를 찾아주는 명령어 입니다.
     -
   * - `geoconverter <commands/geoconverter>`_
     - gemotery / geojson 형태를 서로 바꿔주는 명령어 입니다.
     -
   * - `geoip <commands/geoip>`_
     - 이 명령어는 ip가 포함된 필드를 기반으로 위, 경도 등의 추가정보를 제공합니다.
     -
   * - `geomap <commands/geomap>`_
     - 이 명령어는 업로드 되어있는 컬렉션을 기반으로 사용자가 요청하는 테이블에 지역 경계(geometry) 정보를 제공합니다.
     -
   * - `geomatric <commands/geomatric>`_
     - ``데이터의 계산값`` / ``필터와의 관계`` 와 같은 연산을 하는 명령어 입니다.
     -
   * - `georelation <commands/georelation>`_
     - geometry 타입의 데이터를 필터 조건을 이용해 필터링 하는 명령어 입니다.
     -
   * - `geostats <commands/geostats>`_
     - 이 명령어는 위, 경도 데이터를 포함한 두 개의 필드를 기반으로 그룹(지정 지역 클러스터)별 통계정보를 제공합니다.
     -
   * - `hdfs <commands/hdfs>`_
     - HDFS에 읽기 및 쓰기를 하는 명령어 입니다.
     -
   * - `head <commands/head>`_
     - 상위 records를 원하는 갯수 만큼 검색 결과에 출력 합니다.
     -
   * - `img2tsv <commands/img2tsv>`_
     - 이미지 파일을 읽어서 TSV 포멧으로 변환 저장하는 명령어 입니다.
     -
   * - `indexer <commands/indexer>`_
     - 설정한 컬럼의 데이터를 기반으로 indexing하는 명령어 입니다. 컬럼의 데이터가 다를 경우 같은 값이라도 index 결과가 다를 수 있습니다.
     -
   * - `interpolation <commands/interpolation>`_
     - 시계열 데이터를 이용하여 집계를 할 때, 설정한 간격 사이의 시계열 데이터가 없으면 해당 데이터를 추가(보간) 해주는 명령어 입니다.
     -
   * - `iris <commands/iris>`_
     - IRIS에 읽기를 하는 명령어 입니다.
     -
   * - `job <commands/job>`_
     - 현재 동작하고 있는 DSL 작업들을 관리 할 수 있습니다.
     -
   * - `join <commands/join>`_
     - 이 명령어는 다른 데이터 모델과 join을 할 때 사용됩니다.
     -
   * - `kmeans <commands/kmeans>`_
     - kmeans를 진행하는 명령어 입니다.
     -
   * - `lag <commands/lag>`_
     - 현재 행 이전의 행의 값을 가져오는 명령어입니다.
     -
   * - `lead <commands/lead>`_
     - 현재 행의 다음 행의 값을 가져오는 명령어입니다.
     -
   * - `length <commands/length>`_
     - string length를 계산 하는 명령어입니다.
     -
   * - `merge <commands/merge>`_
     - 이 명령어는 다른 데이터 모델과 join을 할 때 사용됩니다.
     -
   * - `metatron <commands/metatron>`_
     - metatron에 쓰기를 하는 명령어 입니다.
     -
   * - `mkdata <commands/mkdata>`_
     - DSL 에 작성한 데이터를 이용하여 DataFrame 을 만드는 명령어
     -
   * - `mlmodel <commands/mlmodel>`_
     - Data-Discovery-Service ML 관련 명령어 이며, ML 명령어 중 fit 명령어를 통해 생성되는 모델 및 메타데이터를 관리하는 명령어 입니다.
     -
   * - `model <commands/model>`_
     - 모델에 해당하는 데이터 소스의 데이터를 로드합니다.
     -
   * - `model-stats <commands/model-stats>`_
     - 지정된 필드에 대한 distinct count 값을 제공합니다.
     -
   * - `model-statsdetails <commands/model-statsdetails>`_
     - 지정된 필드에 존재하는 값들의 상위 빈도 10개 값을 제공합니다.
     -
   * - `model-timeline <commands/model-timeline>`_
     - 지정된 데이터 모델의 Timeline을 생성합니다.
     -
   * - `modeldata <commands/modeldata>`_
     - 이 명령어는 Data-Discovery-Service ML 관련 명령어 이며, 사용자가 입력한 한 데이터를 데이터 프레임으로 반환 합니다.
     -
   * - `objectstorage <commands/objectstorage>`_
     - objectstorage 읽기 및 쓰기를 하는 명령어 입니다.
     -
   * - `outlier <commands/outlier>`_
     - 여러 그룹을 대상으로 outlier 에 해당하는 그룹을 찾는 명령어 입니다.
     -
   * - `pivot <commands/pivot>`_
     - 테이블을 여러 컬럼들을 축으로 회전 및 각종 통계 정보를 행과 열 별로 구할 수 있습니다.
     -
   * - `postagger <commands/postagger>`_
     - 입력된 컬럼의 문장들을 단어로 쪼개고, 품사를 표기하는 명령어 입니다.
     -
   * - `predict <commands/predict>`_
     - 이 명령어는 Data-Discovery-Service ML 관련 명령어 이며, 사전에 학습되어 있는 ML 모델을 불러와 넘겨받은 데이터에 적용하는 명령어 입니다.
     -
   * - `pylambda <commands/pylambda>`_
     - 해당 명령어는 Python의 lambda 함수를 실행 합니다.
     -
   * - `regex <commands/regex>`_
     - regex 명령을 사용하여 지정된 정규식과 일치하지 않는 결과를 제거합니다.
     -
   * - `rename <commands/rename>`_
     - 이 명령어는 특정한 field를 다른 명칭으로 이름을 바꾸고자 할 때 사용됩니다.
     -
   * - `replace <commands/replace>`_
     - 선택한 컬럼의 데이터의 글자를 변환하는 명령어 입니다.
     -
   * - `replacewords <commands/replacewords>`_
     - 대치어(replace)사전을 통해 단어를 대치합니다.
     -
   * - `round <commands/round>`_
     - 이 명령어는 지정된 실수형 필드의 소수점을 반올림 하는 명령어 입니다.
     -
   * - `sampling <commands/sampling>`_
     - 이 명령어는 Data-Discovery-Service ML 관련 명령어 이며, 사용자가 입력한 수 만큼의 데이터를 반환합니다.
     -
   * - `scaler <commands/scaler>`_
     - scaling을 진행하는 명령어 입니다.
     -
   * - `search <commands/search>`_
     - 이 명령어는 전문 검색(full-text search)을 하는데 사용 됩니다.
     -
   * - `serving <commands/serving>`_
     - Tensorflow Serving을 통해 예측, 모델의 서빙 상태를 확인하는 명령어입니다.
     -
   * - `sort <commands/sort>`_
     - 이 명령어는 검색 결과를 지정된 필드를 기준으로 정렬합니다.
     -
   * - `spath <commands/spath>`_
     - json/xml 과 같은 구조화된 데이터의 key 를 이용해서 해당하는 데이터를 추출하는 명령어입니다.
     -
   * - `split <commands/split>`_
     - 선택한 컬럼을 구분자를 통해서 분리하여 새로운 레코드로 만드는 명령어
     -
   * - `splitter <commands/splitter>`_
     - 레코드형 파일(들)을 train/test 로 분리 저장하는 명령어 입니다.
     -
   * - `sql <commands/sql>`_
     - SQL 형태의 질의를 합니다.
     -
   * - `stats <commands/stats>`_
     - 각종 통계 데이터를 구하는 명령어 입니다.
     -
   * - `substr <commands/substr>`_
     - 이 명령어는 특정한 필드나 문자열을 SUBSTRING 하고자 할 때 사용됩니다.
     -
   * - `summary <commands/summary>`_
     - DF 의 각 컬럼에 대한 집계 정보를 보여주는 명령어 입니다.
     -
   * - `time2sec <commands/time2sec>`_
     - ``시:분:초`` 로 이루어진 데이터를 ``초`` 단위 숫자로 변경해주는 명령어 입니다.
     -
   * - `timediff <commands/timediff>`_
     - 선택한 컬럼 끼리 혹은 원하는 데이터와 컬럼 간의 시간 차이를 구해주는 명령어 입니다.
     -
   * - `timeline <commands/timeline>`_
     - 시간 범위에 따라 group by된 카운트의 숫자를 출력 합니다.
     -
   * - `tokenizer <commands/tokenizer>`_
     - 지정한 필드에 대해 tokenizer를 진행하는 명령어 입니다.
     -
   * - `top <commands/top>`_
     - 이 명령어는 검색 결과를 지정된 필드를 기준으로 정렬 후 상위 값을 출력합니다.
     -
   * - `typecast <commands/typecast>`_
     - 선택한 컬럼의 데이터 타입을 원하는 타입으로 변환시켜 반환합니다.
     -
   * - `union <commands/union>`_
     - 이 명령어는 다른 데이터 모델과 union 을 할 때 사용됩니다.
     -
   * - `unpivot <commands/unpivot>`_
     - 가로로 출력되는 열(COLUMN) 데이터를 세로로 돌려 행(ROW)으로 출력할 수 있습니다.
     -
   * - `value_counts <commands/value_counts>`_
     - 특정 필드에 대한 고유값과 해당 값의 count를 리턴합니다.
     -
   * - `weekdates <commands/weekdates>`_
     - 정수를 이용해 해당 주차의 시작일과 종료일을 찾는 명령어입니다.
     -
   * - `weekend <commands/weekend>`_
     - 이 명령어는 주말/주중 데이터를 필터링하는데 사용합니다.
     -
   * - `weeknumber <commands/weeknumber>`_
     - 데이터의 날짜가 1년의 몇 번째 주인지 계산하는 명령어입니다.
     -
   * - `where <commands/where>`_
     - 데이터를 일정 조건에 따라 filter 합니다.
     -
   * - `ysort <commands/ysort>`_
     - 이 명령어는 검색 결과로 생성된 필드의 순서를 지정된 기준으로 정렬합니다.
     -


.. toctree::
   :glob:

   commands/**


