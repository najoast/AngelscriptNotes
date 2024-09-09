1. 千万不要在 UE 里用 AS 自带构造函数，因为要用 NewObject 创建对象，这样才会有 WorldContext，这样创建的对象没机会给构造函数传参。
2. 不要在类的构造过程中调用 `NewObject` 创建对象，包括直接赋值和 default 创建都是不行的，因为这个时候类还没构造好，`World` 对象是 `nullptr`，创建出来的子对象当然也是没有 `World` 的。
```cpp
class AAuraCharacter : AAuraCharacterBase
{
	// 错误示范1
    UPlayerModuleMgr PlayerModuleMgr = Cast<UPlayerModuleMgr>(NewObject(this, UPlayerModuleMgr::StaticClass(), n"UPlayerModuleMgr"));

	// 错误示范2
	UPlayerModuleMgr PlayerModuleMgr;
    default PlayerModuleMgr =  Cast<UPlayerModuleMgr>(NewObject(this, UPlayerModuleMgr::StaticClass(), n"UPlayerModuleMgr"));
```

3. `UUserWidget.OnInitialized` 是在 `WidgetBlueprint::CreateWidget` 期间就会触发的（这个函数返回前就会从C++里触发 OnInitialized），所以如果你在调用 CreateWidget 之后，还需要手动初始化一些数据，那就不要在 OnInitialized 里放太多逻辑，这个函数相当于 C++ 的构造函数，应改到 `OnCtor` 里。