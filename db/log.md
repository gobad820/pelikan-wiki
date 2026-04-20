# DB 위키 작업 로그

## [2026-04-16] ingest | 카디널리티 + 정규형 (대화 기반)
- 소스: conversation/2026-04-16 카디널리티 질문, 정규형 설명 및 ingest 요청
- 생성: concepts/카디널리티.md (관계 카디널리티 1:1/1:N/M:N, 컬럼 카디널리티, 인덱스 효율, 복합 인덱스 순서)
- 생성: concepts/정규형.md (이상 현상 3종, 1NF~BCNF 단계별 설명, 역정규화 트레이드오프, 면접 포인트)
- 업데이트: index.md (concepts 2개 추가, total_pages: 4, total_sources: 4)

## [2026-04-16] ingest | View (대화 기반)
- 소스: conversation/2026-04-16 View 설명 및 ingest 요청
- 생성: concepts/View.md (정의, 특성, DML 가능 조건, Materialized View 비교, 면접 포인트)
- 업데이트: index.md (concepts 1개 추가, total_pages: 2, total_sources: 2)

## [2026-04-16] ingest | DB 키 종류 (대화 기반)
- 소스: conversation/2026-04-16 DB 키 종류 질문
- 생성: concepts/DB키.md (Super Key → Candidate Key → PK/Alternate Key 계층, FK 별개 개념, 면접 포인트 포함)
- 생성: index.md (초기화, total_pages: 1)
- 생성: log.md (초기화)

---

## [2026-04-16] init | DB 위키 초기화
- 디렉토리 구조 생성: wiki/concepts/, wiki/patterns/, wiki/notes/
- CLAUDE.md 스키마 작성
- index.md, log.md 초기화

> 형식: `## [날짜] 작업유형 | 내용`
