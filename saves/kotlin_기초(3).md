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
개발자가 커스텀 class를 만들때는 equals()를 상속받아 동일성을 확인해주는 구문을 별도 작성해줘야합니다.   

*동일성 체크 예시*
```kotlin
class Car(val name:String, val price:Int){
  override fun equals(otherCar:Any?):Boolean{
    if(otherCar is Car){
      return otherCar.name == name && otherCar.price == price
    }
    else return false
  }
}

val a = Car("카", 200)
val b = Car("카", 200)
val c = a
val d = Car("스포츠카", 2000)

println(a == b) // ture 출력
println(a === b) // false 출력

println(a == c) // ture 출력
println(a === c) // ture 출력

println(a == d) // false 출력
println(a === d) // false 출력
```

# 함수의 여러 기능
kotlin 또한 함수의 오버로딩이 지원되고 있는데 오버로딩이랑 같은 스코프 내에 함수 이름이 같더라도 받는 매개 변수가 다를 시(매개 변수 개수, 매개 변수 자료형의 차이) 이를 다른 함수로 취급해줄 수 있는 것입니다.   
특이한 경우 default argument를 이용하는데 이는 인자 부분을 받지 않았을 때도 미리 정의된 인자로 함수를 수행하는 것을 말합니다. 이때 인자는 매개 변수 순서대로 받게 됩니다.      
이때 여러 개의 인자를 받는데 중간의 인자를 건너 뛰어서 받고 싶다면 named argument를 이용할 수 있습니다.   
여러 개의 같은 타입의 인자를 받고 싶은 경우에는 직접 하나, 하나 작성할 수도 있겠지만 variable number of arguments를 사용할 수도 있습니다. 이때 variable number of arguments는 다른 매개 변수들과 같이 사용 시 맨 뒤에 위치해야 합니다.   
함수를 연산자처럼 쓸 수 있는 infix function도 있습니다. 이때 클래스 안에서 infix 함수가 선언되면 적용할 클래스가 자기 자신이므로 클래스 이름은 작성하지 않습니다.   
위의 여러 기능들은 예시에서 더 자세히 다루겠습니다.   

*함수의 여러 기능 예시*
```kotlin
// 오버로딩
fun read(x:Int){
  println("숫자 ${x}입니다.")
}

fun read(x:String){
  println(x)
}

read(7) // "숫자 7입니다." 출력
read("문자열입니다.") // "문자열입니다." 출력

// default argument
fun read(x:Int = 1){
  println("숫자 ${x}입니다.")
}

read() // "숫자 1입니다." 출력
read(2) // "숫자 2입니다." 출력

// named argument
fun read(x:Int = 1, y:Int = 2, z:Int = 3){
  println("숫자 ${x}, ${y}, ${z}입니다.")
}

read(2, z = 10) // "숫자 2, 2, 10입니다." 출력

// variable number of argbuments
fun read(vararg x:Int){
  print("숫자 ")
  for(i in x) {
    print("${i} ")
  }
  println("입니다.")
}

read(1, 2, 3, 4) // "숫자 1 2 3 4 입니다." 출력

//infix function
infix fun Int.add(x:Int):Int = this + x

class Number{
  var data = 0
  infix fun add(x:Int){
    this.data += x
  }
}

println(5 add 7) // 12 출력 (이때 5는 this인 객체 자신이고 7은 x입니다.

val num = Number()
num.add 10
num.add 5
println(num.data)
```

# 중첩 클래스, 내부 클래스
**중첩 클래스**는 클래스 내부에 다른 클래스가 정의되는 것이고 **내부 클래스**는 클래스 내부에 존재한다는 것은 같지만 독립적으로 객체를 생성할 수 없고 외부 클래스의 속성과 함수의 이용이 가능합니다.   
다시 말해 중첩 클래스는 클래스 내부에 전혀 별도의 클래스가 존재하는 것이고 내부 클래스는 클래스 내부에 그 클래스에 의존적인 다른 클래스가 존재하는 것입니다.   

*중첩 클래스, 내부 클래스 예시*
```kotlin
class Outer {
  var text = "Outer class"

  class Nested{
      fun introduce(){
        println("Nested Class")
      }
  }

  inner class Inner{
    var text = "Inner Class"
    fun introduceInner(){
        println(text)
    }
    fun introduceOuter(){
        println(this@Outer.text) // Outer클래스와 Inner클래스에 같은 이름의 속성이 존재한다면 this@Outerd클래스이름 으로 참조함 
    }
  }
}

Outer.Nested().introduce() // "Nested Class" 출력
val outer = Outer()
val inner = outer.Inner()

inner.introduceInner() // "Inner Class" 출력
inner.introduceOuter() // "Outer Class" 출력

outer.text = "Change Outer Class"
inner.introduceOuter() // "Change Outer Class" 출력
```

