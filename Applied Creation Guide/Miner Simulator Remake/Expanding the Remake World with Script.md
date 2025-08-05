Let's modify the existing components of the Miner Simulator or add new entities and components to create your own World.

# Change Potion Effect
Let's restore a player's MP instead of HP when they use a potion from the mine.
Modify the UsePotion() function of the **Workspace - MyDesk - Components - DefaultPlayerComponents - PlayerMineData** component.
    ![mp](https://mod-file.dn.nexoncdn.co.kr/bbs/169441955335422b6690d1eab4fa09f37984661a38032.gif "mp")

```lua
local recoveryAmount = self.Entity.PlayerStatus.MPMax * 0.2

if recoveryAmount > (self.Entity.PlayerStatus.MPMax - self.Entity.PlayerStatus.MP) then
    recoveryAmount = self.Entity.PlayerStatus.MPMax - self.Entity.PlayerStatus.MP
end

-- Play Effect
_EffectService:PlayEffectAttached("015d476c71a3403ca6bab62c26888d4b", self.Entity, Vector3.zero, 0, Vector3.one)

-- Damage Skin
_DamageSkinService:Play(self.Entity, "0910d8fa5eac47dbbff4f30e9fb07a50", 0, {recoveryAmount}, DamageSkinTweenType.Default, false)
    
self.Entity.PlayerStatus.MP += recoveryAmount
```

<br>
# Trap Creation
Let's create a trap that jumps up when the player entity enters a certain area.
![trap](https://mod-file.dn.nexoncdn.co.kr/bbs/169449983411878a425308beb41d0a2f10740703d1591.gif{"width":"540px"} "trap")
1. Place the **Preset List - Trap - trap-26** in the Scene.
2. Change the SortingLayer under the SpriteRendererComponent for the **trap-26** entity to **Background**.
3. Create a new **Trap_RiseupTrap** entity. Add the **SpriteRendererComponent, TransformComponent, and TriggerComponent** and place it above the trap.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16944195854068319f10165a0477eb4f38d05250c0506.png{"width":"220px"} "1")
4. Move the **trap-26** position to under the Trap_RiseupTrap entity.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1694419605271fc93377519cf4b748cee4b09710805a9.png "2")
5. Create a new `Trap_RiseupTrap` component, and add it to the **Trap_RiseupTrap entity**. 
6. Add a `TriggerEnterEvent` to the Entity Event Handler and write the following. It will make it so that a collision is triggered only when a local player touches the trap.
    
    ```lua
    Event Handler:
    [self]
    HandleTriggerEnterEvent(TriggetEnterEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: TriggerComponent
        -- Space: Server, Client
        ---------------------------------------------------------
        
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity    
        ---------------------------------------------------------
    
        if TriggerBodyEntity ~= _UserService.LocalPlayer then
            return
        end
    }
    ```

7. Add the **boolean** type `IsActivated` property so that the trap can rise to a set height.
    
    ```lua
    Property:
    [None]
    boolean IsActivated = false
    ```

8. Continue with the following below **HandleTriggerEnterEvent**.

    ```lua
    if self.IsActivated == true then
        return
    end
    
    self.IsActivated = true
    local trap = self.Entity:GetChildByName("trap-26")
    
    local riseUp = function()
        trap.TransformComponent:Translate(0,0.5)
    end
    
    -- Up after 0.5 seconds
    _TimerService:SetTimerOnce(riseUp, 0.5)
    
    local down = function()
        trap.TransformComponent:Translate(0,-0.5)
        self.IsActivated = false
    end
    
    -- Down after 2 seconds
    _TimerService:SetTimerOnce(down,2)
    ```

9. Add `TrapTriggerComponent` to the **trap-26** entity and save the appriate collider size.
10. The amount of damage dealt when touching the trap can be set by changing the `Damage` property value from TrapTriggerComponent.


