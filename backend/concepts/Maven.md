---
title: "Maven"
type: concept
tags: [backend, build, maven, dependency, pom]
related:
  - "[[Servlet]]"
  - "[[20260416]]"
created: 2026-04-16
updated: 2026-04-16
sources:
  - BE_01/docs/Maven.md
---

## 정의

Java 프로젝트의 빌드 자동화 및 의존성 관리 도구.
`pom.xml` 파일 하나로 라이브러리 버전 동기화, 빌드, 테스트, 패키징을 관리한다.

## 등장 배경

- 프로젝트 규모가 커지면서 관리 라이브러리가 많아짐
- 팀원 간 라이브러리 버전 불일치 문제 발생
- `pom.xml`로 의존성을 선언하면 Maven이 자동으로 다운로드 및 동기화

## 주요 기능

| 기능 | 설명 |
|------|------|
| 의존성 관리 | `<dependency>` 태그로 라이브러리 선언, Maven Central에서 자동 다운로드 |
| 빌드 자동화 | compile → test → package → install → deploy 단계 자동화 |
| 프로젝트 구조 표준화 | `src/main/java`, `src/main/resources`, `src/test/java` 표준 디렉토리 |
| 플러그인 생태계 | 다양한 플러그인으로 기능 확장 |

## pom.xml 기본 구조

```xml
<project>
  <groupId>com.ssafy</groupId>       <!-- 조직 식별자 (패키지명 관례) -->
  <artifactId>BE_01</artifactId>     <!-- 프로젝트 이름 -->
  <version>0.0.1-SNAPSHOT</version>  <!-- 버전 -->
  <packaging>war</packaging>         <!-- 패키징 방식 -->

  <dependencies>
    <!-- Servlet API (Tomcat이 제공하므로 provided scope) -->
    <dependency>
      <groupId>jakarta.servlet</groupId>
      <artifactId>jakarta.servlet-api</artifactId>
      <version>6.0.0</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>
</project>
```

## Dependency Scope

| Scope | 컴파일 | 런타임 | 테스트 | 설명 |
|-------|--------|--------|--------|------|
| compile (기본) | O | O | O | 모든 단계에서 사용 |
| provided | O | X | O | 런타임에 컨테이너가 제공 (Servlet API 등) |
| runtime | X | O | O | 런타임에만 필요 (JDBC 드라이버 등) |
| test | X | X | O | 테스트 코드에만 사용 (JUnit 등) |

> Servlet API는 `provided` — Tomcat이 런타임에 제공하므로 WAR에 포함하면 안 됨

## 알고리즘과의 관계

- [[Servlet]] — Maven으로 Servlet 프로젝트 의존성 관리 (jakarta.servlet-api)
