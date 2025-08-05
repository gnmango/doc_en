# Course Introduction
Let's learn the concepts of [StateComponent](/apiReference/Components/StateComponent{"target":"_self"}) and StateType and understand the process of playing an animation according to the Entity's state. 

# Learn about State, StateComponent
**State** represents the entity's specific state. The creator can define different states, such as standing, prone, attacking, etc. It may seem obvious that my character is standing still in the World without doing anything, but this means that the State is **IDLE**, where "the character is playing a standing animation". To play state-specific animations like this, you need to leverage [StateAnimationComponent](/apiReference?postId=367{"target":"_self"}).
<br>
State names must be capitalized in English. Even if English lowercase letters are used, they are automatically converted and registered as capital letters when calling functions such as 'AddState()', but the behavior may be different than expected, so name it with the uppercase letters.

><span style="color: #7cafc2">**Tip**
> StateAnimationComponent can only be applied to monsters and NPCs.</span>

StateComponent is responsible for giving states to entities and defining relationships between states. It has <span style="color: #dc9656">**IDLE, DEAD**</span> states by default, so a creator wishing to add a new state needs to keep these two default states in mind and utilize them. 
StateComponent has a State that is automatically added when it is with other components. For example, DefaultPlayer contains by default a PlayerControllerComponent that is responsible for multiple states, so when a certain key is pressed, the state changes and the animation associated with that state (sit, walk, etc.) plays automatically.
#### Relationship between StateComponent and a Specific Component
StateComponent can automatically add a new state or process default states when working with a specific component. The automatically added states are transitioned to the corresponding state when a specific key is entered, a specific condition is satisfied, when performing an action, or when playing an animation. You can make an entity perform a user-defined action when it is in a certain state and use it in conjunction with StateAnimationComponent to play an animation clip. 

* When with PlayerComponent, you can process <span style="color: #dc9656">**DEAD**</span> or respawn.
* When with PlayerControllerComponent, <span style="color: #dc9656">**MOVE, CLIMB, LADDER, CROUCH, JUMP, FALL, ATTACK, ATTACK_WAIT, and SIT**</span> are added. 
* When with AIChaseComponent or AIWanderComponent, <span style="color: #dc9656">**MOVE**</span> is added. When keystroke to move the entity is entered, it transitions to the MOVE state and plays a walking animation. 
#### State Transition and Event Occurrence
Changing from one state to the next is called a state transition (Transition). Various events are fired when a state transition occurs, which can be used by a specific entity to detect the state transition. Events have information about the state before the state transition occurred and the current state (changed state). An event occurs under the following circumstances. 

* When StateComponent's state transition occurs, [StateChangeEvent](/apiReference/Events/StateChangeEvent{"target":"_self"}) is also raised.
* When entering the StateComponent **DEAD** state, [DeadEvent](/apiReference/Events/DeadEvent{"target":"_self"}) occurs.
* If DefaultPlayer has StateComponent and PlayerComponent together, when the `PlayerComponent:Respawn()` function is called, [ReviveEvent](/apiReference/Events/ReviveEvent{"target":"_self"}) occurs.

* HitComponent fires HitEvent, so StateComponent receives HitEvent fired from the entity and transitions to HIT. It automatically transitions to IDLE 0.5 seconds after entering HIT state.
* AttackComponent calls the `IsAttackTarget()` function when searching for an attack target. At this time, if the state of the entity corresponding to the target is DEAD, false is returned.
# StateType
Used to create a user defined State. In StateType, user-defined actions can be performed when entering and exiting a state, or state exit conditions can be defined.
If you want multiple states, we recommend creating and using a new ![StateType](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_state_type.png "StateType") <span style="color: #dc9656">**StateType**</span> for each state. You can add multiple states to one StateType using the If statement. But other states may be added or deleted during production, so it's more efficient to create a StateType for each state. For example, if you create a StateType to create your own new monster, we recommend creating and managing a number of new StateType except for IDLE and DEAD, which are the default states of StateComponent. The name of the new StateType you add will be the type of the StateType you created. 

StateType is a type that defines a state, so it cannot be used in a form that is directly added to an entity like a script component. StateType is automatically assigned when the `AddState()` function of StateComponent is called. After defining the behavior of the state on the Script Editor window, you need to add StateType using `StateComponent:AddState()` to one of the script components of the entity you want to use the StateType for.

 You can define how StateType will behave in `OnEnter()`, `OnUpdate()`, `OnExit()`, and `OnConditionCheck()`. 

