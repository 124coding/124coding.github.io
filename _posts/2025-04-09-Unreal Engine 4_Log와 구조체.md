---
layout: single
title:  "Unreal Engine 4_Log와 구조체"
categories: study
---
**언리얼을 공부하고 배우는 입장으로서 내용에 오류가 있을 수 있습니다.**  
**[베르의 게임 개발 유튜브](https://www.youtube.com/@wergia) 영상을 참고하면서 작성하였습니다.**

# 로그 상세 수준 및 출력
로그란 개발 중에 발생할 수 있는 문제나 상황에 따른 피드백을 눈으로 보여줄 수 있게 해주는 도구로 이는 개발자가 개발을 수월하게 하기 위하여 꼭 필요하고 그렇기에 새로운 언어나 새로운 엔진을 배울때는 로그를 출력하는 방법을 알고 있는 것이 좋습니다.  

## 출력 로그 패널 열기
언리얼에서 로그를 출력할때는 먼저 언리얼 에디터 상에서 출력 로그 패널을 열어야 합니다.  
![Image](https://github.com/user-attachments/assets/bbc68ede-cb22-4499-ab69-76ee3468e8f3)  
언리얼 에디터 상단 메뉴바의 창 -> 개발자 툴 -> 출력 로그를 클릭하면 출력 로그 패널이 열립니다.  
![Image](https://github.com/user-attachments/assets/fc0da97b-6fc2-47a0-926c-ee7d3bf86d50)  

## 로그 출력
언리얼 에디터에 로그를 출력하기 위해서는 UE_LOG() 매크로를 이용합니다. 로그를 출력하기 위해 액터를 하나 만들고 그 액터의 BeginPlay()에 로그를 작성해보도록 하겠습니다.  

```C++
void Atest_actor::BeginPlay()
{
	Super::BeginPlay();
	UE_LOG(LogTemp, Log, TEXT("BeginPlay"));
}
```  

코드를 작성한 액터를 배치하고 플레이하면 로그가 출력된 것을 확인할 수 있습니다.  
![Image](https://github.com/user-attachments/assets/509271a1-2496-4c07-b034-5b586eda71c6)  

## 로그의 상세 수준과 카테고리
로그 매크로의 매개 변수를 바꿔줌으로 로그의 수준과 카테고리를 바꿀 수 있습니다.  

### 로그의 상세 수준
* Fatal -> 가장 치명적인 수준의 로그로 항상 콘솔 및 로그 파일에 출력되며 로그가 비활성화된 상태에서도 모든 작동을 중단시킴
* Error -> 콘솔 및 로그 파일에 출력되며 빨간색으로 표시
* Warning -> 콘솔 및 로그 파일에 출력되며 노란색으로 표시
* Display -> 콘솔 및 로그 파일에 출력
* Log -> 로그 파일에는 출력되지만 게임 내의 콘솔에는 출력되지 않음, 언리얼 에디터의 출력 로그 파일에는 계속 출력
* Verbos -> 로그 파일에는 출력되지만 게임 내의 콘솔에는 출력되지 않음, 일반적으로 자세한 로깅 및 디버깅에 사용
* VeryVerbose -> 로그 파일에는 출력되지만 게임 내의 콘솔에는 출력되지 않음, 대량의 로그를 출력하는 상세한 로깅에 사용

가장 자주 사용되는 Error, Warning, Display만 사용해보도록 하겠습니다.  
```C++
void Atest_actor::BeginPlay()
{
	Super::BeginPlay();
	UE_LOG(LogTemp, Error, TEXT("Error"));
    UE_LOG(LogTemp, Warning, TEXT("Warning"));
    UE_LOG(LogTemp, Display, TEXT("Display"));
}
```  

![Image](https://github.com/user-attachments/assets/4536e18b-65a4-4848-9f5a-4c55c05b7085)  

### 로그 카테고리
로그 카테고리 매개변수는 출력된 로그가 어떤 시스템에서 발생한 로그인지 알려주는 역할을 합니다. 로그는 90개 이상의 카테고리가 있는데 위에 사용한 LogTemp는 특정 카테고리에 속하지 않은 임시 로그라는 의미입니다.  
소스 코드에서 Log까지만 작성하고 컨트롤 + 스페이스를 클릭하여 어떤 카테고리들이 있는지 확인할 수 있습니다.  

본인이 직접 로그 카테고리를 생성하는 방법도 있습니다.  
먼저 현재 언리얼 프로젝트의 이름으로 된 소스 파일과 헤더 파일을 찾습니다.  
그 후 헤더 파일에 다음과 같은 내용을 작성해줍니다.  

```C++
DECLARE_LOG_CATEGORY_EXTERN(LogStudy, Log, All);
```

매개변수를 살펴보면 생성할 카테고리의 이름, 로그 상세 수준, 컴파일 타임에서의 로그 상세 수준 순입니다.  
그 후 소스 파일로 가서 다음과 같이 작성해줍니다.  

```C++
DEFINE_LOG_CATEGORY(LogStudy);
```

생성할 로그 카테고리의 이름을 매개변수로 주어 로그의 카테고리가 생성되게 됩니다.  
이제 생성된 로그 카테고리를 사용하려고 한다면 사용하려고 하는 소스 파일의 헤더에 프로젝트 이름의 헤더를 include해주고 사용하면 됩니다.  

```C++
#include "study.h"

{
	Super::BeginPlay();
	UE_LOG(LogStudy, Log, TEXT("MyLog"));
}
```

![Image](https://github.com/user-attachments/assets/b89e07d4-e655-475f-8e90-b4061663e863)

# 구조체
구조체란 기존의 데이터 타입들을 조합하여 새로운 데이터 타입을 만드는 것입니다.  

```C++
struct Example{
public:
    int a;
    float b;
    string str;
}
```

기본 C++에서는 다음과 같이 사용할 수 있지만 언리얼 프로젝트에서는 C++ 코드에서는 사용이 가능할지라도 언리얼 에디터의 디테일 패널에 노출되지 않고 블루프린트에서도 사용이 불가하기에 다른 방법을 사용하게 됩니다.  
이때 사용하는 것이 USTRUCT입니다.  

먼저 블루프린트에서만 사용할 구조체라면 소스 코드에 따로 작성하는 것이 아닌 언리얼 에디터에서 즉시 만들 수 있습니다.  
![Image](https://github.com/user-attachments/assets/7f0bc5ab-c9b2-4edb-b53b-cd0bbe58a6ed)  
콘텐츠 부라우저에서 우클릭하여 블루프린트 -> 구조체를 클릭하면 생성되게 됩니다. 하지만 이 블루프린트 구조체는 소스 코드에서는 사용이 불가하기에 USTRUCT를 이용합니다. 이렇게 생성된 구조체는 소스 코드에서는 물론 블루프린트에서도 사용할 수 있다는 장점이 있습니다.  

구조체를 작성할때 구조체만을 따로 모으는 헤더를 만드는 것이 좋습니다. 특정 클래스에서만 사용될 구조체라면 그 클래스 헤더 파일의 하단에 작성해도 문제가 없지만 범용적으로는 구조체만 모아놓은 헤더 파일을 include하여 사용하게 되면 코드 재사용성이 높아지기에 더 좋은 코드를 작성할 수 있게 됩니다.  
먼저 C++ 클래스를 새로 만들어줍니다. 콘텐츠 부라우저에서 우클릭하여 새 C++ 클래스 생성 -> 부모 클래스: 없음을 선택합니다.  
![Image](https://github.com/user-attachments/assets/afb09a1b-115e-441f-97c5-cab5a48fce70)  
그럼 다음과 같이 구조체 헤더 파일과 소스 파일이 생성되고 이제 헤더 파일로 가서 generated를 include 해줄 것입니다.  


```C++
#include "MyStructure.generated.h"

USTRUCT(Atomic, BlueprintType)
struct FMyStruct {
	GENERATED_USTRUCT_BODY()
public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	AActor* actor;
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	int32 i;
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	float f;
};
```

구조체를 만들기 위해서는 USTRUCT 매크로를 앞에 붙여줍니다. 지정자로는 Atomic과 BlueprintType이 사용되었는데 Atomic은 이 구조체가 항상 하나의 단위로 직렬화 됨을 의미하고 BlueprintType은 이 구조체가 블루프린트에서 사용가능함을 의미합니다.  
구조체의 이름은 F가 앞에 들어가야하니 FMyStruct로 해주고 GENERATED_USTRUCT_BODY 매크로를 작성해줍니다.  
마지막으로 구조체의 각 프로퍼티를 작성하여주면 됩니다.  
이때 에러가 있다는 듯이 빨간줄이 생길 수 있는데 이는 무시하셔도 좋습니다.  

```C++
#include "MyStructure.h"

public:
    UPROPERTY(EditAnywhere)
    FMyStruct my_struct;
```
이제 작성된 구조체를 액터에 구현하여 사용할 수 있게 해줍니다.  

![Image](https://github.com/user-attachments/assets/bf8b01b9-1bb5-4a0e-84a6-f84454b3e9a3)  
위처럼 구조체에 대한 내용이 디테일 탭에 표현되는 것을 확인할 수 있습니다.  

![Image](https://github.com/user-attachments/assets/5dd5b738-3132-4e10-9e9b-b5cc96f86a84)  
블루프린트에서는 다음과 같이 구조체를 변수로 사용하거나 이벤트 플로우 도중에 구조체를 생성, 분해, 설정을 할 수 있습니다.  
