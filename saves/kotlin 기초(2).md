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
```kotlin
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
위의 코드에서 Person1의 (val name:String, val age:Int)부분은 Person1 클래스의 생성자로 볼 수 있습니다. 
이는 클래스의 속성들을 선언함과 동시에 생성자 역시 선언하는 방법입니다.   
생성자란 새로운 객체를 만들기 위해 호출하는 특수한 함수입니다.   
생성자를 호출하면 클래스의 객체를 만들어 반환받을 수 있게 됩니다.   
생성자는 객체의 속성 초기화뿐만 아닌 객체 생성시 필요 구문을 수행하는 역할도 합니다.   
생성자 키워드는 init을 사용합니다. 이는 기본 생성자의 역할을 합니다.   

생성자를 사용 시 모든 속성을 항상 수동으로 초기화하는 것은 비효율적일 수 있기에 constructor 키워드를 이용하여 보조 생성자를 사용할 수 있습니다.
보조 생성자는 반드시 기본 생성자를 통해 속성을 초기화해줘야 합니다.   

### 생성자 예시
```kotlin
class Person1(val name:String, val age:Int){
  init{
    println("안녕하세요 저는 ${this.age}살인 ${this.name}입니다.")
  }
}

//보조 생성자
class Person2(val name:String, val age:Int){
  init{
    println("안녕하세요 저는 ${this.age}살인 ${this.name}입니다.")
  }

  constructor(name:String) : this(name, 25){ // this를 이용하여 기본 생성자 호출
    println("보조 생성자 사용")
  }
}
```

### 클래스 상속
상속이 필요한 경우는 2가지가 존재하는데   
1번째는 이미 존재하는 클래스를 확장하여 새로운 속성이나 함수를 추가한 클래스를 만들고 싶을 때와   
2번째는 클래스간 중복되는 속성이나 함수를 더 수월하게 관리하기 위함입니다.   
속성과 함수를 물려주는 쪽을 super class, 물려받는 쪽을 sub class라고 합니다.   
kotlin은 기본적으로 상속 불가이기에 클래스 앞에 open 키워드를 붙여줍니다.   

상속은 2가지 규칙이 존재하는데   
1번째 sub class는 super class에 존재하는 속성과 같은 이름의 속성을 가질 수 없고   
2번째 sub class는 반드시 super class의 생성자를 호출해야 합니다.   

```kotlin
open class Animal(val name:String, var age:Int, val type:String){
  fun introduce(){
    println("이 동물의 이름은 ${this.name}이고 나이는 ${this.age}입니다.")
  }
}

class Dog(name:String, age:Int) : Animal(name, age, "개"){
  fun bark(){
    println("멍멍")
  }
}

// sub class의 매개 변수를 추가하는 예제
open class Person(val name:String)

class Student(name:String, val studentId:Int) : Person(name){
  fun introduce(){
    println("학번: ${studentId}")
  }
}

// Dog, Student 객체 생성
var d = Dog("뽀삐", 2)
var s = Student("김호창", 20242024)
```

### 오버라이딩
super class의 함수와 같은 이름과 형태의 함수를 sub class에서 만들 수 없었는데 super class가 허용한다면 다시 구현할 수 있는데 이를 오버라이딩이라고 합니다.   
오버라이딩할 함수를 open 키워드로 선언하고 sub class에서 override 키워드를 이용하여 오버라이드할 함수를 다시 구현할 수 있습니다.   

### 오버라이딩 예시
```kotlin
open class Animal{
  open fun eat(){
    println("음식 먹기")
  }
}

class Dog : Animal(){
  override fun eat(){
    println("개가 음식 먹기")
  }
}
```

### 추상화
super class에서 함수의 구체적인 구현은 없고 단지 이 super class의 모든 sub class는 특정 함수가 반드시 있어야 한다는 점만 명시하여 각 sub class가 비어 있는 함수의 내용을 필요에 따라 구현하도록 만드는 것이 추상화입니다.   
추상화는 선언부만 있고 기능이 구현되지 않은 추상함수 그리고 이를 포함하는 추상클래스로 구성됩니다.   
추상클래스는 미완성 클래스로 단독으로는 객체를 만들 수 없습니다.   

추상화는 다른 방법이 존재하는데 이는 interface입니다.   
interface는 속성, 추상함수, 일반 함수를 갖습니다.   
추상클래스와 interface의 다른 점은 추상클래스는 생성자를 가지지만 interface는 생성자가 없습니다.   
interface에서 구현부가 있는 함수는 open 함수, 구현부가 없는 함수는 abstract 함수로 간주합니다. 그렇기에 별도의 키워드를 필요로 하지 않습니다.   
주의점으로는 여러 인터페이스나 클래스가 같은 이름이나 형태를 가진 함수를 가지고 있다면 sub class에서 꼭 오버라이딩해줘야 합니다.   

### 추상화 예시
```kotlin
abstract class Animal{
  abstract fun eat()
  fun cry(){
    println("웁니다.")
  }
}

class Dog : Animal(){
  override fun eat(){
    prinln("개가 밥을 먹는다.")
  }
}

// interface 예시
interface Animal{
  fun cry(){
    println("웁니다.")
  }
}

interface Eater{
  fun eat()
}

class Dog : Animal, Eater{
  override fun cry(){
    println("멍멍")
  }

  override fun eat(){
    println("개가 밥을 먹는다.)
  }
}
```
