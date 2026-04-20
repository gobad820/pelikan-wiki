# 위키 작업 로그

## [2026-04-20] ingest | 행복한수열 + 트로미노 + 디버깅 문답
- 소스: chat/20260420/행복한수열_Main.java, chat/20260420/트로미노_Main.java (2개)
- 생성: patterns/연속수열카운터.md (cnt=1 리셋, O(N) 단일 카운터, vs 슬라이딩 윈도우 비교)
- 생성: patterns/도형완전탐색.md (트로미노 6방향, 경계 삼항 처리, board[N][M] 주의)
- 생성: problems/CT_행복한수열.md (디버깅 히스토리 5가지 버그 포함)
- 생성: problems/CT_트로미노.md (6방향 정의, 경계 처리, N×M 주의사항)
- 생성: lectures/20260420.md (연속 카운터·도형 탐색·디버깅 체크리스트·인사이트)
- 업데이트: index.md (패턴 2개, 문제 2개, 강의노트 1개 추가, total_pages: 67)
- 스킵: 없음

## [2026-04-15] ingest | day0415 SWEA 2117 홈방범서비스 (오프라인 풀이)
- 소스: live/day0415/Solution_2117_홈방범서비스_김상해.java, Solution_2117_홈방범서비스_김상해2.java
- 생성: problems/SWEA_2117_홈방범서비스.md (완전탐색+맨해튼 거리, v1→v2 최적화 히스토리 포함)
- 업데이트: lectures/20260415.md (풀이 목록 SWEA 2117 추가, sources/tags 갱신, 복습 항목 추가)
- 업데이트: index.md (문제 1개 추가, total_pages: 62, total_sources: 45)

## [2026-04-15] ingest | day0415 KMP + Rabin-Karp 문자열 패턴 매칭
- 소스: live/day0415/KMP.md (완성), live/day0415/Rabin-Karp.md (신규 작성)
- 생성: patterns/KMP.md (pi 배열 구성 알고리즘, O(N+M) 보장, Java 템플릿)
- 생성: patterns/RabinKarp.md (롤링 해시, 다중 패턴 HashSet 확장, 이중 해시 언급)
- 생성: lectures/20260415.md (KMP vs Rabin-Karp 비교표, 복습 체크리스트)
- 업데이트: index.md (패턴 2개, 강의노트 1개 추가, total_pages: 61, total_sources: 43)
- 소스 수정: KMP.md 예제 구현 섹션 완성 (pi 배열 시각화 + Java 구현 추가)
- 소스 생성: Rabin-Karp.md 전체 내용 작성 (개념, 롤링 해시, KMP 비교표)

## [2026-04-15] ingest | EditNumberSequence + SoldierManagement 추가 (보완)
- 소스: mogakal/swea/d4/EditNumberSequence.java, mogakal/swea/d5/SoldierManagement.java
- 생성: concepts/노드풀.md (배열 기반 링크드 리스트, GC 최소화 패턴)
- 생성: concepts/버킷링크드리스트.md (팀×점수 2D 버킷 + Lazy version 무효화)
- 생성: problems/SWEA_D4_EditNumberSequence.md
- 생성: problems/SWEA_D5_SoldierManagement.md
- 생성: patterns/버킷링크드리스트.md (updateTeam O(1) 체인 병합 템플릿 포함)
- 생성: patterns/노드풀.md (풀 크기 계산 방법, init 리셋 패턴 포함)
- 업데이트: index.md (patterns 2개, concepts 2개, 문제 2개 추가, total_pages: 58)

## [2026-04-15] ingest | mogakal/baekjoon/gold + mogakal/swea/d4+d5 (전체)
- 소스 분석: _2098.java(TSP/비트마스크DP), _2110.java(파라메트릭서치), _13460.java(4D-BFS), _4195.java(UnionFind+HashMap), 공통조상찾기.java(배열LCA), EditNumberSequence.java(커스텀LinkedList), SoldierManagement.java(버킷LinkedList)
- 생성: patterns/비트마스크DP.md (TSP 교과서, bottom-up, INF 오버플로우 주의)
- 생성: patterns/이분탐색.md (기본 이분탐색 + 파라메트릭 서치 템플릿)
- 생성: problems/BOJ_2098_외판원순회.md
- 생성: problems/BOJ_2110_공유기설치.md
- 생성: problems/BOJ_13460_구슬탈출2.md
- 생성: problems/BOJ_4195_친구네트워크.md
- 업데이트: patterns/BFS_DFS.md (BOJ 13460 대표 문제 추가, sources 갱신)
- 업데이트: patterns/UnionFind.md (BOJ 4195 대표 문제 섹션 추가, sources 갱신)
- 업데이트: index.md (패턴 2개, 문제 4개 추가, total_pages: 52, total_sources: 39)
- 스킵: 공통조상찾기(배열LCA — BFS_DFS 커버), EditNumberSequence(커스텀LinkedList — 단독 패턴 불필요), SoldierManagement(버킷LinkedList — 고급 구현, 패턴화 생략)

