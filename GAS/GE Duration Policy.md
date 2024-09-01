![[DurationPolicy.png]]

|          | BaseValue | CurrentValue |     |
| -------- | --------- | ------------ | --- |
| Instant  | ✅         |              |     |
| Duration |           | ✅            |     |
| Infinite |           | ✅            |     |
| Periodic | ✅         |              |     |
* Instant
	* Permanent change to the Base Value
* Duration & Infinite
	* Change Current Value
	* Undone if/when the effect is removed
* Periodic
	* Treated like Instant: Permanently change the Base Value
> [[Udemy GAS Course]], 41