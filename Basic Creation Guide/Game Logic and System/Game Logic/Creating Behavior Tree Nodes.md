# Course Introduction
This will teach you about nodes that are used to create behaviour trees. You can follow along with the examples to create an AI monster that moves in accordance with the behaviour tree.

##### Reference Guide
* [Creating AI with Behavior Trees](/docs?postId=562{"target":"_blank"})

##### API Reference
* [BTNode](/apiReference?postId=455{"target":"_blank"})
* [AIComponent](/apiReference?postId=361{"target":"_blank"})
* [ParallelNode](/apiReference?postId=456{"target":"_blank"})
* [RandomSelectorNode](/apiReference?postId=457{"target":"_blank"})
* [SelectorNode](/apiReference?postId=458{"target":"_blank"})
* [SequenceNode](/apiReference?postId=459{"target":"_blank"})
# Creating Behavior Trees
#### Creating Composite Nodes
Composite node entities can be created with each node's creator. A composite node's creator is as follows.

| Node | Constructor |
| --- | --- |
| Sequence | SequenceNode(string name = "SequenceNode") |
| Selector | SelectorNode(string name = "SelectorNode") |
| RandomSelector | RandomSelectorNode(string name = "RandomSelectorNode") |
| Parallel | ParallelNode(string name = "ParallelNode") |

```lua
[server only]
void OnBeginPlay()
{
	local seqNode = SequenceNode("sq")
	local selNode = Selector("sel")
	local randomNode = RandomSelector("rand")
	local prNode = Parallel("pr")
}
```

#### AttachChild()
A composite node can have a child node. You can use `AttachChild()` to create a child node. A Composite Node can have both an Action Node and another Composite Node as a child, and you can use it to create a complex tree structure. 

#### AttachChildAt()
A tree structure will run sequentially in the order child nodes are registered. There are two ways to change the execution sequence. The first method is to change the sequence within the code. The second method is to use the `AttachChildAt()` function. 

The `AttachChildAt()` function allows you to register a node at a certain position regardless of the order it was registered. You can use the `AttachChildAt()` function in the same way you use the `AttachChild()` function. The only difference is that you have to pass the node's position as the second parameter. 
The order starts from 0. If you register a new node to a position already occupied by another node, existing nodes will be pushed to the next position. 
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17168956058276319a1ddbdab49bdb2588b236c875a84.png "Attach")

```lua
[server only]
void OnBeginPlay()
{
	--CREATE NODE
	local sequence = SequenceNode("sq")
	local actionA = self:CreateNode("ActionA")
	local actionB = self:CreateNode("ActionB")
	local actionC = self:CreateNode("ActionC")
	local actionD = self:CreateNode("ActionD")
	
	--SET TREE
	sequence:AttachChild(actionA)
	sequence:AttachChild(actionB)
	sequence:AttachChild(actionC)
	sequence:AttachChildAt(actionD,1)
}
```

#### Removing Nodes
When removing a registered child node, you can use `DetachChild()`, `DetachChildAt()`. 

* `DetachChild()` passes the entity of the child node that will be removed as parameter to remove it.
* `DetachChildAt()` passes the position of the child node that will be removed as parameter to remove it.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17168956211348925ff93ffd347379cb977b79e710f7c.png "detach")

```lua
[server only]
void OnBeginPlay()
{
	--CREATE NODE
	local sequence = SequenceNode("sq")
	local actionA = self:CreateNode("ActionA")
	local actionB = self:CreateNode("ActionB")
	local actionC = self:CreateNode("ActionC")
	
	--SET TREE
	sequence:AttachChild(actionA)
	sequence:AttachChild(actionB)
	sequence:AttachChild(actionC)
	
	--DETACH NODE
	sequence:DetachChildAt(1)
	sequence:DetachChild(actionC)
}
```

#### Root Node Setting 
**Root Node** uses `SetRootNode()`. Root node passes the node as parameter to the top-level node. You can set the highest node as Root node to give AI to an entity and operate the BT.

```lua
[server only]
void OnBeginPlay()
{
	--CREATE NODE
	local sequence = SequenceNode("sq")
	local actionA = self:CreateNode("ActionA")
	local actionB = self:CreateNode("ActionB")
	local actionC = self:CreateNode("ActionC")
	
	--SET TREE
	sequence:AttachChild(actionA)
	sequence:AttachChild(actionB)
	sequence:AttachChild(actionC)
	
	--SET ROOT NODE
	self:SetRootNode(sequence)
}
```

