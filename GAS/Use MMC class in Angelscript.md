默认情况下，AngelscriptGAS 插件没有提供给 AS 使用 MMC 的能力，因为有一个关键的地方没有暴露给脚本。我不知道 UE 当初为什么这么做，因为 AS 大部分继承的是蓝图的能力，正是由于 UE 官方没有给蓝图暴露，才导致 AS 里也用不了的。

# 传统 MMC 使用方法
在继续之前先来看一下在 C++ 里是怎么使用 MMC 类的：
参考[这里](https://github.com/DruidMech/GameplayAbilitySystem_Aura/commit/e74efc40929e671fd11b9a1b94f5a4f17b02d2d4)

MMC_MaxHealth.h
```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameplayModMagnitudeCalculation.h"
#include "MMC_MaxHealth.generated.h"

/**
 * 
 */
UCLASS()
class AURA_API UMMC_MaxHealth : public UGameplayModMagnitudeCalculation
{
	GENERATED_BODY()
public:
	UMMC_MaxHealth();

	virtual float CalculateBaseMagnitude_Implementation(const FGameplayEffectSpec& Spec) const override;

private:

	FGameplayEffectAttributeCaptureDefinition VigorDef;
};
```

MMC_MaxHealth.cpp
```cpp
#include "AbilitySystem/ModMagCalc/MMC_MaxHealth.h"

#include "AbilitySystem/AuraAttributeSet.h"
#include "Interaction/CombatInterface.h"

UMMC_MaxHealth::UMMC_MaxHealth()
{
	VigorDef.AttributeToCapture = UAuraAttributeSet::GetVigorAttribute();
	VigorDef.AttributeSource = EGameplayEffectAttributeCaptureSource::Target;
	VigorDef.bSnapshot = false;

	RelevantAttributesToCapture.Add(VigorDef);
}

float UMMC_MaxHealth::CalculateBaseMagnitude_Implementation(const FGameplayEffectSpec& Spec) const
{
	// Gather tags from source and target
	const FGameplayTagContainer* SourceTags = Spec.CapturedSourceTags.GetAggregatedTags();
	const FGameplayTagContainer* TargetTags = Spec.CapturedTargetTags.GetAggregatedTags();

	FAggregatorEvaluateParameters EvaluationParameters;
	EvaluationParameters.SourceTags = SourceTags;
	EvaluationParameters.TargetTags = TargetTags;

	float Vigor = 0.f;
	GetCapturedAttributeMagnitude(VigorDef, Spec, EvaluationParameters, Vigor);
	Vigor = FMath::Max<float>(Vigor, 0.f);

	ICombatInterface* CombatInterface = Cast<ICombatInterface>(Spec.GetContext().GetSourceObject());
	const int32 PlayerLevel = CombatInterface->GetPlayerLevel();

	return 80.f + 2.5f * Vigor + 10.f * PlayerLevel;
}
```

# 为什么在 AS 里不能用
有以下几个问题：
1. VigorDef.AttributeToCapture、VigorDef.AttributeSource、VigorDef.bSnapshot 这些变量没暴露
2. RelevantAttributesToCapture 是 BlueprintReadOnly
3. GetCapturedAttributeMagnitude 暴露给蓝图的接口和 C++ 不一样，其中 FGameplayAttribute 拿不到（后面找到方法了）

随着我的研究，上面几个问题被一一解决了。解决方法：
1. 使用 `UAngelscriptGameplayEffectUtils::CaptureGameplayAttribute` 替代。这个是 AngelscriptGAS 插件提供的（我都能想到当时他们的纠结……）。
2. 在 C++ 里派生 UGameplayModMagnitudeCalculation，实现一个 Add 接口，然后 AS 从这个类派生。
3. 使用 `UAngelscriptAttributeSet::GetGameplayAttribute` 可以拿到，也是 AngelscriptGAS 提供的能力。

### AngelscriptModMagnitudeCalculation
上面第2点需要往插件里添加新的 C++ 类，我是放到 `Engine\Plugins\AngelscriptGAS\Source\Public\AngelscriptModMagnitudeCalculation.h` 里了，具体添加类的步骤参考 [[Add new C++ class to Plugin]]。

AngelscriptModMagnitudeCalculation.h
```cpp
#pragma once

#include "CoreMinimal.h"

#include "GameplayModMagnitudeCalculation.h"

#include "AngelscriptModMagnitudeCalculation.generated.h"

UCLASS(abstract, meta = (ChildCanTick))
class ANGELSCRIPTGAS_API UAngelscriptModMagnitudeCalculation : public UGameplayModMagnitudeCalculation
{
	GENERATED_BODY()

public:
	UFUNCTION(BlueprintCallable, Category = "Ability|GameplayEffect")
	void AddRelevantAttributeToCapture(const FGameplayEffectAttributeCaptureDefinition& CaptureDefinition);
};
```

AngelscriptModMagnitudeCalculation.cpp
```cpp
#include "AngelscriptModMagnitudeCalculation.h"

void UAngelscriptModMagnitudeCalculation::AddRelevantAttributeToCapture(const FGameplayEffectAttributeCaptureDefinition &CaptureDefinition)
{
	RelevantAttributesToCapture.Add(CaptureDefinition);
}
```

# 最终在 Angelscript MMC 的示例
```cpp
class UMMC_MaxHealth : UAngelscriptModMagnitudeCalculation
{
	FGameplayAttribute VigorAttribute;
	FGameplayEffectAttributeCaptureDefinition VigorCaptureDefinition;

	UMMC_MaxHealth()
	{
		VigorAttribute = UAngelscriptAttributeSet::GetGameplayAttribute(UAuraAttributeSet::StaticClass(), n"Vigor");

		VigorCaptureDefinition = UAngelscriptGameplayEffectUtils::CaptureGameplayAttribute(
			UAuraAttributeSet::StaticClass(), n"Vigor", 
			EGameplayEffectAttributeCaptureSource::Target, false);

		this.AddRelevantAttributeToCapture(VigorCaptureDefinition);
	}

	UFUNCTION(BlueprintOverride)
	float32 CalculateBaseMagnitude(FGameplayEffectSpec Spec) const
	{
		FGameplayTagContainer SourceTags = Spec.CapturedSourceTags.AggregatedTags;
		FGameplayTagContainer TargetTags = Spec.CapturedTargetTags.AggregatedTags;

		float32 VigorValue = GetCapturedAttributeMagnitude(Spec, VigorAttribute, SourceTags, TargetTags);
		return VigorValue * 10 + 77;
	}
}
```