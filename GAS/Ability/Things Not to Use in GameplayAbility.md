- Replication Policy
	- Useless. Don't use it. Refer to Epic's Ability System Questions for an [explanation](https://epicgames.ent.box.com/s/m1egifkxv3he3u3xezb9hzbgroxyhx89) from Epic.
	- Gameplay Abilities are replicated from Server to owning Client already.
		- Note: Gameplay Abilities don't run on Simulated Proxies (use GEs and GCs)
- Server Respects Remote Ability Cancellation
	- Means when the local Client's ability ends, the server's will end
		- Not typically a good idea; it's the Server's version that matters
		- **Never trust Clients**
- Replicate Input Directly
	- Always replicates input press/release events to the Server
		- Epic discourages it
		- It's a bad idea that send input data from Client to Server every frame or every tick, it's like a DDoS attack.
> Direct Input state replication. These will be called if `bReplicateInputDirectly` is true on the ability and is generally not a good thing to use. (Instead, prefer to use Generic Replicated Events). (The comments of UAbilitySystemComponent::ServerSetInputPressed())

> [1] [[Udemy GAS Course]] 99 