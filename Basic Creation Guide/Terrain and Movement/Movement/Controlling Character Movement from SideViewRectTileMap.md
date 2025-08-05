# Course Introduction
Because DefaultPlayer includes RigidbodyComponent, KinematicbodyComponent, and SideviewbodyComponent, the subject of movement control will vary depending on the mode the map was produced with. Among them, a map produced with SideViewRectTile mode controls the character's movement with SideviewbodyComponent. 
Today let us learn about SideviewbodyComponent and look at the use methods with a simple example.

##### API Reference
[SideviewbodyComponent](/apiReference/Components/SideviewbodyComponent{"target":"_self"})

> <span style="color: #585858">**Learn more**
> Please refer to [Creating Maps with SideViewRectTile Mode] for the details of SideViewRectTile.</span>

# About SideviewbodyComponent
SideviewbodyComponent is used to control character movement in a side-scrolling system in SideviewRectTile maps.
![workspace](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_workspace.png "workspace") Workspace - ![defaultplayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_myavatar_no.png "defaultplayer") DefaultPlayer - ![move](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/move.png "move") SideviewbodyComponent.

![sideviewbody](https://mod-file.dn.nexoncdn.co.kr/bbs/165871206539396a3c85df20c4f439e76b91157a57a82.png "sideviewbody")

| Property | Description |
| :---: | --- |
| ApplyClimbableRotation | When true, the character on the rotating or slanted ladder will be affected by the shape of the ladder. When false, the character is not affected by the slant or rotation of the ladder. |
| EnableDownJump | Turns Jump Down on or off. |
| DownJumpSpeed | Controls the speed of the bounce back up when jumping down. The greater the value, the higher the bounce. |
| JumpDrag | Controls the rate of the jump speed. The greater the value, the faster the fall. |
| JumpSpeed | Controls the speed of the bounce when jumping. The greater the value, the higher the bounce. |
| Enable | If it is True, it enables SideviewbodyComponent. |

# SideviewbodyComponent Use
Let's learn how to utilize SideviewbodyComponent.
We recommend referring to [Create Map with SideViewRectTile Mode] first and creating a map below, and then doing the example in this guide.
![play](https://mod-file.dn.nexoncdn.co.kr/bbs/1658478611789db5a20547f4f40a5bec1c0b961ad97a7.gif "play")

## Creating a Double Jump
The character's movement is controlled `MoveVelocity`. Let us make a simple double jump feature through `MoveVelocity`.
<br>
1. Under MyDesk ![mydesk](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_folder_no.png "mydesk"), create a new script component ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") called DoubleJump, and then add it to DefaultPlayer ![DefaultPlayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_myavatar_no.png "DefaultPlayer").

2. As shown below, add the `IsDoubleJumping` Property and `TryDoubleJump` Function to DoubleJump ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent").
    ```lua
    Property:
    [None]
    boolean IsDoubleJumping = false
     
    Method:
    [client only]
    void TryDoubleJump()
    {
        local sb = self.Entity.SideviewbodyComponent
 
        -- If the character has not performed the double jump yet but is still in the air, you should change the Y-axis value of the speed.
        if self.IsDoubleJumping == false and sb:IsOnGround() == false then
            local jumpSpeed = sb.JumpSpeed
            local vel = sb.MoveVelocity
     
            vel.y = jumpSpeed
            sb.MoveVelocity = vel
     
            -- Once the character performs the double jump, it changes IsDoubleJumping to true.
            self.IsDoubleJumping = true
        end
    }
    ```
     If the character has not performed the double jump yet but is still in the air, you should change the Y-axis value of the speed.
    IsDoubleJumping Property is a state variable to be changed to true when the double jump is performed and to be false again when landing.

3. In addition, add the `OnUpdate` Function as below.
    ```lua
    Method:
    [client only]
    void OnUpdate(number delta)
    {
        local sb = self.Entity.SideviewbodyComponent
 
        -- Once the character lands, it changes IsDoubleJumping to false.
        if sb:IsOnGround() then
            self.IsDoubleJumping = false
        end
    }
    ```
  
4. As shown below, KeyDownEvent ![event](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_mod_event_no.png "event") should be added, too.
    ```lua
    Event Handler:
    [service: InputService]
    HandleKeyDownEvent(KeyDownEvent event)
    {  
        -- Parameters
        local key = event.key
        --------------------------------------------------------
 
        if key == KeyboardKey.LeftAlt or key == KeyboardKey.Space then
            self:TryDoubleJump()
        end
    }
    ```
   
5. Click **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") and test. If you input LeftAlt or Space key, a double jump is available.
![doublejump](https://mod-file.dn.nexoncdn.co.kr/bbs/165872170517526609c30a97c4a2e9a175212254925b8.gif "doublejump")

## Creating Slippery Tiles
If you apply `MoveVelocity`, you can make the character's movement meet the game concept that the creator intends. In this example, let us create the movement as walking on a slippery road by adding acceleration and frictional force concepts to meet the field concept full of snow and ice.
<br>
1.  Under MyDesk ![mydesk](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_folder_no.png "mydesk"), create a new script component ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") called CustomPlayerController, and then add it to DefaultPlayer ![DefaultPlayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_myavatar_no.png "DefaultPlayer").
   
2. Add Property as below after opening CustomPlayerController ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent").
    ```lua
    Property:
    [None]
    Vector2 InputDirection = Vector2(0,0)
    [None]
    boolean IsJumpKeyPressed = false
    [None]
    boolean IsCrouchKeyPressed = false
    ```
  
3. To add the `OnBeginPlay` Function and handle the user input directly, you should enable PlayerControllerComponent.
    ```lua
    [client only]
    void OnBeginPlay()
    {
        -- To handle the user input directly, you should disable PlayerControllerComponent.
        local controller = self.Entity.PlayerControllerComponent
        controller.Enable = false
    }
    ```
    
4. KeyDownEvent ![event](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_mod_event_no.png "event") and KeyUpEvent ![event](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_mod_event_no.png "event") should be added to record the key input state.
    ```lua
    Event Handler:
    [service: InputService]
    HandleKeyDownEvent(KeyDownEvent event)
    {
        -- Parameters
        local key = event.key
        --------------------------------------------------------
     
        if key == KeyboardKey.RightArrow then
            self.InputDirection.x = self.InputDirection.x + 1
        end
        if key == KeyboardKey.LeftArrow then
            self.InputDirection.x = self.InputDirection.x - 1
        end
        if key == KeyboardKey.UpArrow then
            self.InputDirection.y = self.InputDirection.y + 1
        end
        if key == KeyboardKey.DownArrow then
            self.InputDirection.y = self.InputDirection.y - 1
            self.IsCrouchKeyPressed = true
        end
        if key == KeyboardKey.LeftAlt or key == KeyboardKey.Space then
            self.IsJumpKeyPressed = true%%
        end
    }
     
    [service: InputService]
    HandleKeyUpEvent(KeyUpEvent event)
    {
        -- Parameters
        local key = event.key
        --------------------------------------------------------
     
        if key == KeyboardKey.RightArrow then
            self.InputDirection.x = self.InputDirection.x - 1
        end
        if key == KeyboardKey.LeftArrow then
            self.InputDirection.x = self.InputDirection.x + 1
        end
        if key == KeyboardKey.UpArrow then
            self.InputDirection.y = self.InputDirection.y - 1
        end
        if key == KeyboardKey.DownArrow then
            self.InputDirection.y = self.InputDirection.y + 1
            self.IsCrouchKeyPressed = false
        end
        if key == KeyboardKey.LeftAlt or key == KeyboardKey.Space then
            self.IsJumpKeyPressed = false
        end
    }
    ```
    
5. Let us calculate the character's movement based on acceleration and frictional force. First, add properties as follows.
    ```lua
    Property:
    [None]
    number MaxSpeed = 4 -- Maximum speed
    [None]
    number Accel = 10 -- Acceleration
    [None]
    number Drag = 8 -- Frictional force 
    [None]
    number AirAccel = 6 -- Aerial acceleration
    [None]
    number AirDrag = 4 -- Aerial frictional force
    ```
    
6. For better handling, let us apply the acceleration and frictional force values differently in the air. 
Add the `GetAccel` and `GetDrag` Functions as below.
    ```lua
    -- It returns the proper acceleration values on the ground and in the air.
    Method:
    number GetAccel()
    {
        local body = self.Entity.SideviewbodyComponent
     
        if body:IsOnGround() then
            return self.Accel
        else
            return self.AirAccel
        end
    }
    
    -- It returns the proper frictional force values on the ground and in the air.
    number GetDrag()
    {
        local body = self.Entity.SideviewbodyComponent
     
        if body:IsOnGround() then
            return self.Drag
        else
            return self.AirDrag
        end
    }
    ```

7. Set it as decelerating when there is no key input from the user and accelerating when there is. 
Add `UpdateVelocity` and `SignOf` Functions as below.
    ```lua
    number SignOf(number value)
    {
        if value >= 0 then
            return 1
        else
            return -1
        end
    }
        
    void UpdateVelocity(number delta)
    {
        local body = self.Entity.SideviewbodyComponent
        local vel = body.MoveVelocity
     
        if self.InputDirection.x == 0 then
            -- Decelerate when there is no input.
            if vel.x ~= 0 then
                local sign = self:SignOf(vel.x)
                local drag = self:GetDrag()
                vel.x = vel.x - drag * sign * delta
             
                if self:SignOf(vel.x) ~= sign then
                    vel.x = 0
                end
            end
        else
            -- Accelerate when there is input.
            local sign = self:SignOf(vel.x)
            local accel = self:GetAccel()
            vel.x = vel.x + self.InputDirection.x * accel * delta
            vel.x = math.min(vel.x * sign, self.MaxSpeed) * sign
        end
 
        -- Apply the final calculation result.
        body.MoveVelocity = vel
    }
    ```
    
8. Make the `OnUpdate` function call `UpdateVelocity`.
    ```lua
    [client only]
    void OnUpdate(number delta)
    {  
        -- Speed control
        self:UpdateVelocity(delta)
    }
    ```
   
9. Click **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") and test. You can see that the movement to the left and the right is smooth and slipping.
    ![slide](https://mod-file.dn.nexoncdn.co.kr/bbs/1658730031714e5dd719655354d66878ba051e490182e.gif "slide")
However, since PlayerControllerComponent is inactive, the character's jump motion or movement animation does not work yet. You need to add a function to control the motion and animation for normal operation.
   
10. Open ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") CustomPlayerController again and then add the `UpdateAction` function that controls the motions of characters.
    ```lua
    void UpdateAction()
    {
        local controller = self.Entity.PlayerControllerComponent
        local body = self.Entity.SideviewbodyComponent
     
        -- Prone
        if self.IsCrouchKeyPressed and body:IsOnGround() then
            controller:ActionCrouch()
        end
     
        -- Jump and Down Jump
        if self.IsJumpKeyPressed and body:IsOnGround() then
            if self.IsCrouchKeyPressed then
                controller:ActionDownJump()
            else
                controller:ActionJump()
            end
        end
    }
    ```                                    
    
11. Also add the `UpdateAnimationState` function that controls the animations of characters.
    ```lua
    void UpdateAnimationState()
    {
        local body = self.Entity.SideviewbodyComponent
        local state = self.Entity.StateComponent
     
        if body:IsOnGround() then
            if self.InputDirection.x ~= 0 then
                -- Walking animation
                state:ChangeState("MOVE")
            elseif self.IsCrouchKeyPressed then
                -- Prone animation
                state:ChangeState("CROUCH")
            else
                -- Standing animation
                state:ChangeState("IDLE")
            end
        else
            -- Floating animation
            state:ChangeState("FALL")
        end
    }
    ```

12. You also should add the `UpdateLookDirection` function that controls the character's left and right gaze.
    ```lua
    void UpdateLookDirection()
    {
        local controller = self.Entity.PlayerControllerComponent
     
        -- The Direction the player faces
        if self.InputDirection.x ~= 0 then
            if self:SignOf(self.InputDirection.x) < 0 then
                -- It looks toward the left side.
                controller.LookDirectionX = -1
            else
                -- It looks toward the right side.
                controller.LookDirectionX = 1
            end
        end
    }
    ```
    
13. Let us make the `OnUpdate` function call `UpdateAction`, `UpdateAnimationState`, and `UpdateLookDirection`.
    ```lua
    void OnUpdate(number delta)
    {  
        -- Speed control
        self:UpdateVelocity(delta)
     
        -- Addition: Motion control
        self:UpdateAction()
     
        -- Addition: Animation control
        self:UpdateAnimationState()
     
        -- Addition: Left and right gaze control
        self:UpdateLookDirection()
    }
    ```
   
14. ![workspace](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_workspace.png "workspace") Workspace - ![defaultplayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_myavatar_no.png "defaultplayer") DefaultPlayer - ![property](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_inspector.png "property") Property - ![move](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/move.png "move") SideviewbodyComponent checks `EnableDownJump`. Enter 3.3 for the `DownJumpSpeed` value.
![sideviewbody2](https://mod-file.dn.nexoncdn.co.kr/bbs/1658731157148bf72bff0378b464f9addc47a64839d56.png "sideviewbody2")

15. Click **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") and test. Check if the character's motion, animation, and gaze controls work normally.
![slide2](https://mod-file.dn.nexoncdn.co.kr/bbs/1658731907252ed230a57a59f4b899f347a4ba97f0ebf.gif "slide2")

## Creating More Slippery Tiles
If you use the `GetUnderfootTile` function, you can find out what tile the character is currently stepping on. Let us create an even more slippery feature by reducing the frictional force when stepping on an icy tile.

1. Under MyDesk ![mydesk](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_folder_no.png "mydesk"), create a new script component ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") called IceTileChecker, and then add it to DefaultPlayer ![DefaultPlayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_myavatar_no.png "DefaultPlayer").

2. Add Property as below after opening IceTileChecker ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent").
    ```lua
    Property :
    [None]
    number IceDrag = 2
    [None]
    number DefaultDrag = 0
    ```

3. Add the `OnBeginPlay` function and compose its content as follows.
    ```lua
    [client only]
    void OnBeginPlay()
    {
        self.DefaultDrag = self.Entity.CustomPlayerController.Drag
    }
    ```

4. Let us add the `OnUpdate` function and make the tile the character steps on much more slippery if the tile is "Ice" by using the `GetUnderfootTile` function.
    ```lua
    [client only]
    void OnUpdate(number delta)
    {
        local customController = self.Entity.CustomPlayerController
        local body = self.Entity.SideviewbodyComponent
        local underfootTile = body:GetUnderfootTile()
     
        customController.Drag = self.DefaultDrag
     
        if underfootTile == nil then
           return
        end
     
        if underfootTile.Name == "Ice" then
            customController.Drag = self.IceDrag
        end
    }
    ```

5. Click **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") and test. You see that it is much more slippery on an icy tile rather than a snow tile.
![iceslide](https://mod-file.dn.nexoncdn.co.kr/bbs/16587333763832e690f96aaf64196b8a4bd6dfef571cd.gif "iceslide")