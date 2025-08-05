This folder pertains to models, components, and logic.
# PlayerInGameStatus

#### Property

| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | number | teamNumber | Indicates the player's team number. 1 is Team A, the tagger, and 2 is Team B, the runaway.|
| None | number | hp | Indicates the player's HP. |
| None | boolean | alive | Indicates whether the player is alive or dead. |
| None | SyncTable<string, string> | playerInteractionState | Table that manages the player's interaction status.  |
| None | SyncTable<string, boolean> | playerStatusEffect | Table that manages the player's abnormal status. |
| None | SyncTable<string, number> | playerStatusEffectStack | Table that manages the player's abnormal status stack. |
 |None |SyncTable<string, number>  | playerStatusEffectAmount | Table that manages the player's abnormal status change. |
| None | SyncTable<string, number> | playerStatusEffectTime | Table that manages the player's abnormal status duration. |
| None | SyncTable<string, number> | playerStatusEffectTimer | Table that manages the player's abnormal status duration timer. |

#### Functions
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| server | void | InteractionStateChangeUI | Checks the interaction status corresponding to the current status. Calls the UI that matches the interaction. |
| server | void | InvicibleSet | Function that makes the player invincible. |
| server | void | ChangeSpeedSet | Function that changes the speed of the player. |
| server only | void | OnEndPlay | Checks whether the player has left the game or intro map. |

#### Entity Event Handler

| Synchronization | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| server only | self | TriggerEnterEvent |  Event that checks whether the player has been hit.|

# PlayerInfo
Component that manages the player's status in out-of-game.
#### Property

| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| None | number | money | The player's goods value. |
| None | number | rankPoint | The player's rank value. |

#### Function

| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| server only | void | OnMapEnter | Function that calls the function of `MapEnterLogic` for the map each time the map is changed. |
| client only | void | OnInitialize | Function that removes actions of the `W`, `A`, `S`, and `D` keys. |

#### Entity Event Handler

| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| client only | InputService | KeyDownEvent | Function that calls the emote function.|