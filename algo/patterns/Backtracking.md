---
title: "백트래킹 - 순열 / 조합 / 부분집합"
type: pattern
tags: [backtracking, permutation, combination, subset, bitmask, pruning]
related:
  - "[[BFS_DFS]]"
  - "[[DP_Memoization]]"
created: 2026-04-06
updated: 2026-04-14
sources:
  - live/day0205/PermuatationInputTest.java
  - live/day0206/SubsetTest.java
  - live/day0209/NextPermutationInputTest.java
  - live/day0210/NQueenTest.java
  - live/day0414/Solution_2112_보호필름_김상해_부분집합.java
---

# 백트래킹 - 순열 / 조합 / 부분집합

## 언제 쓰나
- **순열(Permutation)**: 순서 있는 선택 nPr — 중복 없이 r개 배치
- **조합(Combination)**: 순서 없는 선택 nCr — start 포인터로 앞 원소 재사용 방지
- **부분집합(Subset)**: 각 원소를 포함/미포함 — 2^n 가지
- **가지치기(Pruning)**: 불필요한 탐색 제거 (N-Queen, 합 조건 등)

## 핵심 아이디어
재귀 트리를 그리되, **조건 불만족 시 즉시 return** (백트래킹).
- 순열: `isSelected[]` 또는 비트마스크 `flag`로 중복 방지
- 조합: 재귀 시 `start`를 현재 인덱스+1로 넘겨 순서 고정
- 부분집합: 포함/미포함 두 가지 분기
- 다음 순열: 배열 in-place 조작으로 O(N) 전체 순열 순회

## 시간복잡도
- 순열: O(N!)
- 조합: O(N! / (N-R)! / R!)
- 부분집합: O(2^N)
- 다음 순열 순회: O(N! × N)

## Java 코드 템플릿

### 패턴 1: 순열 (boolean 배열)
```java
static int n, r, numbers[], input[];
static boolean[] isSelected;

void permutation(int depth) {
    if (depth == r) {
        // numbers[] 처리
        return;
    }
    for (int i = 0; i < n; i++) {
        if (isSelected[i]) continue;
        isSelected[i] = true;
        numbers[depth] = input[i];
        permutation(depth + 1);
        isSelected[i] = false;
    }
}
```

### 패턴 2: 순열 (비트마스크)
```java
void permutation(int depth, int flag) {
    if (depth == r) {
        // numbers[] 처리
        return;
    }
    for (int i = 0; i < n; i++) {
        if ((flag & (1 << i)) != 0) continue; // i번 비트가 켜져 있으면 이미 선택됨
        numbers[depth] = input[i];
        permutation(depth + 1, flag | (1 << i));
    }
}
// 호출: permutation(0, 0);
```

### 패턴 3: 조합
```java
void combination(int depth, int start) {
    if (depth == r) {
        // numbers[] 처리
        return;
    }
    for (int i = start; i < n; i++) {
        numbers[depth] = input[i];
        combination(depth + 1, i + 1); // i+1: 현재 이후 원소만
    }
}
// 호출: combination(0, 0);
```

### 패턴 4: 부분집합
```java
static boolean[] isSelected;

void subset(int cnt) {
    if (cnt == n) {
        // isSelected[] 상태로 부분집합 처리
        return;
    }
    isSelected[cnt] = true;
    subset(cnt + 1);
    isSelected[cnt] = false;
    subset(cnt + 1);
}
// 호출: subset(0);
```

### 패턴 5: 다음 순열 (Next Permutation)
```java
boolean nextPermutation() {
    int i = arr.length - 1;
    while (i > 0 && arr[i - 1] >= arr[i]) i--;
    if (i == 0) return false; // 마지막 순열

    int j = arr.length - 1;
    while (arr[i - 1] >= arr[j]) j--;
    swap(i - 1, j);

    int k = arr.length - 1;
    while (i < k) swap(i++, k--); // i 이후 역순 정렬

    return true;
}

// 사용법: Arrays.sort(arr) → do { ... } while (nextPermutation());
```

### 패턴 6: N-Queen (대각선 가지치기)
```java
static boolean[] col, slash, bslash;
// col[c]: c열 사용 여부
// slash[c + row]: 우상향 대각선 (c+row 값이 같으면 같은 대각선)
// bslash[c - row + n]: 우하향 대각선

void setQueen(int row) {
    if (row == n + 1) { totalCnt++; return; }
    for (int c = 1; c <= n; c++) {
        if (col[c] || slash[c + row] || bslash[c - row + n]) continue;
        col[c] = slash[c + row] = bslash[c - row + n] = true;
        setQueen(row + 1);
        col[c] = slash[c + row] = bslash[c - row + n] = false;
    }
}
```

## 주의사항 / 자주 하는 실수
- 순열 boolean 배열: **재귀 후 반드시 `isSelected[i] = false` 복원** (백트래킹의 핵심)
- 비트마스크 순열: `n <= 30` 범위 내에서 사용 (int 32비트 한계)
- 조합: `combination(depth+1, i+1)` — `i+1`이 아닌 `start+1`로 쓰면 틀림
- 다음 순열: **오름차순 정렬 후** 시작해야 전체 순열 순회 가능
- N-Queen 대각선: `slash[c+row]`, `bslash[c-row+n]` 인덱스 범위 확인 (배열 크기 2n+1, 2n)

## 대표 문제 (최적화 백트래킹)

- [[SWEA_2112_보호필름]] (SWEA 2112) — 행 단위 스킵/A/B 3분기, 최소 주입 횟수
  - 핵심: `cnt >= answer` 가지치기, 스킵 먼저 탐색해 answer 빠르게 갱신
  - `board[idx].clone()` 으로 값 복사 후 `board[idx] = backup` 복원

## 관련 패턴
- [[BFS_DFS]] — 그래프 탐색 기반 백트래킹
- [[DP_Memoization]] — 중복 부분 문제가 많을 때 백트래킹 → DP로 최적화
