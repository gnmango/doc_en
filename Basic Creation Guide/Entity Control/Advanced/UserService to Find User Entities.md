# Course Introduction
This course explains how to use `UserService` to get **UserEntity** in context.
Let's use a simple script example to examine the functions of `UserService`.

# Before We Begin
The following guides will be useful in understanding this course.
[Server and Client]
[Execution Space Control]
[Event System]
[Entity Event System]

# Ready for Examples?
First, click **Create Scripts - Create Component** in the context menu of **Workspace - MyDesk** to create a new script component. Then, change the name to <span style="color: #dc9656">**Test**</span>. Add the **Test** component to the **Hierarchy - Common** entity.
![UserService_2](https://mod-file.dn.nexoncdn.co.kr/bbs/1659503134261e00f6eb7162b47738b3b6a65843ebbf6.png "UserService_2")
<br>
Double-click the **Test** component to open the Script Editor.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1635462948231d6af9c28df8e41f599fc4b0a19942bd9.png "3")
<br>
Now let's go through the example and learn about `UserService`.

# User Entrance/Exit Event
When a user enters or leaves the world, the entry and exit events occur in `UserService`.
You can add additional processing at the time of user entry and exit by adding an event handler to the script component.

### UserEnterEvent
This event occurs when a user enters the world.
You can add processing when the user enters by adding an event handler in the script component.

The following is an example of outputting the **UserId** of the entered user as a log when the user enters the world.
Through this, we'll learn how to add and utilize the **UserEnterEvent** handler.
You can add the **UserEnterEvent** handler as follows.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/163697876177574026aa5b0a140b5a611de58c3191e29.png "4")
<br>
The added handler receives the **event** as a parameter, and through this, it receives the **UserId** that entered the current game. 
If you write the code as follows, whenever a user enters the game, the **UserId** is output to the Console window.
```lua
Event Handler:
[service: UserService]
HandleUserEnterEvent (UserEnterEvent event)
{
    -- Parameters
    local UserId = event.UserId
    --------------------------------------------------------
    -- Output the UserId of the entering user to the Console window
    log("User Enter! : "..UserId)
}
```
<br>
Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and test.
Add a new client as shown below.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/163546298427140ee964bea9b4eaaa80eab3d2d65555d.png "7")
<br>
Logs are output in the Console window when a new user enters the game.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1672227283289a41676c3ff0c4a90907a28f386e37728.png "6")

### UserLeaveEvent
This event occurs when a user exits the world.
The following is an example of outputting the **UserId** of the exiting user to the Console window as a log when the user exits the world.
Add the **UserLeaveEvent** handler and compose the code as follows.

