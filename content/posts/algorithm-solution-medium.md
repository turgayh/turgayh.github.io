---
title: "Algorithm Solution - Medium"
date: 2020-07-04T18:59:43+03:00
draft: false
aliases:
  - /posts/2020/07/algoritma-cozumu---orta/
tags:
  - Algorithms
  - Dynamic Programming
  - Optimization
---

## Problem

- Let N be an integer and L a permutation of the numbers from 1 to N.
- Example: N = 4 and L = [4,2,1,3]
- We want to sort L, but under a specific rule:
- A chosen element can only be moved **either to the far left or to the far right**.
- Find the minimum number of moves needed to sort the array.

- Examples
  - `N = 2   L = [1,2]   result 0   (already sorted)`
  - `N = 6   L = [5,2,1,3,4,6]   result 3`

## Solution

- Find the longest set of consecutive increasing values (increasing by exactly 1) that appear in order.
- In the example L = [5,2,1,3,4,6] this is `2, 3, 4`. **Call its length S.**
- If we move every other element to the left or right, 2, 3, 4 end up next to each other.
- So the array can be sorted in `N - S` moves (this can be proven by induction).

- To find the length S of the longest consecutive increasing sequence, keep an array `A` where `A[v]` is the length of the consecutive run ending at value `v`:
  - `N`: length of the array
  - `L`: the array
  - `A`: lengths of the longest consecutive increasing runs, initialised as `A = [0] * (N + 1)`
  - Walking through `L` left to right: `A[L[i]] = A[L[i] - 1] + 1`
  - Then `S = max(A)`.

## Constraints

- 1 <= N <= 10^6

## Code - Python

```python
N = int(input())
L = list(map(int, input().split()))  # read space-separated values from a single line

A = [0] * (N + 1)

for i in range(N):
    A[L[i]] = A[L[i] - 1] + 1
print(N - max(A))
```

## Code - C++

```cpp
#include<bits/stdc++.h>

using namespace std;

const int N = 1000005;

int arr[N], n;
int lsis[N];

int main() {

    cin >> n;

    for(int i = 1 ; i <= n ; i++ ){
        cin >> arr[i];
    }

    for(int i = 1 ; i <= n ; i++) {
        lsis[arr[i]] = lsis[arr[i]-1]+1;
    }

    cout << n - *max_element(lsis+1,lsis+1+n) << endl;
}
```
