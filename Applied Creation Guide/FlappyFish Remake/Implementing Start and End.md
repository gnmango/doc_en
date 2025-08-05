# Death Determination
1. Modify the **GameLogic**'s `OnBeginPlay()` function.
Change the code as shown below to initialize to the index after the last index of the pooled obstacles.

    ```lua
    [client only]
    void OnBeginPlay()
    {
        for i = 1, 5 do
        	local entity = _SpawnService:SpawnByModelId(_EntryService:GetModelIdByName("Model_Obstacle"), "obstacle", Vector3(i * self.intervalX + self.startPosX, 0, 0), _UserService.LocalPlayer.CurrentMap)
        	
        	table.insert(self.obstacles, entity)
        	
        	entity.ObstacleComponent:Init(i)
        end
        
        self.index = #self.obstacles + 1
    }
    ```

2. Modify the `Init()` function.

    ```lua
    local i = 1
    
    for _, entity in pairs(self.obstacles) do
    	---@type Entity
    	local newEntity = entity
    	
    	newEntity.TransformComponent.Position = Vector3(i * self.intervalX + self.startPosX, 0, 0)
    	newEntity.ObstacleComponent:Init(i)
    	
    	i += 1
    end
    
    self.index = #self.obstacles + 1
    ```

3. Modify the **ObstacleComponent**'s `OnUpdate()` function.
Modify the below of `if playerPos.x - 5 > position.x then`.
    ```lua
    [client only]
    void OnUpdate(number delta)
    {
        self:Init(_GameLogic.index)
        _GameLogic.index += 1       
    }    
    ```

#### Creating Death Determination Component on Obstacle Entity
1. Place the **Model_Obstacle** in the Scene.
2. Create the new <span style="color: #dc9656">**TouchAndDieComponent**</span> and add it to the Top and Bottom entities.

3. In Hierarchy, press **Create Model From Entity** from the context menu of Model_Obstacle to model it.

    > **Tip**
    > It is best to do the modeling after deleting the existing Model_Obstacle.
    
4. Create the new **isDead** property in the FishPlayerComponent.
    ```lua
    Property:
    [None]
    boolean isDead = false
    ```

5. Create the new `Die()` function. Write the following so that you can stop the movement and return to the original position by using the isDeath property.

    ```lua    
    Method:
    [client only]
    void Die ()
    {
        if self.isDead then
            return 
        end    
        
        self.isDead = true
        self.Entity.TransformComponent.Position = Vector3.zero
    
        wait(1)
    
        self.isDead = false
    }
    ```

6. Modify the contents of the `TriggerEnterEvent` Handler in the **TouchAndDieComponent** as follows.
Call the `Die()` function when the player collides with the TriggerComponent. 

    ```lua
    Event Handler:
    [client only] [self]
    HandleTriggerEnterEvent (TriggerEnterEvent event)
    {
    
        if TriggerBodyEntity == _UserService.LocalPlayer then
            _UserService.LocalPlayer.FishPlayerComponent:Die()
        end
    }
    ```

8. Modify the **FishPlayerComponent**'s `Die()` function. Call the `Init()` function under the `wait 1`.

    ```lua
    _GameLogic:Init()
    ```

#### Determining death if the player falls below a certain level
Add the new `CheckPosY()` function in the **FishPlayerComponent**. Let's end the game by calling the `Die()` function when the entity's Y coordinate drops below a certain value.

```lua
[client only]
void CheckPosY ()
{
    if self.Entity.TransformComponent.Position.y < -15 then
        self:Die()
    end
}
```

#### Collision Effect
Let's shake the camera when the fish hits an obstacle to give the collision effect.
In the `Die()` function, use the `CameraComponent:ShakeCamera()` to make the camera shake when something hits an obstacle. Shake the camera when the isDead becomes true in the FishPlayerComponent's `Die()` function.

```lua
[client only]
void Die()
{
    if self.isDead then
        return
    end
    
    self.isDead = true
    -- Add Camera Collision Effect
    self.Entity.CameraComponent:ShakeCamera(1,0.2)
    
    wait(1)
    
    self.isDead = false
    self.Entity.CameraComponent.Position = Vector3.zero
}
```

# Implementing Start and End of the Game

#### Touch to Start

1. Create the new **started** property in the **FishPlayerComponent**.

    ```lua
    [None]
    boolean started = false
    ```

2. Edit the contents of the `OnUpdate` function as shown below.
    ```lua
    if not self.isDead and self.started then 
    	self:Move(delta)
    end
    
    self:CheckPosY()
    ```
3. Write the following in the first line of the `Floating()` function. If the started is not true, change it to true.

    ```lua
    if not self.started then 
        self.started = true
    end
    ```

4. Modify the `Die()` function to change the started to false when the player dies.
Add the contents to the line below the `self.Entity.CameraComponent:ShakeCamera(1,0.2)`. 

    ```lua
    self.started = false
    ```

    