> 형식: `## [날짜] 작업유형 | 내용`
> 파싱: `grep "^## \[" log.md | tail -10`

## [2026-04-14] ingest | day0414 DFS 풀이 추가 (2차)
- 소스: Solution_5643_키순서_김상해_2DFS.java (신규 발견)
- 스킵: 나머지 4개 파일 — 이미 1차 ingest됨
- 업데이트: problems/SWEA_5643_키순서.md (DFS 접근법 섹션 + 풀이 버전 비교표 추가)
- 업데이트: patterns/BFS_DFS.md (대표 문제 + 관련 패턴 [[플로이드워셜]] 추가)
- 업데이트: lectures/20260414.md (DFS 풀이 행 추가, 복습 항목 갱신)

## [2026-04-14] ingest | day0414 플로이드워셜 + 백트래킹
- 소스: FloydWarshall.java/md, Solution_5643(×2), Solution_1263, Solution_2112_부분집합
- 생성: patterns/플로이드워셜.md (k→i→j 루프 순서 근거, 오버플로우 주의사항 포함)
- 생성: problems/SWEA_5643_키순서.md (버그 2개 기록: 루프 순서 오류, answer 미초기화)
- 생성: problems/SWEA_1263_사람네트워크2.md (정석 구현, long 사용)
- 생성: problems/SWEA_2112_보호필름.md (v1/v2 풀이 버전 비교)
- 생성: lectures/20260414.md (플로이드워셜 + 백트래킹 복습 항목 포함)
- 업데이트: patterns/Backtracking.md (SWEA 2112 대표 문제 추가, sources 갱신)
- 업데이트: index.md (패턴 1개, 강의노트 1개, 문제 3개 추가, total_pages: 44, total_sources: 32)
- 스킵: patterns/백트래킹.md → 기존 Backtracking.md와 병합

## [2026-04-08] ingest | online/day0408 LIS
- 생성: patterns/LIS.md (O(N²) DP + O(N log N) 이진탐색 tails 배열, 주의사항 포함)
- 업데이트: lectures/20260408.md (LIS 핵심 코드 + 인사이트 + 다음 복습 항목 추가)
- 업데이트: patterns/DP_Tabulation.md (LIS related 링크 추가)
- 업데이트: index.md (패턴 1개 추가, 20260408 강의노트 설명 갱신, total_pages: 39, total_sources: 27)
- 수정 사항: 원본 LIS.md의 오류 정정 — "이진탐색 사용 조건: 입력 배열 정렬 필요"는 잘못됨. tails 내부만 정렬 유지, 입력 정렬 불필요.

---

## [2026-04-08] ingest | offline/day0408 0/1 Knapsack
- 생성: patterns/Knapsack_01.md (2D 점화식 + 1D 역순 공간 최적화 템플릿)
- 생성: lectures/20260408.md
- 생성: problems/SWEA_3282_01Knapsack.md
- 생성: problems/SWEA_5215_햄버거다이어트.md
- 업데이트: patterns/DP_Tabulation.md (Knapsack_01 related 추가)
- 업데이트: index.md (패턴 1개, 강의노트 1개, 문제 2개 추가, total_pages: 38)

---

## [2026-04-07] ingest | offline/day0407 DP 문제 5종
- 생성: problems/B_1010_다리놓기.md
- 생성: problems/B_1149_RGB거리.md
- 생성: problems/B_11726_2xn타일링.md
- 생성: problems/B_1463_1로만들기.md
- 생성: problems/B_2579_계단오르기.md
- 업데이트: patterns/DP_Tabulation.md (2D 상태 DP, 파스칼 DP, 다중 전이 DP 템플릿 추가)
- 업데이트: lectures/20260407.md (오프라인 문제 5종 + related 링크 추가)
- 업데이트: index.md (problems 섹션 5개 항목 추가, total_pages: 34)

---

## [2026-04-07] ingest | 전체 Java 소스 일괄 ingest + 포맷 통일
- 생성: lectures/20260312.md, lectures/20260313.md, lectures/20260407.md
- 포맷 수정: 전체 패턴·강의 파일 frontmatter related 필드를 인라인 배열 → YAML 블록 리스트로 통일
- 업데이트: index.md (total_pages: 29, total_sources: 14)

---

## [2026-04-06] ingest | day0406/FibonacciTest.java
- 생성: patterns/DP_Memoization.md, patterns/DP_Tabulation.md
- 생성: lectures/20260406.md
- 업데이트: index.md (total_pages: 3, total_sources: 1)

---

## [2026-04-06] init | 위키 초기화
- 디렉토리 구조 생성: patterns/, lectures/, problems/, concepts/
- CLAUDE.md 스키마 작성
- index.md, log.md 초기화
