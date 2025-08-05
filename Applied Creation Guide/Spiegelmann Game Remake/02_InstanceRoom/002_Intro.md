# IntroManager
Component that manages the system of the intro map.
#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | Component | ingameInfo | Variable that has the InGameInfo component of the common. |
| None | SyncTable<string, string> | playerIntroReadyState | Checks the player's ready status on the intro map. 'key' saves the Player Id, and 'value' saves the player's status as Ready or Idle. |
| None | number | inPlayerNum | The number of players matched. |
| None | number | readyCount | The number of players who are ready on the intro map. |
| None | number | introTime | The preparation timeout for the intro map. |
| None | Entity (/maps/002_Intro/ASpawnSet) | aSpawnSet | The parent entity of Team A's SpawnLocation. |
| Sync | Entity (/maps/002_Intro/BSpawnSet) | bSpawnSet | The parent entity of Team B's SpawnLocation. |
| None | boolean | gameStop | Checks whether the game is progressing normally. Changes to true if the player leaves the game while the intro is being prepared. |


#### Method
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| server only | void | OnBeginPlay | Function that checks if all players have successfully connected. Calls the IntroStart() function. |
| server only | void | IntroStart | Function that moves the player to the start position and turns on the intro timer. |
| server | void | IntroReadyStateChange | Function that changes the player's ready status in the intro map. |
| server | void | InGameMove | Function that moves the player to the in-game map. |


#### Event Handler

| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| client only | [entity] ReadyBtn (/ui/001_IntroGroup/ReadyBtn) | ButtonClickEvent | Event that changes the game ready status on the intro map to "Ready" when the Ready button is pressed. |

# IntroUI
Logic that manages all the UIs of the intro map.
#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | Entity (/ui/002_IntroGroup/IntroTimer) |  timeTxtUI | Text UI that displays the remaining time. |
| None | Entity (/ui/002_IntroGroup/ReadyBtn)| readyBtnUI | Intro Ready Button UI. |
| None | Entity (/ui/002_IntroGroup/UserCharacterPanel/ATeamScroll) | aTeamScrollLayout | The team member panel scroll layout for Team A. |
| None | Entity (/ui/002_IntroGroup/UserCharacterPanel/ATeamScroll/ATeamScroll0) | aTeamScroll0 | The team member display panel for Team A. |
| None | Entity (/ui/002_IntroGroup/UserCharacterPanel/BTeamScroll) | bTeamScrollLayout  | The team member panel scroll layout for Team B. |
| None | Entity (/ui/002_IntroGroup/UserCharacterPanel/BTeamScroll/BTeamScroll0) |  bTeamScroll0 | The team member display panel for Team B. |
| None | Component | ingameInfo | InGameInfo component of the common.|


#### Method
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client | void | ReadyCheckUI | Function that checks the ready status of a player to enable/disable the ready mark in the player scroll panel. |
| client | void | IntroTimerUI | Function that displays the remaining intro ready time. |
| client | void | PlayerScrollSet | Function that checks the matching players and refreshes the UI with information such as the player's avatar and nickname in the top TeamScroll of the team. |