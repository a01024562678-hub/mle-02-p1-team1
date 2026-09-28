# car_search_rag 작업 메모

이 파일은 현재 프로젝트 아래 작업에 적용한다. 공통 Python 모듈은 `car_search_rag/common/`에 둔다.

## 현재 구현

- `car_search/`의 각 `xxx.py → xxx_service.py → xxx.sql`이 한 기능 세트다. `xxx.py`가 진입점이고 Service가 `SqlSession`으로 SQL을 직접 호출한다. `__init__.py` 없이 namespace import를 사용한다.
- `common/sql_session.py`는 프로젝트 아래의 `*.sql`을 재귀적으로 aiosql에 등록하고 연결·트랜잭션·결과 변환을 담당한다. `DatabaseManager`가 `.env`의 `DB_URL`을 읽는다.
- 커피 진입점은 `car_search/coffee_search.py`다. PDF가 필요하면 `car_search_rag/data/` 아래에 둔다. DB 등록은 상세 데이터를 갱신하고 임베딩 API를 호출한다.
- Python import는 `car_search_rag.*`로 통일하고 실행 경로는 저장소의 `src/` 기준으로 설정한다.

## 작업 방식

- SQL 내용은 특별한 이유 없이 변경하지 않는다. 새 DB 코드도 기존 연결 방식을 사용한다.
- 부작용이 작은 import와 SQL 등록을 먼저 검증한다. 실제 DB·API 호출은 필요할 때만 한다.
- Sphinx 원본은 상위 `docs/source/`이며 `docs/build/html/`은 생성 결과다.
