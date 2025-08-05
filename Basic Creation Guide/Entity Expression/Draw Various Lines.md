# Course Introduction
You can draw various lines in the World freely by using LineRendererComponent.

# LineRendererComponent
LineRendererComponent can designate the number of points to draw straight lines and curve lines you want on MapleTile and RectTile. You can draw one line for one LineRendererComponent, so if you want to express several lines, you should use the entity where several LineRendererComponents are included. The main properties are as follows.
#### IsSmooth
It draws smooth curve lines on enable and sharp broken lines on disable.
![IsSmoothExample](https://mod-file.dn.nexoncdn.co.kr/bbs/1657956230168af666af68a884bebbd5e2bf9694bc4cd.png "IsSmoothExample")
#### Loop
On enable, it draws a closed line by connecting the start and end points. The default value is the disabled state.
![LoopExample](https://mod-file.dn.nexoncdn.co.kr/bbs/16579562450262b41e742a8cb4ddf835fd8b7db4c87cd.png "LoopExample")
#### Points 
The group of dots that form a line. 

* Click the <span style="color: #dc9656">**Points Edit**</span>: [Edit] button to activate Edit. You can modify, add, and delete the point location in the Scene.
* <span style="color: #dc9656">**Size**</span>: Indicates the number of points connecting lines. The number starts from 0, and the points will be added as much as the value you entered for Size. 
* <span style="color: #dc9656">**[Numbers]**</span>: Refer to the number of connecting points of a line.
* <span style="color: #dc9656">**Color**</span>: Specifies the color of the line. When you specify different colors for each Size from one component, the point where the colors are crossed will be mixed up naturally.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16944139559805794266b75bd4670835226315bec0354.png "1")
* <span style="color: #dc9656">**Position**</span>: Indicates the position of the point of connecting lines. You can draw the line upon making at least two positions. You can draw a line by changing the x and y values by setting the first [1] as (0,0) basis. 
* <span style="color: #dc9656">**Width**</span>: Sets the thickness of the line. A greater number makes the line thicker.<br>![WidthExample](https://mod-file.dn.nexoncdn.co.kr/bbs/1657949740811c064ec15791d40c0a2fc0c98dbbf57b6.png "WidthExample")
# Draw a Line in the World
#### Draw a Straight Line
You can draw straight lines by setting two Points and the Position.

1. ![entity](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_entity_no.png "entity") Create EmptyEntity and add LineRendererComponent.
2. Enter 3 for Size, and set Color and Width.
3. Enter (0,0) and (4,0) for each Position.
![line](https://mod-file.dn.nexoncdn.co.kr/bbs/166358584146243474ff4f1644094891eed7a6da66834.png "line")
#### Draw Triangle
You can draw triangles by connecting the start and end points by using three Points. 

1. ![entity](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_entity_no.png "entity") Create EmptyEntity and add LineRendererComponent.
2. Enter 3 for Size, and set Color and Width.
3. Enter (0,0), (1,1), and (2,0) for each Position.
5. Activate Loop Property.
![triangle](https://mod-file.dn.nexoncdn.co.kr/bbs/1663844235905ace8de03857e44f1b9c55a8c83d2199d.png "triangle")

#### Draw Shape with Curve Line
You can draw a curve line by using four Points and IsSmooth Property.

1. ![entity](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_entity_no.png "entity") Create EmptyEntity and add LineRendererComponent.
2. Enter 4 for Size, and set Color and Width.
3. Enter (0,0), (1,1), (2,-1), and (3,0) for each Position.
4. Activate IsSmooth and Loop Property.
![loop](https://mod-file.dn.nexoncdn.co.kr/bbs/16579497886734ab7d8aba66a4ceab4f245a45535c6cd.png "loop")

#### Drawing with Material
You can use Material's SpritePattern Shader to draw lines. For questions about how to use the Material, please refer to [Utilizing Material](docs/?postId=828{"target":"_self"}).
You can use both user resources and cloned MSW resources as sprites available for LineRendererComponent. To learn how to clone MSW resources, see [Managing Resources](docs/?postId=690{"target":"_self"}).

1. Select **Workspace - MyDesk - Create Material** to make a new Material.
2. In the Properties window of the Material you've created, select **Shader - LineRenderer - SpritePattern**.
3. **Select the image you'd like to use for SpriteRUID**. If necessary, adjust the Offset and Scale values.
4. ![entity](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_entity_no.png "entity") Create EmptyEntity and add LineRendererComponent.
5. In the **MaterialId** of LineRendererComponent, press ![open](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_open_folder.png "open") and specify the Material.
6. Change the Size and Position values to draw the line.
![03](https://mod-file.dn.nexoncdn.co.kr/bbs/16842378305042dc663a11c3b49eebd58b8804d407e88.png{"width":"450px"} "03")

><span style="color: #7cafc2">**Tip.**
> Both the user resource and the cloned MSW resource must have their lab mode set to **Repeat**.
> ![Repeat](https://mod-file.dn.nexoncdn.co.kr/bbs/168422631874542cfe7bb765e4d659b113e430d223b99.png{"width":"340px"} "Repeat")

> <span style="color: #7cafc2">**Tip.**
> If the number of LinePoints exceeds 10,000, any LinePoints beyond that limit will not be drawn. 
> In addition, using IsSmooth will reduce the total number of LinePoints that can be drawn to approximately 1,000.
> </span>

# Use Example
Let us create a model following the player, then make it draw and disappear lines following the player's movement.

![TrailExample](https://mod-file.dn.nexoncdn.co.kr/bbs/1657952569571be162fe5c3c64716b82f6a51e091f1cc.gif "TrailExample")

1. ![model](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_model_no.png "model") Create Trail model, and add TransformComponent and LineRendererComponent.
2. Create a new ![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "component") TrailDrawer component, and add it to the Trail model.
    ```lua
    [None]
    Vector3 PrevPos = Vector3(0,0,0)
    [None]
    number Threshold = 0.1
    [None]
    number PointLife = 2
    [None]
    SyncTable<number> PointLives
    [None]
    Entity Target = nil
    
    Method:
    [client only]
    void OnUpdate(number delta)
    {
        self:EraseTail(delta)
        
        local currPos = self.Target.TransformComponent.WorldPosition
        local dist = Vector3.Distance(self.PrevPos, currPos)
        
        if dist < self.Threshold then
        	return
        end
        
        self.PrevPos = currPos
        self:AddPoint(currPos)
    }
    
    void EraseTail(number delta)
    {
        local currPos = self.Target.TransformComponent.WorldPosition
        
        for i = 1, #(self.PointLives) do
        	local life = self.PointLives[i]
        	life = life - delta
        	self.PointLives[i] = life
        end
        
        if #(self.PointLives) == 1 then
        	self:RemovePointAt(1)
        	self:AddPoint(currPos)
        	return
        end
        
        if self.PointLives[1] <= 0 then
        	self:RemovePointAt(1)
        end
    }
    
    void AddPoint(Vector3 pos)
    {
        local line = self.Entity.LineRendererComponent
        local pointPos = Vector2(pos.x, pos.y + 0.25)
        
        -- Draw Random Color
        local r = _UtilLogic:RandomDouble()
        local g = _UtilLogic:RandomDouble()
        local b = _UtilLogic:RandomDouble()
        line.Points:Add(LinePoint(pointPos, Color(r, g, b), 1))
        table.insert(self.PointLives, self.PointLife)
    }
        
    void RemovePointAt(number idx)
    {
        -- Delete Line
        table.remove(self.PointLives, idx)
        self.Entity.LineRendererComponent.Points:RemoveAt(idx)
    }
    
    
    void Init(Entity target)
    {
        self.Target = target
        
        local pos = target.TransformComponent.WorldPosition
        
        self.PrevPos = pos
        
        self:AddPoint(pos)
    }
    ```

3. Create a new ![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "component") TrailSpawner component and add it to ![DefaultPlayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_myavatar_no.png "DefaultPlayer") DefaultPlayer.
4. To spawn the line following the ![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "component") TrailSpawner component movement, you should write as below using `SpawnService`. In SpawnByModelId, you should enter the ![model](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_model_no.png "model") Trail's ID. If you use the ID in the example, it will not work properly. So please be careful in writing. 
    ```lua
    Property:
    [None]
    Entity TrailEntity = nil
    
    Method:
    [client only]
    void OnBeginPlay ()
    {
        self.TrailEntity = _SpawnService:SpawnByModelId("8ae67c69-32ac-40e3-82b1-03d1409be467", "Trail", Vector3.zero, nil)
        self.TrailEntity.TrailDrawer:Init(self.Entity)
    }
    
    void OnEndPlay ()
    {
        if self.TrailEntity ~= nil then
        	self.TrailEntity:Destroy()
        end
    }
    ```
##### Reference Guide
* [Utilization of RectTileMap]
* [Changing Location, Size, Rotation of Entities]