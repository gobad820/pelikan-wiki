---
title: "SWEA 3282 - 0/1 Knapsack"
type: problem
tags:
  - dp
  - knapsack
  - tabulation
related:
  - "[[Knapsack_01]]"
  - "[[DP_Tabulation]]"
  - "[[SWEA_5215_햄버거다이어트]]"
created: 2026-04-08
updated: 2026-04-08
sources:
  - offline/day0408/Solution_3282_01Knapsack_김상해_2차원배열.java
  - offline/day0408/Solution_3282_01Knapsack_김상해_1차원배열_공간복잡도최적화.java
---
# SWEA 3282 - 0/1 Knapsack

## 문제 요약
N개 물건, 각각 무게 V와 가치 C. 최대 무게 K인 배낭에 담을 수 있는 최대 가치.
각 물건은 최대 1번만 선택 가능.

## 접근법

### 정의
- `dp[i][j]` = i번째 물건까지 고려, 무게 한도 j에서의 최대 가치

### 점화식
```
dp[i][j] = dp[i-1][j]                         (V[i] > j, 못 넣음)
dp[i][j] = max(dp[i-1][j], dp[i-1][j-V[i]] + C[i])   (V[i] ≤ j)
```

### 기저값
- `dp[0][j] = 0` (물건 0개 → 가치 0)

### 정답
- `dp[N][K]`

## 2D 풀이 코드
```java
for (int i = 1; i <= N; i++) {
    for (int j = 0; j <= K; j++) {
        dp[i][j] = (V[i] > j)
            ? dp[i - 1][j]
            : Math.max(dp[i - 1][j], dp[i - 1][j - V[i]] + C[i]);
    }
}
answer = dp[N][K];
```

## 1D 최적화 코드
```java
dp = new int[K + 1];
for (int i = 1; i <= N; i++) {
    for (int j = K; j >= V; j--) {
        dp[j] = Math.max(dp[j], dp[j - V] + C);
    }
}
answer = dp[K];
```

## 핵심 포인트
- 1D 역순 순회: 현재 아이템을 중복 선택하지 않기 위함
- 다중 TC이므로 매 TC마다 dp 배열 재초기화 필수

## 복잡도
- 시간: O(N × K)
- 공간: O(N × K) → 1D 최적화 시 O(K)
