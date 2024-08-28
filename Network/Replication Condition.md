
The condition when the variable should be replicated.

Available Options –

| Option                         | Description                                                                                |
| ------------------------------ | ------------------------------------------------------------------------------------------ |
| None                           | No special condition set. The default option.                                              |
| Initial Only                   | Will only attempt to send on the initial bunch.                                            |
| Owner Only                     | Will only send to the actor's owner.                                                       |
| Skip Owner                     | Will only send to every connection EXCEPT the owner.                                       |
| Simulated Only                 | Will only send to simulated actors.                                                        |
| Autonomous Only                | Will only send to autonomous actors.                                                       |
| Simulated Or Physics           | Will only send to simulated OR bRepPhysics actors.                                         |
| Initial or Owner               | Will only send on the initial packet, or to the actors owner.                              |
| Custom                         | No special condition, but wants the ability to toggle on/off via SetCustomIsActiveOverride |
| Replay Or Owner                | Unknown                                                                                    |
| Replay Only                    | Unknown                                                                                    |
| Only No Replay                 | Unknown                                                                                    |
| Simulated Or Physics no Replay | Unknown                                                                                    |
| Skip Replay                    | Unknown                                                                                    |
> [1] https://unrealdirective.com/articles/blueprint-variables-what-you-need-to-know
> [2] https://angelscript.hazelight.se/scripting/networking-features/