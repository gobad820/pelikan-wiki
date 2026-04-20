---
title: "Front Controller Pattern"
type: concept
tags: [backend, web, servlet, pattern, architecture, jakarta]
related:
  - "[[Servlet]]"
  - "[[Filter]]"
  - "[[20260420]]"
created: 2026-04-20
updated: 2026-04-20
sources:
  - ws/backend/BE_02/docs/review.md
  - ws/backend/BE_02/src/main/java/com/ssafy/live/controller/MainServlet.java
  - ws/backend/BE_02/src/main/java/com/ssafy/live/controller/ControllerHelper.java
  - ws/backend/BE_02/src/main/java/com/ssafy/live/controller/GuguService.java
---

## 정의

모든 클라이언트 요청을 단일 진입점(Front Controller)에서 받아 처리를 분배하는 아키텍처 패턴.
Spring MVC의 `DispatcherServlet`이 이 패턴의 대표적인 구현체이다.

## 기존 서블릿 방식의 문제점

| 문제 | 설명 |
|------|------|
| 1:1 매핑 | 요청마다 새로운 서블릿 생성 필요 |
| 코드 중복 | 로깅, 예외 처리, 응답 방식을 각 서블릿에 반복 구현 |
| 유지보수 어려움 | 요청 처리 로직이 여러 서블릿에 분산 |

## Front Controller 패턴의 장점

| 장점 | 설명 |
|------|------|
| 단일 진입점 | 모든 요청이 Front Controller를 거쳐 일관성 있는 처리 |
| 공통 처리 | 로깅, 인증, 인코딩을 한 곳에서 처리 |
| 유연한 확장성 | 기존 구조 변경 없이 새 기능 추가 가능 |
| 코드 간결성 | 보일러플레이트 감소, 가독성 향상 |

## 요청 구분 방식 (action 파라미터)

```
GET /main?action=guguform     → guguform() 호출
GET /main?action=gugu&dan=3   → gugu() 호출
POST /main (action=write)     → write() 호출
```

## 구현 예제

### Front Controller (MainServlet)

```java
@WebServlet("/main")
public class MainServlet extends HttpServlet implements ControllerHelper {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        String action = preProcessing(request, response);
        switch (action) {
        case "guguform":
            guguform(request, response);
            break;
        case "gugu":
            gugu(request, response);
            break;
        }
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        String action = preProcessing(request, response);
        try {
            switch (action) {
            case "write":
                write(request, response);
                break;
            }
        } catch (ServletException | IOException e) {
            e.printStackTrace();
        }
    }
}
```

### ControllerHelper 인터페이스 (공통 유틸리티)

```java
public interface ControllerHelper {

    default String preProcessing(HttpServletRequest request, HttpServletResponse response) {
        String action = request.getParameter("action");
        if (action == null || action.isBlank()) action = "index";
        return action;
    }

    default void responseHtml(String title, String content, HttpServletResponse response)
            throws IOException {
        String html = "<html><body><h1>%s</h1><div>%s</div></body></html>"
                        .formatted(title, content);
        response.setContentType("text/html;charset=UTF-8");
        response.getWriter().append(html);
    }
}
```

### Service 클래스 (비즈니스 로직 분리)

```java
public class GuguService {
    public String gugu(int dan) {
        StringBuilder builder = new StringBuilder();
        for (int i = 1; i < 10; i++) {
            builder.append(dan * i + "<br>");
        }
        return builder.toString();
    }
}
```

## Switch 표현식과 Checked Exception 주의

> [!note] 화살표 Switch와 Checked Exception (Java 14+ 기준)
> **Switch 문(statement)** 에서의 화살표(`->`)는 람다가 아니므로 Checked Exception이 **정상 전파**된다.
> Checked Exception 전파 제약은 값을 반환하는 **Switch 식(expression)** 에서만 해당된다.
>
> ```java
> // Switch 문(statement) — Checked Exception 전파 가능 (Java 14+)
> switch (action) {
>     case "write" -> write(request, response);  // IOException 전파 가능
> }
>
> // Switch 식(expression) — Checked Exception 전파 불가
> var result = switch (action) {
>     case "write" -> write(request, response);  // 컴파일 오류
> };
> ```

## URL Mapping 방식

| 방식 | 예시 | 특징 |
|------|------|------|
| action 파라미터 | `/main?action=hello` | 단순, Form에서 hidden input으로 전달 |
| 와일드카드 URL | `/board/*`, `*.do` | RESTful에 가까운 URL 구조 |
| `/` 매핑 | `@WebServlet("/")` | 매핑되지 않은 모든 요청 처리 (Spring MVC 기본) |

## 관련 개념

- [[Servlet]] — Front Controller의 기반 기술
- [[Filter]] — Front Controller 앞단에서 공통 처리 수행