* <span style="color: #dc9656">**OnEnter()**</span>: Execute the action when entering the state for the first time.
* <span style="color: #dc9656">**OnUpdate()**</span>: Executes an action while the state is maintained.
* <span style="color: #dc9656">**OnExit()**</span>: Executes when changing states.
* <span style="color: #dc9656">**OnConditionCheck(string nextStateName)**</span>: Specifies the transition condition. You can use the nextStateName parameter to return a different result depending on the connected state.
    * You can branch to nextStateName, and when true, a state transition is made.

#### Create StateType
Press <span style="color: #dc9656">**MyDesk - Create Scripts - Create StateType**</span> to create a new ![StateType](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_state_type.png "StateType") StateType and define the state. You can create a state transition condition using `OnConditionCheck()`. You can create **MoveDown** StateType as shown below and use it like the example code below.

```lua
Method:
void OnUpdate()
{
    self.ParentComponent.Entity.TransformComponent:Translate(0, -0.01)
}
    
boolean OnConditionCheck(string nextStateName)
{
    local rotationAngle = self.ParentComponent.Entity.TransformComponent.Rotation.z
    if rotationAngle ~= 0 and rotationAngle % 90 == 0 then
        return true
    else
        return false
    end
}
```

#### Add a State
You can add a state to an entity as in the example code below using `StateComponent:AddState(string stateName, Type stateType)`.

```lua
self.Entity.StateComponent:AddState("MOVEUP", MoveUp) 
 ```

#### Defining the State to Be Transitioned
Use `StateComponent:AddCondition(string stateName, string nextStateName, boolean reverseResult = false)` to enter the next state to pass from and connect it to the current state. If you write it like the example code below, the state transitions from **MOVEDOWN** to **MOVEUP**.

```lua
self.Entity.StateComponent:AddCondition("MOVEDOWN", "MOVEUP", false)
```

#### Delete State Transition Condition
Use `StateComponent:RemoveCondition(string stateName, string nextStateName)` to delete the condition for the transition from the current state to the next state. Write the following to delete the condition that was added as `AddCondition()` above (state transition from **MOVEDOWN** to **MOVEUP**).

```lua
self.Entity.StateComponent:RemoveCondition("MOVEDOWN", "MOVEUP")
```

# Use Example
Let's make a wooden crate that changes state when the player attacks. It utilizes the default state IDLE, DEAD, and the new BROKEN state created by the creator. By adding specific animation or sprite for each state, you can intuitively check the state change. 
In this example, the state transitions from IDLE → BROKEN → DEAD → IDLE, and different sprites and animations are connected and played for each state.

