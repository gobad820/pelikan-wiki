---
title: "KMP (Knuth-Morris-Pratt)"
type: pattern
tags: [string, pattern-matching, kmp, failure-function]
related:
  - "[[RabinKarp]]"
  - "[[BFS_DFS]]"
created: 2026-04-15
updated: 2026-04-15
sources:
  - live/day0415/KMP.md
---

> [!abstract] 핵심 요약
> 패턴을 전처리해 **pi 배열(실패 함수)**을 구성하고, 불일치 시 이미 일치한 접두사 길이만큼 패턴 포인터를 점프해 텍스트 포인터를 뒤로 돌리지 않는 문자열 검색 알고리즘. O(N+M) 보장.

## 언제 쓰나

- 텍스트에서 패턴 문자열의 모든 등장 위치를 찾아야 할 때
- 단순 이중 루프 O(NM) 대신 O(N+M)이 요구될 때
- 패턴이 반복 구조를 가질 때 (pi 배열이 효과적)
- 단일 패턴 검색 → KMP, 다중 패턴 → 아호-코라식으로 확장

## 핵심 아이디어

1. **pi 배열**: `pi[k]` = 패턴의 `[0..k]` 부분 문자열에서 **접두사 = 접미사**인 최대 길이
   - 예) 패턴 `"ABABC"` → pi = `[0, 0, 1, 2, 0]`
2. **검색 루프**: 텍스트 포인터 `i`는 절대 뒤로 가지 않음. 불일치 시 `j = pi[j-1]`로 패턴 포인터만 점프
3. **일치 완료**: `j == m`이면 `i - m + 1`이 시작 인덱스, `j = pi[j-1]`로 중첩 탐지

### pi 배열 시각화 (패턴: `"ABABC"`)

| k      | 0 | 1 | 2 | 3 | 4 |
|--------|---|---|---|---|---|
| 문자   | A | B | A | B | C |
| pi[k]  | 0 | 0 | 1 | 2 | 0 |

- `pi[3] = 2`: `"ABAB"`의 공통 접두사/접미사 → `"AB"` (길이 2)

## 복잡도

| 항목 | 복잡도 | 비고 |
|------|--------|------|
| 시간 | O(N+M) | N=텍스트 길이, M=패턴 길이. 추정 아님, 증명 가능 |
| 공간 | O(M) | pi 배열 크기 |

## Java 코드 템플릿

```java
// pi 배열 (실패 함수) 구성 — O(M)
static int[] buildPi(String pattern) {
    int m = pattern.length();
    int[] pi = new int[m];
    int j = 0;
    for (int i = 1; i < m; i++) {
        while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
            j = pi[j - 1]; // 불일치 → 이전 최장 접두사로 점프
        }
        if (pattern.charAt(i) == pattern.charAt(j)) {
            pi[i] = ++j;
        }
    }
    return pi;
}

// KMP 검색 — O(N)
static List<Integer> kmp(String text, String pattern) {
    int n = text.length(), m = pattern.length();
    int[] pi = buildPi(pattern);
    List<Integer> result = new ArrayList<>();
    int j = 0;
    for (int i = 0; i < n; i++) {
        while (j > 0 && text.charAt(i) != pattern.charAt(j)) {
            j = pi[j - 1]; // text[i]는 재사용 — i를 뒤로 돌리지 않음
        }
        if (text.charAt(i) == pattern.charAt(j)) {
            j++;
        }
        if (j == m) {
            result.add(i - m + 1); // 패턴 시작 인덱스
            j = pi[j - 1];         // 중첩 패턴 탐지를 위해 점프
        }
    }
    return result;
}
```

## 대표 문제

- BOJ 1786 찾기 (KMP 기본형) — pi 배열 + 모든 등장 위치 출력
- BOJ 1305 광고 — pi 배열만으로 주기 계산 (`m - pi[m-1]`)

## 주의사항 / 자주 하는 실수

> [!warning] 실수 포인트
> - **buildPi 루프 시작**: `i=1`부터 시작. `i=0`이면 자기 자신과 비교 → `pi[0] = 0` 고정
> - **일치 후 j 처리**: `j == m` 후 `j = pi[j-1]` 필수. `j = 0`으로 리셋하면 중첩 패턴 누락
> - **i 역행 없음**: 검색 루프에서 `i`를 감소시키면 O(N+M) 보장 깨짐
> - **pi 배열 = KMP 자기 자신**: buildPi 내부는 패턴을 텍스트로, 패턴을 패턴으로 KMP 실행하는 구조

## 관련 패턴

- [[RabinKarp]] — 해시 기반 대안. 다중 패턴에 유리하나 해시 충돌 위험
- [[BFS_DFS]] — 아호-코라식(다중 패턴 KMP)은 트라이 + BFS로 실패 링크 구성
