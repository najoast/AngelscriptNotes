Steps:
1. Close Visual Studio  
2. Go to your plugin classes folder  
3. Add 2 empty files TestActor.h and TestActor.cpp  
4. Then , "Generate Visual Studio Project  
5. Open Visual Studio , then the files created

TestActor.h
```cpp
#pragma once

#include "GameFramework/Actor.h"
#include "TestActor.generated.h"

UCLASS()
class  ATestActor : public AActor
{
	GENERATED_UCLASS_BODY()

};
```

TestActor.cpp
```cpp
#include "MyPluginPCH.h"
#include "TestActor.h"

ATestActor::ATestActor(const FObjectInitializer& ObjectInitializer) : Super(ObjectInitializer)
{

}
```

注：实际添加的文件根据自己的情况来，不一定是上面的模板。可以参考同一个插件下其他类的实现。

> https://forums.unrealengine.com/t/adding-an-actor-class-to-plugin/37740/3
