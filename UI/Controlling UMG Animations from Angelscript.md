```cpp
class UYourWidget : UUserWidget
{
    UPROPERTY(Transient, Meta = (BindWidgetAnim), NotEditable)
    protected UWidgetAnimation Anim_FadeExit;
  

    UFUNCTION(BlueprintOverride)
    void OnInitialized()
    {
        if (Anim_FadeExit != nullptr)
        {
            PlayAnimation(Anim_FadeExit);
        }
    }
 
    UFUNCTION(BlueprintOverride)
    void OnAnimationFinished(const UWidgetAnimation Animation)
    {
        if (Animation == Anim_FadeExit)
        {
            RemoveFromParent();
        }
    }
}
```

