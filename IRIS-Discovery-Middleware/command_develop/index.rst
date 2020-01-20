명령어 개발 가이드
==================

- 앙고라 명령어를 개발 할 때 규칙 및 참고 사항을 기록한 문서

명령어 인터페이스 [BaseCommand]
-------------------------------

구조
``````

.. code-block:: bash

    - class BaseCommand
     +- def __init__()
     +- def parse()
     +- def execute()


명령어 작성 [XXXCommand]
-------------------------

작성 규칙
``````````

1. 생성할 명령어로 python 파일(.py) 생성
2. ``명령어 이름 + Command`` 로 클래스 생성 (낙타 표기법 사용)
3. Lexer, Paser 구현 및 인터페이스에 구성된 method overwriting

ex) round 명령어 개발

    1. ``round.py`` 생성
    2. ``RoundCommand`` 클래스 작성

상속
`````

- BaseCommand 상속

  - ``BaseCommand``를 상속한 ``AAACommand`` 상속가능
  - 최고 조상이 ``BaseCommand``

- ex) ``class RoundCommand(BaseCommand)``

inner class [Lexer, Parser]
````````````````````````````

- Lexer

  - 명령어로 입력된 string 을 파싱하여 토큰으로 만드는 class

  - ex) ``.. | round 3 col = HOST`` 쿼리 입력

    - Lexer에 들어오는 인자: ``3 col = HOST``
    - 토큰화된 결과:

.. code-block:: bash

    LexToken(NUMBER, 3, 1, 0)
    LexToken(COL,'col',1,2)
    LexToken(EQUAL,'=',1,6)
    LexToken(TOKEN,'HOST',1,8)

- Parser

  - Lexer 로 토큰화 된 결과를 조합하여 python 객체로 리턴하는 class

  - ex)

    - 위의 LexToken 를 이용해서 문법작성

.. code-block:: none

    """
    round_expr : NUMBER
                | NUMBER COL EQUAL TOKEN
                | NUMBER COL EQUAL LIST
                | NUMBER IDX EQUAL NUMBER
                | NUMBER IDX EQUAL LIST
                | LIST COL EQUAL TOKEN
                | LIST COL EQUAL LIST
                | LIST IDX EQUAL NUMBER
                | LIST IDX EQUAL LIST
    """

|

    - 필요한 토큰만 이용해서 python 객체로 변경

.. code-block:: none

    if len(p) == 2:
        p[0] = [None, p[1]]
    elif isinstance(p[1], int):
        if isinstance(p[4], list):
            p[0] = [p[2].lower(), map(lambda f: (f, p[1]), p[4])]
        else:
            p[0] = [p[2].lower(), [(p[4], p[1])]]
    elif isinstance(p[1], list):
        if not isinstance(p[4], list):
            raise AngoraException("If precision is List, then col/idx must be List.")
        if len(p[1]) != len(p[4]):
            raise AngoraException("Precision and col/idx must have equal length.")
        p[0] = [p[2].lower(), zip(p[4], p[1])]

|

    -   결과: ``['col', [('HOST', 4)]]``

method [\__init__, parse, execute]
```````````````````````````````````

- __init__

  - lexer, parser 를 인스턴스화

- parse

  - 인자: ``def parse(self, raw_args, options=None)``

    - ``raw_args``: 쿼리에서 명령어 뒤쪽 부분

      - ex) ``round 3 col = HOST`` 쿼리 시, ``raw_args``: ``3 col = HOST``

    - ``options``: engine에서 넘겨주는 파라미터로 다음 값이 있음

      - ``size``: UI로 리턴할 row 개수

      - ``sid``: 작업id

      - ``sample``: 샘플링

      - ``schema``: 스키마

      - ``just_model_count``: UI로 부터 받은 쿼리의 마지막에 count 명령어 유무

  - 처리 내용:

    - ``parser 클래스``를 통해 명령어가 분석된 결과를 처리하여 리턴

    - 보통 ``parser 클래스``에서 명령어를 분석하여 python 객체로 리턴을 하기 때문에 ``parse 함수``에서는 결과를 리턴

    - 만약, 추가 작업이 필요하다면 ``parse 함수`` 에서 구현

