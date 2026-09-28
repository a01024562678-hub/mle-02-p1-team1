# car_search_rag 프로젝트 구조

```text
car_search_rag/
├─ car_search/
│  ├─ coffee_search.py
│  ├─ coffee_search_service.py
│  ├─ coffee_search.sql
│  ├─ document.py
│  ├─ document_service.py
│  ├─ document.sql
│  ├─ query_examples.py
│  ├─ query_examples_service.py
│  ├─ query_examples.sql
│  ├─ transaction_sample.py
│  ├─ transaction_sample_service.py
│  └─ transaction_sample.sql
├─ common/
├─ docs/
├─ AGENTS.md
└─ run_car_search.bat
```

각 `xxx.py`가 실행을 시작하고 Service가 업무 흐름과 SQL 호출을 담당한다. `common/sql_session.py`는 네 SQL 파일을 aiosql로 등록하고 기존 `DatabaseManager`로 연결한다. SQL은 파일 위치를 기준으로 찾는다.

커피 등록은 `coffee_search.py`에서 시작해 `coffee_search_service.py`가 PDF의 KR 텍스트를 추출하고 임베딩한 뒤 머신과 상세를 저장한다. PDF가 필요하면 `car_search_rag/data/`에 둔다.

`document_service.py`는 문서 다섯 건 조회, `query_examples_service.py`는 브랜드별 집계와 조건 조회, `transaction_sample_service.py`는 임시 테이블 트랜잭션 예제다.

저장소 루트에서 `python src/car_search_rag/car_search/coffee_search.py`로 실행하거나 `src\car_search_rag\run_car_search.bat`을 사용한다. 다른 기능도 `src/car_search_rag/car_search/`의 해당 진입점을 실행한다. `.env`의 `DB_URL`을 사용하며 커피 등록은 임베딩 API도 필요하다.
