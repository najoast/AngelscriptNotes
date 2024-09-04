## PreAttributeChange
- Changes to CurrentValue
	- before the change happens
- Triggered by changes to Attributes
	- Attribute Accessors (Getter/Setter/etc)
	- Gameplay Effects
- Does **not** permanently change the modifier, just the value returned from querying the modifier
- Later operations recalculate the Current Value from all modifiers
	- We need to clamp again

`PreAttributeChange` is really only good for clamping. It's not really good for kicking off any logic in response to attribute changes. But that's not to say that we can't do that. This is just not the place for it.

注意：在此函数内进行 Clamp，实际上只是对 CurrentValue 生效，并不能修改 BaseValue。这可能会导致一些表现上的Bug，即实际看到的数值和真实数值不一致。具体Bug见[[Udemy GAS Course]]的第64课。解决办法：Clamping of `Attributes` is discussed in [PreAttributeChange()](https://github.com/tranek/GASDocumentation?tab=readme-ov-file#concepts-as-preattributechange) for changes to the `CurrentValue` and [PostGameplayEffectExecute()](https://github.com/tranek/GASDocumentation?tab=readme-ov-file#concepts-as-postgameplayeffectexecute) for changes to the `BaseValue` from `GameplayEffects`.

> PreAttributeChange just changes the value returned from querying the modifier. But here in PostGameplayEffectExecute we're actually setting the final value.

## PostGameplayEffectExecute

Now there is another function that's extremely useful for the attribute set, and we're going to override that. the function is called `PostGameplayEffectExecute`.

```cpp
void UAttributeSet::PostGameplayEffectExecute(const FGameplayEffectModCallbackData& Data)
```

This function is executed after a `GameplayEffect` changes an attribute and we have access to a lot of information via the `Data` parameter. The `Data` parameter give us to just about every entity involved in this gameplay effect being executed, which is huge. It's a very powerful thing, so it can be useful for us to collect a bunch of data about the gameplay effect and store it.