---
title: "[BinarySearch] 이분탐색"
excerpt: "[Binary Search] "
categories: [Algorithm]
tags: [cpp, algorithm, codingtest, study, baekjoon]
toc: true
toc_sticky: true
---

# 이분탐색/이진탐색

## 이분 탐색이란?

![fail to bring](/assets/Image/cppStudy/algorithm/binarysearch.gif)

+ `이분 탐색` 알고리즘은 리스트에서 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하는 방법이다.  
+ 리스트 혹은 배열 내부의 **데이터가 정렬되어 있어야** 사용할 수 있는 알고리즘이다.  
+ 변수 3개`start, end, mid`를 사용하여 탐색한다. 찾으려는 데이터와 중간점 위치의 데이터의 비교를 반복하며 원하는 데이터를 찾는다.  

+ **재귀적인 방법, 반복문, STL**을 이용해 이분 탐색을 실행할 수 있다. 

> O(log N)  

## 변수

`int low, high` : 검사 범위의 시작점과 끝점의 인덱스를 가리키기 위한 변수로, **low는 시작점(start), high는 끝점(end)**을 가리킨다.  
```cpp
    vector<int> nums;
    int low = 0;
    int high = nums.size() - 1;
```

`int mid` : 검사 범위 내에서 실질적으로 검사하는 인덱스를 가리키는 변수로, **검사 범위의 중간에 있는 값**이다.  
```cpp        
    int mid = (low + high) / 2;
```

## 이분 탐색 단계

**리스트 혹은 배열은 무조건 정렬된 상태이어야 한다**

1. 검사 범위에서 중간값`mid`을 선택하여 찾고있는 값이 맞는지 확인한다.  

2. 만약 찾는 값이 맞으면 해당 값 리턴한다.
3. 만약 찾는 값이 중간 값보다 크면 검사 범위를 큰 쪽으로 다시 탐색한다. 
    `mid > target` 이면 `high = mid - 1`  

4. 반대로 찾는 값이 중간 값보다 작으면 검사 범위를 작은 쪽으로 다시 탐색한다.  
    `mid < target` 이면 `low = mid + 1`  

5. 앞의 과정을 원하는 값을 찾을 때까지 반복한다.  
6. 반복하다가 더 이상 찾지 못하고 검사할 곳도 없으면`low > high` 돌아간다.

## 이분 탐색 과정

정렬된 배열 `2 5 8 12 16 23 38 56 72 91`이 있다고 해보자.  
찾는 수는 **8**.  
low = 0, high = 9

![fail to bring](/assets/Image/cppStudy/algorithm/binarysearch2.jpg)
![fail to bring](/assets/Image/cppStudy/algorithm/binarysearch3.jpg)
![fail to bring](/assets/Image/cppStudy/algorithm/binarysearch4.jpg)

## 이분 탐색 구현

아래 코드는 배열에 찾는 값이 있는지 없는지만 확인하기 위해 리턴형을 부울형으로 했지만 필요에 따라서 인덱스 혹은 값을 리턴하는 형으로 바꾸면 될 것 같다.   

### 반복문

```cpp
bool binary_search(vector<int> arr, int target) {
	int low = 0;
	int high = arr.size() - 1;
	while (low <= high) {
		int mid = (low + high) / 2;
		if (target == arr[mid]) return true;
		if (target < arr[mid]) high = mid - 1;
		else if (target > arr[mid]) low = mid + 1;
	}
	return false;
}
```
low가 high보다 커지면 배열 안에 찾는 값이 없다는 뜻이므로 low가 high보다 작을 동안만 검사한다.   

### 재귀

```cpp
bool binary_search(vector<int> arr, int low, int high, int target) {
	if (low > high) return false;
	int mid = (low + high) / 2;

	if (arr[mid] == target) return true;
	if (arr[mid] > target) return binary_search(arr, low, mid - 1, target);
	else return binary_search(arr, mid + 1, high, target);
}
```

### STL

C++ STL에서 지원하는 binary_search()를 사용할 수도 있다. <small>STL 짱💞</small>
![fail to bring](/assets/Image/cppStudy/algorithm/binarysearch5.png)  

기본형  
```cpp
template <class ForwardIterator, class T>
  bool binary_search (ForwardIterator first, ForwardIterator last, const T& val)
```

STL이 지원하는 이분탐색 메소드는 세 개의 매개변수`Start`, `End`, `Target`를 받는다.  
리턴형은 bool 부울형이다.

```cpp
    vector<int> nums;
    int target = 8;
    bool isFound = binary_search(nums.begin(), nums.end(), target);
```


### 참고한 블로그

<https://velog.io/@kimdukbae/%EC%9D%B4%EB%B6%84-%ED%83%90%EC%83%89-%EC%9D%B4%EC%A7%84-%ED%83%90%EC%83%89-Binary-Search>
<https://m42-orion.tistory.com/69>