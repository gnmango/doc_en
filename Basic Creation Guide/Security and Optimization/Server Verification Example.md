# Course Introduction
When the player entity calls a server function, it sends data via 'packets'. When sending important data in packets, the server should validate the received packets one more time to improve security, rather than reflecting them immediately.
In this guide, we'll learn how to write a server verification script and configure a stable server.

##### Reference Guide
* [Preparation for Packet Modulation]
* [WorldConfig]

# Precautions for Verification
You must add a server verification device to prevent client modulation and packet modulation. When creating a World, not all decisions need to be made on the server, but decisions about important data should be made on the server. Even if the request is sent to the server after being validated on the client, it is best to re-validate on the server if the data is important.
For UI entities, it is best to send UI behaviors from the server to the client and implement them to be reflected, since they only exist on the client. You should also avoid putting important data for the World directly in packets sent from the client. You should avoid putting important data including gold and experience points in packets sent from the client.

# Example Server Configuration to Prevent Packet Modulation
Let's create a system that interacts with NPCs to exchange keys for Meso. Understand the number of cases where the exchange could be modulated and add a verification method. 

#### Creating UI
1. Add 2 items of **UISprite, UIText** to DefaultGroup to create a UI that shows the amount of gold and keys you have. 
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1706514331040538a923e7d024f43a83255f2192034cc.png{"width":"340px"} "2")
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1713870204930fa91eb3313cd42b5932269d713784fe1.png{"width":"340px"} "3")
2. Add **UISprite, UIInputText, UIText** to DefaultUI to create a UI for the exchange and rename it as below.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/170607103930210c9ec96e8d7461392a7468ded173cda.png{"width":"340px"}  "4")
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1713924692487c458e7057dcf4f598ab8fae58a1c1e3d.png{"width":"340px"} "5")
3. Untick **Enable** in the UI for the exchange you created to hide it.

