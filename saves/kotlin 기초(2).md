---
layout: single
title:  "Kotlin 기초(2)"
categories: study
---

# 클래스
클래스는 속성과 함수로 이루어져 있고 키워드는 class를 사용합니다.
기본 자료형(Int, Double...)도 코틀린 내부에서는 모두 class로 만들어져 있습니다.   
클래스는 인스턴스(객체)를 생성하기 위해 사용됩니다.   

## 클래스, 객체 생성
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

## 클래스 생성자
위의 코드에서 Person1의 (val name:String, val age:Int)부분은 Person1 클래스의 생성자로 볼 수 있습니다. 
이는 클래스의 속성들을 선언함과 동시에 생성자 역시 선언하는 방법입니다.   
생성자란 새로운 객체를 만들기 위해 호출하는 특수한 함수입니다.   
생성자를 호출하면 클래스의 객체를 만들어 반환받을 수 있게 됩니다.   
생성자는 객체의 속성 초기화뿐만 아닌 객체 생성시 필요 구문을 수행하는 역할도 합니다.   
생성자 키워드는 init을 사용합니다. 이는 기본 생성자의 역할을 합니다.   

생성자를 사용 시 모든 속성을 항상 수동으로 초기화하는 것은 비효율적일 수 있기에 constructor 키워드를 이용하여 보조 생성자를 사용할 수 있습니다.
보조 생성자는 반드시 기본 생성자를 통해 속성을 초기화해줘야 합니다.   

*생성자 예시*
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

## 클래스 상속
상속이 필요한 경우는 2가지가 존재하는데   
1번째는 이미 존재하는 클래스를 확장하여 새로운 속성이나 함수를 추가한 클래스를 만들고 싶을 때와   
2번째는 클래스간 중복되는 속성이나 함수를 더 수월하게 관리하기 위함입니다.   
속성과 함수를 물려주는 쪽을 super class, 물려받는 쪽을 sub class라고 합니다.   
kotlin은 기본적으로 상속 불가이기에 클래스 앞에 open 키워드를 붙여줍니다.   

상속은 2가지 규칙이 존재하는데   
1번째 sub class는 super class에 존재하는 속성과 같은 이름의 속성을 가질 수 없고   
2번째 sub class는 반드시 super class의 생성자를 호출해야 합니다.   

*상속 예시*
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

## 오버라이딩
super class의 함수와 같은 이름과 형태의 함수를 sub class에서 만들 수 없었는데 super class가 허용한다면 다시 구현할 수 있는데 이를 오버라이딩이라고 합니다.   
오버라이딩할 함수를 open 키워드로 선언하고 sub class에서 override 키워드를 이용하여 오버라이드할 함수를 다시 구현할 수 있습니다.   

*오버라이딩 예시*
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

## 추상화
super class에서 함수의 구체적인 구현은 없고 단지 이 super class의 모든 sub class는 특정 함수가 반드시 있어야 한다는 점만 명시하여 각 sub class가 비어 있는 함수의 내용을 필요에 따라 구현하도록 만드는 것이 추상화입니다.   
추상화는 선언부만 있고 기능이 구현되지 않은 추상함수 그리고 이를 포함하는 추상클래스로 구성됩니다.   
추상클래스는 미완성 클래스로 단독으로는 객체를 만들 수 없습니다.   

추상화는 다른 방법이 존재하는데 이는 interface입니다.   
interface는 속성, 추상함수, 일반 함수를 갖습니다.   
추상클래스와 interface의 다른 점은 추상클래스는 생성자를 가지지만 interface는 생성자가 없습니다.   
interface에서 구현부가 있는 함수는 open 함수, 구현부가 없는 함수는 abstract 함수로 간주합니다. 그렇기에 별도의 키워드를 필요로 하지 않습니다.   
주의점으로는 여러 인터페이스나 클래스가 같은 이름이나 형태를 가진 함수를 가지고 있다면 sub class에서 꼭 오버라이딩해줘야 합니다.   

*추상화 예시*
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

# 프로젝트 구조
프로젝트는 kotlin에서 가장 큰 틀이며 여러 개의 module로 이루어지고   
module은 직접 만들 수도 있고 필요한 경우 이미 구현된 라이브러리 모듈을 가져와 붙일 수도 있기에 굉장히 편리한 기능 단위이며 파일이나 폴더가 들어갈 수 있고 kotlin 코드파일(.kt)뿐만 아닌 모듈과 관련된 설정 및 리소스 파일 등도 포함될 수 있습니다.   
이러한 프로젝트, 모듈, 폴더 및 파일이 물리적인 구조를 담당합니다.   

논리적인 구조로는 패키지가 존재하면 패키지는 소스 코드의 소속을 지정하기 위한 논리적 단위입니다.   
코드내에서 사용하는 이름이 용도에 따라 서로 충돌하지 않도록 유니크한 패키지 이름을 지어주는 것이 좋습니다.   
일반적으로 패키지의 이름은 개발 회사의 도메인을 거꾸로 배열하고 그 뒤에 프로젝트 명을 붙이고 그 뒤에 기능별로 세분화한 이름을 붙입니다.   
ex) io.github.124coding.(프로젝트 이름).(기능)

코드 파일을 패키지에 넣는 방법은 코드 파일 맨 윗줄에 package 키워드를 적고 패키지 이름을 적으면 됩니다.   
패키지를 명시하지 않으면 자동으로 default 패키지로 묶이게 됩니다.   