- execute

  - 인자: ``def execute(self, sqlCtx, df=None, parsed_args=None, partition=None, options=None)``

    - ``sqlCtx``: spark sqlContext 객체

    - ``df``: 앞 명령어에서 처리된 dataframe

    - ``parsed_args``: parse 함수에서 리턴된 python 객체

    - ``partition``: 데이터의 파티션

  - 처리 내용:

    - ``parsed_args``를 이용해서 ``df`` 를 명령어에 맞게 조작(실행계획 생성)하는 함수

    - 결과는 dataframe 객체 리턴


- 추가 필요 함수는 각 명령어에 수요에 따라 작성


예제 코드 (round 명령어)
-------------------------

.. code-block:: none

    # coding: utf-8
    from ply import yacc, lex
    from pyspark.sql import functions as F
    from pyspark.sql.utils import AnalysisException
    from exceptions import IndexError

    from angora.cmds.base import BaseCommand
    from angora.exceptions import AngoraException


    class RoundCommand(BaseCommand):
        # TODO: Move those lexer and parser into `BaseCommand` to generalize them.
        # Currently, we are relying on the old parser. After replacing them all,
        # we should change `BaseCommand` and move these into `BaseCommand`.
        class RoundLexer(object):
            tokens = ['EQUAL', 'NUMBER', 'COL', 'IDX', 'LIST', 'TOKEN']

            t_EQUAL = r'\='
            t_TOKEN = r'[^ \t=]+'

            def t_NUMBER(self, t):
                r'\d+'
                t.value = int(t.value)
                return t

            def t_COL(self, t):
                r'col|COL'
                return t

            def t_IDX(self, t):
                r'idx'
                return t

            def t_LIST(self, t):
                r'\[[^\[\]]+\]'
                t.value = t.value.replace('[', '').replace(']', '').replace(' ', '').split(',')
                return t

            # Define a rule so we can track line numbers
            def t_newline(self, t):
                r'\n+'
                t.lexer.lineno += len(t.value)

            t_ignore = ' \t'

            def t_error(self, t):
                raise SyntaxError("Syntax error near '%s'" % t.value)

            def build(self, **kwargs):
                self.lexer = lex.lex(module=self, **kwargs)
                return self.lexer

        class RoundParser(object):
            tokens = ['EQUAL', 'NUMBER', 'COL', 'IDX', 'LIST', 'TOKEN']

            def p_round_expr(self, p):
                """
                round_expr : NUMBER
                            | NUMBER COL EQUAL TOKEN
                            | NUMBER COL EQUAL LIST
                            | NUMBER IDX EQUAL NUMBER
                            | NUMBER IDX EQUAL LIST
                            | LIST COL EQUAL TOKEN
                            | LIST COL EQUAL LIST
                            | LIST IDX EQUAL NUMBER
                            | LIST IDX EQUAL LIST
                """
                if len(p) == 2:
                    p[0] = [None, p[1]]
                elif isinstance(p[1], int):
                    if isinstance(p[4], list):
                        p[0] = [p[2].lower(), map(lambda f: (f, p[1]), p[4])]
                    else:
                        p[0] = [p[2].lower(), [(p[4], p[1])]]
                elif isinstance(p[1], list):
                    if not isinstance(p[4], list):
                        raise AngoraException("If precision is List, then col/idx must be List.")
                    if len(p[1]) != len(p[4]):
                        raise AngoraException("Precision and col/idx must have equal length.")
                    p[0] = [p[2].lower(), zip(p[4], p[1])]

            def p_error(self, p):
                if p is None:
                    raise AngoraException("At least a field should be specified.")
                else:
                    raise SyntaxError("Syntax error near '%s'" % p.value)

            def build(self, **kwargs):
                self.parser = yacc.yacc(module=self, **kwargs)
                return self.parser

        def __init__(self):
            super(RoundCommand, self).__init__()
            self.lexer = RoundCommand.RoundLexer().build()
            self.new_parser = RoundCommand.RoundParser().build(write_tables=False, debug=False)

        def parse(self, raw_args, options=None):
            try:
                round_expr_list = self.new_parser.parse(raw_args, lexer=self.lexer)
            except SyntaxError as e:
                e.message = "%s [sort %s]" % (e.message, raw_args)
                raise e
            return round_expr_list

        def execute(self, sqlCtx, df=None, parsed_args=None, partition=None, options=None):
            category, round_expr_list = parsed_args
            if category is None:
                round_size = round_expr_list
                for field in df.columns:
                    try:
                        df = df.withColumn(field, F.format_number(field, round_size))
                    except:
                        pass
            elif category == 'col':
                try:
                    for field, round_size in round_expr_list:
                        df = df.withColumn(field, F.format_number(field, int(round_size)))
                except AnalysisException:
                    raise AngoraException("There is no field [%s]." % field)
            else:
                fields = df.columns
                try:
                    for field_idx, round_size in round_expr_list:
                        df = df.withColumn(fields[int(field_idx)], F.format_number(fields[int(field_idx)], int(round_size)))
                except IndexError:
                    raise AngoraException("There is no field_idx [%s]." % field_idx)
            return df