#### Creating Decorator Nodes
To create a Decorator node, you can use BTNodeType. Create a new BTNodeType, then add **Child** or **child** of any type as property to create a Decorator Node.
If you'd like to execute a child node, call the `Behave()` function.

```lua
Property: 
[None]
any Child = nil

Method: 
any OnBehave(number delta)
{
	if self.Child == nil then
		return BehaviourTreeStatus.Failure
	end
	
	local result = self.Child:Behave(delta)
	
	if result == BehaviourTreeStatus.Success then
		return BehaviourTreeStatus.Failure
	
	elseif result == BehaviourTreeStatus.Failure then
		return BehaviourTreeStatus.Running
	else
		return BehaviourTreeStatus.Success
	end
}
```

You can create an entity node and assign a child node to the created Decorator BTNodeType, like shown below.

```lua
[server only]
void OnBeginPlay()
{
	local aiComponent = self.Entity.AIComponent
	
	local childNode = aiComponent:CreateLeafNode("childNode", function(delta) return BehaviourTreeStatus.Success end)
	local decorator = self:CreateNode("Decorator")
	decorator.Child = childNode
}
```

#### Behavior Tree Completion
After configuring the Root Node setting, the AIComponent will operate according to the created behaviour tree.
![15](https://mod-file.dn.nexoncdn.co.kr/bbs/1645011460737db32f055d8984de6a762c4129e12207c.png{"width":"720px"} "15")

```lua
[server only]
void OnBeginPlay()
{
	--CREATE COMPOSITE NODE
	local sequence = SequenceNode("Sequence")
	local sequenceA = SequenceNode("SequenceA")
	local sequenceB = SequenceNode("SequenceB")
	local sequenceC = SequenceNode("SequenceC")
	local selectorA = SelectorNode("SelectorA")
	local randomSelectorA = RandomSelectorNode("RandomSelectorA")
	local parallelA = ParallelNode("ParallelA")
	local parallelB = ParallelNode("ParallelB")
		
	--CREATE ACTION NODE
	local actionA1 = self:CreateNode("ActionA", "ActionA1")
	local actionA2 = self:CreateNode("ActionA", "ActionA2")
	local actionB1 = self:CreateNode("ActionB", "ActionB1")
	local actionB2 = self:CreateNode("ActionB", "ActionB2")
	local actionC1 = self:CreateNode("ActionC", "ActionC1")
	local actionC2 = self:CreateNode("ActionC", "ActionC2")
	local actionD1 = self:CreateNode("ActionD", "ActionD1")
	local actionD2 = self:CreateNode("ActionD", "ActionD2")
	local actionE = self:CreateNode("ActionE", "ActionE")
	local actionF = self:CreateNode("ActionF", "ActionF")
	local actionG = self:CreateNode("ActionG", "ActionG")
	
	--SET TREE
	sequence:AttachChild(sequenceA)
	sequence:AttachChild(parallelA)
	sequenceA:AttachChild(randomSelectorA)
	sequenceA:AttachChild(actionA1)
	parallelA:AttachChild(sequenceB)
	parallelA:AttachChild(actionB1)
	parallelA:AttachChild(selectorA)
	randomSelectorA:AttachChild(sequenceC)
	randomSelectorA:AttachChild(actionA2)
	sequenceB:AttachChild(actionB2)
	sequenceB:AttachChild(actionC1)
	selectorA:AttachChild(parallelB)
	selectorA:AttachChild(actionD1)
	sequenceC:AttachChild(actionC2)
	sequenceC:AttachChild(actionD2)
	parallelB:AttachChild(actionE)
	parallelB:AttachChild(actionF)
	parallelB:AttachChild(actionG)
}
```


# Behaviour Tree Utilization Example
Let's use AIComponent and BTNodeType to create an AI monster than can detect players and wander around within a certain range.  There are three major elements in this example.

* When the player is far away, the monster will wander around, moving in random directions. 
* When the player approaches, the monster will start chasing the player.
* When the player moves far away, the monster will stop giving chase and start wandering again.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17168958937950674e59a9a8744ca81444187e01c8e85.png "Tree Behaviour")

