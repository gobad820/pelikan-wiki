---
title: "0/1 배낭 문제 (0/1 Knapsack)"
type: pattern
tags:
  - dp
  - knapsack
  - tabulation
  - bottom-up
related:
  - "[[DP_Tabulation]]"
  - "[[SWEA_3282_01Knapsack]]"
  - "[[SWEA_5215_햄버거다이어트]]"
created: 2026-04-08
updated: 2026-04-08
sources:
  - offline/day0408/Solution_3282_01Knapsack_김상해_2차원배열.java
  - offline/day0408/Solution_3282_01Knapsack_김상해_1차원배열_공간복잡도최적화.java
  - offline/day0408/Solution_5215_햄버거다이어트_김상해_dp.java
---
# 0/1 배낭 문제 (0/1 Knapsack)

## 언제 쓰나
- N개의 물건 각각을 **넣거나 안 넣거나** (0 또는 1) 선택할 수 있을 때
- 각 물건에 무게(비용)와 가치(이득)가 있고, 최대 수용 무게 K 내에서 **가치 최대화**
- 분할 가능 배낭(Fractional Knapsack)과 달리 물건을 쪼갤 수 없음

## 핵심 아이디어
각 물건을 **넣는 경우** vs **안 넣는 경우** 중 최적을 선택.
물건 i를 넣으면 이전 상태 `dp[i-1][j - V[i]]`에서 `C[i]`만큼 가치가 증가.

## 시간복잡도
- O(N × K) — N: 물건 수, K: 최대 무게

## 2D 배열 풀이 (기본형)

`dp[i][j]` = i번째 물건까지 고려했을 때, 무게 한도 j에서의 최대 가치

### 점화식
```
V[i] > j → dp[i][j] = dp[i-1][j]          // 물건이 너무 무거워서 못 넣음
V[i] ≤ j → dp[i][j] = max(dp[i-1][j], dp[i-1][j-V[i]] + C[i])
```

### Java 코드 템플릿
```java
int[][] dp = new int[N + 1][K + 1];
int[] V = new int[N + 1]; // 무게 (Volume)
int[] C = new int[N + 1]; // 가치 (Cost/Value)

for (int i = 1; i <= N; i++) {
    for (int j = 0; j <= K; j++) {
        dp[i][j] = (V[i] > j)
            ? dp[i - 1][j]
            : Math.max(dp[i - 1][j], dp[i - 1][j - V[i]] + C[i]);
    }
}
int answer = dp[N][K];
```

## 1D 배열 풀이 (공간 최적화)

2D 배열에서 행을 롤링(이전 행만 참조)하므로 1D로 줄일 수 있다.

### 핵심: **역순 순회 필수**
- `j = K → V` 방향(역순)으로 채워야 한다.
- 순방향으로 채우면 같은 아이템이 여러 번 선택돼 **분할 가능 배낭**이 됨.
- 역순이면 `dp[j - V]`가 아직 이번 아이템을 반영하지 않은 값 → 중복 선택 방지.

### Java 코드 템플릿
```java
int[] dp = new int[K + 1]; // 초기값 0

for (int i = 1; i <= N; i++) {
    int v = V[i], c = C[i];
    for (int j = K; j >= v; j--) {   // 역순!
        dp[j] = Math.max(dp[j], dp[j - v] + c);
    }
}
int answer = dp[K];
```

## 대표 문제
- [[SWEA_3282_01Knapsack]] — 기본 0/1 Knapsack (2D + 1D 두 풀이)
- [[SWEA_5215_햄버거다이어트]] — 칼로리 = 무게, 맛 = 가치로 치환한 응용

## 주의사항 / 자주 하는 실수

| 실수 | 원인 | 해결 |
|------|------|------|
| 1D 순방향 순회 | 중복 선택 발생 → Fractional Knapsack이 됨 | **역순(K → V[i])** 순회 |
| `j >= 0` 루프 조건 | `j < V[i]`인 경우도 갱신 시도 → 배열 음수 인덱스 | `j >= V[i]` 조건 |
| 초기화 없이 재사용 | 다중 TC에서 이전 결과 오염 | TC마다 `dp` 배열 재초기화 |
| 2D에서 `dp[i-1]` 대신 `dp[i]` 참조 | 같은 행 갱신 → 중복 선택 | `dp[i-1]` 행만 참조 |

## 관련 패턴
- [[DP_Tabulation]] — 타뷸레이션 Bottom-Up DP 일반 패턴
- **분할 가능 배낭(Fractional Knapsack)** — 그리디로 해결 (가치/무게 비율 정렬)
- **Unbounded Knapsack** — 물건을 여러 번 선택 가능, 1D 순방향 순회
