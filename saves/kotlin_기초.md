---
layout: single
title:  "Kotlin 기초"
categories: study
---

# Kotlin 설명
Kotlin은 안드로이드 및 웹 개발에서 자바를 대체할 목적으로 개발된 언어입니다.
Kotlin은 자바의 몇몇 약점을 개선하고 자바 가상머신과는 호환될 수 있게 만들어져 따라서 Kotlin은 기존에 자바로 개발이 가능했던 웹서비스, 안드로이드 개발 등에 이용됩니다.

# Kotlin 기초   
기본적으로 Kotlin은 문장에 끝에 세미콜론(;)을 사용하지 않습니다.   
Kotlin에서 클래스 표기는 파스칼 표기법 ex) MyClass   
Kotlin에서 함수나 변수의 표기는 카멜 표기법 ex) myFunction   
## 주석   
// <- 한 줄 주석
```
//주석입니다.
```
/**/ <- 여러 줄 주석
```
/* 주석입니다.
주석입니다.
주석입니다. */
```
## 자료형과 변수   
Kotlin은 var이나 val로 선언하는 2가지 방법이 있는데    
var은 언제든지 읽기, 쓰기가 가능한 일반적인 변수이고   
val은 선언시 초기화만 가능한 변수로 중간에 값을 바꿀 수 없습니다.   
runtime시 변경되지 말아야 할 값은 안전하게 val로 선언하는게 일반적입니다.   

변수는 클래스 내에 있으면 속성(property)라고 불리고 이 외에는 로컬 변수(Local variable)이라고 불립니다.   
변수 선언 시 초기화를 시키면서 자동적으로 자료형을 정해줄 수도 있지만 초기화를 하지 않고 자료형을 지정해주는 방법도 존재합니다.   
밑의 방법이 그 예시로   
```
var a = 10 //초기화를 통한 변수형 지정
var b : Int // 변수형을 int로 갖게 함
b = 10 // OK
b = "Hello, World!" // 오류
var c : Int? // null을 허용하는 변수
```
위에서 var c : Int?를 작성했는데 Kotlin은 기본적으로 변수에 null값이 할당 되는 것을 막습니다. 이러한 특징으로 특정 변수에 null을 허용하게 해주고 싶다면 자료형 뒤에 ?를 붙여서 사용하게 됩니다.   

### Kotlin 기본 자료형
자바와 거의 동일한 기본 자료형입니다.   
자료형을 굳이 명시해주지 않아도 Kotlin이 자료형을 자동으로 추론해줍니다.   
#### 정수형   
정수형의 리터럴은 10진수, 16진수, 2진수로 표기할 수 있습니다. (8진수 지원 X)
```
byte <- 8bits
short <- 16bits
int <- 32bits
Long <- 64bits

var intValue : Int = 16
var longValue : Long = 16L // long 자료형 변수는 L을 붙임
var intValueHex : Int = 0x10 // 16진수는 앞에 0x를 붙임
var intValueBin : Int = 0b10000 // 2진수는 앞에 0b를 붙임
```
#### 실수형   
Kotlin에서 실수형 기본 자료형은 Double입니다.   
```
float <- 32bits
double <- 64bits

var doubleValue : Double= 0.145
var doubleValue : Double = 0.145e10 // 필요시 e를 이용한 지수 표기법
var floatValue : Float = 0.145f // float 자료형 변수는 f를 붙임
```
#### Char형
Kotlin은 기본적으로 UTF-16 BE로 관리하여 글자 하나하나가 2bytes 메모리 공간을 갖고 한글이 깨지지 않습니다.
```
var charValue : Char = 'a'
var koreanCharValue : Char = '가'
```
#### Boolean형
```
var tureValue : Boolean = ture
var falseValue : Boolean = false
```
#### 문자열
```
var stringValue : String = "Hello, World!"
var multiLineStringValue : String = """Hello,
World!""" // 여러 줄의 문자열
```
### 형변환
정수형, 실수형, char형에 대해서만 형변환이 가능합니다.   
Kotlin은 명시적 형변환만 지원하고 암시적 형변환은 지원하지 않습니다.
```
toByte()
toShort()
toInt()
toLong()
toFloat()
toDouble()
toChar()

ex)
var a : Int = 1234
var b : Double = a.toDouble()
```
## 배열
Kotlin에서 배열 선언은 arrayOf함수를 통해 이루어집니다.   
한번 배열 크기를 정하면 바꿀 수 없습니다.   
```
var arrInt = arrayOf(0, 1, 2, 3, 4)
var arrIntNull = arrayOfNulls<Int>(5) // NULL값으로 채워진 배열로 이때에는 <자료형>이 필요
var arrInt : Array<Int> = arrayOf(0, 1, 2, 3, 4) // 다른 변수들처럼 자료형을 명시해주는 방법도 가능

arrInt[2] = 10 // arrInt = (0, 1, 10, 3, 4)
println(arrInt[4]) // 4 출력
```

## 함수
함수는 특정한 연산 혹은 결과값을 연산하는데 사용됩니다.   
main()과 println()같은 것들도 모두 함수입니다.   
```
fun add(num1 : Int, num2 : Int) : Int{
  return num1 + num2
}

fun anyFun(num : Any){} // Any는 어떤 자료형이든 상관없이 호환되는 Kotlin의 최상위 자료형
```
Kotlin에서 위처럼 작성되어 매개 변수의 자료형 표기를 다른 언어와 다르게 하고 반환받는 자료형을 함수 옆에 표기해주어야 합니다.   
반환값이 없다면 반환받는 자료형을 생략해줘도 됩니다.
### 단일 표현식 함수
Kotlin은 단순한 계산과 같은 일만 하는 함수를 편하게 표현하기 위해 단일 표현식 함수를 사용할 수 있습니다.   
```
fun add(num1 : Int, num2 : Int) = num1 + num2 // 이때는 반환형의 타입 추론이 가능하여 반환받는 자료형을 써주지 않아도 됨
```

## 조건문과 비교 연산자
### if-else
```
var a = 11
var b = 10

if(a == b){
  println("a와 b는 같습니다.")
} else {
  println("a와 b는 다릅니다.")
}
```
### when
when은 다른 언어의 switch와 비슷한 역할을 합니다.
```
when(something){
  1 -> println("1입니다.")
  "Hi" -> println("Hi!")
  is Long -> println("Long형 변수입니다.")
  !is Long -> println("Long형 변수가 아닙니다.")
  else -> println("아무것도 해당되지 않습니다.") // switch문의 default와 같음
### 비교 연산자
