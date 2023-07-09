---
published : true
title: "[Divide and Conquer] 분할정복 - not completed!"
excerpt: "순환관계식과 마스터정리/병합정렬/퀵정렬/이진트리/피보나치"
categories: [Algorithm]
tags: [algorithm, study, lecture]
toc: true
toc_sticky: true
---

## 병합정렬

```python
def merge_sort(li, left, right):
    if(left < right):
        mid = (left + right) // 2
        merge_sort(li, left, mid)
        merge_sort(li, mid+1, right)
        merge(li, left, mid, right)

def merge(li, left, mid, right):
    k = left
    i = left
    j = left
    while i <= mid and j <= right:
        if A[i] <= A[j]:
```