# Course Introduction
When creating a World, you sometimes want to create a separate space like a special dungeon that only certain users can enter. This is when you can utilize `RoomService`.
Let's look at `RoomService`, which creates an independent space that can be separated from other users and move users. To utilize instance maps, the concepts of static rooms and static maps will be explored together.

# Static Room
One World includes one <span style="color: #dc9656">**Static Room (Static Room)**</span>. A static map is created for a static room. Users can freely move between static maps. 

# Static Map
<span style="color: #dc9656">**A static map (Static Map)**</span> is created within the World instance. The default for creator-created maps in a World is a static map. When a World is created, a static map is also created.

# Instance Room
<span style="color: #dc9656">**Instance Room (Instance Room)**</span> is a basket to hold **instance maps**. Creators can create instances by including specific instance maps in the instance room. 
Even different instance rooms can be packed with instance maps of the same composition. This is because if rooms are configured with the same instance map configuration, a key is used to differentiate them into distinct instance rooms.

It is also possible to create different instance rooms at the same time. If an instance room has already been created, even if a new user connects to the static room, the created instance room will not be affected in any way. This is because after creating an instance room and moving the user to the instance room, only the information of the instance room is loaded.  

In order to send users to the instance map of the instance room, they must first go through the static map of the static room. You can specify MapName as the static map to return to when you leave the instance room and return to the static map. If you haven't decided on a static map to return to, you'll be taken back to your start map.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/167220613027481fbeaab9552405b9d54bf9c2d7e9ed3.png{"width":"740px"} "1")
#### Key
Because different instance rooms can be created, even though they are composed of the same instance map, an <span style="color: #dc9656">**identification key**</span> is required to distinguish instance maps. The <span style="color: #dc9656">**Key's**</span> intended use is manipulation.

# Instance Map 
The map defaults to a static map. If the creator wants to use a specific map as an instance map, the **InstanceMap** property of the Map's **MapComponent** must be <span style="color: #dc9656">**true**</span>. 

# RoomService
`RoomService` can create or destroy instance rooms or transfer users to a specific instance room. For detailed information, please refer to [RoomService](/apiReference/Services/RoomService{"target":"_self"}).

#### Move to Instance Room
Users can move between instance maps created within an instance room. However, you cannot move from one instance room to another instance room.
Also, you cannot move between a static room and an instance room except via `RoomService`. For example, even if you create a portal on an instance map that can be moved to a static map, it is normal to not be able to move it. 
Let's take a look at the diagram below. Users can freely move within Maps 01, 02, and 03 created in the static room. If you want to move specific users from the static map Map01 to Instance Room A, use `RoomService:MoveUsersToInstanceRoom()`. Users who moved to Instance Room A can move within map 04, 05, 06, 07, which is an instance map created in the room. However, they cannot go directly from Instance Room A to Instance Room B. If you want to move from Instance Room A to InstanceRoom B, you have to go back to static map Map 01 and move to another instance room via `MoveUsersToInstanceRoom()`. 
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/167514939683374cba8eb14d44671bed868c1e16516a3.png{"width":"880px"} "3")
#### Accessing Data from Instance Rooms
Static Rooms and Instance Rooms are separate spaces and cannot access each other's data. Therefore, entries such as Service and Logic exist in each space.
One World uses one DataStorage. So, keep in mind that both static rooms and created instance rooms use the same DataStorage.

# Use Example
Among the users in the waiting room, let's move 3 users who pressed the Ready button to the map where the game will be played.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16728923078067133fe9f934c494fb88daec6cd7da953.png "4")
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1672892340962e23f260871484cac83c21fcfefaadb2c.gif "1")
#### Ready
This example uses the start map as the static room to send to the instance map, and the static room to return.
1. Create waiting maps and instance maps.  
    * Static Map Name: waitingmap
    * Instance Map Name: dungeon