#### Placing NPC
1. Place the **NPC** preset in the Scene and add **TriggerComponent**.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1706070955652fdf1e41bb11243df8e185796b7f9c95a.png{"width":"340px"} "1")
2. In **CollisionGroup**, add a new **Npc** group. Set the Matrix as below so that the Npc group can conflict with Npc, Default.
![npc](https://mod-file.dn.nexoncdn.co.kr/bbs/1706514288932b68b705f4d6e49df8b8be935dc051b67.png "npc")
3. Change **CollisionGroup** of the Npc's TriggerComponent to **Npc**.
#### Write Client Functions
Let's make an NPC that can interact with the player. Make a UI that will open when the player is in trigger range of the NPC if the `E` key is pressed and close if the `ESC` key is pressed. 

1. Create a new **MyPlayerComponent** and add it to **DefaultPlayer**.
2. In **MyPlayerComponent**, write as below.

    ```lua
    Property:
    [None]
    Entity triggerNpcEntity = nil
    [Sync]
    number key = 0
    [None]
    number lastExchangeTime = 0
    [None]
    number talkableDistance = 1
    [Sync]
    number gold = 0
    [None]
    Entity goldUI = /ui/DefaultGroup/GoldUI
    [None]
    Entity keyUI = /ui/DefaultGroup/KeyUI
    
    Method:
    [client only]
    void OnSyncProperty(String name, any value)
    {
        if name == "key" then
            -- Update the key UI
            self.keyUI.TextComponent.Text = tostring(self.key)
        end
        
        if name == "gold" then
            -- Update the gold UI
            self.goldUI.TextComponent.Text = tostring(self.gold)
        end
    }
    
    Event Handler:
    [service: InputServie]
    HandleKeyDownEvent(KeyDownEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: InputService
        -- Space: Client
        ---------------------------------------------------------
        -- Parameters
        local key = event.key
        ---------------------------------------------------------
        
        -- Check that it is a local player.
        if self.Entity ~= _UserService.LocalPlayer then
            return
        end
        
        -- Confirm the key input.
        if key == KeyboardKey.E then
            -- Check whether there is an NPC you can talk to
            if self.triggerNpcEntity == nil then
                log('no trigger')
                return
            end
            
            self.triggerNpcEntity.NpcComponent:OnRequestOpenDialog(true)
        elseif key == KeyboardKey.Escape then
            if self.triggerNpcEntity ~= nil then
                self.triggerNpcEntity.NpcComponent:OnRequestOpenDialog(false)
            end
        end
    }
    
    [client only][self]
    HandleTriggerLeaveEvent(TriggerLeaveEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: TriggerComponent
        -- Space: Server, Client
        ---------------------------------------------------------
        log('triggerLeave')
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        ---------------------------------------------------------
        if self.triggerNpcEntity == TriggerBodyEntity then
            self.triggerNpcEntity.NpcComponent:OnRequestOpenDialog(false)
            self.triggerNpcEntity = nil
        end
    }
    
    [client only][self]
    HandleTriggerEnterEvent(TriggerEnterEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: TriggerComponent
        -- Space: Server, Client
        ---------------------------------------------------------
        log('triggerEnter')
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        ---------------------------------------------------------
        if TriggerBodyEntity.TriggerComponent.CollisionGroup == CollisionGroups.Npc then
            self.triggerNpcEntity = TriggerBodyEntity
        end
    }
    ```

2. Create a new **NpcComponent** and add it to the **NPC** entity.
3. Write as below in **NpcComponent** so that you can exchange keys for gold when the Exchange button is pressed.

    ```lua
    Property:
    [None]
    Entity DialogEntity = ui/DefaultGroup/DialogBackground
    [None]
    TextInputComponent QuantityInput = ui/DefaultGroup/DialogBackground/Dialog/Count
    
    Method:
    [client]
    void OnRequestOpenDialog (boolean isOpen)
    {
        self.DialogEntity.Enable = isOpen
    }
    
    Event Handler:
    [entity: ExchangeButton(/ui/DefaultGroup/DialogBackground/Dialog/ExchangeButton)]
    HandleButtonClickEvent(ButtonClickEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
        -- Parameters
        local Entity = event.Entity
        ---------------------------------------------------------
        
        -- Check the cooldown.
        local myPlayerComponent = _UserService.LocalPlayer.MyPlayerComponent
        if DateTime.UtcNow.Elapsed - myPlayerComponent.lastExchangeTime < 1000 then
            log('Less than 1 second has passed since the previous request')
            return
        end
        
        local keyQuantity = tonumber(self.QuantityInput.Text)
        if keyQuantity > 0 then
            myPlayerComponent.lastExchangeTime = DateTime.UtcNow.Elapsed
            
            myPlayerComponent:VerifyExchangeKeyToGold(keyQuantity, myPlayerComponent.triggerNpcEntity)
        end
    }
    ```

#### Elements of Server Function Verification
Let's look at an example of server verification when a player with a key item interacts with an NPC who exchanges the key for Meso.
There are 4 server verification factors to check when the player exchanges a key for 1,000 Meso with an NPC.

1. **Verifying that the request was sent from a non-local client.**
There are two ways to verify.
The first is to use the [WorldConfig]'s **PlayerEntityAuthority**. The second is to use the **senderUserId** to verify that the player entity that sent the packet matches the player entity that requested the exchange.
Use the senderUserId to verify that the player entity that sent the packet matches the player entity that requested the exchange.

    ```lua
    -- Verify that the request was sent from a non-local client.
    if senderUserId ~= self.Entity.PlayerComponent.UserId then
        log('Request is sent from a non-local client')
        return
    end
    ```

2. **Verifying if there are an unusually high number of repeat requests.**
The client checks for the **cooldown time**, just in case packets are sent randomly over and over again. It also needs to be checked again on the server. 
If the server function is called when the time specified by the creator has not passed, it may be an abnormal packet. Because a normal client would not call the server function until the cooldown time has passed.


    ```lua
    -- Add a cooldown time to verify that requests have not been sent in an abnormally repetitive manner.
    if DateTime.UtcNow.Elapsed - self.lastExchangeTime < 1000 then
        log('Less than 1 second has passed since the previous request.')
        return
    end
    ```

3. **Verify that the player entity is in the location where the NPC can be triggered.**
Write to send packets when the player entity is in the NPC trigger location.
The server calculates the distance between the player and the NPC again, just in case the `ExchangeKeyToGold()` function is called from a location where the trigger cannot be triggered to send packets. If the player entity is at a distance where it is impossible to trigger but a packet request is received, it may be an unusual packet request.

    ```lua
    -- Verify that the player entity is in the location where the NPC can be triggered.
    if npcEntity == nil then
        log('No trigger npc')
        return
    end

    local playerPos = self.Entity.TransformComponent.WorldPosition
    local npcPos = npcEntity.TransformComponent.WorldPosition
    local distance = Vector2.Distance(Vector2(playerPos.x, playerPos.y), Vector2(npcPos.x, npcPos.y))

    if distance > self.talkableDistance then
        log("The player is not within the trigger range. It is an abnormal packet.")
        return
    end
    ```

4. **Verify that the number of keys requested for exchange is not less than the number of keys the player has.**
Check the exchange conditions again to make sure that it is impossible to exchange if the player presses the Exchange button but has fewer keys than the number of keys requested.

    ```lua
    -- Verify that the number of keys requested for exchange is not less than the number of keys the player has.
    if self.key < keyQuantity then
        log('The player doesn't have enough keys.')
        return
    end
    ```

#### Write Server Functions
Let's check the above 4 verification factors, and write server functions that can exchange keys with Meso.

1.  Open `MyPlayerComponent` and write the `OnBeginPlay()` function. Write as below to give 10 keys and reset the gold to 0 when the player enters the World.

    ```lua
    [server only]
    void OnBeginPlay()
    {
        -- Set the values of the key and gold.
        self.key = 10
        self.gold = 0
    }
    ```

2. Write a new `VerifyExchangeKeyToGold()` function in **MyPlayerComponent**. Set the execution space control as **server** and add **keyQuantity, npcEntity** parameters.
Write as below to give Meso if the server verification is successful.

    ```lua
    Method:
    [server]
    void VerifyExchangeKeyToGold(number keyQuantity, Entity npcEntity)
    {
        -- Verifying that the request was sent from a non-local client
        if senderUserId ~= self.Entity.PlayerComponent.UserId then
            log('Request is sent from a non-local client')
            return
        end
        
        -- Verify that requests have not been sent in an abnormally repetitive manner.
        -- Verify by adding reuse cooldown.
        if DateTime.UtcNow.Elapsed - self.lastExchangeTime < 1000 then
            log('Less than 1 second has passed since the previous request')
            return
        end
        
        -- Check whether the player entity is in the location where the NPC can be triggered.
        if npcEntity == nil then
            log('No trigger npc')
            return
        end
        
        local playerPos = self.Entity.TransformComponent.WorldPosition
        local npcPos = npcEntity.TransformComponent.WorldPosition
        local distance = Vector2.Distance(Vector2(playerPos.x, playerPos.y), Vector2(npcPos.x, npcPos.y))
        
        if distance > self.talkableDistance then
            log("The player is not within the trigger range. It is an abnormal packet.")
            return
        end
        
        -- Check whether the number of keys requested for exchange is not less than the number of keys the player has
        if self.key < keyQuantity then
            log('The player doesn't have enough keys.')
            return
        end
        
        -- Calls the exchange function if the verification is successful.
        self:ExchangeKeyToGold(keyQuantity)
       }
    ```
    
4.  Write a new `ExchangeKeyToGold()` function. Set the execution space control to **server only**, since this is the function that actually gives gold.
Write to give gold based on the amount of keys if the verification was successful.

    ```lua
    [server only]
    void ExchangeKeyToGold(number keyQuantity)
    {
        self.key = self.key - keyQuantity
        self.gold = self.gold + keyQuantity * 1000
        self.lastExchangeTime = DateTime.UtcNow.Elapsed
    }
    ```