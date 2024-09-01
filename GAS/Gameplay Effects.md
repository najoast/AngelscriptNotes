## Overview

* Data only
* Don't subclass `UGameplayEffect`
* Change Attributes through:
	* Modifiers
		* Modifier Op
			* Add
			* Multiply
			* Divide
			* Override: replace the attributes value with the value provided for the magnitude. The magnitude used in these operations is produced from the magnitude calculation. `Magnitude Calculation Type`:
				* **Scalable Float**
					* Specify a hardcoded value for the magnitude directly, or
					* Use a table which scales the magnitude based on the Gameplay Effect level.
				* **Attribute Based**
					* uses another attributes value. For example:
						* Add a value to the player's health equal to the player's strength
						* Add to the player's health a value equal to strength times ten
				* **Custom Calculation Class**
					* Create a class designed to capture other values such as attributes or really any other variables that we want and use them in a calculation that can be arbitrarily complex. 
					* We typically call these Modifier Magnitude Calculations (MMC), and these are a powerful way to change a single attribute based on a custom calculation.
				* **Set by Caller**
					* Assigning a magnitude associated with a name or a gameplay tag.
					* This is useful if you need to set the modifiers magnitude based on code logic at the time that we create the gameplay effect or apply it.
	* Executions (More powerful)
		* Full name: Gameplay Effect Execution Calculation
		* These can change more than one attribute
		* They can really do anything else that we choose to code in them
		* They're the most powerful way to modify attributes in a gameplay effect

## Duration Policy
* Instant
* Has Duration
* Infinite
	* It's not that it never goes away.
	* We can remove infinite effects, it's just that they stay until we manually remove them.
	* These are useful in situations where we may not know how long that effect should be applied and it should be removed in response to some gameplay.

## Stacking

GE can stack and they have their own policies for how to stack.

## Add Gameplay Tags

## Grant Abilities

***
GE can be applied directly, but usually we create a more lightweight version of them, known as a GE Spec. This concept of a spec is common in GAS and is a form of optimization. 

The spec contains the bare bones information needed to perform the modifications and the only actual instance of the GE class that typically gets instantiated is the class default object (CDO).

Most of the things we're covering here are just ways that it's typically done.

The GE class is just versatile enough to never need to be subclassed into child classes. If you need complex attribute modifications, you use custom executions or magnitude calculations rather than subclassing the GE into a child class.