---
layout: single
title:  "Lower_bound, Upper_bound"
categories: algorithm
tag: [baekjoon, c++]
toc: true
---

# Lower_bound, Upper_bound 개념
Lower_bound는 key 이상의 값이 나오는 최초 위치를 구하는 탐색 방법이고   
Upper_bound는 key값을 초과하는 최초 위치를 구하는 탐색 방법입니다.   
Lower_bound와 Upper_bound는 이진 탐색을 기본으로 하기에 배열이 정렬이 되어 있어야 합니다.   

# Lower_bound
Lower_bound의 예시를 들면
* 입력
```
  8 <- 배열 크기
  1 2 4 6 6 7 10 10 <- 배열
  6 <- 원하는 값
```
* 출력
```
  3
```   
만약 없는 값에 대해 Lower_bound를 하게 되면
* 입력
```
  8 <- 배열 크기
  1 2 4 6 6 7 10 10 <- 배열
  8 <- 원하는 값
```
* 출력
```
  6
```
8 이상의 값이 index 6에서 최초로 나오기에 리턴 값이 6이 됩니다.   
만약 배열에서 가장 큰 값을 초과하는 값을 원하게 되면 배열의 크기를 리턴 받게 됩니다.
* 입력
```
  8 <- 배열 크기
  1 2 4 6 6 7 10 10 <- 배열
  20 <- 원하는 값
```
* 출력
```
  8
```

## Lower_bound 과정 시각화
위의 과정 중 1번째 과정을 시각화하겠습니다.   
처음 start = 0, end = 8, mid = (8 + 0) / 2를 갖습니다.   
![캡처](https://github.com/124coding/124coding.github.io/assets/114299892/26047666-e414-45e7-a599-13cac63bf846)   

6은 key값과 같기에 end = mid가 됩니다.   
start = 0, end = 4, mid = (4 + 0) / 2   
![캡처](https://github.com/124coding/124coding.github.io/assets/114299892/d8c480c0-2b4f-4b9d-9aef-ad03b9f80520)   

4는 key값 보다 작기에 start = mid + 1이 됩니다.   
start = 3, end = 4, mid = (3 + 4) / 2   
![캡처](https://github.com/124coding/124coding.github.io/assets/114299892/78e5b4ad-3495-4fa6-9a7f-07a8cae9a1f7)   

6은 key값과 같기에 end = mid가 됩니다.   
이렇게 되면 start와 end가 3으로 같아지기에 알고리즘이 끝나고 end를 리턴 받게 됩니다.   

## Lower_bound코드
```c
int lower_bound(int* arr, int key, int size) {
	int s = 0, e = size, m;

	while (e > s) {
		m = (s + e) / 2;
		if (arr[m] < key) s = m + 1;
		else e = m;
	}
	return e;
}
```   

# Upper_bound   
Upper_bound의 예시를 들면   
* 입력
```
  8 <- 배열 크기
  1 2 4 6 6 7 10 10 <- 배열
  6 <- 원하는 값
```
* 출력
```
  5
```
값이 존재하는 경우를 제외하면 Lower_bound와 같은 리턴을 받습니다.   

## Upper_bound 과정 시각화
위의 과정을 시각화하면   
처음 start = 0, end = 8, mid = (8 + 0) / 2를 갖습니다.   
![캡처](https://github.com/124coding/124coding.github.io/assets/114299892/26047666-e414-45e7-a599-13cac63bf846)   

6은 key값과 같기에 start = mid + 1이 됩니다.   
start = 5, end = 8, mid = (5 + 8) / 2가 됩니다.   
![캡처](https://github.com/124coding/124coding.github.io/assets/114299892/758e2e15-bdcb-4f0c-aacb-d6689643ad2b)   

10은 key값보다 크기에 end = mid가 됩니다.   
start = 5, end = 6, mid = (5 + 6) / 2가 됩니다.   
![캡처](https://github.com/124coding/124coding.github.io/assets/114299892/74d7ced1-d86c-48f9-a6d8-907b6f5b4bf0)   

7은 key값보다 크기에 end = mid가 됩니다.   
이렇게 되면 start와 end가 5로 같아지기에 알고리즘이 끝나고 end를 리턴 받게 됩니다.   

## Upper_bound코드
```c
int upper_bound(int* arr, int key, int size) {
	int s = 0, e = size, m;

	while (e > s) {
		m = (s + e) / 2;
		if (arr[m] <= key) s = m + 1;
		else e = m;
	}
	
	return e;
}
```
