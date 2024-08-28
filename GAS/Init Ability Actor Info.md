Must be done after possession (the Controller has been set for the Pawn)

* Player-Controlled Character
	* ASC Lives on the Pawn
		* Server: `PossessedBy`
		* Client: `AcknowledgePossession`
	* ASC Lives on the PlayerState
		* Server: `PossessedBy`
		* Client: `OnRep_PlayerState`
* AI-Controlled Character
	* ASC Lives on the Pawn
		* Server & Client: `BeginPlay`

> [[Udemy GAS Course]], 3-23
