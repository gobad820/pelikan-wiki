---
title: "SWEA D4 EditNumberSequence (수열 편집)"
type: problem
tags: [swea, d4, linked-list, node-pool, implementation]
related:
  - "[[노드풀]]"
created: 2026-04-15
updated: 2026-04-15
sources:
  - mogakal/swea/d4/EditNumberSequence.java
---

> [!info] 문제 정보
> - **플랫폼**: SWEA
> - **난이도**: D4
> - **유형**: 자료구조 구현 (링크드 리스트)

## 문제 요약

N개의 초기 수열에 M번 연산(I 삽입, D 삭제, C 변경)을 수행한 뒤, L번째 원소를 출력한다. 없으면 -1.

## 접근법

> [!tip] 핵심 접근
> 노드 풀(배열 기반 링크드 리스트)로 `new` 없이 삽입/삭제 구현. add(index)/delete(index)/get(index) 세 연산.

### 사고 흐름

1. 단순 배열: 삽입/삭제 O(N) 이동 → 가능하지만 링크드 리스트가 더 직관적
2. `new Node()` 반복 호출 → 노드 풀로 교체
3. `init()` 마다 `cnt = 0` 리셋 + 풀 전체 재초기화
4. `add(index)`: index==0이면 head 교체, 아니면 (index-1)번째 노드 탐색 후 삽입
5. `delete(index)`: index==0이면 head = head.next, 아니면 (index-1)번째 노드의 next 교체
6. `get(index)`: 0번 → head, listSize번 → tail 단축, 그 외 순회

### 핵심 코드

```java
static Node[] pool = new Node[10000];
static int cnt = 0;
static Node head, tail;
static int listSize = 0;

static Node getNewNode(int value) {
    Node cur = pool[cnt++];
    cur.value = value;
    cur.next = null;
    return cur;
}

static void add(int index, int value) {
    Node n = getNewNode(value);
    listSize++;
    if (head == null) { head = tail = n; return; }
    if (index == 0) { n.next = head; head = n; return; }
    Node curr = head;
    for (int i = 1; i < index; i++) curr = curr.next;
    n.next = curr.next;
    curr.next = n;
    if (n.next == null) tail = n; // tail 갱신
}
```

## 복잡도

| 항목 | 복잡도 | 근거 |
|------|--------|------|
| 시간 | O(N + M × L) | 추정: 각 연산 O(L) 순회, L은 리스트 길이 |
| 공간 | O(MAX_POOL) | 추정: 노드 풀 크기 |

## 배운 점 / 인사이트

> [!note] 기억할 것
> - **노드 풀 init 패턴**: `cnt = 0` + 풀 전체 `pool[i] = new Node()` — 매 TC마다 재생성
> - **tail 포인터 활용**: `get(listSize)` = tail로 O(1), 나머지는 O(N)
> - **경계값 처리**: `index < 0 || listSize < index` 체크 (add는 listSize까지 허용, delete는 listSize-1까지)
> - **연산 C (변경)**: `get(idx).value = newValue` — 노드 재삽입 없이 값만 교체