|

테스트 코드
-----------

- 개발한 명령어가 정상 적으로 작동하는지 확인하는 테스트 코드를 작성
- unittest 로 작성
- angora 팀만 git에 접근 권한이 있기 때문에 아래 포멧으로 테스트 코드를 작성하여 angoa 팀에게 전달

구조
`````

- ``test_external_command.py`` 파일 생성

- ``TestExteranlCommand`` 클래스 생성

- 명령어 별로 테스트 함수 작성

  - 이름 규칙: ``test_exteranl_command_XXX`` 로 구성

- ex) ``AAACommand``, ``XXXCommand``에 대한 테스트 코드작성

.. code-block:: bash

    - class TestExternalCommand
     +- def test_query_AAA()
     +- def test_query_XXX()


- 이 후 명령어 추가시 해당 파일에 함수만 추가하여 angora 팀에게 전달.

예제코드
--------

- ``test_external_command.py`` (명령어 테스트 파일)

  - 각 명령어에 맞는 unittest를 작성해 주세요.

.. code-block:: none

    from query_api_caller import test_qeury

    class TestExternalCommand:
        def test_query_[명령어이름](self):
            print 'xxx command unittest\n'
            q = "model name = 'WEATHER' model_owner = choi " \
                "| pivot count(*),avg(AVG_TEMP) as AAA,avg(LOWEST_TEMP) as BBB " \
                "| xxx 3 col=[AAA, BBB]"

            results = test_query(q=q)['results'][0]

            ret = True
            if len(results) <= 0:
                ret = False

            if len(results[1][results[1].find('.')+1:]) != 3 or len(results[2][results[2].find('.')+1:]) != 3:
                ret = False

            self.assertTrue(ret)


- import 해서 사용 할 ``query_api_caller.py``

  - 이 파일은 그대로 사용하시면 됩니다.

.. code-block:: none

    #!/usr/bin/env python
    # -*- coding: UTF-8 -*-
    import json
    from httplib import HTTPConnection
    import sys

    addr = "192.168.100.180"
    #addr = "localhost"

    def test_query(q):
        #print "query : [%s]" % q
        # We are going to shutdown this restful server for each test.
        host, port = (addr, 6036)
        parameters = {}
        parameters['q'] = q

        parameters['size'] = 1000

        http_conn = HTTPConnection(host, port)

        # Generate token.

        http_conn.request(
            "POST",
            "/angora/auth",
            json.dumps({"id": "test", "password": "test"}))
        token = json.load(http_conn.getresponse())["token"]
        http_conn.close()

        headers = {}
        headers["Accept"] = "application/json"
        headers["Content-Type"] = "application/json"
        headers["Authorization"] = "Angora %s" % token
        body = json.dumps(parameters)

        http_conn = HTTPConnection(host, port)
        print "URL : /angora/query/jobs"

        # Query / Fetch Session ID

        http_conn.request("POST", "/angora/query/jobs", body=body, headers=headers)
        r = json.load(http_conn.getresponse())
        try :
            sid = r["sid"]
        except Exception, e:
            print r
            sys.exit()

        http_conn.close()

        # Fetch results

        http_conn = HTTPConnection(host, port)
        print "URL : /angora/query/jobs/%s" % sid
        http_conn.request(
            "GET",
            "/angora/query/jobs/%s" % sid,
            headers=headers)
        test = http_conn.getresponse().read()
        return json.loads(test)
