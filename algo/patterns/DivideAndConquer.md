---
title: "분할 정복 (Divide & Conquer)"
type: pattern
tags: 
  - divide-and-conquer
  - recursion
  - quadtree
related:
  - "[[BFS_DFS]]"
  - "[[DP_Memoization]]"
created: 2026-04-06
updated: 2026-04-07
sources:
  - day0212/MakeSpaceTest.java
---
# 분할 정복 (Divide & Conquer)

## 언제 쓰나
- 문제를 **균등한 부분으로 분할** 후 각각 해결하여 합치는 구조
- 쿼드트리, 병합 정렬, 거듭제곱 계산
- "전체가 균일하면 즉시 처리, 아니면 4등분(또는 2등분)" 형태

## 핵심 아이디어
1. **기저 조건**: 구역이 단색(전부 0 또는 전부 1)이면 바로 처리
2. **분할**: 4개 (또는 2개) 부분으로 나눔
3. **정복**: 각 부분에 재귀 적용
4. **결합**: 결과 합산

## 시간복잡도
- 쿼드트리: 최선 O(1), 최악 O(N² log N)
- 병합 정렬: O(N log N)

## Java 코드 템플릿

### 패턴: 쿼드트리 (2D 배열 분할)
```java
static int[][] spaces;
static int white, green;

void cut(int r, int c, int size) {
    int sum = 0;
    for (int i = r; i < r + size; i++)
        for (int j = c; j < c + size; j++)
            sum += spaces[i][j];

    if (sum == size * size) { ++green; return; } // 전부 1 (초록)
    if (sum == 0)           { ++white; return; } // 전부 0 (흰색)

    // 균일하지 않으면 4등분
    int half = size / 2;
    cut(r,        c,        half);
    cut(r,        c + half, half);
    cut(r + half, c,        half);
    cut(r + half, c + half, half);
}
// 호출: cut(0, 0, N);  // N은 2의 거듭제곱이어야 함
```

## 주의사항 / 자주 하는 실수
- 입력 크기 N이 **2의 거듭제곱**이어야 균등 분할 가능 — 문제 조건 확인
- `sum == size * size`로 전체 1인지 확인 (합이 넓이와 같으면 전부 1)
- 범위 계산: `r + size`, `c + size` — `r + half`와 혼동 주의
- 재귀 깊이: N=1024 → 최대 10레벨 → 스택 오버플로 걱정 없음

## 관련 패턴
- [[BFS_DFS]] — 트리 탐색 방식과 유사
- [[DP_Memoization]] — 분할 결과에 중복이 있으면 메모이제이션 추가 가능
