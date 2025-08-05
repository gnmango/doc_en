# OutroManager

#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | SyncTable<string, number>| playerOutroTime | Manages the result check time per player. The 'key' is the PlayerId, and the 'value' is the result check time value. |

#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| server only | void | OutroTimerSet | Function that calls the timer for result check time. Moves the players to the lobby when the time is up. |
| server | void | MoveToLobby | Function that moves the players to the lobby. |


#### Entity Event Handler

| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| client only | [entity] NextPhaseBtn (/ui/004_OutroGroup/ResultFrame/PageControllers/NextPhaseBtn)| ButtonClickEvent | Call the MoveToLobby() function to move the players to the lobby when the button is clicked.|

# OutroUI
#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | Entity | outroTimer | Timer Number UI. 
| None | Entity | resultUI | The UI that displays the win/loss image.|
| None | Entity | getRankPointUI | Ranking Point UI. |
| None | Entity | getMoneyUI | Goods UI. |
| None | Entity | playerAvatarUI | Avatar UI. |
| None | Entity | nickNameTxtUI | Nickname UI. |
#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client | void | outroTimerUI | Function that displays the result check time in the UI. |
| client | void | outroGameResultUI |  Function that displays the win/loss result and changes the facial expression of the UI avatar.|
| client | void | outroGetRankPointUI | Function that displays the ranking point based on game results in the UI. |
| client | void | outroGetMoneyUI | Function that displays the goods acquired based on the result of the game in the UI. |

# InGameInfo
Component that manages the entire intro, in-game, and outro systems.
#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | string | ingameMapName | Stores randomly assigned in-game map names. |
| None | SyncTable<string, number>  | playerTeamNumber | Stores the player's team number. The 'key' is the PlayerId, and the 'value' is the value of the player's team number. |
| None | SyncTable<string, number>  | playerNumber | The player's team number.  |
| None | SyncTable<string, boolean>  | playerLoadingCheck | Checks that the players are in-game.  |
| None | number | playerCount | Stores the number of people matched in the game. |
| None | string | winnerTeam | Stores the winning team at the end of the game. |
| None | number | aTeamNum | The number of players on Team A. |
| None | number | bTeamNum | The number of players on Team B. |
#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| server only | void | GetGameReward | Calls a function that checks the game results and stores the values of goods and link points. |

#### Entity Event Handler

| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| server only | RoomService | PlayerTeamSetEvent | Stores the player's team number and in-team number. |
| server only | RoomService| InGameSetEvent | Stores randomly assigned in-game map names and the number of users matched. |
