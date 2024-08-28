For `Mixed` Replication Mode: The `OwnerActor`'s Owner must be the `Controller`. For Pawns, this is set automatically in `PossessedBy()`.
The `PlayerState`'s Owner is automatically set to the `Controller`.
Therefore, if your `OwnerActor` is not the `PlayerState`, and you use Mixed Replication Mode, you must call `SetOwner()` on the `OwnerActor` to set its owner to the `Controller`.

> [[Udemy GAS Course]], 3-23