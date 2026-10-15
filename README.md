# Algorithm
Algorithm Study

## 목차
1. [시간복잡도](#1-시간복잡도)
   - [시간복잡도란?](#11-시간복잡도란)
   - [점근 표기법](#12-점근-표기법)
   - [시간복잡도 계산 규칙](#13-시간복잡도-계산-규칙)
   - [시간복잡도 크기 비교](#14-시간복잡도-크기-비교)
   - [입력 크기(N)별 허용 시간복잡도](#15-입력-크기n별-허용-시간복잡도)
   - [재귀 함수의 시간복잡도](#16-재귀-함수의-시간복잡도)
   - [자료구조 연산별 시간복잡도](#17-자료구조-연산별-시간복잡도)
   - [정렬 알고리즘 시간복잡도](#18-정렬-알고리즘-시간복잡도)
   - [그래프 · 문자열 알고리즘 시간복잡도](#19-그래프--문자열-알고리즘-시간복잡도)
   - [공간복잡도](#110-공간복잡도)
2. [시간복잡도별 알고리즘](#2-시간복잡도별-알고리즘)
3. [알고리즘별 해결할 수 있는 문제](#3-알고리즘별-해결할-수-있는-문제)
4. [문제 키워드로 알고리즘 찾기](#4-문제-키워드로-알고리즘-찾기)

---

## 1. 시간복잡도

### 1.1 시간복잡도란?
**시간복잡도(Time Complexity)** 는 입력 크기 `N`이 커질 때 알고리즘의 **연산 횟수가 얼마나 빠르게 증가하는지**를 나타낸 것이다.
실제 실행 시간(초)은 하드웨어·언어에 따라 달라지므로, 시간복잡도는 "입력 대비 증가율"만 본다.

- 코딩 테스트에서는 보통 **최악의 경우(Worst Case)** 를 기준으로 판단한다.
- 대략 **1초에 1억(10⁸) 번의 단순 연산**이 가능하다고 가정한다. (C++/Java 기준)
- **Python은 C++보다 수 배~수십 배 느리므로** 1초에 약 10⁷ 번 정도로 보수적으로 잡는 것이 안전하다. (PyPy 사용 시 개선)

### 1.2 점근 표기법

| 표기법 | 의미 | 설명 |
| --- | --- | --- |
| **O (Big-O)** | 상한 (최악) | `f(N) ≤ c·g(N)` — "아무리 오래 걸려도 이 정도" |
| **Ω (Big-Omega)** | 하한 (최선) | `f(N) ≥ c·g(N)` — "아무리 빨라도 이 정도는 걸림" |
| **Θ (Big-Theta)** | 상한 = 하한 | `c₁·g(N) ≤ f(N) ≤ c₂·g(N)` — "정확히 이 정도" |

> 일반적으로 시간복잡도를 말할 때는 **Big-O**를 사용한다.

### 1.3 시간복잡도 계산 규칙

| 규칙 | 예시 |
| --- | --- |
| 상수는 무시한다 | `O(3N + 5)` → `O(N)` |
| 가장 높은 차수만 남긴다 | `O(N² + 100N + 1000)` → `O(N²)` |
| 순차 실행되는 코드는 **더한다** (결국 큰 쪽만 남음) | `O(N) + O(N²)` → `O(N²)` |
| 중첩된 반복문은 **곱한다** | `for i in N: for j in M:` → `O(N·M)` |
| 반복마다 범위가 절반이 되면 `log` | `while n > 0: n //= 2` → `O(log N)` |
| 로그의 밑은 무시한다 | `O(log₂N)` = `O(log₁₀N)` = `O(log N)` |
| 입력이 여러 개면 변수를 분리한다 | 그래프 탐색 `O(V + E)`, 두 문자열 비교 `O(N·M)` |

```python
# O(N)
for i in range(n):
    total += i

# O(N^2)
for i in range(n):
    for j in range(n):
        total += i * j

# O(log N)
while n > 1:
    n //= 2

# O(N log N)
for i in range(n):        # N번
    j = 1
    while j < n:          # log N번
        j *= 2
```

### 1.4 시간복잡도 크기 비교

```
O(1) < O(log N) < O(√N) < O(N) < O(N log N) < O(N²) < O(N³) < O(2ᴺ) < O(N!)
```

| N | log N | N log N | N² | N³ | 2ᴺ | N! |
| ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| 10 | 3 | 33 | 100 | 1,000 | 1,024 | 3,628,800 |
| 20 | 4 | 86 | 400 | 8,000 | 약 10⁶ | 약 2.4 × 10¹⁸ |
| 100 | 7 | 664 | 10⁴ | 10⁶ | 약 1.3 × 10³⁰ | 약 9.3 × 10¹⁵⁷ |
| 1,000 | 10 | 약 10⁴ | 10⁶ | 10⁹ | - | - |
| 10⁵ | 17 | 약 1.7 × 10⁶ | 10¹⁰ | - | - | - |
| 10⁶ | 20 | 약 2 × 10⁷ | 10¹² | - | - | - |

> `N = 10⁵`일 때 `O(N²)`은 100억 번 → 시간 초과, `O(N log N)`은 약 170만 번 → 여유롭게 통과.

### 1.5 입력 크기(N)별 허용 시간복잡도
시간 제한 **1초** 기준 (C++/Java). 문제를 읽고 **N의 범위를 먼저 보면 사용할 알고리즘을 좁힐 수 있다.**

| 입력 크기 N | 허용 시간복잡도 | 사용할 수 있는 알고리즘 예시 |
| --- | --- | --- |
| N ≤ 10 | O(N!) | 순열 완전탐색, 백트래킹 |
| N ≤ 20 | O(2ᴺ), O(2ᴺ·N) | 부분집합 완전탐색, 비트마스크 |
| N ≤ 16 | O(2ᴺ·N²) | 비트마스크 DP (외판원 순회) |
| N ≤ 40 | O(2^(N/2)·N) | 중간에서 만나기 (Meet in the Middle) |
| N ≤ 500 | O(N³) | 플로이드-워셜, 구간 DP, 3중 반복문 |
| N ≤ 2,000 ~ 5,000 | O(N²) | 2중 반복문, LCS, 배낭 DP, 단순 정렬 |
| N ≤ 100,000 ~ 1,000,000 | O(N log N) | 정렬, 이분 탐색, 힙, 다익스트라, 세그먼트 트리 |
| N ≤ 10,000,000 | O(N) | 투 포인터, 누적합, 슬라이딩 윈도우, BFS/DFS |
| N ≤ 10¹² ~ 10¹⁴ | O(√N) | 소수 판별, 약수 구하기 |
| N > 10¹⁴ (예: 10¹⁸) | O(log N), O(1) | 이분 탐색, 분할 정복 거듭제곱, 수학 공식 |

### 1.6 재귀 함수의 시간복잡도

| 점화식 | 시간복잡도 | 예시 |
| --- | --- | --- |
| T(N) = T(N/2) + O(1) | O(log N) | 이분 탐색, 분할 정복 거듭제곱 |
| T(N) = 2T(N/2) + O(1) | O(N) | 트리 순회, 세그먼트 트리 구축 |
| T(N) = 2T(N/2) + O(N) | O(N log N) | 병합 정렬, 퀵 정렬(평균) |
| T(N) = T(N-1) + O(1) | O(N) | 팩토리얼 재귀 |
| T(N) = T(N-1) + O(N) | O(N²) | 퀵 정렬(최악), 재귀 선택 정렬 |
| T(N) = 2T(N-1) + O(1) | O(2ᴺ) | 하노이 탑, 부분집합 생성 |
| T(N) = T(N-1) + T(N-2) + O(1) | O(1.618ᴺ) | 메모이제이션 없는 피보나치 |
| T(N) = N·T(N-1) | O(N!) | 순열 생성 |

> 재귀 호출 수 × 호출 1번당 작업량으로 계산한다. **메모이제이션**을 하면 `상태 수 × 상태당 계산량`이 된다. (피보나치: O(2ᴺ) → O(N))

### 1.7 자료구조 연산별 시간복잡도

| 자료구조 | 접근 | 탐색 | 삽입 | 삭제 | 비고 |
| --- | :---: | :---: | :---: | :---: | --- |
| 배열 (동적 배열) | O(1) | O(N) | O(N) | O(N) | 맨 뒤 삽입/삭제는 O(1) (분할 상환) |
| 연결 리스트 | O(N) | O(N) | O(1) | O(1) | 삽입/삭제 위치를 알고 있을 때 |
| 스택 | - | - | O(1) | O(1) | push / pop |
| 큐 | - | - | O(1) | O(1) | enqueue / dequeue |
| 덱 (Deque) | O(1)* | O(N) | O(1) | O(1) | 양 끝 삽입/삭제 (*구현에 따라 다름) |
| 해시 테이블 (Map/Set) | - | O(1) | O(1) | O(1) | 평균 기준, 최악 O(N) |
| 이진 힙 (우선순위 큐) | O(1) | O(N) | O(log N) | O(log N) | 최댓값/최솟값 조회 O(1) |
| 균형 이진 탐색 트리 | O(log N) | O(log N) | O(log N) | O(log N) | C++ `set/map`, Java `TreeMap` |
| 세그먼트 트리 | - | O(log N) | - | - | 구축 O(N), 구간 쿼리·갱신 O(log N) |
| 펜윅 트리 (BIT) | - | O(log N) | - | - | 누적합 쿼리·점 갱신 O(log N) |
| 유니온 파인드 | - | O(α(N)) | - | - | find/union, 사실상 O(1) |
| 트라이 (Trie) | - | O(L) | O(L) | O(L) | L = 문자열 길이 |

### 1.8 정렬 알고리즘 시간복잡도

| 정렬 | 최선 | 평균 | 최악 | 추가 공간 | 안정 정렬 |
| --- | :---: | :---: | :---: | :---: | :---: |
| 버블 정렬 | O(N)* | O(N²) | O(N²) | O(1) | O |
| 선택 정렬 | O(N²) | O(N²) | O(N²) | O(1) | X |
| 삽입 정렬 | O(N) | O(N²) | O(N²) | O(1) | O |
| 병합 정렬 | O(N log N) | O(N log N) | O(N log N) | O(N) | O |
| 퀵 정렬 | O(N log N) | O(N log N) | O(N²) | O(log N) | X |
| 힙 정렬 | O(N log N) | O(N log N) | O(N log N) | O(1) | X |
| 계수 정렬 | O(N + K) | O(N + K) | O(N + K) | O(N + K) | O |
| 기수 정렬 | O(d·(N + K)) | O(d·(N + K)) | O(d·(N + K)) | O(N + K) | O |
| 팀 정렬 (Tim Sort) | O(N) | O(N log N) | O(N log N) | O(N) | O |

- `*` 버블 정렬은 교환이 없으면 조기 종료하도록 최적화했을 때 O(N)
- `K` = 값의 범위, `d` = 자릿수
- Python `sort()`, Java `Collections.sort()`(객체)는 **팀 정렬**, C++ `std::sort`는 **인트로 정렬**(퀵 + 힙 + 삽입)로 최악에도 O(N log N)

### 1.9 그래프 · 문자열 알고리즘 시간복잡도

| 분류 | 알고리즘 | 시간복잡도 |
| --- | --- | --- |
| 그래프 탐색 | BFS / DFS | O(V + E) |
| 최단 경로 | 다익스트라 (우선순위 큐) | O(E log V) |
| 최단 경로 | 0-1 BFS | O(V + E) |
| 최단 경로 | 벨만-포드 | O(V·E) |
| 최단 경로 | 플로이드-워셜 | O(V³) |
| 최소 스패닝 트리 | 크루스칼 | O(E log E) |
| 최소 스패닝 트리 | 프림 (우선순위 큐) | O(E log V) |
| 그래프 | 위상 정렬 | O(V + E) |
| 그래프 | SCC (타잔 / 코사라주) | O(V + E) |
| 트리 | LCA (희소 배열) | 전처리 O(N log N), 쿼리 O(log N) |
| 유량 | 에드몬드-카프 | O(V·E²) |
| 유량 | 디닉 | O(V²·E) |
| 매칭 | 이분 매칭 (DFS) | O(V·E) |
| 문자열 | KMP | O(N + M) |
| 문자열 | 라빈-카프 | 평균 O(N + M) |
| 문자열 | 아호-코라식 | O(N + ΣM) |
| 문자열 | 매내처 (Manacher) | O(N) |
| 문자열 | 접미사 배열 + LCP | O(N log N) ~ O(N log² N) |

### 1.10 공간복잡도
알고리즘이 사용하는 **메모리 양**의 증가율. 코딩 테스트의 메모리 제한은 보통 128MB ~ 512MB이다.

- `int`(4바이트) 기준 **256MB ≈ 약 6,700만 개**, `long long`(8바이트) 기준 약 3,300만 개
- 2차원 배열 `N × N`은 `N = 10,000`이면 1억 개 → 메모리 초과 가능
- Python은 객체 오버헤드가 커서 같은 개수라도 훨씬 많은 메모리를 사용한다.
- 재귀 깊이도 공간을 사용한다 (Python 기본 재귀 한도 1,000 → `sys.setrecursionlimit` 필요)

---

## 2. 시간복잡도별 알고리즘

| 시간복잡도 | 이름 | 대표 알고리즘 |
| --- | --- | --- |
| O(1) | 상수 시간 | 배열 인덱스 접근, 해시 조회, 스택/큐 연산, 수학 공식 |
| O(α(N)) | 역 아커만 | 유니온 파인드 (경로 압축 + 랭크) |
| O(log N) | 로그 시간 | 이분 탐색, 힙 삽입/삭제, 분할 정복 거듭제곱, 유클리드 호제법 |
| O(√N) | 제곱근 시간 | 소수 판별, 약수 구하기, 소인수분해 |
| O(N) | 선형 시간 | 선형 탐색, 투 포인터, 슬라이딩 윈도우, 누적합, BFS/DFS |
| O(N log log N) | - | 에라토스테네스의 체 |
| O(N log N) | 선형 로그 시간 | 병합/힙/퀵 정렬, 다익스트라, 크루스칼, LIS(이분 탐색) |
| O(N√N) | - | Mo's 알고리즘, 제곱근 분할법 |
| O(N²) | 이차 시간 | 버블/선택/삽입 정렬, LCS, 배낭 DP, 2중 반복문 |
| O(N³) | 삼차 시간 | 플로이드-워셜, 구간 DP, 행렬 곱셈 |
| O(2ᴺ) | 지수 시간 | 부분집합 완전탐색, 비트마스크 DP |
| O(N!) | 팩토리얼 시간 | 순열 완전탐색, 백트래킹 |

### O(1) — 상수 시간
입력 크기와 상관없이 항상 같은 시간이 걸린다.
- 배열 인덱스 접근 `arr[i]`
- 해시 테이블 삽입/조회/삭제 (평균)
- 스택 push/pop, 큐 enqueue/dequeue
- 수학 공식 (`1 + 2 + ... + N = N(N+1)/2`)
- 누적합 배열을 이용한 구간 합 조회
- 유니온 파인드 find/union — 엄밀히는 O(α(N))이지만 사실상 상수

```python
def range_sum(prefix, l, r):   # 누적합 배열이 있으면 구간 합은 O(1)
    return prefix[r] - prefix[l - 1]
```

### O(log N) — 로그 시간
매 단계마다 탐색 범위가 **절반(또는 일정 비율)으로 줄어든다.**
- 이분 탐색, `lower_bound` / `upper_bound`
- 힙(우선순위 큐) 삽입/삭제
- 균형 이진 탐색 트리 (C++ `set/map`, Java `TreeMap`) 연산
- 세그먼트 트리 / 펜윅 트리 쿼리·갱신
- 분할 정복을 이용한 거듭제곱 `aᴺ`
- 유클리드 호제법 (최대공약수) — O(log min(a, b))
- LCA 쿼리 (희소 배열)

```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:            # 매 반복마다 범위가 절반으로 → O(log N)
        mid = (lo + hi) // 2
        if arr[mid] == target:
            return mid
        if arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```

### O(√N) — 제곱근 시간
- 소수 판별 (√N 까지만 나눠보기)
- 약수 구하기, 소인수분해
- 제곱근 분할법 (쿼리 1개당 O(√N))

```python
def is_prime(n):
    if n < 2:
        return False
    i = 2
    while i * i <= n:          # √n 까지만 확인
        if n % i == 0:
            return False
        i += 1
    return True
```

### O(N) — 선형 시간
입력을 **한 번(또는 상수 번) 훑는다.**
- 선형 탐색, 최댓값/최솟값 찾기
- 누적합 배열 만들기, 차분 배열
- 투 포인터, 슬라이딩 윈도우
- 카데인 알고리즘 (최대 연속 부분합)
- 모노토닉 스택 (오큰수, 히스토그램)
- 계수 정렬 — O(N + K)
- BFS / DFS / 위상 정렬 / 0-1 BFS — O(V + E)
- 트리 DP, 트리의 지름
- KMP, Z 알고리즘 — O(N + M), 매내처 — O(N)

```python
def two_pointer(arr, target):  # 연속 부분 수열의 합이 target인 개수 (원소가 모두 양수일 때)
    count = left = total = 0
    for right in range(len(arr)):   # right, left 모두 최대 N번 이동 → O(N)
        total += arr[right]
        while total > target:
            total -= arr[left]
            left += 1
        if total == target:
            count += 1
    return count
```

### O(N log log N)
- 에라토스테네스의 체 (1 ~ N 사이 모든 소수 구하기)

```python
def sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, n + 1, i):
                is_prime[j] = False
    return is_prime
```

### O(N log N) — 선형 로그 시간
**정렬**이 들어가거나, **N번의 O(log N) 연산**을 수행한다.
- 병합 정렬, 힙 정렬, 퀵 정렬(평균), 언어 내장 정렬
- 정렬 후 그리디
- N번의 이분 탐색, 파라메트릭 서치 — O(N log X)
- 힙에 N번 삽입/삭제
- 다익스트라 — O(E log V), 크루스칼 — O(E log E)
- 가장 긴 증가하는 부분 수열 (LIS, 이분 탐색 활용)
- 좌표 압축, 스위핑, 볼록 껍질
- 세그먼트 트리로 Q개 쿼리 처리 — O((N + Q) log N)
- 병합 정렬로 역순쌍(Inversion) 개수 세기

```python
def merge_sort(arr):           # 깊이 log N × 각 층 병합 N → O(N log N)
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left, right = merge_sort(arr[:mid]), merge_sort(arr[mid:])
    result, i, j = [], 0, 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i]); i += 1
        else:
            result.append(right[j]); j += 1
    return result + left[i:] + right[j:]
```

### O(N√N)
- Mo's 알고리즘 — O((N + Q)√N), 오프라인 구간 쿼리
- 제곱근 분할법으로 N개의 쿼리 처리

### O(N²) — 이차 시간
**2중 반복문**, 모든 쌍을 확인한다.
- 버블 정렬, 선택 정렬, 삽입 정렬
- 모든 쌍 (i, j) 확인하는 완전탐색
- LIS — O(N²) DP 버전
- LCS, 편집 거리 — O(N·M)
- 배낭 문제 — O(N·W) (W = 무게 한도)
- 동전 DP — O(N·K)
- 인접 행렬 기반 다익스트라 — O(V²)
- 벨만-포드 — O(V·E)

```python
def lcs(a, b):                 # O(N·M)
    dp = [[0] * (len(b) + 1) for _ in range(len(a) + 1)]
    for i in range(1, len(a) + 1):
        for j in range(1, len(b) + 1):
            if a[i - 1] == b[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return dp[len(a)][len(b)]
```

### O(N³) — 삼차 시간
**3중 반복문**
- 플로이드-워셜 (모든 정점 쌍 최단 경로)
- 구간 DP (행렬 곱셈 순서, 파일 합치기)
- 단순 행렬 곱셈

```python
def floyd_warshall(dist, n):   # O(V³)
    for k in range(n):         # 거쳐가는 정점
        for i in range(n):
            for j in range(n):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
```

### O(2ᴺ) — 지수 시간
각 원소를 **선택한다 / 안 한다** 두 가지로 나눈다.
- 부분집합 완전탐색
- 비트마스크 완전탐색
- 비트마스크 DP — O(2ᴺ·N²) (외판원 순회)
- 중간에서 만나기 — O(2^(N/2)·N)
- 하노이 탑

```python
def subsets(arr, idx=0, chosen=[]):   # 원소마다 2가지 선택 → O(2ᴺ)
    if idx == len(arr):
        print(chosen)
        return
    subsets(arr, idx + 1, chosen + [arr[idx]])   # 선택
    subsets(arr, idx + 1, chosen)                # 선택 안 함
```

### O(N!) — 팩토리얼 시간
**모든 순서(순열)** 를 확인한다.
- 순열 완전탐색 (외판원 문제 브루트포스)
- 백트래킹 (N-Queen 등, 가지치기로 실제로는 더 빠름)

```python
def permutations(arr, chosen=[], used=None):  # N × (N-1) × ... × 1 → O(N!)
    used = used or [False] * len(arr)
    if len(chosen) == len(arr):
        print(chosen)
        return
    for i in range(len(arr)):
        if not used[i]:
            used[i] = True
            permutations(arr, chosen + [arr[i]], used)
            used[i] = False
```

---

## 3. 알고리즘별 해결할 수 있는 문제
> 대표 문제: **BOJ** = [백준 온라인 저지](https://www.acmicpc.net), **PG** = [프로그래머스](https://school.programmers.co.kr)

### 3.1 완전 탐색

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 브루트 포스 | 경우의 수 × 검사 비용 | 입력이 작아 모든 경우를 직접 확인할 수 있는 문제 | [BOJ 2798 블랙잭](https://www.acmicpc.net/problem/2798)<br>[BOJ 2309 일곱 난쟁이](https://www.acmicpc.net/problem/2309)<br>[BOJ 1018 체스판 다시 칠하기](https://www.acmicpc.net/problem/1018)<br>[PG 모의고사](https://school.programmers.co.kr/learn/courses/30/lessons/42840)<br>[PG 카펫](https://school.programmers.co.kr/learn/courses/30/lessons/42842) |
| 백트래킹 | O(N!), O(2ᴺ)<br>(가지치기로 감소) | 순열/조합 생성, 조건을 만족하는 배치 찾기, 제약 조건 만족 문제 | [BOJ 15649 N과 M (1)](https://www.acmicpc.net/problem/15649)<br>[BOJ 9663 N-Queen](https://www.acmicpc.net/problem/9663)<br>[BOJ 2580 스도쿠](https://www.acmicpc.net/problem/2580)<br>[BOJ 14888 연산자 끼워넣기](https://www.acmicpc.net/problem/14888)<br>[PG 피로도](https://school.programmers.co.kr/learn/courses/30/lessons/87946) |
| 비트마스크 | O(2ᴺ·N) | 부분집합 열거, 집합 상태(방문 여부, 켜진 스위치 등)를 정수 하나로 표현 | [BOJ 11723 집합](https://www.acmicpc.net/problem/11723)<br>[BOJ 1182 부분수열의 합](https://www.acmicpc.net/problem/1182) |
| 중간에서 만나기 | O(2^(N/2)·N) | N ≤ 40인 부분집합 합 문제 — 절반씩 나눠 완전탐색 후 합치기 | [BOJ 1208 부분수열의 합 2](https://www.acmicpc.net/problem/1208)<br>[BOJ 7453 합이 0인 네 정수](https://www.acmicpc.net/problem/7453) |
| 시뮬레이션 / 구현 | 문제에 따라 다름 | 문제에 주어진 규칙을 그대로 코드로 옮기는 문제 | [BOJ 14503 로봇 청소기](https://www.acmicpc.net/problem/14503)<br>[BOJ 3190 뱀](https://www.acmicpc.net/problem/3190)<br>[BOJ 14499 주사위 굴리기](https://www.acmicpc.net/problem/14499) |

### 3.2 그리디

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 그리디 (탐욕법) | 보통 O(N log N)<br>(정렬 포함) | 매 순간 가장 좋은 선택이 전체 최적해를 보장하는 문제<br>- 활동 선택 (끝나는 시간 기준 정렬)<br>- 거스름돈 (동전이 배수 관계일 때)<br>- 대기 시간 최소화 (짧은 작업 먼저) | [BOJ 11047 동전 0](https://www.acmicpc.net/problem/11047)<br>[BOJ 1931 회의실 배정](https://www.acmicpc.net/problem/1931)<br>[BOJ 11399 ATM](https://www.acmicpc.net/problem/11399)<br>[BOJ 13305 주유소](https://www.acmicpc.net/problem/13305)<br>[BOJ 1202 보석 도둑](https://www.acmicpc.net/problem/1202)<br>[PG 체육복](https://school.programmers.co.kr/learn/courses/30/lessons/42862)<br>[PG 구명보트](https://school.programmers.co.kr/learn/courses/30/lessons/42885)<br>[PG 단속카메라](https://school.programmers.co.kr/learn/courses/30/lessons/42884) |

> 그리디는 **정당성 증명**이 필요하다. 반례가 존재하면 DP를 고려한다. (예: 동전이 배수 관계가 아니면 동전 DP)

### 3.3 정렬

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 비교 정렬<br>(병합/퀵/힙/내장 정렬) | O(N log N) | 순서대로 처리해야 하는 문제, 그리디·이분 탐색 전처리, 중복 제거, 커스텀 기준 정렬 | [BOJ 2751 수 정렬하기 2](https://www.acmicpc.net/problem/2751)<br>[BOJ 11650 좌표 정렬하기](https://www.acmicpc.net/problem/11650)<br>[PG K번째수](https://school.programmers.co.kr/learn/courses/30/lessons/42748)<br>[PG 가장 큰 수](https://school.programmers.co.kr/learn/courses/30/lessons/42746)<br>[PG H-Index](https://school.programmers.co.kr/learn/courses/30/lessons/42747) |
| 계수 정렬 | O(N + K) | 값의 범위 K가 작고 N이 매우 클 때 | [BOJ 10989 수 정렬하기 3](https://www.acmicpc.net/problem/10989) |
| 병합 정렬 응용 | O(N log N) | 역순쌍(Inversion) 개수 세기 = 버블 정렬 교환 횟수 | [BOJ 1517 버블 소트](https://www.acmicpc.net/problem/1517) |
| 좌표 압축 | O(N log N) | 값의 범위가 너무 커서 배열 인덱스로 쓸 수 없을 때 순위로 압축 | [BOJ 18870 좌표 압축](https://www.acmicpc.net/problem/18870) |

### 3.4 이분 탐색

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 이분 탐색 | O(log N)<br>(정렬 O(N log N) 별도) | 정렬된 배열에서 값의 존재 여부·개수, `lower_bound`/`upper_bound` | [BOJ 1920 수 찾기](https://www.acmicpc.net/problem/1920)<br>[BOJ 10816 숫자 카드 2](https://www.acmicpc.net/problem/10816) |
| 파라메트릭 서치 | O(log X × 판정 비용) | **"조건을 만족하는 최댓값/최솟값"**, **"최댓값의 최소화"** — 최적화 문제를 "X로 가능한가?"라는 결정 문제로 바꿔 이분 탐색 | [BOJ 2805 나무 자르기](https://www.acmicpc.net/problem/2805)<br>[BOJ 1654 랜선 자르기](https://www.acmicpc.net/problem/1654)<br>[BOJ 2110 공유기 설치](https://www.acmicpc.net/problem/2110)<br>[BOJ 1300 K번째 수](https://www.acmicpc.net/problem/1300)<br>[PG 입국심사](https://school.programmers.co.kr/learn/courses/30/lessons/43238)<br>[PG 징검다리](https://school.programmers.co.kr/learn/courses/30/lessons/43236) |

### 3.5 누적합 · 투 포인터 · 슬라이딩 윈도우

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 누적합 (Prefix Sum) | 전처리 O(N)<br>쿼리 O(1) | 값이 변하지 않는 배열의 구간 합 쿼리가 많을 때, 2차원 구간 합 | [BOJ 11659 구간 합 구하기 4](https://www.acmicpc.net/problem/11659)<br>[BOJ 11660 구간 합 구하기 5](https://www.acmicpc.net/problem/11660)<br>[BOJ 10986 나머지 합](https://www.acmicpc.net/problem/10986) |
| 차분 배열 (imos) | O(N + Q) | 구간에 값을 더하는 갱신이 여러 번 일어난 뒤 최종 결과만 필요할 때 | [PG 파괴되지 않은 건물](https://school.programmers.co.kr/learn/courses/30/lessons/92344) |
| 투 포인터 | O(N) | 연속 부분 수열의 합, 정렬된 배열에서 두 수의 합 | [BOJ 2003 수들의 합 2](https://www.acmicpc.net/problem/2003)<br>[BOJ 1806 부분합](https://www.acmicpc.net/problem/1806)<br>[BOJ 3273 두 수의 합](https://www.acmicpc.net/problem/3273)<br>[BOJ 2470 두 용액](https://www.acmicpc.net/problem/2470) |
| 슬라이딩 윈도우 | O(N) | 고정 길이 구간의 합·최댓값·조건 만족 여부 | [BOJ 2559 수열](https://www.acmicpc.net/problem/2559)<br>[BOJ 12891 DNA 비밀번호](https://www.acmicpc.net/problem/12891)<br>[BOJ 11003 최솟값 찾기](https://www.acmicpc.net/problem/11003) |
| 카데인 알고리즘 | O(N) | 최대 연속 부분합 | [BOJ 1912 연속합](https://www.acmicpc.net/problem/1912) |

### 3.6 자료구조

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 스택 | push/pop O(1) | 괄호 짝 맞추기, 후위 표기식, 되돌리기(Undo)<br>**모노토닉 스택**: 오른쪽/왼쪽에서 가장 가까운 큰(작은) 수 | [BOJ 10828 스택](https://www.acmicpc.net/problem/10828)<br>[BOJ 9012 괄호](https://www.acmicpc.net/problem/9012)<br>[BOJ 17298 오큰수](https://www.acmicpc.net/problem/17298)<br>[BOJ 2493 탑](https://www.acmicpc.net/problem/2493)<br>[BOJ 6549 히스토그램에서 가장 큰 직사각형](https://www.acmicpc.net/problem/6549)<br>[PG 올바른 괄호](https://school.programmers.co.kr/learn/courses/30/lessons/12909)<br>[PG 주식가격](https://school.programmers.co.kr/learn/courses/30/lessons/42584) |
| 큐 | enqueue/dequeue O(1) | 들어온 순서대로 처리, 대기열 시뮬레이션, BFS | [BOJ 18258 큐 2](https://www.acmicpc.net/problem/18258)<br>[BOJ 2164 카드2](https://www.acmicpc.net/problem/2164)<br>[PG 기능개발](https://school.programmers.co.kr/learn/courses/30/lessons/42586)<br>[PG 프로세스](https://school.programmers.co.kr/learn/courses/30/lessons/42587)<br>[PG 다리를 지나는 트럭](https://school.programmers.co.kr/learn/courses/30/lessons/42583) |
| 덱 (Deque) | 양 끝 삽입/삭제 O(1) | 양쪽에서 넣고 빼기, **모노토닉 덱**(슬라이딩 윈도우 최솟값), 0-1 BFS | [BOJ 10866 덱](https://www.acmicpc.net/problem/10866)<br>[BOJ 5430 AC](https://www.acmicpc.net/problem/5430)<br>[BOJ 11003 최솟값 찾기](https://www.acmicpc.net/problem/11003) |
| 힙 / 우선순위 큐 | 삽입·삭제 O(log N)<br>조회 O(1) | 최댓값/최솟값을 반복해서 꺼내기, K번째 수, 실시간 중앙값, 작업 스케줄링, 다익스트라 | [BOJ 1927 최소 힙](https://www.acmicpc.net/problem/1927)<br>[BOJ 11279 최대 힙](https://www.acmicpc.net/problem/11279)<br>[BOJ 11286 절댓값 힙](https://www.acmicpc.net/problem/11286)<br>[BOJ 1655 가운데를 말해요](https://www.acmicpc.net/problem/1655)<br>[BOJ 1715 카드 정렬하기](https://www.acmicpc.net/problem/1715)<br>[PG 더 맵게](https://school.programmers.co.kr/learn/courses/30/lessons/42626)<br>[PG 디스크 컨트롤러](https://school.programmers.co.kr/learn/courses/30/lessons/42627)<br>[PG 이중우선순위큐](https://school.programmers.co.kr/learn/courses/30/lessons/42628) |
| 해시 (Map / Set) | 평균 O(1) | 빠른 존재 확인, 등장 횟수 세기, 중복 제거, 문자열 ↔ 번호 매핑 | [BOJ 1764 듣보잡](https://www.acmicpc.net/problem/1764)<br>[BOJ 1620 나는야 포켓몬 마스터 이다솜](https://www.acmicpc.net/problem/1620)<br>[PG 완주하지 못한 선수](https://school.programmers.co.kr/learn/courses/30/lessons/42576)<br>[PG 의상](https://school.programmers.co.kr/learn/courses/30/lessons/42578)<br>[PG 베스트앨범](https://school.programmers.co.kr/learn/courses/30/lessons/42579) |
| 트라이 (Trie) | 삽입·검색 O(L) | 접두사 검색, 자동완성, 한 문자열이 다른 문자열의 접두사인지 판별 | [BOJ 5052 전화번호 목록](https://www.acmicpc.net/problem/5052)<br>[BOJ 14425 문자열 집합](https://www.acmicpc.net/problem/14425)<br>[PG 전화번호 목록](https://school.programmers.co.kr/learn/courses/30/lessons/42577) |
| 유니온 파인드 | O(α(N)) ≈ O(1) | 두 원소가 같은 집합인지, 그룹 합치기, 무방향 그래프 사이클 판정, 크루스칼 | [BOJ 1717 집합의 표현](https://www.acmicpc.net/problem/1717)<br>[BOJ 1976 여행 가자](https://www.acmicpc.net/problem/1976)<br>[BOJ 4195 친구 네트워크](https://www.acmicpc.net/problem/4195)<br>[BOJ 20040 사이클 게임](https://www.acmicpc.net/problem/20040) |
| 세그먼트 트리 | 구축 O(N)<br>쿼리·갱신 O(log N) | 값이 바뀌는 배열에서 구간 합/최솟값/최댓값/곱 쿼리 | [BOJ 2042 구간 합 구하기](https://www.acmicpc.net/problem/2042)<br>[BOJ 10868 최솟값](https://www.acmicpc.net/problem/10868)<br>[BOJ 11505 구간 곱 구하기](https://www.acmicpc.net/problem/11505) |
| 느리게 갱신되는 세그먼트 트리 (Lazy Propagation) | 구간 갱신·쿼리 O(log N) | 구간 전체에 값 더하기 + 구간 합 쿼리 | [BOJ 10999 구간 합 구하기 2](https://www.acmicpc.net/problem/10999) |
| 펜윅 트리 (BIT) | 쿼리·갱신 O(log N) | 구간 합 + 점 갱신 (세그먼트 트리보다 구현이 짧음) | [BOJ 2042 구간 합 구하기](https://www.acmicpc.net/problem/2042) |
| 희소 배열 (Sparse Table) | 전처리 O(N log N)<br>쿼리 O(log N) | 정적 배열의 구간 최솟값(RMQ), 함수를 K번 적용한 결과, LCA | [BOJ 17435 합성함수와 쿼리](https://www.acmicpc.net/problem/17435) |
| Mo's 알고리즘 | O((N + Q)√N) | 갱신이 없는 구간 쿼리를 오프라인으로 처리 (구간 내 서로 다른 수의 개수 등) | [BOJ 13547 수열과 쿼리 5](https://www.acmicpc.net/problem/13547) |

### 3.7 그래프 탐색

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| DFS (깊이 우선 탐색) | O(V + E) | 연결 요소 개수, 경로 존재 여부, 모든 경로 탐색, 사이클 탐지, 백트래킹 | [BOJ 1260 DFS와 BFS](https://www.acmicpc.net/problem/1260)<br>[BOJ 2667 단지번호붙이기](https://www.acmicpc.net/problem/2667)<br>[BOJ 1012 유기농 배추](https://www.acmicpc.net/problem/1012)<br>[BOJ 11724 연결 요소의 개수](https://www.acmicpc.net/problem/11724)<br>[PG 타겟 넘버](https://school.programmers.co.kr/learn/courses/30/lessons/43165)<br>[PG 네트워크](https://school.programmers.co.kr/learn/courses/30/lessons/43162)<br>[PG 여행경로](https://school.programmers.co.kr/learn/courses/30/lessons/43164) |
| BFS (너비 우선 탐색) | O(V + E) | **가중치가 없는(모두 같은) 그래프의 최단 거리**, 최소 이동 횟수, 여러 시작점에서 동시에 퍼지는 현상, 상태 공간 탐색 | [BOJ 2178 미로 탐색](https://www.acmicpc.net/problem/2178)<br>[BOJ 7576 토마토](https://www.acmicpc.net/problem/7576)<br>[BOJ 1697 숨바꼭질](https://www.acmicpc.net/problem/1697)<br>[BOJ 2206 벽 부수고 이동하기](https://www.acmicpc.net/problem/2206)<br>[PG 게임 맵 최단거리](https://school.programmers.co.kr/learn/courses/30/lessons/1844)<br>[PG 단어 변환](https://school.programmers.co.kr/learn/courses/30/lessons/43163)<br>[PG 가장 먼 노드](https://school.programmers.co.kr/learn/courses/30/lessons/49189) |
| 이분 그래프 판별 | O(V + E) | 정점을 두 그룹으로 나눴을 때 같은 그룹끼리 간선이 없는지 (2-색칠) | [BOJ 1707 이분 그래프](https://www.acmicpc.net/problem/1707) |

### 3.8 최단 경로

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 다익스트라 | O(E log V) | **음수 간선이 없는** 그래프에서 한 정점 → 모든 정점 최단 거리 | [BOJ 1753 최단경로](https://www.acmicpc.net/problem/1753)<br>[BOJ 1916 최소비용 구하기](https://www.acmicpc.net/problem/1916)<br>[BOJ 11779 최소비용 구하기 2](https://www.acmicpc.net/problem/11779) |
| 0-1 BFS | O(V + E) | 간선 가중치가 0 또는 1뿐인 그래프의 최단 거리 (덱 사용) | [BOJ 13549 숨바꼭질 3](https://www.acmicpc.net/problem/13549) |
| 벨만-포드 | O(V·E) | **음수 간선**이 있는 최단 거리, **음수 사이클** 탐지 | [BOJ 11657 타임머신](https://www.acmicpc.net/problem/11657)<br>[BOJ 1865 웜홀](https://www.acmicpc.net/problem/1865) |
| 플로이드-워셜 | O(V³) | **모든 정점 쌍**의 최단 거리 (V ≤ 400 정도), 정점 간 도달 가능 여부 | [BOJ 11404 플로이드](https://www.acmicpc.net/problem/11404)<br>[BOJ 11403 경로 찾기](https://www.acmicpc.net/problem/11403)<br>[PG 순위](https://school.programmers.co.kr/learn/courses/30/lessons/49191) |

### 3.9 그래프 심화

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 크루스칼 (MST) | O(E log E) | 모든 정점을 최소 비용으로 연결 (간선이 적은 그래프에 유리) | [BOJ 1197 최소 스패닝 트리](https://www.acmicpc.net/problem/1197)<br>[BOJ 1922 네트워크 연결](https://www.acmicpc.net/problem/1922)<br>[PG 섬 연결하기](https://school.programmers.co.kr/learn/courses/30/lessons/42861) |
| 프림 (MST) | O(E log V) | 모든 정점을 최소 비용으로 연결 (간선이 많은 그래프에 유리) | [BOJ 1197 최소 스패닝 트리](https://www.acmicpc.net/problem/1197) |
| 위상 정렬 | O(V + E) | 선후 관계가 있는 작업의 순서 정하기, DAG 위에서의 DP (작업 완료 최소 시간) | [BOJ 2252 줄 세우기](https://www.acmicpc.net/problem/2252)<br>[BOJ 1766 문제집](https://www.acmicpc.net/problem/1766)<br>[BOJ 1005 ACM Craft](https://www.acmicpc.net/problem/1005) |
| SCC (타잔 / 코사라주) | O(V + E) | 서로 오갈 수 있는 정점 묶음 찾기, 2-SAT | [BOJ 2150 Strongly Connected Component](https://www.acmicpc.net/problem/2150)<br>[BOJ 11280 2-SAT - 3](https://www.acmicpc.net/problem/11280) |
| 단절점 / 단절선 | O(V + E) | 제거하면 그래프가 끊어지는 정점/간선 찾기 | [BOJ 11266 단절점](https://www.acmicpc.net/problem/11266)<br>[BOJ 11400 단절선](https://www.acmicpc.net/problem/11400) |
| 네트워크 플로우 | O(V·E²) (에드몬드-카프)<br>O(V²·E) (디닉) | 최대 유량, 최소 컷 | [BOJ 6086 최대 유량](https://www.acmicpc.net/problem/6086) |
| 이분 매칭 | O(V·E) | 두 그룹 사이의 최대 매칭 (사람 ↔ 작업 배정) | [BOJ 11375 열혈강호](https://www.acmicpc.net/problem/11375) |

### 3.10 트리

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 트리 순회 | O(N) | 부모 찾기, 서브트리 크기, 깊이 계산 | [BOJ 11725 트리의 부모 찾기](https://www.acmicpc.net/problem/11725)<br>[BOJ 15681 트리와 쿼리](https://www.acmicpc.net/problem/15681) |
| 트리의 지름 | O(N) (DFS 2번) | 트리에서 가장 먼 두 정점 사이의 거리 | [BOJ 1167 트리의 지름](https://www.acmicpc.net/problem/1167)<br>[BOJ 1967 트리의 지름](https://www.acmicpc.net/problem/1967) |
| 트리 DP | O(N) | 자식 서브트리의 결과를 합쳐 최적값 계산 (최소 정점 커버, 최대 독립 집합) | [BOJ 2533 사회망 서비스(SNS)](https://www.acmicpc.net/problem/2533)<br>[BOJ 1949 우수 마을](https://www.acmicpc.net/problem/1949) |
| LCA (최소 공통 조상) | 전처리 O(N log N)<br>쿼리 O(log N) | 두 정점의 공통 조상, 트리 위 두 정점 간 거리 | [BOJ 11437 LCA](https://www.acmicpc.net/problem/11437)<br>[BOJ 11438 LCA 2](https://www.acmicpc.net/problem/11438) |

### 3.11 다이나믹 프로그래밍 (DP)

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 1차원 DP | O(N) | 이전 몇 개 상태로 현재 상태가 결정되는 문제 (계단, 타일링, 경우의 수) | [BOJ 1463 1로 만들기](https://www.acmicpc.net/problem/1463)<br>[BOJ 9095 1, 2, 3 더하기](https://www.acmicpc.net/problem/9095)<br>[BOJ 2579 계단 오르기](https://www.acmicpc.net/problem/2579)<br>[BOJ 1149 RGB거리](https://www.acmicpc.net/problem/1149)<br>[PG N으로 표현](https://school.programmers.co.kr/learn/courses/30/lessons/42895)<br>[PG 도둑질](https://school.programmers.co.kr/learn/courses/30/lessons/42897) |
| 2차원 DP (격자/삼각형) | O(N·M) | 격자 경로의 수, 경로의 최대 합 | [BOJ 1932 정수 삼각형](https://www.acmicpc.net/problem/1932)<br>[BOJ 10844 쉬운 계단 수](https://www.acmicpc.net/problem/10844)<br>[PG 정수 삼각형](https://school.programmers.co.kr/learn/courses/30/lessons/43105)<br>[PG 등굣길](https://school.programmers.co.kr/learn/courses/30/lessons/42898) |
| LIS (최장 증가 부분 수열) | O(N²) / O(N log N) | 가장 긴 증가하는 부분 수열과 그 변형 | [BOJ 11053 가장 긴 증가하는 부분 수열](https://www.acmicpc.net/problem/11053)<br>[BOJ 12015 가장 긴 증가하는 부분 수열 2](https://www.acmicpc.net/problem/12015) |
| LCS / 편집 거리 | O(N·M) | 두 문자열의 최장 공통 부분 수열, 문자열 유사도 | [BOJ 9251 LCS](https://www.acmicpc.net/problem/9251)<br>[BOJ 15483 최소 편집](https://www.acmicpc.net/problem/15483) |
| 배낭 문제 (Knapsack) | O(N·W) | 무게(용량) 제한 안에서 가치 최대화 | [BOJ 12865 평범한 배낭](https://www.acmicpc.net/problem/12865) |
| 동전 DP | O(N·K) | 동전 조합의 경우의 수 / 최소 동전 개수 (그리디가 안 될 때) | [BOJ 2293 동전 1](https://www.acmicpc.net/problem/2293)<br>[BOJ 2294 동전 2](https://www.acmicpc.net/problem/2294) |
| 구간 DP | O(N³) | 구간을 나누어 합치는 비용의 최솟값 | [BOJ 11049 행렬 곱셈 순서](https://www.acmicpc.net/problem/11049)<br>[BOJ 11066 파일 합치기](https://www.acmicpc.net/problem/11066) |
| 비트마스크 DP | O(2ᴺ·N²) | 방문한 정점 집합을 상태로 표현 (외판원 순회) | [BOJ 2098 외판원 순회](https://www.acmicpc.net/problem/2098) |

> **DP 적용 조건**: ① 큰 문제를 작은 문제로 나눌 수 있고 (최적 부분 구조) ② 같은 작은 문제가 반복해서 등장한다 (중복 부분 문제).
> 시간복잡도 = **상태의 개수 × 상태 하나를 계산하는 비용**

### 3.12 분할 정복

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 분할 정복 거듭제곱 | O(log N) | 지수가 매우 큰 `aᴺ mod M` | [BOJ 1629 곱셈](https://www.acmicpc.net/problem/1629) |
| 행렬 거듭제곱 | O(K³·log N) | 선형 점화식의 N번째 항 (N이 10¹⁸처럼 매우 클 때) | [BOJ 10830 행렬 제곱](https://www.acmicpc.net/problem/10830)<br>[BOJ 11444 피보나치 수 6](https://www.acmicpc.net/problem/11444) |
| 영역 분할 (쿼드트리) | O(N² log N) | 2차원 영역을 재귀적으로 4등분(9등분)하며 처리 | [BOJ 2630 색종이 만들기](https://www.acmicpc.net/problem/2630)<br>[BOJ 1992 쿼드트리](https://www.acmicpc.net/problem/1992)<br>[BOJ 1780 종이의 개수](https://www.acmicpc.net/problem/1780) |

### 3.13 수학 · 정수론

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| 소수 판별 | O(√N) | 수 하나가 소수인지 판별 | [BOJ 1978 소수 찾기](https://www.acmicpc.net/problem/1978)<br>[PG 소수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/42839) |
| 에라토스테네스의 체 | O(N log log N) | 범위 안의 모든 소수 구하기 | [BOJ 1929 소수 구하기](https://www.acmicpc.net/problem/1929)<br>[BOJ 6588 골드바흐의 추측](https://www.acmicpc.net/problem/6588) |
| 유클리드 호제법 | O(log min(a, b)) | 최대공약수, 최소공배수, 분수 약분 | [BOJ 2609 최대공약수와 최소공배수](https://www.acmicpc.net/problem/2609)<br>[BOJ 1735 분수 합](https://www.acmicpc.net/problem/1735) |
| 이항 계수 (파스칼 삼각형) | O(N·K) | N이 작을 때 nCr | [BOJ 11050 이항 계수 1](https://www.acmicpc.net/problem/11050)<br>[BOJ 11051 이항 계수 2](https://www.acmicpc.net/problem/11051) |
| 모듈러 역원 (페르마 소정리) | O(N + log P) | N이 클 때 `nCr mod P` (P는 소수) | [BOJ 11401 이항 계수 3](https://www.acmicpc.net/problem/11401) |

### 3.14 문자열

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| KMP | O(N + M) | 긴 문자열에서 패턴의 모든 등장 위치, 문자열의 반복 주기 | [BOJ 1786 찾기](https://www.acmicpc.net/problem/1786)<br>[BOJ 1305 광고](https://www.acmicpc.net/problem/1305) |
| 아호-코라식 | O(N + ΣM) | 여러 개의 패턴을 한 번에 검색 | [BOJ 9250 문자열 집합 판별](https://www.acmicpc.net/problem/9250) |
| 매내처 (Manacher) | O(N) | 모든 위치를 중심으로 하는 최장 팰린드롬 | [BOJ 13275 가장 긴 팰린드롬 부분 문자열](https://www.acmicpc.net/problem/13275) |
| 접미사 배열 + LCP | O(N log N) | 반복되는 가장 긴 부분 문자열, 서로 다른 부분 문자열의 개수 | [BOJ 9248 Suffix Array](https://www.acmicpc.net/problem/9248) |

### 3.15 기하

| 알고리즘 | 시간복잡도 | 해결할 수 있는 문제 | 대표 문제 |
| --- | --- | --- | --- |
| CCW (외적) | O(1) | 세 점의 방향(시계/반시계/일직선), 선분 교차 판정 | [BOJ 11758 CCW](https://www.acmicpc.net/problem/11758)<br>[BOJ 17387 선분 교차 2](https://www.acmicpc.net/problem/17387) |
| 신발끈 공식 | O(N) | 다각형의 넓이 | [BOJ 2166 다각형의 면적](https://www.acmicpc.net/problem/2166) |
| 볼록 껍질 (Convex Hull) | O(N log N) | 점들을 모두 감싸는 가장 작은 볼록 다각형 | [BOJ 1708 볼록 껍질](https://www.acmicpc.net/problem/1708) |
| 스위핑 | O(N log N) | 정렬 후 한 방향으로 훑으며 처리 (겹치는 선분 길이 등) | [BOJ 2170 선 긋기](https://www.acmicpc.net/problem/2170) |

---

## 4. 문제 키워드로 알고리즘 찾기

### 문제 접근 순서
1. **입력 크기 N과 시간 제한**을 보고 허용되는 시간복잡도를 계산한다. ([1.5 표](#15-입력-크기n별-허용-시간복잡도) 참고)
2. 해당 시간복잡도로 가능한 **알고리즘 후보**를 추린다. ([2장](#2-시간복잡도별-알고리즘) 참고)
3. 문제의 **키워드**로 후보를 좁힌다. (아래 표 참고)
4. 엣지 케이스(최솟값/최댓값 입력, 오버플로우, 빈 입력)를 확인한다.

### 키워드 → 알고리즘

| 문제에 이런 표현이 나오면 | 떠올릴 알고리즘 |
| --- | --- |
| 모든 경우의 수, N이 매우 작음 (N ≤ 10~20) | 완전 탐색, 백트래킹, 비트마스크 |
| 매 순간 가장 좋은 것을 선택, 정렬하면 보이는 규칙 | 그리디 |
| 정렬된 배열에서 찾기, N이 10⁹ 이상 | 이분 탐색 |
| ~의 최댓값의 최소화 / 최솟값의 최대화 | 파라메트릭 서치 |
| 연속된 부분 배열(구간)의 합 | 누적합, 투 포인터, 슬라이딩 윈도우 |
| 구간 합/최솟값 + 값 변경이 반복됨 | 세그먼트 트리, 펜윅 트리 |
| 괄호, 가장 가까운 큰(작은) 수 | 스택 (모노토닉 스택) |
| 최댓값/최솟값을 반복해서 꺼냄, K번째로 큰 수 | 힙 (우선순위 큐) |
| 빠르게 존재 여부 확인, 개수 세기 | 해시 (Map / Set) |
| 접두사, 자동완성 | 트라이 |
| 같은 그룹인지, 그룹 합치기, 사이클 여부 | 유니온 파인드 |
| 최소 이동 횟수, 최단 거리 (가중치 없음) | BFS |
| 최소 비용, 최단 거리 (가중치 양수) | 다익스트라 |
| 음수 가중치, 음수 사이클 | 벨만-포드 |
| 모든 지점 간 최단 거리, N ≤ 400 | 플로이드-워셜 |
| 모든 정점을 최소 비용으로 연결 | 최소 스패닝 트리 (크루스칼, 프림) |
| 선후 관계, 작업 순서, 선수 과목 | 위상 정렬 |
| 연결된 영역의 개수, 섬의 개수 | DFS / BFS |
| 경우의 수를 `10⁹ + 7`로 나눈 나머지 | DP, 조합론 |
| 작은 문제의 답으로 큰 문제의 답을 구함 | DP |
| 두 문자열의 공통 부분, 문자열 변환 최소 횟수 | LCS, 편집 거리 (DP) |
| 무게 제한, 가치 최대화 | 배낭 문제 (DP) |
| N이 10¹⁸처럼 매우 큰 점화식 / 거듭제곱 | 분할 정복 거듭제곱, 행렬 거듭제곱 |
| 소수, 약수 | 소수 판별 O(√N), 에라토스테네스의 체 |
| 문자열에서 패턴 찾기 | KMP, 라빈-카프 |
| 트리에서 두 노드의 공통 조상, 노드 간 거리 | LCA |