## Preparation
1. Switch to **RectTile** mode and place tiles.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1717483867395248704741b464dfa9bbf21b93f26f45e.png "1")

2. Select **Workspace - MyDesk - Create Material** and add a new Alert material.

    * Shader: Rainbow
    * Blend: 0.5
    * Rotate: 0
    * Spread: 0.25
    * TimeOffset: 0
    * TimeScale: 1

    ![Material](https://mod-file.dn.nexoncdn.co.kr/bbs/1717483885157c4309e9ef2e34691a871092dd75b1906.png{"width":"340px"} "Material")

3. Select **Preset List - Monster - monster2358** and add a preset. Leave TransformComponent, StateAnimationComponent, and SpriteRendererComponent in place and add the following three components.
    * StateComponent, MovementComponent, KinematicbodyComponent
 
![monster](https://mod-file.dn.nexoncdn.co.kr/bbs/17174839092271a14772637854b71abaf190445ea359b.png{"width":"340px"} "monster")


## Create AIPatrolComponent
1. Select **Workspace - BaseEnvironment - NativeScripts -  Component - AIComponent** and click Extend. Rename it as **AIPatrolComponent**.
2.  Add AIPatrolComponent to the monster you placed in the map.
![AIPatrolComponent](https://mod-file.dn.nexoncdn.co.kr/bbs/1717488266930702706135fff47409f16850ee919f377.png{"width":"340px"} "AIPatrolComponent")

3. Add the `DrawRange()` function. Use **PolygonRendererComponent** to show the range where the monster can detect a player, like below.

    ```lua
    Property:
    [None]
    number DetectionRange = 5
    
    Method:
    [server only]
    void DrawRange()
    {
        ---@type PolygonRendererComponent
        local polygon = self.Entity:AddComponent("PolygonRendererComponent")
        polygon.Color = Color(0, 1, 0, 0.25)
    
        local res = 24
        local radius = self.DetectionRange
        for i = 1, res do
            local rad = 2 * math.pi * (i / tonumber(res))
            local x = math.cos(rad) * radius
            local y = math.sin(rad) * radius
            polygon.Points:Add(Vector2(x, y))
        end
    }
    ```
    
4. Create a `Flip()` function. Write as follows to flip the monster's sprite horizontally, in accordance with its movement direction.

    ```lua
    [server only]
    void Flip()
    {
        local kb = self.Entity.KinematicbodyComponent
        local sprite = self.Entity.SpriteRendererComponent
    
        if kb.MoveVelocity:Magnitude() > 0 then
            if kb.MoveVelocity.x < 0 then
                sprite.FlipX = false
            else
                sprite.FlipX = true
            end
        end
    }
    ```

5. Create a `DetectPlayer()` function and write the following, so it can sense the distance between itself and the player.
If the distance from the player is closer than the range, player entity will be recorded in the memory; if the distance is further than the range, player entity will be removed from memory. Detected players will be used in **ActionFollow, DecoHasTarget, DecoHasNoTarget**.

    ```lua
    [server only]
    void DetectPlayer()
    {
        local target = nil
        local minDist = self.DetectionRange
        local myPos = self.Entity.TransformComponent.WorldPosition:ToVector2()
    
        local users = _UserService.UserEntities
        for userId, user in pairs(users) do
            local userPos = user.TransformComponent.WorldPosition:ToVector2()
            local dist = userPos:Distance(myPos)
            if dist < minDist then
                target = user
                minDist = dist
                break
            end
        end
    
        if target == nil then
            self:SetMemory("PlayerInRange", nil)
        else
            self:SetMemory("PlayerInRange", target)
        end
    }
    ```
6. Add `OnUpdate`, and then write as follows to call `DetectPlayer()`, `Flip()` functions.

    ```lua
    void OnUpdate(number delta)
    {
        self:DetectPlayer()
        self:Flip()
    }
    ```

7. Add 'OnBeginPlay', and then write the following to call up the `DrawRange()` function.

    ```lua
    void OnBeginPlay()
    {
        self:DrawRange()
    }
    ```

8. Add a `Memory` property.

    ```lua
    Property:
    [None]
    table Memory = {}
    ```

9. Create a `SetMemory()` function and write the following, so it can store the value to key.

    ```lua
    [server only]
    void SetMemory(string key, any value)
    {
        self.Memory[key] = value
    }
    ```

10. Create a `GetMemory()` function and write the following, so it can get the value stored in key.

    ```lua
    [server only]
    any GetMemory(string key)
    {
        return self.Memory[key]
    }
    ```
    
## Create BTNodeType
Let's create a tree structure like below using BTNodeType. In the example, you will create eight BTNodeType nodes. These nodes will operate together to make the monster move around and chase the player.

1. Select **Workspace - MyDesk - Create Scripts - BTNodeType** and create eight new BTNodeType nodes. These eight BTNodeType nodes will serve different roles, as shown below.

| Node | Description | 
| --- | --- | 
| ActionChangeStateAlert| Modifies the monster's material. The detection range will be shown in red. |
| ActionChangeStateCalm | Reverts the monster's material to the original. The detection range will be shown in green. |
| ActionFollow | Tracks down a player. |
| ActionMoveRandom | Moves in a random direction. |
| ActionStop | Stops moving. |
| ActionWait | Stands still for a certain amount of time. |
| DecoHasNoTarget | The Decorator executes the child node if no players are detected and returns Failure if a player is detected. |
| DecoHasTarget | The Decorator executes the child node if a player is detected and returns Failure if no players are detected. |


#### ActionChangeStateAlert
Write the following to change the monster's appearance to the Rainbow material in **ActionChangeStateAlert**, and change the color of the detection range in PolygonRendererComponent.

```lua
Method:
any OnBehave(number delta)
{
    local sprite = self.ParentAI.Entity.SpriteRendererComponent
    local polygon = self.ParentAI.Entity.PolygonRendererComponent

    if sprite == nil or polygon == nil then
        return BehaviourTreeStatus.Failure
    end

    sprite:ChangeMaterial("material://EntryID")
    polygon.Color = Color(1, 0, 0, 0.25)

    return BehaviourTreeStatus.Success
}
```


#### ActionChangeStateCalm
Write the following to show the detection range in green and set the monster's material to be turned off when no player entities are detected within the detection range.

```lua
Method:
any OnBehave(number delta)
{
    -- Modifies the monster's appearance.
    -- Modifies the sprite's material to basic material and the color of the polygon renderer to green.

    local sprite = self.ParentAI.Entity.SpriteRendererComponent
    local polygon = self.ParentAI.Entity.PolygonRendererComponent

    if sprite == nil or polygon == nil then
        return BehaviourTreeStatus.Failure
    end

    sprite:ChangeMaterial(nil)
    polygon.Color = Color(0, 1, 0, 0.25)

    return BehaviourTreeStatus.Success
}
```

#### ActionFollow
Write the following to let the monster chase the player when it enters the specified range. Use `AIPatrolComponent:GetMemory()` to check for the identified players.

```lua
Method:
any OnBehave(number delta)
{
    ---@type AIPatrolComponent
    local parentAI = self.ParentAI
    local transform = parentAI.Entity.TransformComponent
    local target = parentAI:GetMemory("PlayerInRange")

    if target == nil then
        return BehaviourTreeStatus.Failure
    end

    local diff = target.TransformComponent.WorldPosition - transform.WorldPosition
    diff = diff:ToVector2()
    local dist = diff:Magnitude()

    if dist > 0 then
        local dir = diff:Normalize()
        local movement = self.ParentAI.Entity.MovementComponent
        movement:MoveToDirection(dir, 0)
        return BehaviourTreeStatus.Running
    end

    return BehaviourTreeStatus.Success
}
```

#### ActionMoveRandom
Write the following so that the monster can move around in different directions.

```lua
Property:
[None]
Vector2 MoveDirection = Vector2(0,0)
[None]
number MoveTime = 0
[None]
number ElapseTime = 0

Method:
void OnInit()
{
    -- Sets new movement direction and time on each execution.

    local randomX = _UtilLogic:RandomDouble() - 0.5
    local randomY = _UtilLogic:RandomDouble() - 0.5
    
    self.MoveDirection = Vector2(randomX, randomY):Normalize()
    self.MoveTime = _UtilLogic:RandomDouble() * 2 + 2
    self.ElapsedTime = 0
    
    local movement = self.ParentAI.Entity.MovementComponent
    movement:MoveToDirection(self.MoveDirection, 0)
}

any OnBehave(number delta)
{
    -- Moves in accordance with the time and direction specified in OnInit().
    -- Returns success when the monster touches the player.
    
    local movement = self.ParentAI.Entity.MovementComponent
    
    self.ElapsedTime += delta
    
    if self.ElapsedTime >= self.MoveTime then
        movement:MoveToDirection(Vector2.zero, 0)
        return BehaviourTreeStatus.Success        
    end

    return BehaviourTreeStatus.Running       
}
```

#### ActionStop
Write the following, so the monster can stop moving.

```lua
Method:
any OnBehave(number delta)
{
    -- Stops moving.
    local movement = self.ParentAI.Entity.MovementComponent

    if movement ~= nil then
        movement:Stop()
    end

    return BehaviourTreeStatus.Success
}
```

#### ActionWait
Write the following, so the monster stands still and waits for a certain amount of time. Returns Running before the specified time passes, and returns Success otherwise.

```lua
Property:
[None]
number ElapsedTime = 0
[None]
number Time = 0

Method:
void OnInt()
{
    self.ElapsedTime = 0
}

any OnBehave(number delta)
{
    self.ElapsedTime += delta

    if self.ElapsedTime >= self.Time then
        return BehaviourTreeStatus.Success
    else
        return BehaviourTreeStatus.Running
    end
}
```

#### DecoHasNoTarget
Write the following, so the child node is executed if no players are detected and returns Failure otherwise.

```lua
Property:
[None]
any Child = nil

Method:
any OnBehave(number delta)
{
    ---@type AIPatrolComponent
    local parentAI = self.ParentAI
    local target = parentAI:GetMemory("PlayerInRange")

    if target == nil then
        return self.Child:Behave(delta)
    else
        return BehaviourTreeStatus.Failure
    end
}
```

#### DecoHasTarget
Write the following, so the child node is executed if a player is detected and Failure is returned otherwise.

```lua
Property:
[None]
any Child = nil

Method:
any OnBehave(number delta)
{
    ---@type AIPatrolComponent
    local parentAI = self.ParentAI
    local target = parentAI:GetMemory("PlayerInRange")

    if target == nil then
        return BehaviourTreeStatus.Failure
    else
        return self.Child:Behave(delta)
    end
}
```

## Creating Behaviour Trees
1. Add a new `CreateBehaviourTree()` function in **AIPatrolComponent**. Create a tree structure as follows.

    ```lua
    [server only]
    void CreateBehaviourTree()
    {
        local root = SelectorNode("Root")
    
        local seqAttack = SequenceNode("Attack")
        local actAlertState = self:CreateNode("ActionChangeStateAlert", "ChangeState: Alert")
        local actFollow = self:CreateNode("ActionFollow", "Follow")
    
        seqAttack:AttachChild(actAlertState)
        seqAttack:AttachChild(actFollow)
    
        local decoHasTarget = self:CreateNode("DecoHasTarget", "HasTarget (Attack)")
        decoHasTarget.Child = seqAttack
    
        local seqPatrol = SequenceNode("Patrol")
        local seqMove = SequenceNode("Move")
        local actCalmState = self:CreateNode("ActionChangeStateCalm", "ChangeState: Calm")
        local actStop = self:CreateNode("ActionStop", "Stop")
        local actMoveRandom = self:CreateNode("ActionMoveRandom", "MoveRandom")
        local actWait = self:CreateNode("ActionWait", "Wait")
        actWait.Time = 3
    
        seqMove:AttachChild(actWait)
        seqMove:AttachChild(actMoveRandom)
    
        local decoHasNoTarget = self:CreateNode("DecoHasNoTarget", "HasNoTarget (Move)")
        decoHasNoTarget.Child = seqMove
    
        seqPatrol:AttachChild(actCalmState)
        seqPatrol:AttachChild(actStop)
        seqPatrol:AttachChild(decoHasNoTarget)
    
        root:AttachChild(decoHasTarget)
        root:AttachChild(seqPatrol)
    
        self:SetRootNode(root)
    }
    ```

2. Modify as shown below, so the `OnBeginPlay()` function calls the `CreateBehaviourTree()` function.

    ```lua
    void OnBeginPlay()
    {
        self:CreateBehaviourTree()
        self:DrawRange()
    }
    ```