---
layout: single
title:  "Kotlin 기초(2)"
categories: study
---

## 클래스
클래스는 속성과 함수로 이루어져 있고 키워드는 class를 사용합니다.
기본 자료형(Int, Double...)도 코틀린 내부에서는 모두 class로 만들어져 있습니다.   
클래스는 인스턴스(객체)를 생성하기 위해 사용됩니다.   

### 클래스, 객체 생성
```
// 클래스 생성
class Person1(val name:String, val age:Int) // 속성만 가지는 클래스

class Person2(val name:String, val age:Int){
  fun introduce(){
    println("안녕하세요 저는 ${age}살인 ${name}입니다.")
  }
}

// 객체 생성
val a = Person1("김호창", 30)
val b = Person2("박찬호", 25)
b.introduce() // 자기소개 출력
```

### 클래스 생성자
위의 코드에서 Person1의 (val name:String, val age:Int)부분은 Person1 클래스의 생성자로 볼 수 있습니다. 이는 클래스의 속성들을 선언함과 동시에 생성자 역시 선언하는 방법입니다.   
생성자란 새로운 객체를 만들기 위해 호출하는 특수한 함수입니다.   
생성자를 호출하면 클래스의 객체를 만들어 반환받을 수 있게 됩니다.   
생성자는 객체의 속성 초기화뿐만 아닌 객체 생성시 필요 구문을 수행하는 역할도 합니다.
생성자 키워드는 init을 사용합니다.   

생성자를 사용 시 모든 속성을 수동으로 초기화하는 것은 비효율적이기에

### 생성자 예시
```
class Person(val name:String, val age:Int){
  init(){
    println("안녕하세요 저는 ${this.age}살인 ${this.name}입니다.")
  }
}