![example](https://mod-file.dn.nexoncdn.co.kr/bbs/16559702783894245b9b6f57f41bc80bea36df066edd1.gif "example")

><span style="color: #7cafc2">**Tip**
>Use the Create - New - **Basic** template to configure the player's basic attack and hit. Check if the ![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "component") PlayerHit and ![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "component") PlayerAttack components exist in the template of DefaultPlayer. </span>

List of Animations and Sprites RUID
* IDLE: b871399ae18d41c4b76c2ae79feefa7d
* BROKEN: 76815f621e354f57b5adb5b9faaf9eb3
* Apple: 014e7c98ad374cf49e7bf7ea72844241

1. Press Workspace - MyDesk - Create Model to create <span style="color: #dc9656">**![Model](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_model_no.png "Model") Model_Box**</span>. Add TransformComponent, SpriteRendererComponent, StateComponent, and HitComponent. Add the box RUID (b871399ae18d41c4b76c2ae79feefa7d) to SpriteRUID of SpriteRendererComponent.
![modelbox](https://mod-file.dn.nexoncdn.co.kr/bbs/165596740975009ece0c49b59481fa2b86a0d736f770d.png "modelbox")

2. Press Workspace - MyDesk - Create Model to create <span style="color: #dc9656">**![Model](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_model_no.png "Model") Model_Apple**</span>. Add TransformComponent and SpriteRenderComponent. Add the apple RUID (014e7c98ad374cf49e7bf7ea72844241) to SpriteRUID of SpriteRendererComponent.
![modelapple](https://mod-file.dn.nexoncdn.co.kr/bbs/16559673922967908df7d77a04ff2b354bcb7740742da.png "modelapple")

3. Press MyDesk - Create Scripts - Create StateType to create<span style="color: #dc9656">**![StateType](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_state_type.png "StateType") BrokenBox**</span>. Compose the behavior when entering the Broken state in the new BrokenBox as follows. 
The ID of the model to be called from `SpawnService:SpawnByModelId()` is Model_Apple.

    ```lua
    Property:
    boolean exitFlag = false
    number timerId = 0
    
    Method:
    void OnEnter()
    {
        self.exitFlag = false
        self.timerId = _TimerService:SetTimerOnce(function() self.exitFlag = true end, 1.1)
        
        local spriteRenderer = self.ParentComponent.Entity.SpriteRendererComponent
        spriteRenderer.SpriteRUID = "76815f621e354f57b5adb5b9faaf9eb3"  --BROKEN RUID
        
        _TimerService:SetTimerOnce(function() 
        	_SpawnService:SpawnByModelId("model://4ac51a4c-ecf6-4ddf-b32a-516fc66234be", "Item", self.ParentComponent.Entity.TransformComponent.Position + Vector3(0,0.12,0), self.ParentComponent.Entity.Parent) 
        end, 0.21) 
        
        -- Model_Apple's model ID
    }
    ```

4. In `OnConditionCheck()`, when moving to the next state and when the state ends, write as follows. 

   ```lua
    boolean OnConditionCheck(string nextStateName)
    {
        return self.exitFlag
    }
    
    void OnExit()
    {
        self.ParentComponent.Entity.SpriteRendererComponent.SpriteRUID = "b871399ae18d41c4b76c2ae79feefa7d" --IDLE RUID
        _TimerService:ClearTimer(self.timerId)
    }
    ```

5. Create the new <span style="color: #dc9656">**ItemBoxComponent**</span> and add it to <span style="color: #dc9656">**![Model](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_model_no.png "Model") Model_Box**</span>.
Add the new state <span style="color: #dc9656">**BROKEN**</span>, and add the state transition from BROKEN to DEAD.

    ```lua
    Method:
    [server only]
    void OnBeginPlay()
    {
        local state = self.Entity.StateComponent
        
        state:AddState("BROKEN", BrokenBox)
        state:AddCondition("BROKEN", "DEAD")
    }
    ```

6. In the state <span style="color: #dc9656">**ItemBoxComponent**</span>, use the StateChangeEvent that occurs when the state changes and write the timing at which the box reappears and the entity change at the time of state transition as follows.

    ```lua
    Event Handler:
    [self]
    HandleStateChangeEvent(StateChangeEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: StateComponent
        -- Space: Server, Client
        ---------------------------------------------------------
        
        -- Parameters
        -- local CurrentStateName = event.CurrentStateName
        -- local PrevStateName = event.PrevStateName
        ---------------------------------------------------------
        
        -- There's no BROKEN state on client side of StateComponent.
        if self:IsServer() then
        	if CurrentStateName == "HIT" then
        		self.Entity.StateComponent:ChangeState("BROKEN")
        	elseif CurrentStateName == "DEAD" then
        		_TimerService:SetTimerOnce(function() self.Entity.StateComponent:ChangeState("IDLE") end, 5)
        	end
        else
        	if CurrentStateName == "IDLE" then
        		self.Entity.SpriteRendererComponent.Color.a = 1
        	end
        end
    }
    ```

7. If you want to make it invisible after the animation is finished playing, in <span style="color: #dc9656">**ItemBoxComponent**</span>, you can make the last frame transparent using SpriteAnimPlayerEndEvent.

    ```lua
    [self]
   HandleSpriteAnimPlayerEndFrameEvent(SpriteAnimPlayerEndFrameEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ClimbableSpriteRendererComponent
        -- Space: Client
        ---------------------------------------------------------
        -- Sender: SpriteRendererComponent
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        -- local FrameIndex = event.FrameIndex
        -- local ReversePlaying = event.ReversePlaying
        -- local TotalFrameCount = event.TotalFrameCount
        -- local AnimPlayer = event.AnimPlayer
        ---------------------------------------------------------
        
        if TotalFrameCount > 1 then
        	self.Entity.SpriteRendererComponent.Color.a = 0 
        end
    }
    ```

><span style="color: #b8b8b8">**Learn More**
> If you want to give the apple a bouncing feel as in this example, add a new script component to Modle_Apple and use `TweenLogic:MakeTween()`.</span>


##### Reference Guide
* [Controlling Avatar Animations](/docs/?postId=545{"target":"_self"})