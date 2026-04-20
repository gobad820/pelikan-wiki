---
title: "Servlet Listener"
type: concept
tags: [backend, web, listener, servlet, lifecycle, jakarta]
related:
  - "[[Servlet]]"
  - "[[Filter]]"
  - "[[20260420]]"
created: 2026-04-20
updated: 2026-04-20
sources:
  - ws/backend/BE_02/docs/Listener.md
  - ws/backend/BE_02/src/main/java/com/ssafy/live/listener/MyContextListener.java
---

## 정의

웹 애플리케이션에서 발생하는 특정 이벤트를 모니터링하고, 이벤트 발생 시 자동으로 호출되는 콜백 객체.
Servlet Container가 이벤트를 감지하고 등록된 Listener를 호출한다.

## 주요 Listener 인터페이스

| 인터페이스 | 이벤트 | 주요 용도 |
|-----------|--------|-----------|
| `ServletContextListener` | 웹앱 시작/종료 | 공유 자원 초기화/반납 |
| `ServletContextAttributeListener` | ServletContext 속성 변경 | 전역 속성 변경 감지 |
| `HttpSessionListener` | 세션 생성/소멸 | 동시 접속자 수 추적 |
| `HttpSessionAttributeListener` | 세션 속성 변경 | 세션 데이터 변경 감지 |
| `ServletRequestListener` | 요청 시작/종료 | 요청별 처리 시간 측정 |

## ServletContextListener

### 핵심 특성

- 웹 애플리케이션 전체 생명주기 모니터링
- **Eager 초기화**: 서버 시작 시 즉시 `contextInitialized()` 호출 (Servlet `init()`보다 먼저)
- 모든 서블릿이 공유하는 자원 초기화에 적합

### 구현 예제

```java
@WebListener
public class MyContextListener implements ServletContextListener {

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        // 웹앱 시작 시 단 1회 호출
        // DB ConnectionPool, 설정 파일 로드, 캐시 예열 등
        System.out.println("개별 서블릿에서 초기화하기 부담스러운 무거운 자원 초기화");

        // 공유 자원을 ServletContext에 저장 → 모든 서블릿에서 접근 가능
        // DataSource ds = createConnectionPool();
        // sce.getServletContext().setAttribute("dataSource", ds);
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        // 웹앱 종료 시 단 1회 호출
        // contextInitialized에서 초기화한 자원 정리
        System.out.println("init 과정에서 초기화한 자원에 대한 정리 작업");
    }
}
```

## Servlet.init() vs ServletContextListener 비교

| 구분 | `Servlet.init()` | `contextInitialized()` |
|------|-----------------|----------------------|
| 호출 시점 | 해당 서블릿 최초 요청 시 (Lazy) | 웹앱 시작 시 즉시 (Eager) |
| 적용 범위 | 해당 서블릿에서만 사용 | 모든 서블릿이 공유 |
| 용도 | 서블릿 전용 자원 | DB ConnectionPool 등 공유 자원 |

## 주의사항 / 자주 하는 실수

> [!warning] 실수 포인트
> - `@WebListener` 어노테이션 없으면 Container가 Listener를 인식하지 못함
> - `contextInitialized()`에서 초기화한 자원은 반드시 `contextDestroyed()`에서 반납해야 메모리 누수 방지
> - Listener에서 예외 발생 시 웹앱 시작 자체가 실패할 수 있음 — 초기화 로직은 try-catch로 보호

## 관련 개념

- [[Servlet]] — Listener가 모니터링하는 웹 컴포넌트
- [[Filter]] — Servlet의 3대 컴포넌트 (Servlet, Filter, Listener)
