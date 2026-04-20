---
title: "CT 트로미노 (Tromino)"
type: problem
tags: [simulation, brute-force, geometry, array, codetree]
related:
  - "[[도형완전탐색]]"
created: 2026-04-20
updated: 2026-04-20
sources:
  - chat/20260420/트로미노_Main.java
---

> [!info] 문제 정보
> - **플랫폼**: Code Tree
> - **유형**: 시뮬레이션, 완전 탐색
> - **핵심**: N×M 보드에서 트로미노(3칸 도형) 6가지 방향 중 합이 최대인 배치를 구하라

## 문제 요약

N×M 정수 보드에서 트로미노(L자형 4방향 + 직선형 2방향)를 배치했을 때 3칸의 합이 최대가 되는 값을 출력한다.

## 접근법

> [!tip] 핵심 접근
> 도형 6종류를 기준 셀 (y,x) 기준 상대 오프셋으로 정의해 완전 탐색. 경계 조건은 삼항 연산자로 일괄 처리.

### 도형 정의

```
L자형 4방향 (기준셀=중심 하단):
  ┘ : (y,x), (y,x+1), (y-1,x)     — 오른쪽 + 위
  └ : (y,x), (y,x-1), (y-1,x)     — 왼쪽 + 위
  ┐ : (y,x), (y,x+1), (y+1,x)     — 오른쪽 + 아래
  ┌ : (y,x), (y,x-1), (y+1,x)     — 왼쪽 + 아래

직선형 2방향:
  ─ : (y,x-1), (y,x), (y,x+1)     — 가로 3칸
  │ : (y-1,x), (y,x), (y+1,x)     — 세로 3칸
```

### 핵심 코드

```java
// L자형 4방향
for (int y = 0; y < N; y++) {
    for (int x = 0; x < M; x++) {
        int s0 = (x+1 >= M || y-1 < 0) ? 0 : board[y][x] + board[y][x+1] + board[y-1][x];
        int s1 = (x-1 < 0  || y-1 < 0) ? 0 : board[y][x] + board[y][x-1] + board[y-1][x];
        int s2 = (x+1 >= M || y+1 >= N) ? 0 : board[y][x] + board[y][x+1] + board[y+1][x];
        int s3 = (x-1 < 0  || y+1 >= N) ? 0 : board[y][x] + board[y][x-1] + board[y+1][x];
        answer = Math.max(answer, Math.max(s0, Math.max(s1, Math.max(s2, s3))));
    }
}
// 직선형 2방향
for (int y = 0; y < N; y++) {
    for (int x = 0; x < M; x++) {
        int h = (x-1 < 0 || x+1 >= M) ? 0 : board[y][x-1] + board[y][x] + board[y][x+1];
        int v = (y-1 < 0 || y+1 >= N) ? 0 : board[y-1][x] + board[y][x] + board[y+1][x];
        answer = Math.max(answer, Math.max(h, v));
    }
}
```

## 복잡도

| 항목 | 복잡도 | 근거 |
|------|--------|------|
| 시간 | O(N·M) | 모든 셀 × 고정 상수(6가지 도형) |
| 공간 | O(N·M) | 보드 배열 |

## 주의사항

> [!warning] 실수 포인트
> - `board = new int[N][N]`으로 선언 시 N≠M 케이스에서 AIOOBE 발생 → `new int[N][M]` 필수
> - 경계 조건에서 열 한계는 `>= M`, 행 한계는 `>= N`을 각각 사용
> - 삼항 연산자로 경계 외 케이스를 0으로 처리 시, 값이 음수인 경우 잘못된 최댓값이 될 수 있음 (이 문제는 양수 입력 가정)

## 배운 점 / 인사이트

> [!note] 기억할 것
> - 도형 문제는 **종이에 모든 방향을 그린 뒤** 빠짐없이 열거하는 것이 핵심
> - 경계 조건은 삼항 연산자로 한 줄에 처리하면 코드가 간결해짐
> - 행 크기(N)와 열 크기(M)가 다를 때 배열 선언과 경계 조건을 각각 주의
