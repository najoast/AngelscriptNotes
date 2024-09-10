A `GameplayAbility's` `Instancing Policy` determines if and how the `GameplayAbility` is instanced when activated.

|`Instancing Policy`|Description|Example of when to use|
|---|---|---|
|Instanced Per Actor|Each `ASC` only has one instance of the `GameplayAbility` that is reused between activations.|This will probably be the `Instancing Policy` that you use the most. You can use it for any ability and provides persistence between activations. The designer is responsible for manually resetting any variables between activations that need it.|
|Instanced Per Execution|Every time a `GameplayAbility` is activated, a new instance of the `GameplayAbility` is created.|The benefit of these `GameplayAbilities` is that the variables are reset everytime you activate. These provide worse performance than `Instanced Per Actor` since they will spawn new `GameplayAbilities` every time they activate. The Sample Project does not use any of these.|
|Non-Instanced|The `GameplayAbility` operates on its `ClassDefaultObject`. No instances are created.|This has the best performance of the three but is the most restrictive in what can be done with it. `Non-Instanced` `GameplayAbilities` cannot store state, meaning no dynamic variables and no binding to `AbilityTask` delegates. The best place to use them is for frequently used simple abilities like minion basic attacks in a MOBA or RTS. The Sample Project's Jump `GameplayAbility` is `Non-Instanced`.|

> https://github.com/tranek/GASDocumentation?tab=readme-ov-file#467-instancing-policy