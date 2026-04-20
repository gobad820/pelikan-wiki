---
title: 투 포인터 (Two Pointer)
type: pattern
tags:
  - two-pointer
  - array
  - sliding-window
related:
  - "[[Backtracking]]"
created: 2026-04-06
updated: 2026-04-07
sources:
  - day0203/TwoPointerTest1_두수의합.java
  - day0203/TwoPointerTest2_구간합.java
---

# 투 포인터 (Two Pointer)

## 언제 쓰나
- 정렬된 배열에서 **두 원소의 합** = target 개수 찾기
- 합이 K인 **연속 구간** 개수 찾기 (슬라이딩 윈도우)
- O(N²) 완전탐색을 O(N)으로 줄이고 싶을 때

## 핵심 아이디어
**양끝 포인터**: 정렬 후 s=0, e=n-1에서 시작 → 합에 따라 포인터 이동
**슬라이딩 윈도우**: start/end 같은 방향 이동 → 구간 합 유지

## 시간복잡도
- O(N log N) - 정렬 포함  
- O(N) - 포인터 이동

## Java 코드 템플릿

### 패턴 1: 두 수의 합 (양끝 포인터)
```java
Arrays.sort(arr);
int s = 0, e = n - 1, ans = 0;
while (s < e) {
    int sum = arr[s] + arr[e];
    if (sum == target)      { ++ans; ++s; --e; }
    else if (sum < target)  { ++s; }
    else                    { --e; }
}
```

### 패턴 2: 구간 합 = K (슬라이딩 윈도우)
```java
int start = 0, end = 0, sum = 0, cnt = 0;
while (end < n) {
    if (sum >= k) {
        if (sum == k) ++cnt;
        sum -= arr[start++];
    } else {
        sum += arr[end++];
    }
}
```

## 주의사항 / 자주 하는 실수
- 양끝 포인터는 반드시 **정렬** 후 사용
- `s < e` 조건 (같을 때 종료 - 같은 원소 중복 사용 방지)
- 슬라이딩 윈도우: `sum >= k`일 때 먼저 처리 후 start 이동

## 관련 패턴
- [[Backtracking]] — 완전탐색 대비 최적화 방향