코틀린은 자바와 달리 폴더명과 패키지 명을 일치시키지 않아도 되며 파일 상단에 패키지만 명시해주면 컴파일러가 알아서 처리합니다.   
같은 패키지 내의 코드들은 변수, 함수, 클래스를 공유하여 사용할 수 있는 반면 다른 패키지 import를 이용하여 사용이 가능합니다.   
만약 다른 패키지를 import하여 사용 중 이름이 중복되는 변수, 함수, 클래스가 있다면 패키지 명을 앞에다가 붙여주면 됩니다.   

코틀린은 파일명과 클래스 명이 일치하지 않아도 되며 하나의 파일에 여러개의 클래스를 넣어도 알아서 컴파일이 가능합니다. 이는 파일 내의 package 키워드를 기준으로 구분하기 때문입니다.   

# 스코프, 접근 제한자
변수나 함수, 클래스같은 멤버의 공용 범위를 제어하는 단위 **스코프**라고 합니다.   
이러한 스코프의 외부에서 내부로의 접근을 제어하는 것을 **접근 제한자**라고 합니다.   

## 스코프
스코프가 지정되는 범위에는 패키지, 클래스, 함수 등이 존재하며 하나의 패키지 안에 변수, 함수, 클래스같은 다양한 멤버들이 존재하여 그것을 크게 하나의 스코프로 볼 수 있고 함수나 클래스 안의 또 다른 변수, 함수들이 존재한다면 이러한 함수나 클래스 또한 패키지 안의 작은 하나의 스코프 단위로 볼 수 있습니다.   

스코프는 3가지 규칙이 존재하는데
1번째 스코프 외부에서는 스코프 내부의 멤버를 "참조연산자"로만 참조가 가능합니다. 이는 dot(.)을 얘기하는 것입니다.   
2번째 동일 스코프 내에서는 멤버를 공유할 수 있습니다.   
3번째 하위 스코프에서는 상위 스코프의 멤버를 재정의할 수 있습니다.   

*3번째 규칙 ex)*
```kotlin
val a = "패키지 스코프"

fun main(){
  val a = "메인 스코프"
  println(a) // "메인 스코프" 출력
}
```

## 접근 제한자
접근 제한자로는 **public, internal, private, protected**가 존재합니다.   
접근 제한자들은 변수, 함수, 클래스같은 멤버 선언 시 가장 앞에 붙여 사용합니다.   
상황에 따라 2가지의 기능으로 나뉘게 되는데   

*1번째 패키지 스코프*   
public이 기본값이며 어떤 패키지에서도 접근이 가능합니다.   
internal은 같은 모듈 내에서만 접근이 가능합니다.   
private은 같은 파일 내에서만 접근이 가능합니다.   
~~protected~~는 패키지 스코프에서는 사용하지 않습니다.   

*2번째 클래스 스코프*   
public이 기본값이며 클래스 외부에서 늘 접근이 가능합니다.   
private은 클래스 내부에서만 접근이 가능합니다.   
protected는 클래스 자신과 상속받은 클래스에서 접근이 가능합니다.   
~~internal~~은 클래스 스코프에서는 사용하지 않습니다.   

# 고차함수와 람다함수
## 고차함수
**고차 함수**란 함수를 클래스에서 만들어 낸 객체처럼 취급하는 방법으로 함수를 매개 변수처럼 넘겨 줄 수도 있고 결과값으로 반환받을 수도 있는 방법입니다.   
코틀린은 모든 함수를 고차함수로 사용 가능합니다.   
함수를 인자로 받을때는 ::를 앞에 붙여주어야 합니다.   

*고차함수 예시*
```kotlin
fun hello(str:String){
  println("$str 함수 hello")
}

fun helloPrint(function:(String)->Unit){ // function으로 함수를 매개 변수로 받고 이 함수의 매개 변수는 String이며 반환형이 없기에 Unit
  function("helloPrint")
}

helloPrint(::hello) // "helloPrint 함수 hello" 출력
```
## 람다함수
이때 위처럼 매개 변수로 넘길 함수는 따로 정의할 필요 없이 **람다 함수**를 이용하면 편하게 할 수 있습니다.   
람다 함수는 함수를 람다식으로 표현하는 방법으로 람다 함수는 그 자체로 고차 함수이기에 별도의 연산자 없이 변수에 담을 수 있습니다.   

*람다함수 예시*
```kotlin
fun helloPrint(function:(String)->Unit){ 
  function("helloPrint")
}

val ramda1:(String)->Unit = { str -> println("$str 람다1") }
helloPrint(ramda1) // "helloPring 람다" 출력

val ramda2 = { str:String -> println("$str 람다2") } // ramda1과 같은 결과 이때, 타입 추론으로 반환형이 없는 형태임을 kotlin이 자동적으로 알게 됨
helloPrint(ramda2)
```

람다함수는 3가지 특별케이스가 존재하는데   
1번째 람다함수도 일반 함수처럼 여러 구문을 사용할 수 있습니다.   
2번째 매개 변수가 없는 람다함수는 실행할 구문만 나열하면 됩니다.   
3번째 매개 변수가 하나라면 it을 사용합니다.   

*람다 특별케이스 예시*
```kotlin
val ramda1 = { i:Int, j:Int ->
  var k:Int
  k = i + j
  k += (i - j)
  k // 가장 마지막 문구를 반환받게 됨
}

val ramda2 = { println("헬로") }

val ramda3:(String) -> Unit = { println("$it 람다")}
```
# 스코프함수
**스코프함수**는 기본 제공하는 함수들로 함수형 언어의 특징을 좀 더 편리하게 사용할 수 있는 장점이 있습니다.   
