Jaguar
====================

Jaguar는 작은량의 데이터를 빠르고 손쉽게 처리하기 위한 데이터 엔진으로 RDBMS, File 등에서 데이터를 읽어 들이고 DSL을 통하여 데이터를 처리하여 그 결과를 사용자에게 전달 합니다.

Jaguar는 다음과 같은 특징을 가지고 있습니다.

- 단일 프로세스로 데이터를 처리
- 데이터를 읽어들이기 위해 사용자가 직접 각 데이터 소스에 맞는 SQL 작성
- 데이터 처리를 위해 DSL 문법을 제공
- CRUD(Create, Read, Update, Delete) 의 작업 중 Read 만 가능

데이터 처리를 위해 기본적으로 DSL 기반의 문법을 사용 하지만, 처음 데이터를 읽어 들이기 위해서는 데이터가 저장되어 있는 DB 종류에 따른 SQL을 사용자가 직접 입력하여 사용하게 됩니다.
따라서 Jaguar에서 데이터 처리는 다음과 같은 과정을 통해 이루어 지게 됩니다.

.. code-block::

    데이터 호출 DSL -> 데이터 처리 DSL -> 데이터 처리 DSL ....

.. toctree::
    :hidden:

    dsl-policy.rst
    dsl-call.rst
    dsl-join.rst
    dsl-function.rst
