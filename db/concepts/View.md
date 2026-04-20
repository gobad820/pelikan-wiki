---
title: "View (뷰)"
type: concept
tags: [db, view, virtual-table, security, materialized-view]
related:
  - "[[DB키]]"
  - "[[인덱스]]"
  - "[[정규화]]"
created: 2026-04-16
updated: 2026-04-16
sources:
  - conversation/2026-04-16 View 설명 및 ingest 요청
---

> [!abstract] 핵심 요약
> View는 하나 이상의 테이블로부터 유도된 **가상 테이블**이다. 실제 데이터를 저장하지 않고, 정의된 SQL 쿼리의 결과를 테이블처럼 보여준다.

## 정의

```sql
-- 생성
CREATE VIEW 뷰이름 AS
SELECT 컬럼1, 컬럼2
FROM 테이블명
WHERE 조건;

-- 삭제
DROP VIEW 뷰이름;

-- 수정
CREATE OR REPLACE VIEW 뷰이름 AS ...;
```

---

## 핵심 특성

| 항목 | 설명 |
|------|------|
| **가상성** | 실제 데이터 저장 X, 쿼리 실행 결과를 보여줌 |
| **독립성** | 원본 테이블 구조 변경 시 뷰 정의는 유지됨 |
| **보안성** | 특정 컬럼/행만 노출해 민감 데이터 숨김 가능 |
| **재사용성** | 복잡한 쿼리를 이름 붙여 반복 사용 |

---

## 사용 예시

```sql
-- 원본 테이블
CREATE TABLE employee (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    salary INT,        -- 민감 정보
    dept VARCHAR(30)
);

-- 급여를 숨긴 뷰 생성
CREATE VIEW public_employee AS
SELECT id, name, dept
FROM employee;

-- 뷰 사용 (일반 테이블처럼 조회)
SELECT * FROM public_employee;
```

---

## 장단점

**장점**
- 보안: 민감 컬럼(급여, 주민번호 등) 숨김
- 편의성: 복잡한 JOIN 쿼리를 뷰로 단순화
- 논리적 독립성: 테이블 구조가 바뀌어도 뷰 인터페이스 유지

**단점**
- 성능: 매 조회 시 내부 쿼리 재실행 → 인덱스 직접 활용 어려움
- 제한적 DML: 조건 충족 시에만 INSERT/UPDATE/DELETE 가능

---

## View를 통한 DML (수정 가능 조건)

뷰를 통해 데이터를 변경하려면 아래 조건을 **모두** 만족해야 한다.

| 조건 | 설명 |
|------|------|
| 단일 테이블 기반 | JOIN 뷰는 DML 불가 |
| GROUP BY / DISTINCT 없음 | 집계 뷰는 DML 불가 |
| 집계 함수 없음 | SUM, COUNT 등 사용 시 불가 |
| 서브쿼리 없음 | SELECT 절에 서브쿼리 있으면 불가 |

> [!warning] 자주 하는 실수
> "뷰는 가상 테이블이니까 INSERT/UPDATE도 자유롭게 된다"고 생각하는 것.
> 위 조건 중 하나라도 어기면 DML이 거부된다.

---

## Materialized View (구체화 뷰)

일반 뷰와 달리 **결과를 실제로 저장**하는 뷰.

| 항목 | 일반 View | Materialized View |
|------|-----------|-------------------|
| 데이터 저장 | ❌ (가상) | ✅ (실제 저장) |
| 조회 성능 | 느림 (매번 쿼리 실행) | 빠름 (저장된 데이터 조회) |
| 데이터 최신성 | 항상 최신 | REFRESH 필요 |
| 지원 DB | 모든 RDBMS | Oracle, PostgreSQL 등 |

> [!tip] 언제 Materialized View를 쓰나
> - 집계 결과 조회가 잦고, 데이터 변경이 드문 경우
> - 대용량 테이블의 복잡한 JOIN 결과를 캐싱할 때
> - 실시간성보다 성능이 중요한 리포트 쿼리

---

## 면접 포인트

> [!note] 자주 나오는 질문

**Q. View와 테이블의 차이?**
→ 테이블은 실제 데이터를 저장하고, 뷰는 쿼리 정의만 저장하는 가상 테이블이다.
조회 시 뷰의 내부 쿼리가 실행되어 결과를 반환한다.

**Q. View를 사용하는 이유?**
→ 보안(민감 컬럼 숨김), 복잡한 쿼리 단순화, 논리적 데이터 독립성 확보.

**Q. View에서 INSERT/UPDATE가 항상 가능한가?**
→ 아니다. 단일 테이블 기반 + GROUP BY/집계함수/DISTINCT/서브쿼리가 없는 경우에만 가능.

**Q. View와 Materialized View의 차이?**
→ 일반 View는 가상 테이블로 매번 쿼리를 재실행하지만, Materialized View는 결과를 실제로 저장해 조회 성능이 빠르다. 단, 데이터 최신성을 위해 주기적 REFRESH가 필요하다.

---

## 관련 개념

- [[DB키]] — 뷰 정의 시 PK를 포함하면 DML 가능성 높아짐
- [[인덱스]] — 뷰는 원본 테이블의 인덱스를 간접 활용, 직접 인덱스 생성 불가
- [[정규화]] — 정규화된 테이블을 뷰로 JOIN해 비정규화된 형태처럼 제공 가능
