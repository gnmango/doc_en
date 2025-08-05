# Course Introduction
You can use RoomService to communicate between static and instance rooms within a single world instance. This guide is example-oriented, and the basic concepts are the same as with [WorldInstanceService](/apiReference?postId=1034{"target":"_self"}). 
We recommend that you study the [World Instance](/docs/?postId=984{"target":"_self"}), [World Instance Communication](/docs/?postId=999{"target":"_self"}), and [Creating Instance Maps](/docs/?postId=540{"target":"_self"}) guides first.

# Communicate with RoomService
Since static and instance rooms cannot communicate directly, the [RoomService](/apiReference?postId=1032{"target":"_self"}) function must be used to share events or data with each other. RoomService can create instance rooms and move users to them. You can also share a state between rooms within the same world instance, using the same shared memory. When using shared memory, you can create or obtain it using the `GetSharedMemory()` function. Use the `DeleteSharedMemoryAndWait()` or `DeleteSharedMemoryAsync()` function when deleting shared memory.

# Communication between Static Rooms and Instance Rooms
Let's create an example of checking the instance room state in real time in a static room. We'll change the state of the instance room between playing and waiting every 8 seconds. You can check the state of the changing instance room and share it with the static room. This example uses two Logic and Event Types.

![Example](https://mod-file.dn.nexoncdn.co.kr/bbs/1681289120098258bc9cbd81049859f1cb80d6adf314f.gif "Example")

#### Preparation
1. You need two maps. Use one as a static map and the other as an instance map.
    * **map01**: Static Map
    * **map02**: Instance Map

    > <span style="color: #7cafc2">**Tip**
    > You can use MapComponent's InstanceMap to designate it as an instance map.</span>
    > ![01](https://mod-file.dn.nexoncdn.co.kr/bbs/1681288311051aad86e9170a64def8052f805cdc11628.gif{"width":"260px"} "01")

2. Add two [Button] UIs in **[UI] - DefaultGroup** and rename them **Btn_Enter and Txt_RoomState** respectively.
    * Btn_Enter<br>![Btn_Enter](https://mod-file.dn.nexoncdn.co.kr/bbs/16813830576229c09ca4ff95f4e91950f82bcb34765bd.png{"width":"146px"} "Btn_Enter")
    * Txt_RoomState<br>![Txt_RoomState](https://mod-file.dn.nexoncdn.co.kr/bbs/1681383449551f8ad79acbf0c47d097d10110033af6fc.png{"width":"146px"} "Txt_RoomState")

#### Display Room State
1. In the context menu of **Workspace - MyDesk, click Create Scripts - Create Logic** to create a new **StaticRoomLogic**.
2. Add a new **ButtonEnter** property and change the return type to Entity. Connect **Btn_Enter**.
    ```lua
    Property:
    [None]
    Entity ButtonEnter = /ui/DefaultGroup/Btn_Enter
    ```
3. Write the following to show the button to go to the instance room only in the static room.
    ``` lua
    Method:
    [client only]
    void OnBeginPlay()
    {
        -- Activate the entry button entity only in static rooms.
        if _RoomService:IsInstanceRoom() == true then
        	self.ButtonEnter.Enable = false
        else
        	self.ButtonEnter.Enable = true
        end
    }
    ```

4. In the context menu of **Workspace - MyDesk, click Create Scripts - Create Logic** to create a new **InstanceRoomLogic**.
5. Add two properties as shown below.

    ```lua
    Property:
    [Sync]
    boolean IsPlaying = false
    [None]
    Entity TextRoomState = /ui/DefaultGroup/Txt_RoomState
    ```

6. Add the **OnBeginPlay** function to **InstanceRoomLogic**. Write the following to show only the button displaying the state of the instance room.

    ```lua
    Method:
    [client only]
    void OnBeginPlay ()
    {
        -- Activate state entities only in instance rooms.
        if _RoomService:IsInstanceRoom() == true then
        	self.TextRoomState.Enable = true
        else
        	self.TextRoomState.Enable = false
        end
    }
    ```

#### Display Instance Room State
Let's make the content of the button change according to the value of the IsPlaying property of InstanceRoomLogic. 
Since the logic is executed separately in the static and instance rooms, we need to transfer the changed IsPlaying value in the instance room to the static room. We'll write some code that changes the UI so we can transfer the IsPlaying value as shown below.
![01](https://mod-file.dn.nexoncdn.co.kr/bbs/168119241933126ff4ac0778142d291c516d5db5bd7d9.png "01")
1.  Add `OnUpdate` to StaticRoomLogic.
    ```lua
    [client only]
    void OnUpdate(number delta)
    {
        -- Execute only in static rooms.
        if _RoomService:IsInstanceRoom() == true then
        	return
        end
        
        -- Check the state of the instance room and update the button state.
        local isPlaying = _InstanceRoomLogic.IsPlaying
        self:UpdateButton(isPlaying)
    }
    ```
    
2. Add a new `UpdateButton` function and add a boolean type parameter **isPlaying**. Write the following so that the text displayed on the button can be changed according to the isPlaying state.
    ```lua
    [client]
    void UpdateButton(boolean isPlaying)
    {
        if isPlaying == true then
            local text = "No entry <color=red>(Playing)</color>"
            self.ButtonEnter.TextComponent.Text = text
            self.ButtonEnter.ButtonComponent.Enable = false
        else
            local text = "Entry <color=lime>(Waiting)</color>"
            self.ButtonEnter.TextComponent.Text = text
            self.ButtonEnter.ButtonComponent.Enable = true
        end
    }
    ```
3. Add the **OnUpdate** function to **InstanceRoomLogic**. 
    ```lua
    [client only]
    void OnUpdate(number delta)
    {
        -- Execute only in instance rooms.
        if _RoomService:IsInstanceRoom() == false then
        	return
        end
        
        -- -- Check the room state and update text.
        self:UpdateText(self.IsPlaying)
    }
    ```
4. Add the **UpdateText** function to **InstanceRoomLogic**. Add a boolean type isPlaying parameter.
    ```lua    
    [client only]
    void UpdateText(boolean isPlaying)
    {
        if isPlaying == true then
        	local text = "<color=red>Playing</color>"
        	self.TextRoomState.TextComponent.Text = text
        else
        	local text = "<color=lime>Waiting</color>"
        	self.TextRoomState.TextComponent.Text = text
        end
    }
    ```

#### Enter an Instance Room
Write the following so that the player who presses the enter button can enter the instance room.
1. Add a new **EnterInstanceRoom** to **StaticRoomLogic**. After selecting Add Parameter, add a string type userId parameter.
Use `RoomService` to get the key of the instance room and write the following so that the user can be sent to the instance room.
    ```lua
    [server]
    void EnterInstanceRoom(string userId)
    {
        local roomKey = "ExampleRoom"
        local mapName = "map02"
        
        _RoomService:GetOrCreateInstanceRoom(roomKey)
        _RoomService:MoveUserToInstanceRoom(roomKey, userId, mapName)
    }
    ```
2. Add **ButtonClickEvent** to Event Handler. Change it to **entity** and connect **Btn_Enter**.
Write the `EnterInstanceRoom` function to be executed in the static room as shown below.
    ```lua
    Event Handler:
    [entity: Btn_Enter(/ui/DefaultGroup/Btn_Enter)
    HandleButtonClickEvent(ButtonClickEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        local Entity = event.Entity
        ---------------------------------------------------------
        
        -- Execute only in static rooms.
        if _RoomService:IsInstanceRoom() == true then
        	return
        end
        
        local me = _UserService.LocalPlayer
        self:EnterInstanceRoom(me.Name)
    }
    ```
    
#### Share Instance Room State to Static Room
Let's create a new event and use that event to share the instance room's state with the static room.

1. In the context menu of **Workspace - MyDesk, click Create Scripts - Create Event** to create a new **RoomStateChangedEvent**.
2. Add a boolean type **IsPlaying** property.
    ```lua
    Property:
    boolean IsPlaying = false
    ```
3. Open **InstanceRoomLogic** and add **RoomBeginEvent** to the event handler. 
Write to change the **IsPlaying** state of the instance room once every 8 seconds on the server.
Use the `RoomService:RequestSendEventToRoomAndWait()` function to transfer the IsPlaying state to the static room.

    ```lua
    Event Hanlder:
    [service: RoomService]
    HandleRoomBeginEvent(RoomBeginEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: RoomService
        -- Space: Server
        ---------------------------------------------------------
        
        -- Parameters
        ---------------------------------------------------------
        
        -- Use RoomBeginEvent to perform the first time when an instance room is created.
        
        -- Execute only in instance rooms.
        if _RoomService:IsInstanceRoom() == false then
        	return
        end
        
        -- The server changes the play state once every 8 seconds.
        local timerFunc = function()
        	self.IsPlaying = not self.IsPlaying
        	
        	-- Send the IsPlaying value from the instance room to the static room using an event.
        	local evt = RoomStateChangedEvent(self.IsPlaying)
        	_RoomService:RequestSendEventToRoomAndWait(evt, _RoomService.StaticRoomKey)
        		
        end
        
        _TimerService:SetTimerRepeat(timerFunc, 8)
    }
    ```

5. Add **RoomStateChangedEvent** in **InstanceRoomLogic**. Press [self] to change to [service] and connect RoomService.
Write the following so that the static room can save the IsPlaying value it receives.
    ```lua
    [service: RoomService]
    HandleRoomStateChangedEvent(RoomStateChangedEvent event)
    {
        -- Parameters
        local IsPlaying = event.IsPlaying
        ---------------------------------------------------------
        
        -- The event occurs on the server.
        -- Execute only in static rooms!
        if _RoomService:IsInstanceRoom() == true then
        	return
        end
        
        -- The static room saves the IsPlaying value received from the instance room.
        self.IsPlaying = IsPlaying
    }
    ```

6. Add **RoomEndEvent**.
Write the following so that the IsPlaying state can be initialized when the instance room is destroyed.
    ```lua
    [service: RoomService]
    HandleRoomEndEvent(RoomEndEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: RoomService
        -- Space: Server
        ---------------------------------------------------------
        
        -- Parameters
        ---------------------------------------------------------
        
        -- Execute only in instance rooms.
        if _RoomService:IsInstanceRoom() == false then
        	return
        end
        
        -- Reset the state when the instance room is destroyed.
        self.IsPlaying = false
        local evt = RoomStateChangedEvent(self.IsPlaying)
        _RoomService:RequestSendEventToRoomAndWait(evt, _RoomService.StaticRoomKey)
    }
    ```

# Utilize Shared Memory
Let's add a new UI to the example world created above and make the content of the shared memory appear when entering the instance room.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16812899032225448790431404595a01853837b8e53a1.png "4")
1. Add a Text UI to DefaultGroup and name it **Txt_RoomTitle**.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1681289321574fd8a803ee75f4db9a94e2109a6a1acd2.png{"width":"344px"} "3")
2. Add a new Entity type **TextRoomTitle** property to **InstanceRoomLogic**. Connect the Txt_RoomTitle UI.
    ```lua
    Property:
    [None]
    Entity TextRoomTitle = /ui/DefaultGroup/Txt_RoomTitle
    ```

3. Write the following in OnBeginPlay of **InstanceRoomLogic** so that the Txt_RoomTitle UI is activated when entering the instance room.
    ```lua
    Method:
    [client only]
    void OnBeginPlay()
    {
        if _RoomService:IsInstanceRoom () == true then
            self.TextRoomState.Enable = true
            self.TextRoomTitle.Enable = true
        else
            self.TextRoomState.Enable = false
            self.TextRoomTitle.Enable = false
        end    
    }
    ```
    
4.  Add a string type **Title** property to **InstanceRoomLogic**.
    ```lua
    Property:
    [Sync]
    string Title = ""
    ```
5. Add the code below to the last line of the **UpdateText** function of **InstanceRoomLogic** so that the room name appears.
    ```lua
    self.TextRoomTitle.TextComponent.Text = self.Title
    ```

6. Before creating an instance room, we need to save the room name in shared memory. To do this, add the following content under `local mapName = "map02"` in **EnterInstanceRoom** function of **StaticRoomLogic**.

    ```lua
    local roomTitleVarName = "roomTitle"
    local roomTitle = "Hello, MapleStory Worlds!"
    
    -- Get shared memory.
    local code, mem = _RoomService:GetSharedMemory(roomKey)
    
    if code ~= SharedMemoryResultCode.OK then
    	error ("Failed to GetSharedMemory.")
    	return
    end
    
    -- Save the room title in a variable named 'roomTitle'.
    local setResult = mem:SetVariableAndWait(roomTitleVarName, roomTitle)
    
    if setResult.Code ~= SharedMemoryResultCode.OK then
    	error ("Failed to SetSharedVariableAndWait.")
    	return
    end
    ```

7. Add a new **ReadTitleFromSharedMemory** function to **InstanceRoomLogic**.

    ```lua
    [server only]
    void ReadTitleFromSharedMemory()
    {
        local memKey = "ExampleRoom"
        local varName = "roomTitle"
        
        local code, mem = _RoomService:GetSharedMemory(memKey)
        
        if code ~= SharedMemoryResultCode.OK then
        	error ("Failed to GetSharedMemory.")
        	return
        end
        
        local getResult = mem:GetVariableAndWait(varName)
        
        if getResult.Code ~= SharedMemoryResultCode.OK then
        	error ("Failed to GetSharedVariableAndWait.")
        	return
        end
        
        self.Title = getResult.Info.Value
        
        -- Delete the deprecated shared memory.
        -- When shared memory is deleted, all internal variables are also deleted.
        _RoomService:DeleteSharedMemoryAndWait(memKey)
    }
    ```

8. Add the following to the end of **HandleRoomBeginEvent** to read the title from shared memory.
    ```lua
    -- Read the title from shared memory.
    self:ReadTitleFromSharedMemory()
    ```