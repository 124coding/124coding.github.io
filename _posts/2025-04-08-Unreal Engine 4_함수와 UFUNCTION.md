---
layout: single
title:  "Unreal Engine 4의 함수와 UFUNCTION"
categories: study
---
**언리얼을 공부하고 배우는 입장으로서 내용에 오류가 있을 수 있습니다.**  
**[베르의 게임 개발 유튜브](https://www.youtube.com/@wergia) 영상을 참고하면서 작성하였습니다.**  

# 언리얼 엔진에서의 함수  
언리얼 엔진에서 함수를 만들기 위해서는 두 단계를 거쳐야 합니다.  
우선 헤더 파일에서 이 클래스에 새로 생성할 함수의 원형을 선언해야 합니다. 함수의 반환형, 이름, 매개변수까지만 적고 세미콜론까지만 찍어줍니다.  
```C++
public:	
    UPROPERTY(EditanyWhere, BlueprintReadWrite, Category = "test_add")
    int32 a;

    UPROPERTY(EditanyWhere, BlueprintReadWrite, Category = "test_add")
    int32 b;

    UPROPERTY(VisibleanyWhere, BlueprintReadOnly, Category = "test_add")
    int32 c;

	void test_function_add();
```

소스 파일에서 함수를 구현할건데 반환형, 이 함수를 소유하고 있는 클래스의 이름을 적은 뒤 콜론을 두 개 찍어주고 함수 이름을 적습니다. 그 후 몸통이 되는 부분을 작성해주면 됩니다.  
```C++
void Atest_actor::test_function_add() {
	c = a + b;
}
```

이렇게 함수를 작성하고 이 함수가 특정한 시점에 자동으로 호출되게 해주겠습니다.  
특정한 시점으로는 a나 b같은 프로퍼티가 초기화되거나 변경될 때 일 것입니다.  
언리얼에서 프로퍼티가 초기화될 때 호출되는 함수는 PostInitProperties이고 프로퍼티 수정 시 호출되는 함수는 PostEditChangeProperty입니다.  
이 둘의 원형을 헤더 파일에서 작성해줍니다.  
```C++
public:
    virtual void PostInitProperties() override;
    virtual void PostEditChangeProperty(FPropertyChangedEvent& PropertyChangedEvent) override;
```

이 두 함수는 현재 클래스의 부모 클래스인 AActor에서 상속받는 함수이기에 부모의 함수를 자식이 덮어쓴다는 의미로 virtual과 override 키워드를 작성해주어야 합니다.  
이제 소스 파일로 가서 함수를 구현해주면 됩니다.  
간단하게만 구현해보도록 하겠습니다.  
```C++
void Atest_actor::PostInitProperties()
{
	Super::PostInitProperties();
	test_function_add();
}

void Atest_actor::PostEditChangeProperty(FPropertyChangedEvent& PropertyChangedEvent)
{
	test_function_add();
	Super::PostEditChangeProperty(PropertyChangedEvent);
}
```

이때 Super는 부모의 함수를 불러오는 것으로 이걸 하지 않으면 부모의 함수에서 시행되어야 할 PostInitProperties나 PostEditChangeProperty가 시행되지 않아 문제가 발생할 수 있습니다.  
![Image](https://github.com/user-attachments/assets/a9d54fce-6bc1-4b35-9e1b-bafc1f124d53)  
디테일 탭에 다음과 같이 프로퍼티들이 생성되고  
![Image](https://github.com/user-attachments/assets/4583d115-8c52-406b-800b-3bb0a1e6f952)  
A, B의 값이 바뀌면 그에 맞게 C에 값도 바뀌는 것을 확인할 수 있습니다.  

## 블루프린트에서 함수 사용 UFUNCTION
블루프린트에서 함수를 사용하려면 프로퍼티 앞에 UPROPERTY매크로를 작성하듯 함수 위에 UFUNCTION매크로를 작성해주어야 합니다.  
여기서도 매크로를 작성하였다고 바로 블루프린트에서 사용할 수 있는게 아닌 지정자를 작성해주어야 합니다.  

UFUNCTION 기초 지정자:
* BlueprintCallable -> 블루프린트 또는 레벨 블루프린트 그래프에서 실행 가능
* BlueprintImplementableEvent -> 블루프린트 또는 레벨 블루프린트 그래프에서 구현 가능
* BlueprintPure -> 어떤 식으로든 소유 오브젝트에 영향을 주지 않으며, 블루프린트 또는 레벨 블루프린트 그래프에서 실행 가능
* Client -> 호출되는 오브젝트를 소유한 클라이언트에서만 실행, 메인 함수 이름 뒤에 _Implementation을 붙인 함수를 추가로 선언합니다. 자동 생성 코드는 필요 시 "_Implementation" 메소드를 호출
* Server -> 서버에서만 실행, 메인 함수 이름 뒤에 _Implementation을 붙인 함수를 추가로 선언 후 거기에 코드를 작성, 자동 생성 코드는 필요 시 "_Implementation" 메소드를 호출
* Categry = "" -> 이 프로퍼티를 블루프린트 편집 툴이나 디테일 패널에서 특정 카테고리로 묶음  

블루프린트에서 사용할 모든 함수에는 카테고리를 할당해줘야 블루프린트에서 정상적으로 작동합니다.  
```C++
public:
    UFUNCTION(BlueprintCallable, Category = "test_add")
    void test_function_add();
```

이를 사용하기 위해서는 해당 액터 클래스를 기반으로 블루프린트 클래스를 만들어야 합니다.  
![Image](https://github.com/user-attachments/assets/c4d82cef-5023-4843-b238-1080a68eae5c)  
해당 액터를 우클릭하여 블루프린트 클래스 생성 버튼을 클릭합니다. 블루프린트 클래스를 생성하면 생성된 블루프린트 기반의 블루프린트 에디터가 열리게 됩니다.  

![Image](https://github.com/user-attachments/assets/2adfef10-b1f9-474f-b501-156a6efbbb8b)  
본인이 작성한 함수를 블루프린트에서 사용하려면 이벤트그래프 창에서 우클릭하여 본인이 작성한 함수를 찾아줍니다.  

![Image](https://github.com/user-attachments/assets/c4257c90-7787-43f6-b5f2-9eb20aaaf375)  
다음과 같이 블루프린트를 만들어서 블루프린트의 컴파일을 눌러 컴파일 시켜줍니다.  
그 후 테스트를 위해 액터의 블루프린트를 월드에 배치하고 플레이를 눌러 확인을 하게 되면 설정한 값으로 바뀌어 계산되는 것을 확인할 수 있습니다.  

![Image](https://github.com/user-attachments/assets/6b2ba094-62a4-4776-871e-ebcfd57c3358)  

이처럼 디자이너는 프로그래머가 작성한 기능을 가져와 사용할 수 있습니다. 이와 반대로 디자이너가 작성한 기능을 가져와 프로그래머가 C++에 사용하도록 할 수도 있습니다. 

## 디자이너가 작성한 기능을 C++로
### 1. UNFUNCTION 매크로에 BlueprintImplementableEvent 지정자 추가
```C++
UPROPERTY(EditanyWhere, BlueprintReadWrite, Category = "test")
FString test_str;

UFUNCTION(BlueprintImplementableEvent, Category = "test")
void CallFromCpp();
```
```C++
test_str = TEXT("test");
```

먼저 헤더 파일에 다음과 같은 프로퍼티와 함수를 작성해주고 소스 파일 생성자에서 test_str의 기본 값을 정해줍니다.  

```C++
void Atest_actor::BeginPlay()
{
	Super::BeginPlay();
	CallFromCpp();
}
```
다음으로 작성한 몸체가 없는 CallFromCpp함수를 BeginPlay에서 실행하도록 한 후 언리얼로 돌아가 컴파일해줍니다.  

이제 블루프린트로 돌아가 CallFromCpp 이벤트를 찾아서 생성해줍니다.  
![Image](https://github.com/user-attachments/assets/b2099ddf-8052-47d6-9880-7142bf476875)  
CallFromCpp가 실행되면 test_str 프로퍼티에 _Blueprint 값이 붙게 해줍니다.  
기본적으로 test_str의 값은 test이지만  
![Image](https://github.com/user-attachments/assets/097648e1-5b3e-4130-af69-5e6edfe1ec19)  
플레이를 시켜서 확인해보면  
![Image](https://github.com/user-attachments/assets/d519e33d-ad84-4810-b74b-831a2387020f)  
다음과 같이 test_Blueprint로 test_str의 값이 바뀌어 있는 것을 확인할 수 있습니다.  
블루프린트에서 CallFromcpp를 작성해주지 않으면 이 함수는 빈 함수인 것처럼 동작하게 됩니다.  

### 2. BlueprintNativeEvent 지정자 이용
BlueprintNativeEvent 지정자는 디자이너가 블루프린트로 기능을 만들 수 있게 하면서도 만약 디자이너가 기능을 구현하지 않으면 동작할 기본 기능을 프로그래머가 C++로 만들 수 있게 해줍니다.  

```C++
UFUNCTION(BlueprintNativeEvent, Category = "test")
void CallFromCpp();
virtual void CallFromCpp_Implementation();
```

위처럼 헤드 파일에서 BlueprintNativeEvent를 지정자로 해주고 _Implementation 함수를 가상 함수로 선언해줍니다.

```C++
void Atest_actor::CallFromCpp_Implementation()
{
	test_str.Append(TEXT("_Implementation"));

}
```

소스 파일에서 CallFromCpp_Implementation 함수의 몸체를 구현해줍니다. 여기서는 _Implementation를 추가하는 것으로 하겠습니다. 코드 작성이 완료되면 에디터에서 컴파일을 해줍니다.  

![Image](https://github.com/user-attachments/assets/b2099ddf-8052-47d6-9880-7142bf476875)  
이제 위의 블루프린트에서 작성한 CallFromCpp를 그대로 두고 플레이해보면 

![Image](https://github.com/user-attachments/assets/d519e33d-ad84-4810-b74b-831a2387020f)  
test_Blueprint로 값이 지정되는걸 볼 수 있지만 
![Image](https://github.com/user-attachments/assets/9714b483-66db-4e5d-b4d4-a5d900ee6b0d)  
다음과 같이 CallFromCpp를 블루프린트에서 삭제한 채 블루프린트 컴파일 후 플레이를 하게 되면
![Image](https://github.com/user-attachments/assets/7f27d6a0-1d11-47a7-b9d0-04fec6a26254)  
블루프린트에서 CallFromCpp가 구현되지 않았기에 C++ 코드에서 구현한 CallFromCpp_Implementation 함수가 호출되어서 test_Implementation으로 값이 지정되는걸 확인할 수 있습니다.  

C++에서 구현한 CallFromCpp_Implementation과 블루프린트에서 구현한 CallFromCpp를 같이 구현하는 방법도 존재합니다.  

![Image](https://github.com/user-attachments/assets/c5d0219e-392e-4e52-bc20-b2e9c40ee993)  
블루프린트의 CallFromCpp이벤트를 우클릭하여 부모 함수로의 호출 추가를 클릭하면 부모 이벤트 노드가 생성됩니다.  
![Image](https://github.com/user-attachments/assets/1631dcc9-4fa6-4fde-9db0-8196006486ff)  
위처럼 노드를 구성하여 디자이너가 구현한 CallFromCpp를 실행 전에 부모 CallFromCpp가 실행이 되면  
![Image](https://github.com/user-attachments/assets/fb5c99a0-32a2-4ae6-97ad-cb95ea7acbfe)  
test_str의 값이 test_Implementation_Blueprint가 되고  
![Image](https://github.com/user-attachments/assets/0ea729cb-4cb5-4013-8d92-f15d775d610c)  
위처럼 디자이너가 블루프린트에서 작성한 CallFromCpp이벤트가 먼저 실행되면  
![Image](https://github.com/user-attachments/assets/9aba6efe-3ae1-46c8-a8fb-de785af568d2)  
test_str의 값이 test_Blueprint_Implementation이 되게 됩니다.  
