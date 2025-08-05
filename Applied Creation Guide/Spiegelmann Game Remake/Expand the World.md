# Changing the Matching Members
You can change matching members by changing dataset and property values.
1. Select **Workspace - MyDesk - 02_InstanceRoom - GameSetting - matchTeamData**.
2. Add a new row and add data.

    | PlayerNum | TeamA | TeamB | CloseTargetNum | EnableTargetNum | PlayTime |
    | --- | --- | --- | --- | --- | --- |
    | 9 | 3 | 6 | 7 | 10 | 600 |
3. Select **Workspace - MyDesk - 01_StaticRoom - 001_Lobby - LobbyCompo - LobbyMatchManager**.
4. Change the max number of Matching Members **maxPlayerNum** property to **9**. 
    ```lua
    Property:
    [None]
    number maxPlayerNum = 9
    ```

# Changing Character Attributes
You can change the character's attributes by changing the value of the TeamStatusData dataset.
1. Select **Workspace - MyDesk - 02_InstanceRoom - GameSetting - TeamStatusData**.
2. Change the **Scale** value of the team you want to change. You can also change Jump to change the character's jump elasticity.
![SCALE](https://mod-file.dn.nexoncdn.co.kr/bbs/1702034269711ea3ecf72ae954d5d804d5b87df71c6d7.png "SCALE")

#### Changing Character Gravity
You can add a new value to DataSet and change the code of the **InGameManager** PlayerSetting() function to give the player gravity.

1. Select **Workspace - MyDesk - 02_InstanceRoom - GameSetting - TeamStatusData**.
2. Add the **Gravity** to a new row.
![GRAVITY](https://mod-file.dn.nexoncdn.co.kr/bbs/17020342901120a226f57d7f94cb3ba50c6556e0ca32f.png "GRAVITY")
    | Status | TeamA(1) | TeamB(2) |
    | --- | --- | --- |
    | Gravity  | 0.8 | 1.3 |
3. Select **Workspace - MyDesk - 02_InstanceRoom - 003_InGame - InGameCompo - InGameManager** to open it.
4. Write the following to apply the gravity value just below the PlayerSetting() function's local teamAjump = tonumber(teamStatusData:FindRow("Status", "Jump"):GetItem("TeamA(1)")).

    ```lua
    local teamAGravity = tonumber(teamStatusData:FindRow("Status", "Gravity"):GetItem("TeamA(1)"))
    ```
    
5. Write the following, since the gravity value should be also applied to TeamB.

    ```lua
    local teamBGravity = tonumber(teamStatusData:FindRow("Status", "Gravity"):GetItem("TeamB(2)"))
    ```

6. Add the new ChangeGravitySet() function to the **Workspace - MyDesk - 00_Player - 01_Player_Component - PlayerInGameStatus** component.
Add the new **amount**, a number-type parameter, and write as follows.

    ```lua
    [client]
    void ChangeGravitySet (number amount)
    {
        self.Entity.RigidbodyComponent.Gravity = amount
    }
    ```

7. Write the following under the PlayerSetting() function player.MovementComponent.JumpForce = teamAjump of the **InGameManager** component.

    ```lua
    player.PlayerInGameStatus:ChangeGravitySet(teamAGravity)
    ```

8. Write the following under the player.MovementComponent.JumpForce = teamBjump as the gravity should be also applied to TeamB.

    ```lua
    player.PlayerInGameStatus:ChangeGravitySet(teamBGravity)
    ```

# Adding Emoji
1. Select **Workspace - MyDesk - 00-Player - 03_Player_Emoticon - EmoticonData**.
2. Add the new row and add the key value and data. 

    | Key | RUID | Scale | PivotY | Explain |
    | --- | --- | --- | --- | --- |
    | Alpha6 |  c62a931bade449f0a6487e5bb2160469 | 1 | 1.2 | Slime |

3. Enter the preset RUID for a **Preset List - Monster**.

# Create Map
#### Creating Map, Specifying Bounds
1. The map that executes the game is an Instance Map, so you must enable the InstanceMap of MapComponent.
![INSTANCEMAP](https://mod-file.dn.nexoncdn.co.kr/bbs/1702034473605b61243e921a1491f82f209e23ad151bc.png "INSTANCEMAP")
2. Edit the UseCustomBound property of MapComponent. Restrict to use only specific areas of the map.
![CUSTOMBOUND](https://mod-file.dn.nexoncdn.co.kr/bbs/17020344485213310bbd9b8594f40b3b34bfba4152dbe.png{"width":"960px"} "CUSTOMBOUND")
    > **Tip**
    > You can check the coordinates of the map by pressing the Tab key. 

#### Placing Models
1. Place three models of the **Workspace - MyDesk - 02_InstanceRoom - 003_InGame - InGameObj**.

    * ASpawnLocation: Specify the location where Team A members will be spawned.
    * BSpawnLocation: Specify the location where Team B will be spawned. 
    * Target: The hole entity to be filled. As the right number of players should be spawned, you should place as many as the largest number in the EnableTargetNum row of MatchTeamData.


2. If you want to jump directly to the newly created map, modify the content of the LaunchRoom() function of the LobbyMatchManager component.
2.2. Specify and enter the map you want to move under the `local randomMapName = mapData(_UtilLogic:RandomIntegerRanger(1,mapCount):GetItem("MapName"))` of the random map specifying comment. 
LaunchRoom() has two 2.2 random map specifying comments. You need to modify both comments.

    ```lua
    randomMapName = "003_InGame_003"
    ```

<br>