# 특별한 기능을 가진 클래스
1. Data Class로 이는 데이터를 다루는데 최적화된 클래스로 5가지 기능이 있습니다.   
(1) 내용의 동일성을 판단하는 equals()의 자동구현   
(2) 고유한 코드를 생성하는 hashcode()의 자동구현   
(3) 속성을 보기쉽게 나타내는 toString()의 자동구현   
(4) 객체를 복사하여 새 객체를 만드는 copy()의 자동구현   
(5) 속성을 순서대로 반환하는 componentX()의 자동구현 ex) component1(), component2()   

*Data Class예시*
```kotlin
class Person(val name:String, val age:Int)
data class Data(val name:String, val age:Int)

val a = Person("김호택", 21)

println(a == Person("김호택", 21)) // false 출력
println(a.hashCode()) // 제대로 구현 x
println(a) // toString() 결괏값을 보기 위함, 제대로 구현 x

val b = Data("박준범", 30)

println(b == Data("박준범", 30)) // true 출력
println(b.hashCode()) // hashcode값 출력
println(b) // Data(name=박준범, age=30) 출력

println(b.copy()) // Data(name=박준범, age=30) 출력
println(b.copy("아리") // Data(name=아리, age=30) 출력
println(b.copy(age = 20)) // Data(name=박준범, age=20) 출력

// Component
val list = listOf(Data("아리", 20), Data("나미", 21))

for((a, b) in list){ // 내부적으로 component1(), component2()를 사용
  println("${a}, ${b}") // 아리, 20 , 나미, 21 출력
}
```

2. Enum class로 이는 enum class 내의 상태를 나타내기 위한 여러 객체를 이름을 붙여 여러개 생성해두고 그 중 하나를 선택하여 나타내기 위한 클래스로 이는 관행적으로 모두 대문자로 기술합니다.
또한 enum의 객체들은 고유한 속성을 가질 수 있는데요. enum class의 생성자에 속성을 받도록하면 객체 선언 시 속성도 설정할 수 있습니다.
일반 클래스처럼 함수도 설정할 수 있는데 이때는 객체의 선언이 끝나는 부분에 세미콜론(;)을 추가하여 함수를 기술할 수 있습니다.
말로만 들으면은 감이 잘 안오실테니 예시를 보시죠.

*Enum Class 예시*
```kotlin
enum class State(val message:String){
  SING("노래부르기"),
  EAT("밥먹기"),
  SLEEP("잠자기"); // 객체들 열거

  fun isSleeping() = this == State.SLEEP // 비교 대상이 State 객체 자기자신이므로 this로 비교
}

var state = State.SING
println(state) // SING 출력

state = State.SLEEP
println(state.isSleeping()) // true 출력

state = State.EAT
println(state.message) // "밥먹기" 출력
```

# SET, MAP
1. Set은 리스트와 달리 순서가 없으며 중복이 허용되지 않는 컬렉션입니다. 이로 인해 Set은 인덱스로 위치를 지정하여 객체를 참조할 수는 없으며 contains()를 이용하여 객체가 Set안에 존재하는지만 확인하는 식으로만 사용합니다.
Set또한 리스트와 마찬가지로 그냥 Set과 MutableSet이 존재합니다. 이 또한 객체의 추가, 삭제가 가능한지 여부에 따라 선택되어 사용됩니다.
MutableSet의 추가는 add(), 삭제는 remove()를 사용합니다.

*Set 예시*
```kotlin
val a = mutableSetOf(1, 2, 3)

for (num in a){
  println(num) // 1, 2, 3 출력
}

a.add(4)
println(a) // 1, 2, 3, 4 출력

a.remove(3)
println(a) // 1, 2, 4 출력

println(a.contains(2)) // true 출력
```

2.Map은 객체를 넣을때 key값도 같이 넣어주는 컬렉션입니다. 이러한 구조로 Map은 특정 값을 찾을때 객체의 위치가 아닌 고유한 key를 통해 객체를 참조하는 특징을 가집니다. 이때 같은 key에 다른 객체를 넣게 되면 기존의 객체가 대체되기에 주의가 필요합니다.   
Mat또한 그냥 Map과 MutableMap이 존재합니다. MutableMap의 추가는 put(key, value), 삭제는 remove(key)로 하게 됩니다.   

*Map 예시*
```kotlin
val a = mutableMapOf(1 to "사과",
                     2 to "바나나",
                     3 to "파인애플") // key와 value를 to로 이어줌
  
for(entry in a){
  println("${entry.key} : ${entry.value}")
}
  
a.put(4, "귤")
println(a) // {1=사과, 2=바나나, 3=파인애플, 4=귤} 출력
  
a.remove(3)
println(a) // {1=사과, 2=바나나, 4=귤} 출력

println(a[1]) // "사과" 출력
```

# 컬렉션 함수


#### 본 글은 유튜브 [**디모의 코틀린 강좌**](https://www.youtube.com/watch?v=8RIsukgeUVw&list=PLQdnHjXZyYadiw5aV3p6DwUdXV2bZuhlN)를 참고하여 작성하였습니다.
