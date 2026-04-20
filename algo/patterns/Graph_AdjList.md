---
title: "그래프 - 인접 리스트 표현"
type: pattern
tags: 
  - graph
  - adjacency-list
  - linked-list
  - bfs
  - dfs
related:
  - "[[BFS_DFS]]"
  - "[[UnionFind]]"
  - "[[MST_Kruskal]]"
  - "[[MST_Prim]]"
  - "[[Dijkstra]]"
created: 2026-04-06
updated: 2026-04-07
sources:
  - day0213/AdjListTest.java
  - day0219/AdjListTest.java
---

# 그래프 - 인접 리스트 표현

## 언제 쓰나
- **희소 그래프** (간선 수 E << V²): 인접 리스트가 인접 행렬보다 메모리 효율적
- BFS/DFS/Dijkstra/MST 등 대부분의 그래프 알고리즘의 기반 표현

## 핵심 아이디어
각 정점이 연결된 **이웃 리스트**만 저장.
- **연결 리스트(Node 체인)**: 단방향 삽입 O(1), 읽기 O(degree)
- **ArrayList[]**: 사용하기 편함, Java Generics 배열 경고 발생

## 시간복잡도
- 구축: O(E)
- 순회: O(V + E)

## Java 코드 템플릿

### 패턴 1: 연결 리스트 방식 (헤드 삽입)
```java
static class Node {
    int vertex;
    Node next;
    Node(int vertex, Node next) {
        this.vertex = vertex;
        this.next = next;
    }
}

Node[] adjList = new Node[V]; // null 초기화

// 간선 추가 (양방향)
adjList[from] = new Node(to,   adjList[from]); // 헤드에 삽입 → O(1)
adjList[to]   = new Node(from, adjList[to]);

// 순회
for (Node cur = adjList[v]; cur != null; cur = cur.next) {
    int next = cur.vertex;
    // 처리
}
```

### 패턴 2: ArrayList 방식 (더 직관적)
```java
List<Integer>[] adjList = new List[V + 1]; // 1-indexed
for (int i = 0; i <= V; i++) adjList[i] = new ArrayList<>();

adjList[from].add(to);
adjList[to].add(from);

// BFS와 함께 사용
for (int next : adjList[cur]) {
    if (!vis[next]) { vis[next] = true; q.offerLast(next); }
}
```

### 패턴 3: 가중치 포함 (MST/Dijkstra용)
```java
static class Node {
    int vertex, weight;
    Node next;
    Node(int v, int w, Node next) { vertex = v; weight = w; this.next = next; }
}

adjList[from] = new Node(to, weight, adjList[from]);
adjList[to]   = new Node(from, weight, adjList[to]);
```

## 주의사항 / 자주 하는 실수
- `new List[V+1]` — Java 제네릭 배열 경고(@SuppressWarnings 또는 `Array.newInstance`)
- 1-indexed vs 0-indexed 혼용 주의: 배열 크기를 `V+1`로 일관성 유지
- 헤드 삽입 방식은 **입력 역순**으로 저장됨 — 순서가 중요한 문제는 주의
- 양방향 그래프는 반드시 `from→to`, `to→from` 둘 다 추가

## 관련 패턴
- [[BFS_DFS]] — 인접 리스트를 순회하는 BFS/DFS 구현
- [[MST_Kruskal]] — 간선 기반 MST
- [[MST_Prim]] — 정점 기반 MST (인접 리스트 버전)
- [[Dijkstra]] — 최단 경로 (인접 리스트 + PQ)
- [[UnionFind]] — 연결성 확인 (Kruskal에서 함께 사용)