# Modify Aerial Jump
![DoubleJump](https://mod-file.dn.nexoncdn.co.kr/bbs/169450346531860fedc0682f64970aa3bab855a382db4.gif{"width":"540px"} "DoubleJump")
1. Add the **KeyDownEvent** to ExtendPlayerControllerComponent Entity Event Handler. You need to add conditions so that only players who enter the key using UserService can do the aerial jump.

    ```lua
    Event Handler:
    [service: InputService]
    HandleKeyDownEvent(KeyDownEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: InputService
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        local key = event.key
        ---------------------------------------------------------

        -- Conditions are added using UserService so that only players who enter the key can move the avatar.
        if _UserService.LocalPlayer ~= self.Entity then
            return
        end
        if key == KeyboardKey.Space or key == KeyboardKey.LeftAlt then
            if self.Entity.RigidbodyComponent:IsOnGround() == false then
                self:ActionDoubleJump()
            end
        end
    }
    ```
    > <span style="color: #7cafc2">**Tip**
    > You can see the keys that you can enter from [KeyboardKey](/apiReference/Enums/KeyboardKey{"target":"_self"}).</span>

2. Add the new **LeftAirJumpCount** property to the **ExtendPlayerController** component.

   ```lua
    Property:
    [None]
    number LeftAirJumpCount = 1
   ```
   <br>

3. Create the new ActionDoubleJump() function to **ExtendPlayerControllerComponent**.
    
    ```lua
    Method:
    [client]
    void ActionDoubleJump()
    {
        if self.LeftAirJumpCount <= 0 then
            return
        end
        
        local currentSpeed = self.Entity.RigidbodyComponent.RealMoveVelocity
        
        self.Entity.RigidbodyComponent:SetForce(Vector2(currentSpeed.x,10))
        self.LeftAirJumpCount -= 1
    }
    ```
4. Add FootholdEnterEvent and write as follows so that the player avatar can double jump after touching the terrain. 
When the LocalPlayer touches the terrain using UserService, reset the value of **LeftAirJumpCount** to 1.
    ```lua
    Event Handler:
    [client only]
    HandleFootholdEnterEvent(FootholdEnterEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: RigidbodyComponent
        -- Space: Server, Client
        ---------------------------------------------------------
        
        -- Parameters
        local Entity = event.Entity
        local Foothold = event.Foothold
        ---------------------------------------------------------
        
        if _UserService.LocalPlayer ~= self.Entity then
            return
        end
        
        self.LeftAirJumpCount = 1
    }
    ```
5. In the ActionDoubleJump() function, write as follows to play a sound and effect for each jump.

    ```lua
    _SoundService:PlaySound("2eb793d54f25463dba5917b8b518f0e5", 1)
    _EffectService:PlayEffectAttached("e7addadf676d415cab98b6bd867eaf82", self.Entity, Vector3.zero, 0, Vector3.one)
    ```
<br>
#  Creating UI Tooltips
#### Creating HP Bar Guide UI
Let's make the Description UI appear when you hover over or touch the HP bar.
![DescPanel](https://mod-file.dn.nexoncdn.co.kr/bbs/1694495925525ab55bbc6c25b4713b8ccc053cfcb73a3.gif{"width":"540px"} "DescPanel")
1. Add the new **UI_MouseHoverGuide** component in the **Workspace - MyDesk - Component - UI**.


    ```lua
    Property:
    [None]
    Entity TargetPanel = nil
    
    Event Handler:
    [self]
    HandleButtonStateChangedEvent(ButtonStateChangeEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        local state = event.state
        ---------------------------------------------------------
        
        if (Environment:IsMobilePlatform() == true and state == ButtonState.Pressed) 
        or (Environment:IsPCPlatform() == true and state == ButtonState.Hover) then    
           
            if self.TargetPanel == nil then
                return
            end
                
            self.TargetPanel.Enable = true
        else
            if self.TargetPanel == nil then
                return
            end
            
            self.TargetPanel.Enable = false
        end
    }
    ```
<br>

2. Add the UI_MouseHoverGuide component in the **Hierarchy - ui - HUDGroup - HUD - PlayerInfo - HP**.
3. In the Property Editor window, set the **TargetPanel** property to **HPDescPane or MPDescPanel**.

#### Randomly Displaying Message
Let's change the tip on the loading screen that appears when the player enters the mine from town. You can make the messages in DataSet appear at random.
![RandomeTipMessage](https://mod-file.dn.nexoncdn.co.kr/bbs/1694495955844212538efd0384a5c8412f6df5678f977.gif{"width":"540px"} "RandomeTipMessage")
1. Create a new **TipMessageInfo** DataSet.
2. Create the **Message** column and write the content to appear in the chat balloon.
![TipMessageDataSet](https://mod-file.dn.nexoncdn.co.kr/bbs/169448376328415bd77dd50a74edc9f5b7638786f7641.png "TipMessageDataSet")
3. Add the following code to HandleInteractionEvent2() of the **Portal_TownToMine** component.

    ```lua
    local tipMessageTable = _DataService:GetTable("TipMessageInfo")
    
    local tipMessageEntity = _EntityService:GetEntityByPath("/ui/LoadingGroup/MineEnterance/TipMessage")

    -- Display randomly 1-3 times
    local messageIdx = _UtilLogic:RandomIntegerRange(1,3)
    local message = tipMessageTable:GetCell(messageIdx,1)
    
    tipMessageEntity.TextComponent.Text = message
    ```