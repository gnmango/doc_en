# MapEnterLogic
Logic that contains functions that are called when the player enters another map.
In the OnMapEnter function, calls the function in the logic that corresponds to the map where the player has moved to. 
#### Property

| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| None | Entity (/ui/000_LoadingGroup) | loadingUI | Loading screen UI. |
| None |Entity (/ui/001_LobbyGroup)  |  lobbyUI | Lobby group UI. |
| None | Entity (/ui/002_IntroGroup) | introUI | Intro group UI. |
| None |Entity (/ui/003_InGameGroup) | ingameUI | In-game group UI. |
| None | Entity (/ui/004/004_OutroGroup) | outroUI |Outro group UI.  |
| None | ComponentRef | ingameInfo | InGameInfo component in the common. Set the ComponentRef as InGameInfo component. Set the value as /common.|
| None | ComponentRef| introManager | IntroManager component in the intro map. Set the ComponentRef as IntroManager component. Set the value as /maps/002_Intro.|

#### Function

| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client | void | UISetting | Function that compares the names of the map the player moved to and enables the appropriate UI. |
| server only | void | LobbyMapEnter | Called if the map the player moved to is a lobby map. Gets the goods and rank score values and sets the UI related to the matching. |
| server only | void | IntroMapEnter |Called if the map the player moved to is the intro map. Increments the value of inPlayerNum by 1 to check that the player has accessed the intro map. |
| server only | void | InGameMapEnter | Called if the map the player moved to is an in-game map. Calls the InGameManagerPlayerSetting() function for the player setting to prepare the game. |
| server only | void | OutroMapEnter | Called if the map the player moved to is an outro map. Calls the ingameInfo:GetGameReward() function to get rewards based on the result of the game. |
| client | void | LoadingUISet | Function to enable/disable the Loading Screen UI. |


# PlayerDataLogic
Logic that manages the player's goods and rank data/UI.
#### Property

| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| None | Entity (/ui/001_LobbyGroup/Money/MoneyTxt) | moneyTxtUI | Text UI that shows the value of the goods/rank score used in the Lobby UI. |
| None | Entity (/ui/001_LobbyGroup/RankPoint/RankPointTxt)| rankPointTxtUI | Text UI that shows the value of the goods/rank score used in the Lobby UI. |
| None | boolean | getRewardMoneyCheck |Variable that checks whether the player has received a goods reward based on the result of the game.  |
| None | boolean | getRewardRankPointCheck |Variable that checks whether the player has received a rank score reward based on the result of the game.  |

#### Function

| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| server only | void | GetMoneyData | Function that gets the player's goods from the DB. Function that gets the value of the imported goods. It updates the UI information with the value of the imported goods. |
| server only | void | SetMoneyData | Function that gets the player's goods from the DB and applies the addMoney parameter to save the data again. Refreshes UI information together. |
| server only | void | GetRankPointData | Function that gets a player's rank score from the DB. |
| server only | void | SetRankPointData| Gets the player's rank score from the DB and stores the changed rank score back to the DB. |
| client |  void | MoneyUIUpdate | Function to update the Goods UI. |
| client |  void | RankPointUIUpdate | Function to update the Rank UI. |
| server only | void  | RankIn | Function to register the ranking. Stores the changed rank score, player ID, and nickname to the RankingDB and calls the function to update the UI. |
| server | void | RankUpdate | Loads the stored Ranking DB. Function that updates the Ranking UI by sorting the scores in the DB in descending order. |