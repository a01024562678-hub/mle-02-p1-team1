# mle-02-p1-team1
차량 매뉴얼 기반 AI 질의응답


# 🐾 프로젝트명

> 한 줄 소개: ()를 수집·분석하고, 출처를 붙여 답하는
> RAG 챗봇과 분석 대시보드를 하나의 Streamlit 앱으로 제공합니다.

🔗 데모: (배포 URL 또는 "로컬 실행") · 📊 발표자료: (링크) · 📓 팀 노션: (링크)

---

## 1. 프로젝트 소개

- **문제**: (이 도메인에서 사람들이 겪는 불편 1~2줄)
- **해결**: (우리 앱이 그 불편을 어떻게 줄이는지 1~2줄)
- **기간 / 팀**: 2026.10.00 ~ 2026.00.00 (3일) / 4명

## 2. 데모

| 분석 대시보드 | RAG 챗봇 (출처 표기) |
| --- | --- |
| ![dashboard](docs/images/dashboard.png) | ![chat](docs/images/chat.png) |

## 3. 주요 기능

- **분석 대시보드** — (예: 지역별·기간별 추이, 키워드 상위 20개, 통계 검정 결과)
- **근거 기반 챗봇** — 답변마다 참조 문서를 함께 표시하고, 근거를 찾지 못하면 모른다고 답합니다
- **검색 품질 확인** — 평가셋 기준 Hit@5 / MRR 지표를 앱에서 확인할 수 있습니다

## 4. 아키텍처

(mermaid 다이어그램 — 아래 스니펫 참고)

## 5. 기술 스택

| 구분 | 사용 기술 |
| --- | --- |
| 언어 / 환경 | Python 3.11, uv |
| 데이터 | pandas, scipy, scikit-learn |
| LLM / 임베딩 | OpenAI GPT (모델명 명시) |
| RAG | LangChain, ChromaDB |
| 앱 | Streamlit, Plotly |
| 협업 | GitHub (브랜치 · PR · 리뷰), Notion, FigJam |

## 6. 데이터

- **출처**: OO 공공데이터 API — (링크)
- **수집 기간 / 건수**: 2024-01 ~ 2025-12 / 원본 12,431건 → 전처리 후 11,208건
- **주요 컬럼**: (컬럼명 · 타입 · 의미 5개 내외)
- **전처리 요약**
  - 결측: (컬럼)의 결측 3.2% → (처리 방법)
  - 중복: 공고번호 기준 412건 제거
  - 이상치: (기준과 처리 방법)
  - 도메인 사전: "믹스 / 잡종 / 믹스견" → "믹스" 등 00개 용어 통일
- **상세 명세**: [데이터 명세서](docs/data_spec.md) · [전처리 명세서](docs/preprocess_spec.md)

## 7. 실행 방법

### 사전 준비

- Python 3.12, [uv](https://docs.astral.sh/uv/)
- OpenAI API Key ([발급](https://platform.openai.com/api-keys))

### 설치

    git clone https://github.com/ORG/REPO.git
    cd REPO
    uv sync

### 환경 변수

`.env.example`을 복사해 `.env`를 만들고 키를 채웁니다. `.env`는 커밋하지 않습니다.

    OPENAI_API_KEY=your_key_here

### 수집 → 인덱싱 → 실행

    uv run python src/collect.py      # 1) API 수집      → data/raw/
    uv run python src/preprocess.py   # 2) 전처리        → data/processed/
    uv run python src/index.py        # 3) 임베딩·적재   → chroma_db/  (약 3분)
    uv run streamlit run app.py       # 4) 앱 실행       → http://localhost:8501

## 8. 프로젝트 구조

    REPO/
    ├── app.py                  # Streamlit 통합 앱 (대시보드 + 챗봇)
    ├── src/
    │   ├── collect.py          # M1 API 수집
    │   ├── preprocess.py       # M2 전처리 · 도메인 사전
    │   ├── index.py            # M4 청킹 · 임베딩 · ChromaDB 적재
    │   ├── rag_chain.py        # M5 검색 → 생성 → 출처
    │   └── evaluate.py         # M6 Hit@K · MRR 측정
    ├── notebooks/              # M3 EDA · 통계 검정 (1인 1파일)
    ├── data/
    │   ├── raw/                # 원본 (커밋 제외)
    │   └── processed/          # 전처리 결과
    ├── eval/questions.json     # 평가셋 (질문 + 정답 문서)
    ├── docs/                   # 명세서 · 이미지 · 평가 리포트
    ├── .env.example
    └── README.md

## 9. 검색 품질 평가

평가셋: 정답 문서를 지정한 질문 30개 (`eval/questions.json`)

| 실험 | Hit@5 | Precision@5 | MRR | 비고 |
| --- | --- | --- | --- | --- |
| Before (기본 설정) | 0.63 | 0.31 | 0.44 | chunk 1000 / overlap 0, top-k 5 |
| After (개선) | 0.80 | 0.42 | 0.61 | chunk 500 / overlap 100 |

**개선 실험(M7)**: (무엇이 문제였는지 → 무엇을 바꿨는지 → 왜 좋아졌다고 보는지 2~3줄)

재현: `uv run python src/evaluate.py`

## 10. 팀 소개

| 이름 | 역할 | 담당 | GitHub |
| --- | --- | --- | --- |
| 000 | PM | M0 기획 · 일정 · M9 시연 | @id |
| 000 | 데이터 | M1 수집 · M2 전처리 · M3 분석 | @id |
| 000 | RAG | M4 적재 · M5 체인 · M6·M7 평가 | @id |
| 000 | 대시보드 | M8 통합 앱 · 시각화 | @id |

## 11. 회고 (KPT)

- **Keep**: (계속 가져갈 것)
- **Problem**: (막혔던 지점과 원인)
- **Try**: (다음에 시도할 것)

상세 회고 → (팀 노션 링크)
