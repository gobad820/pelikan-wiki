---
updated: 2026-04-20
total_pages: 67
total_sources: 47
---

# 알고리즘 위키 인덱스

> LLM이 관리하는 알고리즘 학습 지식 저장소.
> 새 소스를 추가하면 자동으로 업데이트됩니다.

---

## 패턴 (patterns/)

| 페이지 | 요약 | 태그 |
|--------|------|------|
| [[TwoPointer]] | 양끝 포인터·슬라이딩 윈도우. O(N²) → O(N) | two-pointer, sliding-window |
| [[BFS_DFS]] | BFS(큐) / DFS(재귀·스택). O(V+E) | bfs, dfs, tree, graph |
| [[Backtracking]] | 순열·조합·부분집합·N-Queen. 재귀 후 상태 복원 | permutation, combination, subset, pruning |
| [[Greedy]] | 끝나는 시간 정렬 → 탐욕 선택. 회의실 배정 | greedy, sorting, activity-selection |
| [[DivideAndConquer]] | 쿼드트리 4분할 재귀. 균일하면 처리, 아니면 분할 | divide-and-conquer, recursion |
| [[Graph_AdjList]] | 연결 리스트·ArrayList 인접 리스트 두 패턴 | graph, adjacency-list, bfs |
| [[UnionFind]] | 경로 압축 + 랭크 기반. findSet/union O(α(N)) | union-find, disjoint-set, mst |
| [[MST_Kruskal]] | 간선 정렬 + union 사이클 감지. O(E log E) | mst, kruskal, union-find |
| [[MST_Prim]] | minEdge 배열 + 비트리 최솟값 선택. O(V²)/O(E log V) | mst, prim, greedy |
| [[Dijkstra]] | 단일 출발 최단경로. 배열 O(V²) vs PQ O(E log V) | dijkstra, shortest-path, priority-queue |
| [[DP_Memoization]] | 재귀+캐시, Top-Down. dp[n]!=-1 체크 | dp, memoization, recursion |
| [[DP_Tabulation]] | 반복문, Bottom-Up. 작은 문제부터 채움 | dp, tabulation, iterative |
| [[Knapsack_01]] | 0/1 배낭: 2D 점화식 + 1D 역순 공간 최적화 | dp, knapsack, tabulation |
| [[LIS]] | 최장 증가 부분 수열: O(N²) DP + O(N log N) 이진탐색 | dp, lis, binary-search |
| [[플로이드워셜]] | 모든 쌍 최단경로, k→i→j 3중 루프, O(N³) | graph, shortest-path, dp, all-pairs |
| [[비트마스크DP]] | 방문 집합을 비트로 표현한 DP. TSP O(N²×2^N) | dp, bitmask, tsp, combinatorial |
| [[이분탐색]] | 정렬 공간 O(log N) 탐색. 파라메트릭 서치로 최솟값 최대화 | binary-search, parametric, greedy |
| [[버킷링크드리스트]] | 키별 버킷 분산 저장. 일괄 변경 O(1) 체인 병합 + Lazy version 무효화 | linked-list, bucket, lazy-deletion, sds |
| [[노드풀]] | new 없이 배열로 노드 사전 할당. cnt=0 리셋으로 TC 재사용, GC 제거 | linked-list, node-pool, sds |
| [[KMP]] | pi 배열(실패 함수) 기반 단일 패턴 검색. O(N+M) 항상 보장 | string, pattern-matching, kmp |
| [[RabinKarp]] | 롤링 해시 기반 패턴 검색. 다중 패턴 HashSet 확장 용이 | string, pattern-matching, hashing |
| [[연속수열카운터]] | cnt=1 리셋 단일 순회 O(N). 슬라이딩 윈도우 대체 | array, consecutive, run-length |
| [[도형완전탐색]] | 트로미노·테트로미노 등 고정 도형 오프셋 열거. 경계 삼항 처리 | simulation, geometry, brute-force |

---

## 강의 노트 (lectures/)

