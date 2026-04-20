---
title: "DP - 타뷸레이션 (Bottom-Up)"
type: pattern
tags: 
  - dp
  - tabulation
  - iterative
  - bottom-up
related:
  - "[[DP_Memoization]]"
  - "[[Knapsack_01]]"
  - "[[LIS]]"
  - "[[BOJ_1010_다리놓기]]"
  - "[[BOJ_1149_RGB거리]]"
  - "[[BOJ_11726_2xn타일링]]"
  - "[[BOJ_1463_1로만들기]]"
  - "[[BOJ_2579_계단오르기]]"
created: 2026-04-06
updated: 2026-04-07
sources:
  - day0406/FibonacciTest.java
  - offline/day0407/Main_B_1010_다리놓기_김상해_상향식.java
  - offline/day0407/Main_B_1149_RGB거리_김상해.java
  - offline/day0407/Main_B_11726_2xn타일링_김상해.java
  - offline/day0407/Main_B_1463_1로만들기_김상해.java
---
# DP - 타뷸레이션 (Bottom-Up)

## 언제 쓰나
- 모든 부분 문제의 답이 필요한 경우
- 재귀 깊이가 깊어 스택 오버플로우 위험이 있을 때
- 점화식의 순서가 명확할 때

## 핵심 아이디어
작은 문제부터 순서대로 반복문으로 채워나간다.
재귀 없이 `dp[i] = dp[i-1] + dp[i-2]` 형태로 테이블을 완성.

## 시간복잡도
- O(N)

## Java 코드 템플릿

```java
static long[] dp = new long[MAXVAL + 1];

static {
    // base case 먼저 설정
    dp[0] = 0;
    dp[1] = 1;
    // 순서대로 채우기
    for (int i = 2; i <= MAXVAL; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
}
```

## day0406 실제 코드 포인트
```java
static {
    dp = new long[MAXVAL+1];
    Arrays.fill(dp, -1);

    dp[1] = 1;
    dp[2] = 1;                          // base case (1-indexed)
    for (int i = 3; i <= 100; i++) {
        dp[i] = dp[i-1] + dp[i-2];     // bottom-up
    }
}
```
> `static {}` 블록으로 클래스 로딩 시 전체 테이블을 미리 채운다.
> 이후 쿼리는 O(1) — `dp[N]` 바로 조회.

## 2D 상태 DP 템플릿

상태를 2번째 차원으로 표현하는 패턴. "이전에 무엇을 선택했는가"로 현재 선택이 제약될 때 사용.

```java
// RGB거리 형태: dp[i][색] = i번째를 해당 색으로 칠할 때 최소 비용
int[][] dp = new int[N][3];
dp[0][0] = cost[0][0];
dp[0][1] = cost[0][1];
dp[0][2] = cost[0][2];
for (int i = 1; i < N; i++) {
    dp[i][0] = Math.min(dp[i-1][1], dp[i-1][2]) + cost[i][0];
    dp[i][1] = Math.min(dp[i-1][0], dp[i-1][2]) + cost[i][1];
    dp[i][2] = Math.min(dp[i-1][0], dp[i-1][1]) + cost[i][2];
}
```

## 2D 파스칼 삼각형 / 조합 DP 템플릿

```java
// dp[N][M] = C(M, N) = mCn
// 점화식: dp[i][j] = dp[i][j-1] + dp[i-1][j-1]
static long[][] dp = new long[30][30];
static {
    for (int i = 1; i < 30; i++) dp[1][i] = i;  // C(M,1) = M
    for (int i = 2; i < 30; i++) {
        dp[i][i] = 1;                             // C(N,N) = 1
        for (int j = i + 1; j < 30; j++) {
            dp[i][j] = dp[i][j-1] + dp[i-1][j-1];
        }
    }
}
```

## 다중 전이 DP 템플릿

조건에 따라 여러 상태에서 전이 가능한 패턴.

```java
// 1로 만들기 형태: 나누기 조건 분기
Arrays.fill(dp, Integer.MAX_VALUE);
dp[1] = 0;
for (int i = 2; i < MAXVAL; i++) {
    dp[i] = dp[i-1] + 1;                                   // -1 연산
    if (i % 3 == 0) dp[i] = Math.min(dp[i], dp[i/3] + 1); // ÷3 연산
    if (i % 2 == 0) dp[i] = Math.min(dp[i], dp[i/2] + 1); // ÷2 연산
}
```

## 대표 문제
- 피보나치 수열 전처리, 백준 1003
- [[BOJ_11726_2xn타일링]] — 피보나치형 1D DP
- [[BOJ_1463_1로만들기]] — 다중 전이 1D DP
- [[BOJ_1149_RGB거리]] — 2D 상태 DP
- [[BOJ_2579_계단오르기]] — 2D 상태 DP (연속 제약)
- [[BOJ_1010_다리놓기]] — 2D 파스칼 DP (조합)

## 주의사항 / 자주 하는 실수
- base case 인덱스 주의 (0-indexed vs 1-indexed)
- `dp[1] = 1, dp[2] = 1` 처럼 직접 지정 vs `dp[0] = 0, dp[1] = 1`은 문제마다 다름
- 배열 크기가 쿼리 범위를 커버해야 함
- 정답이 마지막 인덱스 값이 아닐 수 있음 — 문제가 원하는 상태 값을 확인 (DP 7단계 6번)
- 2D 상태 DP에서 `dp[i-1][같은 상태]` 를 재사용하면 제약 조건 우회 버그 발생

## 관련 패턴
- [[DP_Memoization]] — 같은 결과, Top-Down 방식. 필요한 것만 계산하는 장점
- [[Knapsack_01]] — 대표적인 2D Tabulation + 1D 공간 최적화 패턴
- [[LIS]] — O(N²) Tabulation의 전형적 예시; 이진탐색 결합 시 O(N log N)
