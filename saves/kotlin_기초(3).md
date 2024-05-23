---
layout: single
title:  "Kotlin 기초(3)"
categories: study
---

# 리스트
**리스트**는 데이터를 모아 관리하는 **Collection** 클래스를 상속받는 다양한 sub class 중 가장 단순한 형태로 여러 개의 데이터를 원하는 순서로 넣어 관리하는 형태입니다.   
리스트에는 2가지가 존재하는데   
1번째 그냥 리스트로 생성시에 넣은 객체를 대체, 추가, 삭제가 불가하고 listOf(...)을 이용하여 생성합니다.   
2번째 Mutable리스트 생성시에 넣은 객체를 대체, 추가, 삭제가 가능하고 mutableListOf(...)을 이용하여 생성합니다.   
이는 상황에 맞춰 선택하는게 좋습니다.   

Mutable리스트의 메서드   
1. 요소 추가 add(데이터), add(인덱스, 데이터)   
2. 요소 삭제 remove(데이터), removeAt(인덱스)   
3. 무작위 섞기 shuffle()   
4. 정렬 sort() (오름차순)   
5. 요소 대체 list[인덱스] = ...   

*리스트 예시*
```kotlin
val l = listOf(1, 2, 3)
for(i in l){
  print("$i, ")
}

val ml = mutableListOf(5, 3, 1)
println(ml)

ml.add(1, 4)
ml.removeAt(0)
ml[2] = 2
ml.shuffle()
ml.sort()
```

# 문자열을 다루는 방법들
1. 문자열 길이 알기 = length   
2. 문자열 소문자 변환 = toLowerCase()   
3. 문자열 대문자 변환 = toUpperCase()   
4. 문자열을 나눠서 배열에 담기 = split("문자열") (이때 특이하게 일반 문자열을 넣어도 동작합니다.)   
5. 문자열 배열 하나로 합치기 = joinToString(), joinToString("문자열") -> 문자열을 인자로 넣으면 문자열을 사이에 넣어서 결합해줍니다.   
6. 문자열 일부 사용 = substring(2..5) -> 2 ~ 5 까지의 인덱스의 문자열을 사용합니다.   
7. 문자열 비어있는지 여부 파악 후 bool값 받기 = isNullOrEmpty(), isNullOrBlank()   
   이때 두 메서드의 차이는 공백문자(" ")만 있는가에 대해 isNullOrEmpty는 false이고 isNullOrBlank()는 true를 반환합니다.   
   공백문자는 Space뿐 아닌 눈에 보이지 않는 Tab, Line Feed 등등이 포함됩니다.   
8. 지정 문자열로 시작하는지 여부 파악 = startsWith("문자열")   
9. 지정 문자열로 끝나는지 여부 파악 = endsWith("문자열")   
10. 지정 문자열이 들어갔는지 여부 파악 = contains("문자열")

*문자열 메서드 예시*
```kotlin
val s = "Hello, Im student"
val emptyString = ""
val blankString = " "

println(s.length) // 17 출력
println(s.toLowerCase()) // hello, im student 출력
println(s.toUpperCase()) // HELLO, IM STUDENT 출력
println(s.split(",")) // [Hello,  Im student] 출력
println(s.substring(2..5)) // llo, 출력
println(s.isNullOrEmpty()) // false 출력
println(s.isNullOrBlank()) // false 출력
println(emptyString.isNullOrEmpty()) // true 출력
println(emptyString.isNullOrBlank()) // true 출력
println(blankString.isNullOrEmpty()) // false 출력
println(blankString.isNullOrBlank()) // true 출력
println(s.startsWith("He")) // true 출력
println(s.endsWith("ent")) // true 출력
println(s.contains("Im")) // true 출력
```

# null 처리 방법
1. 객체?.(속성 or 함수) 이는 null safe 연산자로 객체가 null이라면 .뒤를 실행하지 않는 것입니다.   
2. 객체?: 이는 elvis 연산자로 객체가 null이라면 .뒤의 것으로 대체하는 것입니다.   
3. 객체!!.(속성 or 함수) 이는 non-null assertion operator 연산자로 null 여부를 컴파일시 확인하지 않는 의도적으로 방치하는 연산자입니다.   
이러한 처리 방법은 null인지 체크하기 위해 if문을 이용하는 것보다 훨씬 효과적이고 편리합니다.

*null 처리 예시*
```kotlin
var a:String? = null

println(a?.toUpperCase()) // null 출력 (a가 null이기에 null임을 알림)
println(a?:"default".toUpperCase()) // DEFAULT 출력 (default가 null을 대신함)
println(a!!.toUpperCase()) // 오류 발생

// 스코프 함수랑 사용하면 더욱 편리

a?.run{
  println(toUpperCase())
  println(toLowerCase())
} // a가 null이기에 run 실행 x
```
# 변수의 동일성 체크
동일성은 2가지 개념이 존재하는데   
1번째 내용의 동일성 이는 변수 a, b가 있을시 둘 다 1이라는 값을 가진다면 이는 동일하다고 판단하는 것으로 ==을 이용합니다.   
2번째 객체의 동일성 이는 서로 다른 변수가 메모리 상에 같은 객체를 가리키면 동일하다고 판단하는 것으로 ===을 이용합니다.   
이 중 내용의 동일성은 자동으로 판단되는 것이 아닌 코틀린의 모든 클래스가 내부적으로 상속받는 최상위 클래스 Any의 equals() 함수가 반환하는 Boolean값으로 판단합니다.   
개발자가 커스텀 class를 만들때는 

#### 본 글은 유튜브 [**디모의 코틀린 강좌**](https://www.youtube.com/watch?v=8RIsukgeUVw&list=PLQdnHjXZyYadiw5aV3p6DwUdXV2bZuhlN)를 참고하여 작성하였습니다.
