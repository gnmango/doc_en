# Implementing Obstacles and Collision
Let's create an obstacle entity to add a colliding body. The entities that implemented the colliding body will be a model and make the future World dynamic.
#### Creating Obstacle Entity
1. Create the new EmptyEntity and rename it to **Model_Obstacle**.
2. Add the **TransformComponent** to the Model_Obstacle. 
2. Create the new **Top and Bottom** entities and make them a child entity of **Model_Obstacle**. 
A parent-child relationship makes it more manageable. 
![model_Obstacle](https://mod-file.dn.nexoncdn.co.kr/bbs/17067536195231a919e3f9fca42ceaff3c2366a26707e.png "model_Obstacle")
3. Add the **TransformComponent and SpriteRendererComponent** to the Top and Bottom entities.
4. Enter the RUID to the **SpriteRUID** of both entities' SpriteRendererComponent.
    * RUID: 8290994289524b3eb5f760c839e66121
5. Change the **FlipY** value of the Top entity so that the two obstacles are placed in a vertical position. 
![Obstacle_Entity](https://mod-file.dn.nexoncdn.co.kr/bbs/17071165519473b7ece1269b94dd1866002aa8efa4e63.png "Obstacle_Entity")

    > <span style="color: #7cafc2">**Tip**
    > You can increase the size of the obstacle by adjusting the Scale value of TransformComponent.</span>

#### Creating Obstacle Collision Judgment
1. Add the **TriggerComponent** to the **Top, Bottom** entities.
2. Change the ColliderType to **Polygon** and adjust the collision area to match the sprite.
![PolygonPoints](https://mod-file.dn.nexoncdn.co.kr/bbs/170321219966855437ce191d6470a980968d8306aa6d7.png{"width":"340px"} "PolygonPoints")

#### Creating Player Collision Judgment
In order for the player and the obstacle to collide, DefaultPlayer also needs a colliding body.
1. Add the **TriggerComponent** to the DefaultPlayer and change the ColliderType to **Circle**.
2. Adjust **ColliderRadius, ColliderOffset** to match the size of the sprite.

    * CircleRadius: 0.265
    * ColliderOffset: X 0.1 , Y 0.445
    
    ![CircleCollider](https://mod-file.dn.nexoncdn.co.kr/bbs/1703213748330c86431a5087d409c9fce0f0f55f6193a.png "CircleCollider")
3. Create the new **ObstacleComponent** and add it to the **Model_Obstacle** entity.

4. Add the new **top, bottom** properties.

    ```lua
    Property:
    [None]
    Entity top = nil
    [None]
    Entity bottom = nil
    ```

5. Add a OnBeginPlay() function. At the start of the game, use the GetChildByName() function to find the Top and Bottom entities.

    ```lua   
    Function:
    [client only]
    void OnBeginPlay()
    {
        self.top = self.Entity:GetChildByName("Top")
        self.bottom = self.Entity:GetChildByName("Bottom")
    }
    ```
6. Press the context menu to model the entity by pressing the **Create Model From Entity**.
        
# Loading Obstacles
Let's spawn five Model_Obstacles created when the game starts. 

1. Create the new **GameLogic**.
2. Create the new table-type **obstacles** property.
 
    ```lua
    Property:
    [None]
    table obstacles = {}
    ```

3. Add the OnBeginPlay() and Init() functions. Write the following so that you can load obstacles using the SpawnService:SpawnByModelId().

    ```lua
    Method:
    [client only]
    void OnBeginPlay()
    {
        self:Init()
    }

    [client only]
    void Init()
    {
        -- Obstacle Spawn Interval
        local intervalX = 4.5
        
        
        -- Add a value to the obstacles table to spawn five obstacles.
        for i = 1,5 do
            local entity = _SpawnService:SpawnByModelId(_EntryService:GetModelIdByName("Model_Obstacle"),"obstacle",Vector3(i*intervalX,0,0),_UserService.LocalPlayer.CurrentMap)
    
            table.insert(self.obstacles, entity)
        end
    }
    ``` 

4. Press the Start button to confirm that five obstacles have been spawned at regular intervals.
![Obstacle5](https://mod-file.dn.nexoncdn.co.kr/bbs/1703149484170498ee418e7b540899f2ec1d21a2dea16.png "Obstacle5")

#### Changing Obstacles Spawn Method 
Placing all of the obstacles necessary for the game or loading them all at once can put a load on game performance. Creating and reusing a certain number of entities can help improve the performance. Let's reuse the five obstacles. Let's load the five entities, bring the first obstacle to the back of the obstacles, and create it again.

1. Create the new **intervalX, startPosX, index** properties in the **GameLogic**.

    ```lua
    Property:
    [None]
    number intervalX = 4.5
    [None]
    integer startPosX = 5
    [None]
    integer index = 1
    ```

2. Create a new Init() function in the **GameLogic** and write the following for the location where the obstacle will be spawned.
    
    ```lua
    Method:
    [client only]
    void Init()
    {
        if #self.obstacles < 1 then
            for i = 1, 5 do
                local entity = _SpawnService:SpawnByModelId(_EntryService:GetModelIdByName("Model_Obstacle"),"obstacle", Vector3(i * self.intervalX + self.startPosX, 0 ,0), _UserService.LocalPlayer.CurrentMap)
                
                table.insert(self.obstacles, entity)
                
                self.index = i
            end
        
        else
        
            local i = 1
            
            for _, entity in pairs(self.obstacles) do
            
                ---@type Entity
                local newEntity = entity
                
                newEntity.TransformComponent.Position = Vector3(i * self.intervalX + self.startPosX, 0, 0)
                i += 1
            
            end
            
            -- Reset index
            self.index = 1 
        end
    }
    ```
     
3.  Create the new Init() function in the **ObstacleComponent** and write as follows.

    ```lua
    [client only]
    void Init(integer index)
    {
        self.Entity.TransformComponent.Position = Vector3(index * _GameLogic.intervalX + _GameLogic.startPosX + 1, 0, 0)
    }
    ```
    
4. Add the OnUpdate() to the **ObstacleComponent** as each frame should have an obstacle.

    ```lua
    [client only]
    void OnUpdate(number delta)
    {
        local playerPos = _UserService.LocalPlayer.TransformComponent.Position
        local position = self.Entity.TransformComponent.Position
    
        if playerPos.x - 5 > position.x then
           _GameLogic.index += 1
           self:Init(_GameLogic.index)       
        end
    }
    ```

#### Adjusting Obstacles
Let's create Model_Obstacle in a jagged shape. Rather than placing the model directly in the Scene, use the UtilLogic's `RandomIntegerRange()` function to randomly specify the range in which the model will be created.
1. In the **ObstacleComponent**'s Init() function, continue to write the following.

    ```lua
    [client only]
    void Init(integer index)
    {
        local offset = _UtilLogic:RandomDouble() * 2 * _UtilLogic:RandomIntegerRange(-2,1)
        local space = _UtilLogic:RandomDouble() * 3 + 5
                
        self.top.TransformComponent.Position.y = offset + space + 1
        self.bottom.TransformComponent.Position.y = offset -3
    }
    ```

2. Modify the **GameLogic**'s Init() function. Add the following under the table.insert(self.obstacles, entity) to spawn the obstacles.

    ```lua   
    entity.ObstacleComponent:Init(i)
    ```

    > <span style="color: #7cafc2">**Tip**
    > If the screen is not filled with obstacles, you can increase the size of the obstacles. </span>