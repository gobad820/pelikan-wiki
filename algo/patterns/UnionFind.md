---
title: "유니온 파인드 (Union-Find / Disjoint Set)"
type: pattern
tags: 
  - union-find
  - disjoint-set
  - graph
  - mst
related:
  - "[[MST_Kruskal]]"
  - "[[Graph_AdjList]]"
created: 2026-04-06
updated: 2026-04-15
sources:
  - live/day0309/DisjointSetTest.java
  - live/day0310/DisjointSetTest2.java
  - mogakal/baekjoon/gold/_4195.java
---
# 유니온 파인드 (Union-Find / Disjoint Set)

## 언제 쓰나
- **두 원소가 같은 집합인지** 빠르게 확인
- 집합 합치기 (union)
- 그래프 사이클 감지, Kruskal MST

## 핵심 아이디어
- `findSet(a)`: a의 루트(대표자) 반환. **경로 압축**으로 O(α(N)) ≈ O(1)
- `union(a, b)`: 두 집합 합치기. **랭크 기반** 합치기로 트리 높이 최소화

## 시간복잡도
- `findSet`: O(α(N)) — 사실상 O(1)
- `union`: O(α(N))

## Java 코드 템플릿

### 패턴 1: 랭크 기반 (rank[] 배열)
```java
static int[] parents, ranks;

static void makeSets(int N) {
    parents = new int[N + 1];
    ranks = new int[N + 1];
    for (int i = 0; i <= N; i++) parents[i] = i;
}

static int findSet(int a) {
    if (a == parents[a]) return a;
    return parents[a] = findSet(parents[a]); // 경로 압축
}

static boolean union(int a, int b) {
    int rootA = findSet(a), rootB = findSet(b);
    if (rootA == rootB) return false; // 이미 같은 집합
    if (ranks[rootA] < ranks[rootB])      parents[rootA] = rootB;
    else if (ranks[rootA] > ranks[rootB]) parents[rootB] = rootA;
    else { parents[rootB] = rootA; ranks[rootA]++; }
    return true;
}
```

### 패턴 2: 크기 기반 (parents[]에 음수로 크기 저장)
```java
static int[] parents;

static void makeSets(int N) {
    parents = new int[N];
    Arrays.fill(parents, -1); // -1 = 크기 1인 독립 집합 (루트)
}

static int findSet(int a) {
    if (parents[a] < 0) return a; // 음수면 루트
    return parents[a] = findSet(parents[a]);
}

static boolean union(int a, int b) {
    int pA = findSet(a), pB = findSet(b);
    if (pA == pB) return false;
    // 크기가 큰 쪽에 작은 쪽을 붙임 (parents[] 값이 더 음수인 쪽이 더 큰 집합)
    if (parents[pA] <= parents[pB]) { parents[pA] += parents[pB]; parents[pB] = pA; }
    else                            { parents[pB] += parents[pA]; parents[pA] = pB; }
    return true;
}
// 집합 크기: -parents[findSet(x)]
```

## 주의사항 / 자주 하는 실수
- `union()` 반환값: **true = 다른 집합 합침, false = 이미 같은 집합** (사이클 감지에 활용)
- 집합 수 = N - (union 성공 횟수)
- 경로 압축 없으면 O(N) → 반드시 `parents[a] = findSet(parents[a])` 형태로
- 크기 기반 패턴에서 `parents[root] < 0` — 음수값의 절댓값이 집합 크기

## 대표 문제

- [[BOJ_4195_친구네트워크]] (BOJ 4195) — HashMap으로 String→int 매핑 후 Union-Find, 음수 크기 패턴으로 실시간 집합 크기 반환

## 관련 패턴
- [[MST_Kruskal]] — union의 반환값으로 사이클 감지하여 MST 구성
- [[Graph_AdjList]] — 그래프 연결성 확인에 활용
