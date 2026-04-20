---
title: "최소 신장 트리 - 크루스칼 (MST Kruskal)"
type: pattern
tags: 
  - mst
  - kruskal
  - union-find
  - greedy
  - graph
related:
  - "[[UnionFind]]"
  - "[[MST_Prim]]"
  - "[[Graph_AdjList]]"
created: 2026-04-06
updated: 2026-04-07
sources:
  - day0310/MST_Kruskal.java
---
# 최소 신장 트리 - 크루스칼 (MST Kruskal)

## 언제 쓰나
- **간선 중심** MST. 간선 수 E가 적을 때 유리 (희소 그래프)
- Union-Find와 함께 사용
- V-1개 간선 선택 완료되면 종료

## 핵심 아이디어
1. 모든 간선을 **가중치 오름차순** 정렬
2. 간선을 순서대로 보면서 **사이클 생성 안 하면** (= `union()` 성공) 선택
3. V-1개 선택되면 완료

## 시간복잡도
- O(E log E) — 정렬이 지배

## Java 코드 템플릿
```java
static class Edge implements Comparable<Edge> {
    int start, end, weight;
    Edge(int s, int e, int w) { start = s; end = e; weight = w; }
    @Override
    public int compareTo(Edge o) { return Integer.compare(this.weight, o.weight); }
}

// 전처리
Arrays.sort(edgeList); // 가중치 오름차순 정렬
makeSets();            // Union-Find 초기화

int cnt = 0, result = 0;
for (Edge edge : edgeList) {
    if (union(edge.start, edge.end)) { // 사이클 없으면 선택
        result += edge.weight;
        if (++cnt == V - 1) break;    // V-1개 선택 완료
    }
}
// result = MST 총 비용
```

## 주의사항 / 자주 하는 실수
- `cnt == V - 1` 조건으로 조기 종료 → 불필요한 간선 처리 방지
- `union()` false = 사이클 발생 → 그 간선은 건너뜀
- 정렬은 `Arrays.sort()` + `Comparable` 구현으로 처리
- 그래프가 비연결이면 `cnt < V-1` 상태로 루프 종료 → result 무효

## 관련 패턴
- [[UnionFind]] — 사이클 감지를 위한 필수 보조 구조
- [[MST_Prim]] — 정점 중심 MST (밀집 그래프에서 유리)
- [[Graph_AdjList]] — 그래프 표현
