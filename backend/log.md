# 위키 작업 로그

## [2026-04-20] ingest | be:front-controller-app 소스코드 + 오후 문답
- 소스: FrontController.java, LoggingFilter.java, MyContextListener.java (3개)
- 생성: 없음 (기존 페이지 업데이트)
- 업데이트: lectures/20260420.md
  - Switch 문(statement) vs 식(expression) Checked Exception 정오 (Java 14+ 기준 수정)
  - Filter Chain 내부 동작 (ApplicationFilterChain) 섹션 추가
  - HttpServlet 템플릿 메서드 패턴 흐름 섹션 추가
  - @Override 역할 및 HttpFilter.doFilter 자동 생성 안 되는 이유 섹션 추가
  - getParameterMap() 해시값 출력 문제 + 해결 섹션 추가
  - 트러블슈팅: FrontController 메서드명 혼용 버그, Arrays.toString 사용 추가
- 업데이트: concepts/FrontControllerPattern.md (switch 화살표 문법 정오 수정)
- 업데이트: concepts/Filter.md (ApplicationFilterChain 내부 동작, @Override 설명 추가)
- 스킵: 없음

## [2026-04-20] ingest | be:BE_02/docs/* + BE_02 소스코드
- 소스: BE_02/docs/Servlet.md, filter.md, Listener.md, review.md, MainServlet.java, XssFilter.java, LogginFilter.java, MyContextListener.java (8개)
- 생성: concepts/Filter.md (Filter·FilterChain·XSSFilter·HttpServletRequestWrapper)
- 생성: concepts/Listener.md (ServletContextListener·@WebListener)
- 생성: concepts/FrontControllerPattern.md (action 파라미터 분기, switch Checked Exception)
- 생성: lectures/20260420.md (오늘 학습 노트 — Filter/Listener/FrontController + 트러블슈팅)
- 업데이트: BE_02/docs/Servlet.md (멀티스레드 Mermaid, Mutex/Semaphore 코드 예제)
- 업데이트: BE_02/docs/filter.md (Filter 흐름도 Mermaid, Filter 작성 코드, XSS Wrapper 완성본, 순서 제어)
- 업데이트: BE_02/docs/Listener.md (이벤트 표, 구현 예제, Servlet.init() vs contextInitialized() 비교)
- 업데이트: BE_02/docs/review.md (FrontController 코드, switch 화살표 경고, ControllerHelper, Service 분리)
- 업데이트: index.md (total_pages 8, 신규 개념 3개 + 강의노트 1개 추가)
- 스킵: 없음

## [2026-04-16] ingest | be:project/src/main/java/com/ssafy/*
- 소스: HelloServlet.java, LoginServlet.java, LifeCycleServlet.java (3개)
- 생성: 없음
- 업데이트: concepts/Servlet.md
  - Query String & Query Parameter 섹션 신규 추가
    (URL 형식, GET vs POST 비교표, LoginServlet 실전 코드, null 주의 callout)
- 스킵: HelloServlet.java (이미 반영), LifeCycleServlet.java (이미 반영)

## [2026-04-16] ingest | be:BE_01/docs/Servlet.md (Lifecycle 보강)
- 소스: BE_01/docs/Servlet.md (+111줄 신규 추가 감지)
- 생성: 없음
- 업데이트: concepts/Servlet.md
  - Lifecycle 단계표 추가 (4단계: 객체생성→init→service→destroy)
  - LifeCycleServlet 실습 코드 추가 (생성자 포함)
  - 실행 결과 비교 추가 (1번째 요청 / 2번째 요청 / 서버 종료)
  - 싱글톤 동작 인사이트 note 추가
- 스킵: 없음

## [2026-04-16] ingest | be:BE_01/docs/Servlet.md (API 보완)
- 소스: BE_01/docs/Servlet.md (추가 내용 분석)
- 생성: 없음
- 업데이트: concepts/Servlet.md
  - HttpServletRequest에 getServletPath(), getHeader(), getInputStream() 추가
  - Request Parameter 특성 섹션 추가
  - HttpServletResponse에 setStatus(), getOutputStream() 추가
  - Content-Type, Character Encoding 상세 섹션 추가
  - Lifecycle Mermaid에 doPut(), 요청 루프(K→A) 추가
- 스킵: 없음

## [2026-04-16] ingest | be:BE_01/docs/*
- 소스: BE_01/docs/web-architecture.md (신규), BE_01/docs/용어정리.md (비어 있음 → 스킵)
- 생성: concepts/웹아키텍처.md (Web Server/WAS/HTTP/Tomcat/Servlet 관계)
- 업데이트: lectures/20260416.md (웹아키텍처 개념 추가), index.md (total_pages 4)
- 스킵: BE_01/docs/Servlet.md, BE_01/docs/Maven.md (이전 세션에서 완료)

## [2026-04-16] ingest | 로그인 흐름 (PRG 패턴) 추가
- 소스: BE_01/docs/Servlet.md (로그인 시나리오 섹션 추가)
- 업데이트: concepts/Servlet.md (Mermaid 시퀀스 다이어그램 + 단계별 HTTP 메시지 + PRG 패턴)
- 업데이트: lectures/20260416.md (로그인 흐름 요약 + PRG 패턴 표)

## [2026-04-16] ingest | HTTP Request/Response 메시지 구조 추가
- 소스: BE_01/docs/Servlet.md (HTTP 메시지 구조 섹션 추가)
- 업데이트: concepts/Servlet.md (HTTP Request/Response 구조, 주요 헤더 테이블 추가)
- 업데이트: lectures/20260416.md (HTTP 메시지 구조 요약 섹션 추가)

## [2026-04-16] ingest | BE_01 Servlet + Maven 학습 노트
- 소스: BE_01/docs/Servlet.md (완성본), BE_01/docs/Maven.md
- 생성: concepts/Servlet.md (Lifecycle, Request/Response API, HTTP Method/Status)
- 생성: concepts/Maven.md (pom.xml, 의존성 관리)
- 생성: lectures/20260416.md (오늘 학습 정리)
- 업데이트: index.md (개념 2개, 강의노트 1개, total_pages: 3)
