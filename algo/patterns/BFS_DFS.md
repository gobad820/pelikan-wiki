---
title: "BFS / DFS - 트리 & 그래프 탐색"
type: pattern
tags: [bfs, dfs, tree, graph, queue, recursion]
related:
  - "[[Graph_AdjList]]"
  - "[[Backtracking]]"
  - "[[Dijkstra]]"
created: 2026-04-06
updated: 2026-04-15
sources:
  - live/day0204/bfs/CompleteBinaryTree.java
  - live/day0219/AdjListTest.java
  - live/day0414/Solution_5643_키순서_김상해_2DFS.java
  - mogakal/baekjoon/gold/_13460.java
---

# BFS / DFS - 트리 & 그래프 탐색

## 언제 쓰나
- **BFS**: 최단 거리, 레벨(깊이) 단위 탐색, 너비 우선
- **DFS**: 경로 탐색, 백트래킹, 사이클 감지

## 핵심 아이디어
- **BFS**: 큐(Queue) → FIFO. 가까운 노드부터 방문
- **DFS**: 재귀 or 스택 → LIFO. 깊이 우선 방문

## 시간복잡도
- O(V + E) — 정점 + 간선

## Java 코드 템플릿

### BFS (인접 리스트 그래프)
```java
private static void bfs(int start) {
    Deque<Integer> q = new ArrayDeque<>();
    boolean[] vis = new boolean[V];
    vis[start] = true;
    q.offerLast(start);

    while (!q.isEmpty()) {
        int cur = q.pollFirst();
        // 처리
        for (int next : adjList[cur]) {
            if (!vis[next]) {
                vis[next] = true;
                q.offerLast(next);
            }
        }
    }
}
```

### BFS 레벨 구분 (bfs3 패턴)
```java
// 레벨(깊이)를 구분해야 할 때
int breadth = 0;
Deque<Integer> q = new ArrayDeque<>();
q.offerLast(root);
while (!q.isEmpty()) {
    int size = q.size(); // 현재 레벨의 노드 수
    while (--size >= 0) {
        int cur = q.pollFirst();
        // 처리 (breadth = 현재 깊이)
        for (int next : children(cur)) q.offerLast(next);
    }
    ++breadth;
}
```

### 완전 이진 트리에서 인덱스 규칙 (1-indexed)
```java
// 노드 i의 자식: i*2, i*2+1
// 노드 i의 부모: i/2
int[] children = { cur * 2, cur * 2 + 1 };
```

### DFS - 전위/중위/후위 순회
```java
void preOrder(int cur) {   // 루트 → 왼쪽 → 오른쪽
    visit(cur);
    preOrder(cur * 2);
    preOrder(cur * 2 + 1);
}
void inOrder(int cur) {    // 왼쪽 → 루트 → 오른쪽 (정렬된 순서)
    inOrder(cur * 2);
    visit(cur);
    inOrder(cur * 2 + 1);
}
void postOrder(int cur) {  // 왼쪽 → 오른쪽 → 루트
    postOrder(cur * 2);
    postOrder(cur * 2 + 1);
    visit(cur);
}
```

## 주의사항 / 자주 하는 실수
- BFS: `vis[next] = true`는 **큐에 넣을 때** (poll 시점 X) → 중복 방지
- 완전 이진 트리 배열: 1-indexed면 `nodes[1]`이 루트, `nodes[0]` 사용 안 함
- Java Generics 배열 생성 불가: `Array.newInstance(type, size)` 사용

## 대표 문제

- [[SWEA_5643_키순서]] (SWEA 5643) — 정방향 DFS + 역방향 DFS로 도달 가능 노드 수 합산
  - 핵심: `adj` + `rAdj` 두 그래프 구성, `vis[startNode]` 미설정 주의 (DAG 한정 안전)
- [[BOJ_13460_구슬탈출2]] (BOJ 13460) — BFS + 4D 방문 배열 `vis[ry][rx][by][bx]`, 선행 구슬 우선 이동

## 관련 패턴
- [[Graph_AdjList]] — 인접 리스트로 표현한 그래프에서의 BFS
- [[Dijkstra]] — BFS의 가중치 버전
- [[Backtracking]] — DFS 기반 완전탐색
- [[플로이드워셜]] — 도달 가능성을 O(N³)에 전처리, DFS/BFS O(N×(V+E)) 대비 트레이드오프
