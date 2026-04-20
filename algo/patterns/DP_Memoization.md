---
title: "DP - 메모이제이션 (Top-Down)"
type: pattern
tags: 
  - dp
  - memoization
  - recursion
  - top-down
related:
  - "[[DP_Tabulation]]"
created: 2026-04-06
updated: 2026-04-07
sources:
  - day0406/FibonacciTest.java
---

# DP - 메모이제이션 (Top-Down)

## 언제 쓰나
- 재귀로 자연스럽게 표현되는 문제
- 전체 부분 문제 중 일부만 필요할 때 (필요한 것만 계산)
- 점화식이 직관적으로 보이는 경우

## 핵심 아이디어
재귀 함수에 **캐시(dp 배열)** 를 추가해서, 한 번 계산한 값은 다시 계산하지 않는다.
`dp[n] != -1` 이면 이미 계산된 값 → 바로 리턴.

## 시간복잡도
- O(N) — 각 상태를 한 번만 계산

## Java 코드 템플릿

```java
static long[] dp = new long[MAXVAL + 1];

static {
    Arrays.fill(dp, -1); // -1로 초기화 (미계산 표시)
}

static long solve(int n) {
    // base condition
    if (n <= 1) return n;

    // 캐시 히트
    if (dp[n] != -1) return dp[n];

    // 계산 후 저장
    return dp[n] = solve(n - 1) + solve(n - 2);
}
```

## day0406 실제 코드 포인트
```java
// call[] 배열로 각 n이 몇 번 호출됐는지 추적 — 메모이제이션 효과 시각화
static long fibo(int n) {
    ++totalCount;
    ++call[n];
    if (n < 2) return n;
    if (dp[n] != -1) return dp[n];       // 캐시 히트
    return dp[n] = fibo(n-1) + fibo(n-2); // 계산 + 저장
}
```
> `call[n]`을 출력해보면 메모이제이션 없을 때(지수적 호출) vs 있을 때(n 이하 각 1회)의 차이를 확인할 수 있다.

## 대표 문제
- 피보나치 수열
- 백준 1003 피보나치 함수
- 관련 패턴: [[DP_Tabulation]] (같은 문제를 반복문으로 푸는 방식)

## 주의사항 / 자주 하는 실수
- `dp` 배열 초기화를 **-1** 로 해야 함 (0도 유효한 답일 수 있으므로)
- base condition을 빠뜨리면 스택 오버플로우
- `long` 범위 주의 (피보나치는 빠르게 커짐)

## 관련 패턴
- [[DP_Tabulation]] — 같은 결과, Bottom-Up 방식. 스택 오버플로우 위험 없음
