# InGameManager
Component that manages the overall in-game system.
#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | Component | ingameInfo | Variable that has the InGameInfo component of the common. |
| None | string | inGameState | Variable that has the in-game progress status. Has the status of "Ready", "Play", "End", etc. |
| None| number | inGameTime | Variable that has the remaining time during in-game play. |
| None | number | aliveBTeamPlayerCount | Variable that has the number of alive players on Team B during in-game play. |
| None | number | clearTargetCount | Variable that has the number of holes currently blocked by Team B during in-game play. |
| None | number | closeTargetNum | Variable that has the number of holes that Team B must block during in-game play. |
| None | SyncTable< Entity > | deathPlayers | Variable table that has the entities of dead Team B players during in-game play. |
| None | number | ATeamPlayerCount | Variable for the number of Team A players. |

#### Method
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| server only | void | PlayerSetting | Function called when the player moves to the in-game map. It is responsible for changing the player's status according to their team and ensuring they are connected.|
| server only | void | TargetSetting | Function that sets the number of holes at the start of the game to match the total number of players. |
| server only | void | GameReadyCheck | Function called when the player moves to the in-game map. Checks whether all players have finished loading, and if so, calls the GameStart (game start function). |
| server only | void | GameStart | Function that starts the game. Has scripts related to the game start, calls functions for UI settings, changing player position, hole setting, etc., and has an in-game timer. |
| server only | void | GameOver | Function that saves the win/loss data and sends the player to the outro map when the game ends. |
| server only | void | PlayerAttack | Function called when a Team A player collides with a Team B player. Decreases the HP of the Team B player and knocks both players back in opposite directions after further processing according to their HP. |
| server only | void | PlayerDeath | Function called if a Team B player's HP becomes 0 or they escape. Sets the player to the dead status. (Makes the player invisible, hides motions/chat balloons/emotes.) |
| server only | void | PlayerPosCheck | Function that calls the MinimapPlayerUIUpdate function to repeat the timer to receive the player's position and display it on the minimap in real time. |
| server only | void | PlayerSpawn |  Function that randomizes the player's starting position and moves the player at the start of the game.|


# TargetComponent
Component that manages hole objects.
#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | boolean | Clear |Checks whether the hole is blocked. When the targetGage becomes 100, changes the value to true. |
| None | SyncTable<Entity, boolean> | playerInteraction | Checks how many players are currently interacting. |
| None | number | targetGage | The gauge of the hole. If it reaches 100, the hole is considered blocked. |
| None | number | gageOnTimer | Variable that contains the timer to increase the gauge of the hole. |
| None | number | gagePerSecond | Variable that contains the value of the gauge that increases per frequency. |
| None | number | timerCycle | Variable that contains the frequency value of the timer that increases the gauge. |
#### Method
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| server only | void | OnBeginPlay | Function that specifies the value of the variable that needs to be initialized. |
| server only | void | gageUp | Function that calls a timer that increases the gauge if there is a player who is interacting. |
| server only | void | playerInteractionCheck |  Function that checks the player's interaction status.|
| server | void | TargetClear | Function called when the gauge reaches 100. Function that changes the hole to a fully blocked status. |
| server | void | TargetLightSet | Function that controls the effects seen on the hole entities when interacting with them. |

#### Event Handler

| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| server only | self | InteractionEnterEvent | 	Event that changes the player's status to interactable when the player is touched. |
| server only | self | InteractionEvent | Event that changes the player's status to blocking the hole and calls playerInteracitonCheck() when the player interacts. |
| server only | self | InteractionLeaveEvent | Event that changes the interaction status to false when the player moves away, making the player unable to interact. |


# InGameUI
#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | Entity | resultScreen | Screen UI to show the results of the game |
| None | Entity | targetGageUI | UI that displays the target gauge |
| None | Entity | targetGageBar | Gauge Bar UI (purple part) |
| None | Entity | interactionUI | Guide UI displayed when interaction is possible |
| None | Entity | ingameTimeTxt | UI that displays the remaining time |
| None | Entity | bTeamScrollUI | UI that displays information such as the player status of Team B |
| None | Entity | targetListUI | UI that displays the status of the hole to be blocked |
| None | Entity | EmoticonPCUI | Emoji UI used on PC |
| None | Entity | EmoticonMobileUI | Emoji List UI used on mobile |
| None | Entity | chatAndEmoPanelOffBtnUI | Fullscreen Sized Transparent Button UI used to make the List UI Enable 'false' even when the nearby area is touched while the Emoji List UI is displayed |
| None | Entity | minMapPanel |Minimap Panel UI |
| None | Entity | miniMapBackGround | Minimap Background UI |
| None | Entity | minMapTargetIcon0 | Minimap Hole Sample Icon UI |
| None | Entity | minMapATeamIcon0 | Minimap Team A Player Position Sample Icon UI |
| None |Entity | miniMapBTeamIcon0 | Minimap Team B Player Position Sample Icon UI |
| None | SyncTable<string, number> | mapInfo | Variable table that contains the in-game map size and minimap panel size information. (Information Item: Left, Right, Top, Bottom, Width, Height) The 'key' value contains the map information item, and the 'value' contains the value corresponding to the item. |
#### Method
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client | void | ResultScreenUpdate | Function that displays the results window corresponding to the game results. |
| client | void | InGameTimerUIUpdate | Function that displays the in-game timer in the UI. |
| client | void | TargetGageUIUpdate | Function that displays the Gauge UI when the player interacts with the hole.|
| client | void | InteractionUIUpdate | Function that updates the Interaction Guide UI when the player is able to interact with another entity. |
| client | void | TeamBHpUIUpdate | Function that updates the Scrolling UI with the HP of Team B players whenever it changes.  |
| client | void | TeamBInfoUIUpdate | Function that updates the Scrolling UI with the basic information (avatar, nickname) of the Team B players.  |
| client | void | TeamBListUIUpdate | Function that sets the scroll layout of Team B players. |
| client | void | TargetListUpdate | Function that displays the number of holes to be blocked in the UI. |
| client | void | EmoticonListUpdate | Function updates the Emoji list in the UI according to the PC or Mobile environment. |
| client | void | MinimapSetUIUpdate | Function that stores the map information in the mapinfo table and updates the minimap background in the UI. |
| client | void | MinimapPlayerUIUpdate |  Function that refreshes the player's position on the minimap.  |
| client | void | MinimapTargetUIUpdate  | Function that refreshes the hole position on the minimap. |

#### Event Handler

| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| client only | [entity] EBtn(/ui/003_InGameGroup/Mobile_only_UI/EBtn) | ButtonClickEvent | Button click event that opens the emoji panel on mobile. |
| client only| [entity] ChatAndEmoOffBtn(/ui/003_InGameGroup/ChatAndEmoOffBtn) | ButtonClickEvent2 | Button click event that causes the emoji panel to close when the player touches anywhere outside the mobile emoji panel. |