| 날짜 | 주요 내용 |
|------|-----------|
| [[20260203]] | 투 포인터: 양끝 포인터, 슬라이딩 윈도우 |
| [[20260204]] | 완전 이진 트리 BFS: 배열 인덱스 규칙, 레벨 구분 |
| [[20260205]] | 순열 4종 세트: 중복순열·순열·중복조합·조합 |
| [[20260206]] | 부분집합: 포함/미포함 분기, 합 조건 누적 |
| [[20260209]] | 비트마스크 순열 + 다음 순열(Next Permutation) |
| [[20260210]] | N-Queen: 열·대각선 가지치기 |
| [[20260211]] | 그리디 회의실 배정: Comparable 구현 |
| [[20260212]] | 분할 정복 쿼드트리: 2D 배열 4분할 |
| [[20260213]] | 인접 리스트 연결 리스트 방식 |
| [[20260219]] | ArrayList 인접 리스트 + BFS |
| [[20260309]] | Union-Find: 경로 압축 + 랭크 기반 union |
| [[20260310]] | MST 크루스칼: 간선 정렬 + Union-Find |
| [[20260311]] | MST 프림: 인접 리스트 + minEdge 배열 |
| [[20260312]] | 다익스트라: 단순 배열 스캔 O(V²) |
| [[20260313]] | 다익스트라 PQ: O(E log V), 두 가지 스킵 패턴 |
| [[20260406]] | DP 메모이제이션 vs 타뷸레이션, 피보나치 |
| [[20260407]] | DP 설계 7단계, 파스칼 삼각형 |
| [[20260408]] | LIS O(N²) DP + 이진탐색 O(N log N), 0/1 Knapsack 2D/1D |
| [[20260414]] | 플로이드 워셜 (k→i→j), 백트래킹 최적화 |
| [[20260420]] | 연속 카운터 패턴, 트로미노 도형 완전 탐색, 디버깅 방법론 |
| [[20260415]] | KMP pi 배열 + Rabin-Karp 롤링 해시, 문자열 패턴 매칭 |

---

## 문제 유형 (problems/)

| 페이지 | 요약 | 태그 |
|--------|------|------|
| [[BOJ_1010_다리놓기]] | nCr 조합 DP — 파스칼 삼각형 점화식, 상향식+하향식 | dp, combination, pascal |
| [[BOJ_1149_RGB거리]] | 2D 상태 DP — 인접 색 제약을 dp 차원으로 표현 | dp, state-dp |
| [[BOJ_11726_2xn타일링]] | 피보나치형 1D DP — mod 10007, static 전처리 | dp, fibonacci, tiling |
| [[BOJ_1463_1로만들기]] | 다중 전이 DP — 조건부 나누기 연산 | dp, multi-transition |
| [[BOJ_2579_계단오르기]] | 상태 제약 DP — 연속 계단 수를 상태로 | dp, state-dp, conditional |
| [[SWEA_3282_01Knapsack]] | 0/1 Knapsack 기본형 — 2D + 1D 역순 두 풀이 | dp, knapsack |
| [[SWEA_5215_햄버거다이어트]] | Knapsack 응용 — 칼로리=무게, 맛=가치 변수 치환 | dp, knapsack |
| [[SWEA_5643_키순서]] | 플로이드 워셜 전이적 도달 가능성 — 키 순서 확정 가능 학생 수 | graph, floyd-warshall, reachability |
| [[SWEA_1263_사람네트워크2]] | 플로이드 워셜 거리 합 최솟값 — long 사용, 올바른 k→i→j | graph, floyd-warshall |
| [[SWEA_2112_보호필름]] | 백트래킹 최적화 — 스킵 우선 탐색, 3분기 가지치기 | backtracking, simulation |
| [[BOJ_2098_외판원순회]] | TSP 비트마스크 DP 교과서 문제. dp[visited][curr] bottom-up | dp, bitmask, tsp |
| [[BOJ_2110_공유기설치]] | 최솟값의 최대화 파라메트릭 서치. check=그리디 설치 가능 여부 | binary-search, greedy |
| [[BOJ_13460_구슬탈출2]] | 4D BFS 상태 탐색. 두 구슬 위치 (ry,rx,by,bx) + 선행 구슬 처리 | bfs, simulation, 4d-state |
| [[BOJ_4195_친구네트워크]] | HashMap String→int 매핑 + Union-Find 음수 크기 패턴 | union-find, hash-map |
| [[SWEA_D4_EditNumberSequence]] | 노드 풀 링크드 리스트 I/D/C 연산. cnt 리셋으로 TC 재사용 | linked-list, node-pool, d4 |
| [[SWEA_D5_SoldierManagement]] | 버킷 링크드 리스트 + Lazy 무효화. updateTeam O(1), fire O(1) | bucket, lazy-deletion, version, d5 |
| [[SWEA_2117_홈방범서비스]] | 완전탐색 + 맨해튼 거리 누적합. TreeMap→int[] 최적화, static 배열 초기화 버그 | brute-force, manhattan-distance, java-optimization |
| [[CT_행복한수열]] | N×N 보드 행·열 연속 M개 이상 카운트. cnt=1 리셋 패턴, 디버깅 히스토리 포함 | simulation, consecutive, codetree |
| [[CT_트로미노]] | N×M 보드 트로미노 6방향 완전 열거. 경계 삼항 처리, board[N][M] 선언 주의 | simulation, geometry, codetree |

---

## 개념 (concepts/)

| 페이지 | 요약 | 태그 |
|--------|------|------|
| [[노드풀]] | 배열로 노드 미리 할당 — new 없이 링크드 리스트 구성, GC 부담 제거 | linked-list, node-pool, sds |
| [[버킷링크드리스트]] | 팀×점수 2차원 버킷 + Lazy version 무효화. updateTeam O(1) 체인 병합 | linked-list, bucket, lazy-deletion, sds |
