---
title: 최소 신장 트리 - 프림 (MST Prim)
type: pattern
tags:
  - mst
  - prim
  - greedy
  - graph
  - adjacency-list
  - adjacency-matrix
related:
  - "[[MST_Kruskal]]"
  - "[[Dijkstra]]"
  - "[[Graph_AdjList]]"
created: 2026-04-06
updated: 2026-04-07
sources:
  - day0311/MST_Prim_Matrix.java
  - day0311/MST_Prim_AdjList.java
---
# 최소 신장 트리 - 프림 (MST Prim)

## 언제 쓰나
- **정점 중심** MST. 정점 수 V가 적을 때 유리 (밀집 그래프, E ≈ V²)
- 인접 행렬: O(V²) → 밀집 그래프에서 크루스칼보다 빠름
- 인접 리스트 + PQ: O(E log V) → 희소 그래프에서도 사용 가능

## 핵심 아이디어
트리 집합을 하나씩 확장.
1. 시작 정점의 `minEdge[0] = 0`, 나머지 INF
2. 미방문 정점 중 `minEdge` 최솟값 선택 → 트리에 추가
3. 새 정점에서 갈 수 있는 미방문 정점의 `minEdge` 갱신
4. V번 반복

## 시간복잡도
- 인접 행렬: O(V²)
- 인접 리스트 + PQ: O(E log V)

## Java 코드 템플릿

### 패턴 1: 인접 행렬
```java
static int[][] adjMatrix;
static int[] minEdge;
static boolean[] visited;

Arrays.fill(minEdge, Integer.MAX_VALUE);
minEdge[0] = 0;
int result = 0;
for (int c = 0; c < V; c++) {
    // Step1: 비트리 정점 중 최소 minEdge 정점 선택
    int min = Integer.MAX_VALUE, minVertex = -1;
    for (int i = 0; i < V; i++) {
        if (!visited[i] && min > minEdge[i]) { minVertex = i; min = minEdge[i]; }
    }
    if (minVertex == -1) break;
    visited[minVertex] = true;
    result += min;

    // Step2: 새 정점(minVertex) 기준 minEdge 갱신
    for (int i = 0; i < V; i++) {
        if (!visited[i] && adjMatrix[minVertex][i] != 0 && minEdge[i] > adjMatrix[minVertex][i])
            minEdge[i] = adjMatrix[minVertex][i];
    }
}
```

### 패턴 2: 인접 리스트 (연결 리스트 Node)
```java
// Step2만 다름 — 인접 정점만 순회
for (Node temp = adjList[minVertex]; temp != null; temp = temp.next) {
    if (!visited[temp.vertex] && minEdge[temp.vertex] > temp.weight)
        minEdge[temp.vertex] = temp.weight;
}
```

## 주의사항 / 자주 하는 실수
- `minEdge[0] = 0` 전처리 필수 — 첫 번째 반복에서 정점 0이 선택됨
- `adjMatrix[i][j] != 0` 조건: 간선 없는 곳은 0으로 표현 (INF가 아님)
- `c == V ? result : -1` — 비연결 그래프 처리
- 프림은 visited 배열 필요, Dijkstra와 유사하지만 `minEdge`에 **간선 비용** 저장 (누적 X)

## 관련 패턴
- [[MST_Kruskal]] — 간선 중심 MST, 희소 그래프 선호
- [[Dijkstra]] — 구조 유사하지만 최단 거리(누적) vs 최소 간선(단일) 차이
- [[Graph_AdjList]] — 인접 리스트 표현
