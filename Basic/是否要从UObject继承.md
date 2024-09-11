在 Unreal Engine 中，是否让一个子类继承自 `UObject` 取决于该类的预期用途和需求。

### 需要从 `UObject` 继承的场景：
1. **反射系统（Reflection System）**：
    - 如果你需要利用 Unreal Engine 的反射系统（例如，通过 `UCLASS()` 宏来支持反射、属性（属性编辑、序列化）、蓝图可见性等），那么就需要继承自 `UObject`。

2. **垃圾收集（Garbage Collection）**：
    - `UObject` 提供了内置的垃圾收集机制。如果你的子类实例需要与引擎的内存管理一起工作，并确保在不再使用时正确清理内存，那么继承 `UObject` 是必需的。

3. **引擎生态系统支持**：
    - 如果你的类需要使用诸如事件系统、定时器、网络复制等功能，或者需要在内容浏览器中可见，那么 `UObject` 是必需的。

### 不需要从 `UObject` 继承的场景：
1. **简单数据容器或者不需要利用引擎特性**：
    - 如果子类仅用作一些简单的数据结构或容器，且不需要任何引擎功能，如反射、垃圾回收等，那么没有必要继承 `UObject`。这种情况下，继承自普通 C++ 类或结构体是完全可以的。

2. **性能敏感的场景**：
    - 由于 `UObject` 带有垃圾回收等开销，如果在性能敏感的代码路径中使用该类，并且可以完全回避这些特性，则可以考虑避免继承 `UObject`。

### 示例

一个需要继承 `UObject` 的情况下：

```cpp
UCLASS()
class MYGAME_API UMySubClass : public UObject
{
    GENERATED_BODY()

    // Your properties and methods here
};
```

一个不需要继承 `UObject` 的情况下：

```cpp
class MySubClass
{
public:
    MySubClass();
    ~MySubClass();

    // Your properties and methods here
private:
    int SomeData;
};
```

总而言之，是否需要从 `UObject` 继承取决于你是否需要利用它所提供的特性和功能。合理的设计和正确的继承关系有助于提升代码的性能和维护性。