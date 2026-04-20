---
title: "백준 1149 - RGB 거리"
type: problem
tags:
  - dp
  - tabulation
  - 2d-dp
  - state-dp
related:
  - "[[DP_Tabulation]]"
  - "[[BOJ_2579_계단오르기]]"
created: 2026-04-07
updated: 2026-04-07
sources:
  - offline/day0407/Main_B_1149_RGB거리_김상해.java
---

# 백준 1149 - RGB 거리

> N개의 집을 R/G/B로 칠할 때, 인접한 집이 같은 색이 되지 않도록 하는 최소 비용

## 핵심 아이디어

각 집을 칠하는 색의 **상태**를 DP 테이블의 두 번째 차원으로 표현.  
`dp[i][c]` = i번째 집을 색 c로 칠할 때까지의 최소 비용

인접 집의 색이 달라야 하므로, i번째 집을 색 c로 칠하면  
i-1번째 집은 c가 아닌 나머지 두 색 중 최솟값을 선택.

## 점화식

```
dp[i][0] = min(dp[i-1][1], dp[i-1][2]) + cost[i][0]  // i번째를 R로 칠하는 경우
dp[i][1] = min(dp[i-1][0], dp[i-1][2]) + cost[i][1]  // i번째를 G로 칠하는 경우
dp[i][2] = min(dp[i-1][0], dp[i-1][1]) + cost[i][2]  // i번째를 B로 칠하는 경우
```

## 기저값

```
dp[0][0] = cost[0][0]
dp[0][1] = cost[0][1]
dp[0][2] = cost[0][2]
```

## 정답

```
min(dp[N-1][0], dp[N-1][1], dp[N-1][2])
```

## Java 코드

```java
int N = Integer.parseInt(br.readLine());
int[][] cost = new int[N][3];
int[][] dp   = new int[N][3];

for (int i = 0; i < N; i++) {
    st = new StringTokenizer(br.readLine());
    cost[i][0] = Integer.parseInt(st.nextToken());
    cost[i][1] = Integer.parseInt(st.nextToken());
    cost[i][2] = Integer.parseInt(st.nextToken());
}

dp[0][0] = cost[0][0];
dp[0][1] = cost[0][1];
dp[0][2] = cost[0][2];

for (int i = 1; i < N; i++) {
    dp[i][0] = Math.min(dp[i-1][1], dp[i-1][2]) + cost[i][0];
    dp[i][1] = Math.min(dp[i-1][0], dp[i-1][2]) + cost[i][1];
    dp[i][2] = Math.min(dp[i-1][1], dp[i-1][0]) + cost[i][2];
}

System.out.println(Math.min(Math.min(dp[N-1][0], dp[N-1][1]), dp[N-1][2]));
```

## 시간복잡도

- O(N) — N개의 집 × 3가지 색 상태

## 패턴 특징: 2D 상태 DP

이 문제는 **"이전 상태에 따라 현재 선택이 제약되는"** 유형.  
→ `dp[i][상태]` 로 상태를 차원에 추가하는 방식.  
→ [[BOJ_2579_계단오르기]] 도 같은 구조 (연속 밟기 횟수를 상태로).

## 주의사항

- 마지막 정답은 `dp[N-1][c]` 중 최솟값 (N-1 인덱스)
- 배열 크기 주의: `dp[1000][3]`

## 관련 패턴

- [[DP_Tabulation]] — Bottom-Up 2D 상태 테이블
- [[BOJ_2579_계단오르기]] — 같은 "상태 제약" 구조
