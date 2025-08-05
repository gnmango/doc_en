# BroadcastLogic
Logic that manages the system's message system.
#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | Entity | broadcastSType | Small Message UI. |
| None | Entity | broadcastLType | Large Message UI. |
| None | SyncTable<string> | sTypeMessageTable | Message that uses the BroadcastPanel_S. |
| None | SyncTable<string> | lTypeMessageTable | Message that uses the BroadcastPanel_L. |
| None | boolean | sTypeBroadcastOn | Checks that BroadcastPanel_S is in use. |
| None | boolean | lTypeBroadcastOn | Checks that BroadcastPanel_L is in use. |
#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client | void | BroadCastMessageLocalization | Function that performs the localization before entering a message. |
| client | void |  BroadCastMessageIn | Function that checks whether a system message can be called. |
| client | void | PlayingMessage | Function that calls a message corresponding to the type and displays it in the UI. |

# SoundLogic
Logic that calls the various sound effects.
### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client | void | HitSound | Logic that calls the various hit sound effects. |
| client | void | TargetEndSound | Function that calls the sound effect when the hole is blocked. |
| client | void | GameResultSound | Function that calls sound effects based on the game results. |
| client | void | SystemMessageSound | Functions that call the system message sound effects. |
| client | void | HurtSound | Function that calls the sound effects based on the location of the injured player. |
| client | void | ClockSound | Function that calls clock sound effects based on a given type. |
| client | void | DeathSound | Function that calls the sound effects based on the location of the dead player. |

