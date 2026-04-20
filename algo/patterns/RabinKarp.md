---
title: "Rabin-Karp (라빈-카프)"
type: pattern
tags: [string, pattern-matching, hashing, rolling-hash]
related:
  - "[[KMP]]"
created: 2026-04-15
updated: 2026-04-15
sources:
  - live/day0415/Rabin-Karp.md
---

> [!abstract] 핵심 요약
> 패턴과 텍스트 슬라이딩 윈도우의 **해시값을 비교**해 후보를 줄이고, 해시 일치 시에만 실제 문자열 비교를 수행하는 패턴 매칭 알고리즘. 롤링 해시로 윈도우 이동을 O(1)에 처리.

## 언제 쓰나

- **다중 패턴 검색**: 패턴 해시를 HashSet에 넣어 한 번의 슬라이딩으로 N개 패턴 동시 탐색
- 해시값 비교로 대부분의 불일치를 O(1)에 걸러낼 수 있을 때
- 구현 단순성이 중요할 때 (KMP pi 배열 없이도 동작)
- 텍스트 길이 N이 매우 크고 패턴 수도 많을 때

## 핵심 아이디어

### 롤링 해시 (Rolling Hash)

슬라이딩 윈도우 이동 시 이전 해시값에서 O(1)에 새 해시 계산:

```
hash(s[i..i+m-1]) = s[i]×BASE^(m-1) + s[i+1]×BASE^(m-2) + ... + s[i+m-1]

이동 공식:
hash(s[i+1..i+m]) = (hash(s[i..i+m-1]) - s[i]×BASE^(m-1)) × BASE + s[i+m]
```

- `BASE^(m-1)` — 맨 앞 문자 기여값 제거용. 시작 전 한 번만 계산
- 음수 방지: `(값 + MOD) % MOD`

## 복잡도

| 항목 | 복잡도 | 비고 |
|------|--------|------|
| 시간 | O(N+M) 평균 | 해시 충돌 없으면 보장 |
| 시간 | O(NM) 최악 | 모든 윈도우에서 충돌 발생 시 |
| 공간 | O(1) | pi 배열 불필요 |

## Java 코드 템플릿

```java
static final long BASE = 31L;
static final long MOD  = 1_000_000_007L;

static List<Integer> rabinKarp(String text, String pattern) {
    int n = text.length(), m = pattern.length();
    List<Integer> result = new ArrayList<>();
    if (n < m) return result;

    // BASE^(m-1) mod MOD — 맨 앞 문자 제거용
    long power = 1;
    for (int i = 0; i < m - 1; i++) power = power * BASE % MOD;

    // 초기 해시 (패턴 + 첫 윈도우)
    long patHash = 0, winHash = 0;
    for (int i = 0; i < m; i++) {
        patHash = (patHash * BASE + pattern.charAt(i)) % MOD;
        winHash = (winHash * BASE + text.charAt(i))   % MOD;
    }

    for (int i = 0; i <= n - m; i++) {
        if (winHash == patHash) {
            // 해시 일치 → 실제 비교 (충돌 방지 필수)
            if (text.regionMatches(i, pattern, 0, m)) {
                result.add(i);
            }
        }
        // 롤링 해시: 다음 윈도우 O(1)
        if (i < n - m) {
            winHash = (winHash - text.charAt(i) * power % MOD + MOD) % MOD;
            winHash = (winHash * BASE + text.charAt(i + m)) % MOD;
        }
    }
    return result;
}
```

### 다중 패턴 확장

```java
// 패턴 해시 셋으로 다중 패턴 O(N+ΣM) 평균
static Set<Long> buildPatternHashSet(List<String> patterns, int m) {
    Set<Long> hashSet = new HashSet<>();
    for (String p : patterns) {
        long h = 0;
        for (char c : p.toCharArray()) h = (h * BASE + c) % MOD;
        hashSet.add(h);
    }
    return hashSet;
}
```

## KMP vs Rabin-Karp 비교

| 항목 | KMP | Rabin-Karp |
|------|-----|------------|
| 시간 복잡도 | O(N+M) **항상** | O(N+M) 평균, O(NM) 최악 |
| 다중 패턴 | 불리 | **유리** (HashSet 확장) |
| 구현 난이도 | pi 배열 이해 필요 | 해시 공식 이해 필요 |
| 해시 충돌 | 없음 | 이중 해시로 완화 가능 |

## 대표 문제

- BOJ 1786 찾기 — KMP/Rabin-Karp 모두 적용 가능한 기본형
- 다중 패턴 검색 유형 — 패턴 HashSet + 롤링 해시

## 주의사항 / 자주 하는 실수

> [!warning] 실수 포인트
> - **음수 방지**: `winHash - text.charAt(i) * power`는 음수 가능 → `+ MOD) % MOD` 필수
> - **실제 비교 생략 금지**: 해시 일치만으로 매칭 확정하면 충돌로 오답. `regionMatches()` 또는 `substring().equals()` 필수
> - **power 범위**: `text.charAt(i) * power`에서 `long` 오버플로우 주의. `(text.charAt(i) * power % MOD)` 먼저 계산
> - **MOD 선택**: 소수 + 크게 잡아야 충돌 확률 감소. 1_000_000_007 또는 이중 해시 권장

## 관련 패턴

- [[KMP]] — 단일 패턴, O(N+M) 절대 보장. 충돌 위험 없음
