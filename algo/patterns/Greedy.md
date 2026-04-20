---
title: "그리디 (Greedy)"
type: pattern
tags: 
  - greedy
  - sorting
  - activity-selection
related:
  - "[[DP_Tabulation]]"
  - "[[Backtracking]]"
created: 2026-04-06
updated: 2026-04-07
sources:
  - day0211/MeetingRoomTest.java
---
# 그리디 (Greedy)

## 언제 쓰나
- 매 순간 **지역 최적 선택**이 전체 최적으로 이어지는 문제
- 정렬 후 선형 스캔으로 풀리는 경우
- 대표 예: 회의실 배정(Activity Selection), 동전 거스름돈, 최소 비용 스케줄링

## 핵심 아이디어
**끝나는 시간 기준 정렬** → 앞에서부터 겹치지 않는 것만 선택.
"현재 선택이 다음 선택의 기회를 최대화한다"는 그리디 증명 패턴.

## 시간복잡도
- 정렬: O(N log N)
- 스캔: O(N)

## Java 코드 템플릿

### 패턴: 회의실 배정 (Activity Selection)
```java
static class Meeting implements Comparable<Meeting> {
    int start, end;
    Meeting(int s, int e) { start = s; end = e; }

    @Override
    public int compareTo(Meeting o) {
        if (this.end == o.end) return this.start - o.start; // 끝이 같으면 시작 빠른 순
        return this.end - o.end; // 끝나는 시간 오름차순
    }
}

List<Meeting> getSchedule(Meeting[] meetings) {
    Arrays.sort(meetings);
    List<Meeting> result = new ArrayList<>();
    result.add(meetings[0]);
    for (int i = 1; i < meetings.length; i++) {
        // 마지막 선택 회의가 끝난 뒤에 시작하는 회의만 추가
        if (result.get(result.size() - 1).end <= meetings[i].start) {
            result.add(meetings[i]);
        }
    }
    return result;
}
```

## 주의사항 / 자주 하는 실수
- 정렬 기준이 잘못되면 그리디 해가 틀림 — 반드시 **끝 시간 오름차순** 정렬
- 끝 시간이 같을 때 보조 기준(시작 시간) 처리 필요
- `end <= start` 조건: 같을 때 배정 가능 (`<` 가 아니라 `<=`)
- 그리디가 항상 최적이 아님 — DP가 필요한 경우(배낭 문제 등)와 구분

## 관련 패턴
- [[DP_Tabulation]] — 그리디로 안 풀릴 때 DP 고려
- [[Backtracking]] — 완전탐색 대비 최적화 방향
