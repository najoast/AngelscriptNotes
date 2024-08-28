
| Replication Mode | Use Case                       | Description                                                                                                           |
| ---------------- | ------------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| Full             | Single Player                  | Gameplay Effects are replicated to all clients.                                                                       |
| Mixed            | Multiplayer, Player-Controlled | Gameplay Effects are replicated to the owning client only. Gameplay Cues and Gameplay Tags replicated to all clients. |
| Minimal          | Multiplayer, AI-Controlled     | Gameplay Effects are not replicated. Gameplay Cues and Gameplay Tags replicated to all clients.                       |

Note: This is only available for C++, Angelscript only have [[Replication Condition]].