# LobbyMatchManager

#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | number | matchInterval | Variable that sets the matching wait time. |
| None | number | minPlayerNum | Variable that sets the minimum number of users required for matching. |
| None | number | maxPlayerNum | Variable that sets the maximum number of users that can be matched to a single game. |
| None | SyncTable<string, string> | playerReadyState |  Variable table that stores the player's ready status. The 'key' value stores the player's ID, and the 'value' stores the player's status as "Ready", "Idle", "Quit", etc.|
| None | SyncTable<string, number> | playerReadyTimeStamp | Variable table that stores the server time when each player was ready. Used to set the matching priorities. The 'key' value stores the player's ID, and the 'value' stores the server time when the player was ready. |
| None | string | LobbyState | Variable that stores the status of the current lobby. It stores "Start" status if it is matchable and stores "Idle" status otherwise. |
| None | number | instanceRoomNumber |Variable that sets the number of the instance room. To avoid overlapping, it is created by adding 1 to the number of previously created instance rooms when creating an instance room after matching. |


#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
|  server only| void | OnInitialize |Acts as a function to initialize various setting values when the World is created for the first time. |
| server | void | SetPlayerReadyState | Stores the player's ready status in the playerReadyState and the server time in the playerReadyTimeStamp and calls the ReadyPlayerCountCheck to check the number of players. |
| server only | void | ReadyCountTimer | Function that checks the preparation time if the player is ready. Uses the TimerService to record how many seconds the player is matching and calls the UI with the `ReadyStateTxtUI()` function. |
| server | void | ReadyPlayerCountCheck | Function that checks the number of players that are ready. It checks the playerReadyState, prepares the game if there are more than enough players to match, and maintains the matching status if there are not enough players. |
| server | void | StartCountDownTimer | Function that performs the game-ready count by calling a corresponding function when enough players for matching are ready. Calls the LaunchRoom() function when the count reaches 0. |
| server only | void | LaunchRoom | Function that creates instance rooms and sends the matched players to them. Function that checks the matching priorities of the preparation players, randomizes the map and roles, moves the players to the created room, and sends the relevant game data. |
#### Entity Event Handler

| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| server only | UserService | UserEnterEvent | When a player joins the game, sets their ready status to "Idle", the waiting status. All of the following event handlers, including this one, call the SetPlayerReadyState() function. |
| server only | UserService | UserLeaveEvent | When the player leaves the game, sets the ready status to "Quit", the left status. |
| client only | [entity] ReadyBtn(/ui/001_LobbyGroup/ReadyState/ReadyBtn) | ButtonClickEvent | When the player presses the Ready to Play button, sets the player's ready status to "Ready". |
| client only | [entity] CancelBtn(/ui/001_LobbyGroup/ReadyState/CancelBtn) | ButtonClickEvent2 | When the player presses the Ready to Play button, set the player's ready state to "Idle". |


# LobbyUI
Logic that manages the UIs used in the Lobby Map, excluding the matching-related UIs.
#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None| Entity  (/ui/001_LobbyGroup//MenuBtn) | menuBtn  | Menu button.  |
| None | Entity  (/ui/001_LobbyGroup/MenuPanel)| menuPanel | Menu panel. |
| None | Entity (/ui/001_LobbyGroup/RankList/popup/Leaderboard/ScrollLayout) | rankScrollLayout | Ranking list panel. |
| None | Entity (/ui/001_LobbyGroup/RankList/popup/Leaderboard/ScrollLayout/rank0) | rank0 | Ranking list sample. |
| None | Entity (/ui/001_LobbyGroup/RankList/popup/Leaderboard/My_rank)| myrankBoard | Player Information UI.  |
| None | Entity (/ui/001_LobbyGroup/RankList) | rankPanel | Ranking panel. |

#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client | void | RankingUpdateUI | Function that gets the ranking table received from the data logic as a variable to display it in the UI. |

#### Entity Event Handler

| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| client only | [entity] MenuBtn (ui/001_LobbyGroup/MenuBtn) | ButtonClickEvent | Event that displays the menu panel when the Menu button is pressed. |
| client only | [entity] RankBtn (ui/001_LobbyGroup/MenuPanel/RankBtn) | ButtonClickEvent2 | Event that displays the ranking panel when the Ranking button in the menu panel is pressed. |
| client only | [entity] XBtn (/ui/001_LobbyGroup/RankList/popup/XBtn) | ButtonClickEvent3 | Event that turns off the ranking panel when the X button within the ranking panel is pressed. |

# MatchUI
Logic that manages the matching-related UIs used in the Lobby Map.
#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None  | Entity (/ui/001_LobbyGroup/ReadyState/TimeTxt) | readyStateTxtUI | Text UI that displays the current player's matching status. |
| None | Entity (/ui/001_LobbyGroup/ReadyState/ready_count) | readyCountTxtUI | Text UI that displays the number of players currently in the ready status. |
| None | Entity (/ui/001_LobbyGroup/ReadyState/ReadyBtn) | readyBtn | Ready button.  |
| None | Entity (/ui/001_LobbyGroup/ReadyState/CancelBtn) | cancelBtn | Ready Cancel button. |
#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client | void  | ReadyStateTxtUI | Function that displays the current matching status in the readyStateTxtUI. <br>Start: Matched in n seconds. <br> Ready: Matching for n seconds..<br>Idle: Press the Ready to Play button |
| client | void  | ReadyCountUI | Function that displays the users currently in ready status in the readyCountTxtUI. |
| client | void  | ReadyBtnStateChange | Function that displays the Ready/Ready Cancel buttons, depending on the player's ready status. |