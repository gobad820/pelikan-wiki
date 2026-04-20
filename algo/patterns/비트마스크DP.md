---
title: "비트마스크 DP (Bitmask DP)"
type: pattern
tags: [dp, bitmask, tsp, combinatorial-optimization]
related:
  - "[[DP_Tabulation]]"
  - "[[DP_Memoization]]"
  - "[[Greedy]]"
created: 2026-04-14
updated: 2026-04-14
sources:
  - mogakal/baekjoon/gold/_2098.java
---

> [!abstract] 핵심 요약
> 부분집합을 비트로 표현해 DP 상태로 삼는다. 방문한 노드 집합을 정수 하나로 표현하므로 `dp[visited][curr]` 형태로 완전탐색을 메모이제이션.

## 언제 쓰나

- 선택/미선택 상태를 가진 N개 원소 (N ≤ 20 수준)
- **TSP(외판원 순회)**: 방문한 도시 집합 + 현재 위치로 상태 정의
- 순열 최적화: 모든 순서를 열거하면 O(N!)이지만 비트마스크 DP로 O(N² × 2^N)
- 집합 포함 여부를 상태로 표현해야 할 때

## 핵심 아이디어

1. `visited` = 방문한 노드를 비트로 표현한 정수 (0 = 미방문, 1 = 방문)
2. 상태: `dp[visited][curr]` = 지금까지 `visited` 집합을 방문하고, 현재 `curr`에 있을 때 최소 비용
3. 전이: `dp[visited | (1<<next)][next] = min(dp[visited | (1<<next)][next], dp[visited][curr] + cost[curr][next])`
4. Bottom-up으로 방문 집합 크기를 늘려가며 채움

### 비트 연산 핵심

```java
// i번 비트 켜기
visited | (1 << i)
// i번 비트 확인
(visited >> i) & 1
// i번 비트 꺼기
visited & ~(1 << i)
// 전체 방문 확인
visited == (1 << N) - 1
```

## 복잡도

| 항목 | 복잡도 | 비고 |
|------|--------|------|
| 시간 | O(N² × 2^N) | 추정: 상태 N×2^N, 전이 N |
| 공간 | O(N × 2^N) | 추정: dp 배열 크기 |

N = 20이면 약 4억 → 간신히 가능. N = 25 이상이면 다른 방법 필요.

## Java 코드 템플릿

```java
// TSP (외판원 순회) - Bottom-up
static final int INF = Integer.MAX_VALUE / 2;
static int N;
static int[][] cost;   // cost[i][j]: i→j 비용 (0이면 경로 없음)
static int[][] dp;     // dp[visited][curr] = 최소 비용

static int tsp() {
    dp = new int[1 << N][N];
    for (int[] row : dp) Arrays.fill(row, INF);
    dp[1][0] = 0; // 0번 도시에서 시작, 0번만 방문

    for (int visited = 1; visited < (1 << N); visited++) {
        for (int curr = 0; curr < N; curr++) {
            if (dp[visited][curr] == INF) continue;
            if ((visited & (1 << curr)) == 0) continue; // curr 미방문이면 스킵

            for (int next = 0; next < N; next++) {
                if ((visited & (1 << next)) != 0) continue; // 이미 방문
                if (cost[curr][next] == 0) continue;        // 경로 없음
                int nVisited = visited | (1 << next);
                dp[nVisited][next] = Math.min(dp[nVisited][next],
                                              dp[visited][curr] + cost[curr][next]);
            }
        }
    }

    // 모든 도시 방문 후 0번으로 복귀
    int full = (1 << N) - 1;
    int ans = INF;
    for (int curr = 1; curr < N; curr++) {
        if (dp[full][curr] == INF || cost[curr][0] == 0) continue;
        ans = Math.min(ans, dp[full][curr] + cost[curr][0]);
    }
    return ans;
}
```

## 대표 문제

- [[BOJ_2098_외판원순회]] (BOJ 2098) — TSP 교과서 문제, `dp[1<<N][N]` bottom-up

## 주의사항 / 자주 하는 실수

> [!warning] 실수 포인트
> - **시작 노드 처리**: `dp[1<<0][0] = 0` (0번 노드에서 시작, 0번만 방문 상태)
> - **복귀 비용 누락**: 마지막에 출발점으로 돌아오는 `cost[curr][0]` 추가 필수
> - **경로 없음 처리**: `cost[i][j] == 0`이면 경로 없음으로 간주 (BOJ 2098 기준)
> - **INF 오버플로우**: `INF + cost`가 오버플로우되지 않도록 `Integer.MAX_VALUE / 2` 사용
> - **0번 노드 중복 방문 방지**: 최종 루프에서 `curr = 1`부터 시작 (0은 출발점)

## 관련 패턴

- [[DP_Tabulation]] — bottom-up 방식으로 채움
- [[DP_Memoization]] — top-down 방식도 가능 (재귀 + 캐시)
- [[Backtracking]] — N이 작을 때 순열 완전탐색 대안
