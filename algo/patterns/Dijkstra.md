---
title: "다익스트라 (Dijkstra) - 단일 출발지 최단 경로"
type: pattern
tags: 
  - dijkstra
  - shortest-path
  - graph
  - priority-queue
  - greedy
related:
  - "[[BFS_DFS]]"
  - "[[MST_Prim]]"
  - "[[Graph_AdjList]]"
created: 2026-04-06
updated: 2026-04-07
sources:
  - day0312/Dijkstra_AdjList.java
  - day0313/Dijkstra_AdjList_PQ.java
  - day0313/Dijkstra_AdjList_PQ2.java
---

# 다익스트라 (Dijkstra) - 단일 출발지 최단 경로

## 언제 쓰나
- **단일 출발지 → 모든 정점** 최단 경로 (비음수 가중치 필수)
- BFS의 가중치 버전
- 음수 가중치 → Bellman-Ford 사용

## 핵심 아이디어
1. 출발지 `minDist[start] = 0`, 나머지 INF
2. 미방문 중 `minDist` 최솟값 정점 선택 (PQ로 O(log V))
3. 해당 정점 경유 경로로 이웃 `minDist` 갱신 (더 짧으면 업데이트)
4. 모든 정점 처리될 때까지 반복

## 시간복잡도
- 단순 배열 (visited 스캔): O(V²)
- PQ + 인접 리스트: O(E log V)

## Java 코드 템플릿

### 패턴 1: PQ + visited 배열 (권장)
```java
static class Node { int to, weight; Node next; ... }
static class Vertex implements Comparable<Vertex> {
    int no, dist;
    @Override public int compareTo(Vertex o) { return Integer.compare(this.dist, o.dist); }
}

Arrays.fill(minDist, Integer.MAX_VALUE);
minDist[start] = 0;
PriorityQueue<Vertex> pq = new PriorityQueue<>();
pq.offer(new Vertex(start, 0));

while (!pq.isEmpty()) {
    Vertex cur = pq.poll();
    if (visited[cur.no]) continue; // 이미 처리된 정점 스킵
    visited[cur.no] = true;

    for (Node temp = adjList[cur.no]; temp != null; temp = temp.next) {
        if (!visited[temp.to] && minDist[temp.to] > cur.dist + temp.weight) {
            minDist[temp.to] = cur.dist + temp.weight;
            pq.offer(new Vertex(temp.to, minDist[temp.to]));
        }
    }
}
```

### 패턴 2: PQ + dist 비교 (visited 없이)
```java
while (!pq.isEmpty()) {
    Vertex cur = pq.poll();
    // cur.dist > minDist[cur.no] 이면 이미 더 짧은 경로로 처리됨 → 스킵
    if (cur.dist > minDist[cur.no]) continue;

    for (Node temp = adjList[cur.no]; temp != null; temp = temp.next) {
        if (minDist[temp.to] > cur.dist + temp.weight) {
            minDist[temp.to] = cur.dist + temp.weight;
            pq.offer(new Vertex(temp.to, minDist[temp.to]));
        }
    }
}
```

## 두 PQ 패턴 비교

| 구분 | visited[] 사용 | cur.dist > minDist 비교 |
|------|---------------|------------------------|
| 중복 PQ 항목 처리 | 방문 여부로 스킵 | dist 비교로 스킵 |
| 코드 단순성 | visited 배열 추가 필요 | 배열 없이 가능 |
| 성능 | 거의 동일 | 거의 동일 |

## 주의사항 / 자주 하는 실수
- `minDist` 초기화: **반드시 INF로** (`Arrays.fill(minDist, Integer.MAX_VALUE)`)
- PQ 중복 항목: 같은 정점이 여러 번 PQ에 들어갈 수 있음 → 반드시 스킵 처리
- `cur.dist + temp.weight` 오버플로 주의: MAX_VALUE + 양수 = 음수 → long 사용 또는 INF 조건 체크
- 결과 출력: `minDist[end] != Integer.MAX_VALUE ? minDist[end] : -1`
- 음수 가중치 → 다익스트라 사용 불가 (음수 사이클 문제)

## 관련 패턴
- [[BFS_DFS]] — 가중치 없는 최단 경로는 BFS
- [[MST_Prim]] — 구조 유사하지만 목적 다름 (MST vs 최단 경로)
- [[Graph_AdjList]] — 인접 리스트 표현
