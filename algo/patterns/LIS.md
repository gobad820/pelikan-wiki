---
title: "LIS (최장 증가 부분 수열)"
type: pattern
tags:
  - dp
  - lis
  - binary-search
  - tabulation
related:
  - "[[DP_Tabulation]]"
  - "[[20260408]]"
created: 2026-04-08
updated: 2026-04-08
sources:
  - online/day0408/LISTest.java
  - online/day0408/docs/LIS.md
---
# LIS (최장 증가 부분 수열)

> Longest Increasing Subsequence — 주어진 수열에서 순서를 유지하면서 값이 순증가하는 부분 수열 중 길이가 최대인 것.

## 언제 쓰나

- 수열에서 **값이 단조증가**하는 부분 수열의 **최대 길이**를 구할 때
- 연속하지 않아도 되지만 **원래 순서는 유지**해야 할 때
- 대표 유형: "몇 개까지 쌓을 수 있나", "몇 명을 줄 세울 수 있나", 박스 쌓기, 러시아 인형

## 핵심 아이디어

`LIS[i]` = **arr[i]를 마지막 원소로 포함하는** LIS의 길이.

i보다 앞에 있고(`j < i`) 값이 작은(`arr[j] < arr[i]`) 모든 j에 대해
`LIS[j] + 1`의 최댓값을 `LIS[i]`에 저장한다.

$$
LIS[i] = 1 + \max_{j < i,\ arr[j] < arr[i]} LIS[j]
$$

기저값: 모든 원소는 자기 자신만으로도 길이 1의 LIS를 구성 → `LIS[i] = 1`

## 시간복잡도

| 방법 | 시간 | 공간 |
|------|------|------|
| DP O(N²) | O(N²) | O(N) |
| DP + 이진탐색 | O(N log N) | O(N) |

## Java 코드 템플릿

### 방법 1: DP O(N²) — 기본형

```java
int[] arr = ...; // 입력 수열 (0-indexed, 크기 N)
int[] lis = new int[N];
int maxLen = 0;

for (int i = 0; i < N; i++) {
    lis[i] = 1; // 기저값: 자기 자신만 포함
    for (int j = 0; j < i; j++) {
        if (arr[j] < arr[i] && lis[i] < lis[j] + 1) {
            lis[i] = lis[j] + 1;
        }
    }
    maxLen = Math.max(maxLen, lis[i]);
}
System.out.println(maxLen);
```

> `LIS[i] < LIS[j] + 1` 조건으로 불필요한 대입을 줄일 수 있다 (max() 대신).

### 방법 2: DP + 이진탐색 O(N log N) — 고속형

`tails[k]` = 길이 k+1인 LIS들의 마지막 원소 중 **최솟값** (이 배열은 항상 정렬 상태 유지).

원소 x를 처리할 때:
- `tails`에서 x 이상인 첫 번째 위치를 이진탐색 → 해당 위치에 x를 덮어씀
- 위치가 없으면(x가 tails 전체보다 큼) 뒤에 추가 → LIS 길이 +1

```java
int[] arr = ...; // 입력 수열 (크기 N)
int[] tails = new int[N];
int size = 0; // 현재 tails 배열의 유효 길이 = LIS 최대 길이

for (int i = 0; i < N; i++) {
    int x = arr[i];
    // tails[0..size-1]에서 x 이상인 첫 위치 이진탐색 (lower_bound)
    int lo = 0, hi = size;
    while (lo < hi) {
        int mid = (lo + hi) / 2;
        if (tails[mid] < x) lo = mid + 1;
        else hi = mid;
    }
    tails[lo] = x;
    if (lo == size) size++; // 새로운 최대 길이
}
System.out.println(size);
```

> **주의**: `tails` 배열 자체가 실제 LIS 수열은 아니다. 길이만 올바르게 나온다.
> 실제 LIS 수열 복원이 필요하면 별도 `parent[]` 배열로 역추적해야 한다.

## 대표 문제

- 백준 11053 - 가장 긴 증가하는 부분 수열 (O(N²) 기본)
- 백준 12015 - 가장 긴 증가하는 부분 수열 2 (O(N log N) 이진탐색)
- 백준 14002 - 가장 긴 증가하는 부분 수열 4 (LIS 수열 실제 복원)

## 주의사항 / 자주 하는 실수

| 실수 | 원인 | 해결 |
|------|------|------|
| `LIS[i]` 초기화를 0으로 설정 | 최소 1(자기 자신)이어야 함 | 초기값 `LIS[i] = 1` |
| j 루프에서 `arr[j] <= arr[i]` | 같은 값도 포함 → 비감소(non-decreasing) 수열이 됨 | 순증가는 `<`, 비감소는 `<=` |
| 이진탐색에서 `tails` 전체 크기(N)를 사용 | 유효 범위 초과 참조 | `hi = size` (유효 길이만) |
| `tails` 배열을 실제 LIS로 오해 | tails는 길이 계산용 보조 배열 | 복원 필요 시 parent 배열 별도 구성 |
| O(N log N)의 전제를 "입력 정렬 필요"로 오해 | tails가 내부적으로 정렬된 것 | 입력 정렬 불필요, tails만 항상 정렬 상태 |

## 관련 패턴

- [[DP_Tabulation]] — LIS O(N²)는 Bottom-Up Tabulation의 전형적 예시
- **LDS (최장 감소 부분 수열)** — 배열을 뒤집거나 부등호를 반전시켜 LIS로 변환
- **LNDS (최장 비감소 부분 수열)** — 부등호를 `<=`로 변경
- **박스 쌓기 / 러시아 인형** — 2D LIS로 확장 (한 차원 정렬 후 나머지 차원 LIS)
