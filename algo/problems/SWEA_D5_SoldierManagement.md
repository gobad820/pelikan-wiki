---
title: "SWEA D5 SoldierManagement (병사 관리)"
type: problem
tags: [swea, d5, linked-list, bucket, lazy-deletion, version, sds]
related:
  - "[[버킷링크드리스트]]"
  - "[[노드풀]]"
created: 2026-04-15
updated: 2026-04-15
sources:
  - mogakal/swea/d5/SoldierManagement.java
---

> [!info] 문제 정보
> - **플랫폼**: SWEA
> - **난이도**: D5
> - **유형**: 자료구조 설계 (버킷 링크드 리스트 + Lazy 무효화)

## 문제 요약

병사(ID, 팀 1~5, 점수 1~5)를 hire/fire/updateSoldier/updateTeam/bestSoldier 5가지 API로 관리한다. hire/fire/update는 각 최대 10만 회, bestSoldier는 최대 100회.

## 접근법

> [!tip] 핵심 접근
> `t[팀][점수]` 2차원 버킷 링크드 리스트. 팀 전체 점수 변경은 버킷 체인을 O(1)로 병합. fire는 `version[id] = -1` Lazy 무효화.

### 사고 흐름

1. **bestSoldier 횟수 적음(100회)** → 조회는 느려도 됨. hire/fire/update가 빨라야 함
2. 팀 전체 변경: 각 점수 버킷을 순회하며 새 버킷에 체인째 붙이기 → O(5) = O(1)
3. fire: 리스트에서 물리적 제거 시 O(N) 탐색 필요 → version 번호로 Lazy 처리
4. updateSoldier: 새 점수 버킷에 재삽입 → 기존 노드는 버전 불일치로 자동 무효
5. bestSoldier: 점수 높은 버킷부터 순회, 유효 노드 중 최대 ID 반환

### 핵심 설계

```
버킷 구조:
  t[team][score].head → sentinel → [id=3,ver=2] → [id=7,ver=1] → null
                  .tail ────────────────────────────────────────────^

version 배열:
  version[3] = 2  → node.value(2) == version(2) → 유효
  version[7] = 3  → node.value(1) != version(3) → 무효 (fire or updateSoldier됨)
```

### 핵심 코드

```java
// fire: O(1) Lazy 무효화
void fire(int mID) { version[mID] = -1; }

// updateTeam: O(5) 체인 병합 (점수 올리기)
void updateTeam(int mTeam, int mChangeScore) {
    if (mChangeScore > 0) {
        for (int j = 5; j >= 1; j--) {  // 높은 점수부터
            int k = Math.min(5, j + mChangeScore);
            if (j == k || t[mTeam].head[j].next == null) continue;
            t[mTeam].tail[k].next = t[mTeam].head[j].next; // 체인 연결
            t[mTeam].tail[k] = t[mTeam].tail[j];           // tail 갱신
            t[mTeam].head[j].next = null;                  // j 버킷 비우기
            t[mTeam].tail[j] = t[mTeam].head[j];
        }
    }
    // mChangeScore < 0이면 j=1→5 순서로 동일하게
}

// bestSoldier: O(버킷 크기) — 100회 제한이므로 허용
int bestSoldier(int mTeam) {
    for (int j = 5; j >= 1; j--) {
        int ans = 0;
        Node cur = t[mTeam].head[j].next;
        while (cur != null) {
            if (cur.value == version[cur.id])  // 유효 노드만
                ans = Math.max(ans, cur.id);
            cur = cur.next;
        }
        if (ans != 0) return ans;
    }
    return 0;
}
```

## 복잡도

| 항목 | 복잡도 | 근거 |
|------|--------|------|
| hire | O(1) | 추정: tail append |
| fire | O(1) | 추정: version 배열 갱신만 |
| updateSoldier | O(1) | 추정: hire 재호출 |
| updateTeam | O(5) ≈ O(1) | 추정: 점수 범위 5개만 순회 |
| bestSoldier | O(버킷 내 노드 수) | 추정: 무효 노드 포함 순회 |

## 배운 점 / 인사이트

> [!note] 기억할 것
> - **Lazy 무효화 선택 조건**: 삭제 호출이 많고 조회 호출이 적을 때 유효
> - **updateSoldier = 재삽입**: 기존 노드 제거 없이 새 버전으로 새 버킷에 append → 기존 노드는 버전 불일치로 자연 소멸
> - **sentinel 더미 헤드**: head[score]는 더미 노드 → 빈 버킷 체크가 `head[j].next == null`로 단순화
> - **updateTeam 방향**: 올리면 높은 j부터, 내리면 낮은 j부터 — 이미 이동한 버킷을 다시 이동하지 않기 위해
> - **num[id] = team**: hire 시 소속 팀 저장 → updateSoldier에서 팀 재조회 없이 O(1)