![instancemaps](https://mod-file.dn.nexoncdn.co.kr/bbs/1638498213055cf9e5651d3744ef987655fc87ef9add2.png "instancemaps")
2. To use the dungeon map as the instance map, in the Property Editor window, activate the <span style="color: #dc9656">**MapComponent's InstanceMap property**</span> ![Editbox_Check](https://mod-file.dn.nexoncdn.co.kr/bbs/16346176407708cb3de01eaaf48a68ab2dd6fe1b1183f.png "Editbox_Check").<br>![InstanceMap](https://mod-file.dn.nexoncdn.co.kr/bbs/1679381963752d7fcf722dd3543888ed05edabe847044.png "InstanceMap")
3. Create the Ready button by using the ![Tool_UI](https://mod-file.dn.nexoncdn.co.kr/bbs/163453120840744616a62243642e889159a68a78a56c2.png{"width":"20px"} "Tool_UI") UI Editor - Left Default Tool - ![button](https://mod-file.dn.nexoncdn.co.kr/storage/icons/UI/icon_button.png "button") button to **DefaultGroup**.
![instance05](https://mod-file.dn.nexoncdn.co.kr/bbs/1638498359187950b047e63934e33a1a76ac3dbef58cf.png "instance05")

><span style="color: #7cafc2">**Tip**
> You need several users to test the RoomService.
> Press ![Common_SoundPlay](https://mod-file.dn.nexoncdn.co.kr/bbs/1635317657654c59c47ffc44d414db579b8d2fc0715a8.png "Common_SoundPlay")[Start] and click the ![Common_MultiPlayer](https://mod-file.dn.nexoncdn.co.kr/bbs/163453813155295c09654c5824a6d825788e3d97c4b7d.png "Common_MultiPlayer") Add a MultiPlayer button to add users as necessary.</span>
 #### Creating an Instance Room
Create an instance room using the `GetOrCreateInstanceRoom()` function.
1. Create <span style="color: #dc9656">**GameManager script component**</span> and add it to <span style="color: #dc9656">**waitingmap**</span>.<br>![instance02](https://mod-file.dn.nexoncdn.co.kr/bbs/1638498169847ea785d529504411b94811514d2fe0c42.png "instance02")
2. Compose a script to create an instance room at <span style="color: #dc9656">**GameManager Script Component**</span> as follows.
    ```lua
    Property:
    [None]
    integer roomIdx = 0
    
    Method:
    any GetOrCreateInstanceRoom ()
    {
        -- Use RoomService to create an Instance Room and increase roomIdx by 1.
        -- Use roomIdx as a key to create an Instance Room so that a brand new InstanceRoom is created each time.
        local instanceRoom = _RoomService:GetOrCreateInstanceRoom("DungeonMap"..tostring(self.roomIdx))
        self.roomIdx = self.roomIdx + 1
        return instanceRoom
    }
    ```
#### Moving to Particular Instance Room 
Now let's set the conditions for the users who can enter the game-playing instance room. You can set the conditions to match your intent.
In the example, <span style="color: #dc9656">**3 or more users who pressed the Ready button**</span> will be moved to a new instance room.

1. Compose a script at <span style="color: #dc9656">**GameManager Script Component**</span> that regularly checks user's Ready state to move the user to an instance room. 
    ```lua
    Property:
    [None]
    number checkReadyPlayerTimer = 0
    [None]
    integer playerNumPerGame = 0
    
    Method:
    [server only]
    void OnUpdate(number delta)  
    {
        self.checkReadyPlayerTimer = self.checkReadyPlayerTimer + delta
        -- Checks players in Ready state at a set interval.
        if self.checkReadyPlayerTimer >= 5.0 then
            self.checkReadyPlayerTimer = 0.0
             
            -- Gets the number and names of players in Ready state.
            local readyPlayerNum = 0
            local readyPlayers = {}
                for k, v in pairs(_UserService.UserEntities) do
                if v.Player.isReady == true then
                    readyPlayerNum = readyPlayerNum + 1
                    readyPlayers[#readyPlayers+1] = v.Name
                end
            end
            
            -- If the number of players in Ready state is more than the set value, execute logic to move the players to a new Instance Room.
            if readyPlayerNum >= self.playerNumPerGame then
    
                -- Repeat as many times as the number of Instance Rooms you need to make.
                for i = 1, math.floor(readyPlayerNum / self.playerNumPerGame) do
    
                    -- Creates an Instance Room.
                    local instanceRoom = self:GetOrCreateInstanceRoom()
                    local toSendPlayers = {}
    
                    -- Places in each Instance Room.
                    for j = 1, self.playerNumPerGame do
                        local idx = (i-1) * self.playerNumPerGame + j
                        toSendPlayers[#toSendPlayers+1] = readyPlayers[idx]
                    end
    
                    -- Uses players' information to send them to the corresponding Instance Room.
                    _RoomService:MoveUsersToInstanceRoom(instanceRoom.InstanceKey, toSendPlayers)
                end
            end
        end
    }
    ```

2. In **waitingmap**'s Property Editor, enter 3**</span> to GameManager's <span style="color: #dc9656">**playerNumPerGame.
![instance14](https://mod-file.dn.nexoncdn.co.kr/bbs/1638779042401ec936b5ae88046b085ded53f0bae7132.png "instance14")
 
3. Create the <span style="color: #dc9656">**ReadyButton**</span> script component and add it to the <span style="color: #dc9656">**Ready UI button**</span>.<br>When the user is in the Ready state, it sends information to the server that the user is ready.
    ```lua
    Property:
    [None]
    boolean isReady = false
    
    Event Handler:
    [client only] [Entity: UIButton(/ui/DefaultGroup/UIButton)] 
    HandleButtonClickEvent(ButtonClickEvent event) 
    {
        -- Parameters
        local Entity = event.Entity
        --------------------------------------------------------
         
        -- If the player was already in a Ready state upon clicking the button, cancel the Ready state, modify the UI text, and notify the server.
        if self.isReady  == true then
            self.isReady = false
            self.Entity.TextComponent.Text = "Ready"
            _UserService.LocalPlayer.Player:OnCancelReady()
        -- If the player was not in a Ready state upon clicking the button, change to a Ready state, modify the UI text, and notify the server.
        else
            self.isReady = true
            self.Entity.TextComponent.Text = "Cancel"
            _UserService.LocalPlayer.Player:OnReady()
        end
    }
    ```   

5. Create <span style="color: #dc9656">**Player script component**</span> and add it to ![workspace_MyAvatar](https://mod-file.dn.nexoncdn.co.kr/bbs/16346008379922a9b756c3912461db7195808cb554abd.png "workspace_MyAvatar") DefaultPlayer.<br> Synchronize the player's ready status information to the server to check the current user status.
    ```lua
    Property:
    [None]
    boolean isReady = false
    [None]
    number roomIdx = 0
    [None]
    any readyButton = nil
    
    Method:
    [client only]
    void OnBeginPlay() 
    {
        wait(1)
        self.readyButton = _EntityService:GetEntity("Entity ID") -- Enter the Entity ID of readyButton.
        -- The Ready button will not be shown once moved to Instance Room.
        if _UserService.LocalPlayer.CurrentMapName == "dungeon" then
            self.readyButton.Enable = false
        end  
    }
    
    [server]
    void OnReady() 
    {
        self.isReady = true
    }
    
    [server] 
    void OnCancelReady()
    {
        self.isReady = false
    }
    ```
    
#### Moving to Static Room
Let's use the `MoveUsersToStaticRoom()` function to return users to the static room after the game. If you haven't decided on a static map to return to, you'll be taken back to your start map.
* Create and compose <span style="color: #dc9656">**InstanceGameManager script component**</span> and add the script component to dungeon map.
    ```lua
    Method:
    [server only]
    void OnBeginPlay()
    {
        local playFunc = function()
            local users = {}
            -- Gets players in current map.
            for k, v in pairs(_UserService.UserEntities) do
                users[#users+1] = v.Name
            end
            -- Returns the players to the static room.
            _RoomService:MoveUsersToStaticRoom(users)
        end
         
        -- Executes the playFunc function 10 seconds after the game has begun.
        _TimerService:SetTimerOnce(playFunc, 10.0)
    }
    ```

##### Reference Guide
* [UserService to Find User Entities]
* [EntityService to Search for Entities]
* [World Instance]