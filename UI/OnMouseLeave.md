这个回调很坑，你以为只有鼠标离开时会回调吗？错了。实际上点击鼠标时也会回调。

```cpp
void OnMouseLeave(FPointerEvent MouseEvent)
```

点击鼠标时，`MouseEvent` 的 `EffectingButton` 会是对应的鼠标按键：
- `FKeys::LeftMouseButton`
- `FKeys::RightMouseButton`

如果只想响应真正的鼠标离开事件，要这么写：
```cpp
    UFUNCTION(BlueprintOverride)
    void OnMouseLeave(FPointerEvent MouseEvent)
    {
        if (!MouseEvent.GetEffectingButton().IsValid()) {
            // 处理鼠标离开逻辑
        }
    }
```