```lua
Event Handler:
[service: UserService]
HandleUserLeaveEvent (UserLeaveEvent event)
{
    -- Parameters
    local UserId = event.UserId
    --------------------------------------------------------
    -- Output the UserId of the exiting user to the Console window
    log("User Leave! : "..UserId) 
}
```
<br>
Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test.
We need to check the log output when the user leaves, so we add a client.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/163546298427140ee964bea9b4eaaa80eab3d2d65555d.png "7")
<br>
After a new user enters, exit the new user by closing the added client again. When the user exits, check that the log is output as shown below.
![13](https://mod-file.dn.nexoncdn.co.kr/bbs/167222742821522c171b011e14d8783129decfdaa0f35.png "13")

# User Disconnect/Reconnect Event
If the user's network is unstable, the connection may be lost. They can also reconnect again. 
Like this, disconnect/reconnect events occur in `UserService`. You can add a separate process when a user disconnects or reconnects by adding a corresponding event handler in the script component.

### UserDisconnectEvent
This event occurs when the user's network connection is disconnected due to instability.
You can add processing when the connection is lost by adding a corresponding event handler in the script component.

The following is an example of outputting the disconnected **UserId** and the time.
Let's add **UserDisconnectEvent** to the event handler through the script below.
```lua
Event Handler:
[service: UserService]
HandleUserDisconnectEvent (UserDisconnectEvent event)
{
    -- Parameters
    local DisconnectMapName = event.DisconnectMapName
    local TimeNetworkClosed = event.TimeNetworkClosed
    local UserId = event.UserId
    --------------------------------------------------------
    log("UserDisconnectEvent : "..UserId.." : "..DisconnectMapName.." : "..tostring(TimeNetworkClosed))
}
```
<br>
If written as above, the log will output to the Console window when the user is disconnected.
![disconnect](https://mod-file.dn.nexoncdn.co.kr/bbs/1672228212685cf51c1549431482d912ea1a35a6926f5.png "disconnect")

### UserReconnectEvent
This event occurs when the user reconnects.
You can add processing when reconnection occurs by adding the corresponding event handler in the script component.

The following is an example of outputting the reconnected **UserId** and the reconnection time.
Let's add **UserReconnectEvent** to the event handler as below.
```lua
Event Handler:
[service: UserService]
HandleUserReconnectEvent (UserReconnectEvent event)
{
    -- Parameters
    local ReconnectMapName = event.ReconnectMapName
    local TimeNetworkClosed = event.TimeNetworkClosed
    local UserId = event.UserId
    --------------------------------------------------------
    log("UserReconnectEvent : "..UserId.." : "..ReconnectMapName.." : "..tostring(TimeNetworkClosed))
}
```
<br>
If written as above, the log will be output to the Console window when the user reconnects.
![reconnect](https://mod-file.dn.nexoncdn.co.kr/bbs/16722281637717ebb64a50b5841c290651809013e8fb8.png "reconnect")

> <span style="color: #585858">**Learn More**
> Disconnect and reconnect situations are difficult to test in general. 
> Please note that you can leave a log by writing the code as in the example above.</span>

# Importing Entities for All Users in Game
### Dictionary<string, MODEntity> UserEntities
**UserService** provides the **UserEntities** property that retrieves all user entities participating in the game.
**UserEntities** is a Dictionary type. It returns **UserId - MODEntity** as **Key - Value**.
The following is an example that loops through all players and outputs **Entity Name** whenever a user enters the game.
Modify **UserEnterEvent** as follows.
```lua
Event Handler:
[service: UserService]
HandleUserEnterEvent (UserEnterEvent event)
{
    -- Parameters
    local UserId = event.UserId
    --------------------------------------------------------
    --UserService receives all users as Dictionary
    local AllUserEntitiesDic = _UserService.UserEntities                    
    for userId, playerEntity in pairs(AllUserEntitiesDic) do
        --Sort through all users and output userId and Entity.Name
        log("userId : playerEntity = "..userId.." : "..playerEntity.Name)
    end
}
```
<br>
Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test.
**Key** values, **UserId** and **MODEntity.Name** are output to the Console window.
![15](https://mod-file.dn.nexoncdn.co.kr/bbs/167221497602480f42eb437ee456fa0b18f0d9e7192a3.png "15")
<br>
Let's click the Add Client button.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/163546298427140ee964bea9b4eaaa80eab3d2d65555d.png "7")
<br>
Whenever a new user enters the game, it sorts through all users participating in the game, and outputs **UserId** and **MODEntity.Name**.
![17](https://mod-file.dn.nexoncdn.co.kr/bbs/1672228433360de5251fa28be4d35b3717ba6ad4d5ce6.png "17")

# Importing Information for All Users in the Game
### Dictionary<string, User> Users
**UserService** provides the **Users** property that retrieves the information on all users participating in the game.
**Users** returns the **UserId, profile name (nickname), and profile code** of all the users.
The example below traverses through all users and outputs user information whenever a user enters the game.
Modify **UserEnterEvent** as follows:

```lua
Event Handler:
[service: UserService]
HandleUserEnterEvent (UserEnterEvent event)
{
    -- Parameters
    local UserId = event.UserId
    --------------------------------------------------------
    local users = tostring(#_UserService.Users).."\n"
    for i, j in pairs(_UserService.Users) do
     users = users..j.UserId.." "..j.Nickname.." "..j.ProfileCode.."\n"
    end
    
    log(users)
}
```

Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and test.
**The number of users, UserIds, profile names, and profile codes** are output on the Console window.
![001](https://mod-file.dn.nexoncdn.co.kr/bbs/16841317926502cfd39a5736348c085c6ed944866d293.png 01")

Press the Add Client button and make sure that all the user information is output each time a new user enters the game.
![002](https://mod-file.dn.nexoncdn.co.kr/bbs/16841318084419983ed6428cd40b18fe37140c1cab7ca.png 02")

# Loading Number of Users in Game
### Number GetUserCount()
The `GetUserCount()` function returns the number of users currently participating in the game.
Next example is outputting the number of all users in-game whenever a user enters the game. 
Modify **UserEnterEvent** as follows.
```lua
Event Handler:
[service: UserService]
HandleUserEnterEvent (UserEnterEvent event)
{
    -- Parameters
    local UserId = event.UserId
    --------------------------------------------------------
    local userCount = _UserService:GetUserCount()
    log("CurrentUserCount : "..userCount)
}
```
<br>
Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test.
**CurrentUserCount** increases in the Console window every time you add a client.
![19](https://mod-file.dn.nexoncdn.co.kr/bbs/1672215824554745c215f00d04ee39a610cf32aed3948.png "19")
# Importing All Users in a Certain Map
`UserService` provides a function to import users from a certain map.
<br>
### Array GetUsersByMapName(string mapName)
The function uses **mapName** to return an array of user entities from a certain map. Pass **mapName** as a string. Returned array values can be iterated to import entities for individual users.
<br>
The following code uses the `GetUserByMapName()` function to import all users in map01.
Modify **UserEnterEvent** as follows.
```lua
Event Handler:
[service: UserService]
HandleUserEnterEvent (UserEnterEvent event)
{
    -- Parameters
    local UserId = event.UserId
    --------------------------------------------------------
    local mapName = _EntityService:GetEntityByPath("/maps/map01").Name
    local playersArr = _UserService:GetUsersByMapName(mapName)
    for index, player in pairs(playersArr) do
        log("PlayerName : CurrentMap = "..player.Name.." : "..player.CurrentMap.Name)
    end
}
```
<br>
Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test.
Whenever a user is added, the names of all users in map01 and the map name where they are currently located are output.
![21](https://mod-file.dn.nexoncdn.co.kr/bbs/1672216028360db04c07342b346e986b136760d153527.png "21")
<br>
><span style="color: #585858">**Learn More**
><br>
> For more precise results, you can add maps and clients to run the test.</span>
> To learn about creating maps and connecting portals, refer to [Creating and Managing Maps](https://mod-developers.nexon.com/docs?postId=61{"target":"_self"}).
>1. Add map02, and a portal that connects map01 and map02.
> ![UserService_22](https://mod-file.dn.nexoncdn.co.kr/bbs/16346262603985e23a20893c549ff9c1f1994e4cb9b3e.png "UserService_22")
><br>
> 2. Add three more clients during play. Place two users in map01 and the other two in map02.
> ![UserService_23](https://mod-file.dn.nexoncdn.co.kr/bbs/1634626281994fff64361306f49d1ac1da0670453e014.png "UserService_23")
> <br>
> 3. Put each user in different maps, then add users one by one.
>Confirm that all user names in map01 and the map name appear whenever a new user is added.
> ![24](https://mod-file.dn.nexoncdn.co.kr/bbs/167222862898698a874a1c74848cda4a51f1960180331.png "24")

<br>
### Array GetUsersByMapComponent(MapComponent mapComponent)
`GetUsersByMapComponent()` is another function that imports user entities from a certain map.
Pass **mapComponent** as a parameter, not the **mapId** of a specific map.
<br>
The following code uses `GetUsersByMapComponent()` to import all users in map01.
Modify **UserEnterEvent** as follows.
```lua 
Event Handler:
[service: UserService]
HandleUserEnterEvent (UserEnterEvent event)
{
    -- Parameters
    local UserId = event.UserId
    --------------------------------------------------------
    -- Receive the components of map01.
    local mapComponent = _EntityService:GetEntityByPath("/maps/map01").MapComponent    
    -- Pass mapComponent as a parameter to receive the users in map01 as an array.
    local playersArr = _UserService:GetUsersByMapComponent(mapComponent)
    for index, player in pairs(playersArr) do
       log("PlayerName : CurrentMap = "..player.Name.." : "..player.CurrentMap.Name)         
    end
}    
```
<br>
Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and test.
Whenever a user is added, the names of all users in map01 and the map name where they are currently located are output.
![26](https://mod-file.dn.nexoncdn.co.kr/bbs/16722287638923d83b1cd8b494f378f926520c621dce8.png "26")
# Importing Player Entity with UserId
### MODEntity GetUserEntityByUserId(string userId)
`GetUserEntityByUserId()` is the function to return the corresponding player entity through the user ID.
<br>
You might want to import a player entity only with the player's **UserId**. You can use **UserEntities** to import all users and itinerate to find an entity with the same ID. However, it is somewhat inefficient.
<br>
`GetUserEntityByUserId()` is a function that can be used in this situation. Since you only need to pass the user ID as a parameter, you can easily receive the desired user entity.
The following code uses the **UserId** of the user who entered the game, imports the user entity, and outputs the name.
```lua
Event Handler:
[service: UserService]
HandleUserEnterEvent (UserEnterEvent event)
{
    -- Parameters
    local UserId = event.UserId
    --------------------------------------------------------
    local EnterdPlayerEntity = _UserService:GetUserEntityByUserId(UserId)
    log("EnterdPlayerEntityName : "..EnterdPlayerEntity.Name)  
}
```
<br>
Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test.
The name of the user who most recently entered is output whenever a new client is added.
![28](https://mod-file.dn.nexoncdn.co.kr/bbs/1672228927792d3bae34457444e4c80d92776b8025571.png "28")
# Importing My Player (Client-only)
### MODEntity LocalPlayer
The **LocalPlayer** property can import my user entity within the client.
The concept of my character on a particular client is valid only on that client. Therefore, it cannot be used on the server and must only be used on the client.
<br>
Since **UserEnterEventType** only occurs in servers, we'll add a function that can be executed in the client, so we can write code that will output your character's name whenever a user enters the game.
```lua
Method: 
[client]
void PrintLocalPlayerName()
{
    -- Load my user from client.
    local MyPlayer = _UserService.LocalPlayer   
    log("MyPlayerName : "..MyPlayer.Name)
}
Event Handler: 
[service: UserService]
HandleUserEnterEvent (UserEnterEvent event)
{
    -- Parameters
    local UserId = event.UserId
    --------------------------------------------------------
    --Since the event occurs in server, call the client function that can load LocalPlayer.
    self:PrintLocalPlayerName()
}
```
Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test.
No matter which user enters, the output is the current character name in the current client.
![30](https://mod-file.dn.nexoncdn.co.kr/bbs/1672229156613a492a8ce464c496ab49c4a0fb15c5779.png "30")
<br>
> <span style="color: #585858">Learn More
> To learn about executing functions in clients and servers, refer to [Execution Space Control](https://mod-developers.nexon.com/docs?postId=210{"target":"_self"}).</span>