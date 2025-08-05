# Course Introduction 
Let's call a taxi so that Azalin of the brave Cgynus Knights can regroup with the Nightwalker unit that marched into the Labyrinth of Suffering. We must ensure that the taxi stops in front of Azalin instead of driving past her and that she can climb aboard. 

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1747902531299ee5bec9350ac458d9f255a76eba57191.gif "Example")
# Editing a StaticNPC Model
Remove RigidbodyComponent from the StaticNPC model. StaticNPC includes both RigidbodyComponent and TransformComponent. This is because when neither one of them are deleted, the two values will collide when changing the Position value, causing unwanted movement.

1. Select **WorkSpace -  NativeModel - StaticNPC**.
2. Press the context menu from `RigidbodyComponent` in the property editor and select **Remove Component**.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/174790275979588a9e7fb27374ba083dfe66aa85f8f4c.png{"width":"750px"} "1")
# Placing Azalin and the Taxi
Place the tiles down first, then the taxi to the left, then Azalin to the right.

![2](https://mod-file.dn.nexoncdn.co.kr/bbs/174790263914923ddf9288460433e9ba2857f9be5736d.png{"width":"640px"} "2")

1. Search for **npc-3539** from **Preset List - NPC**. Place the taxi on the left of the map.
2. Change the **FlipX** under `SpriteRendererComponent` to **True**.
3. Change the entity's name to **Taxi**.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1747906441205a91842e506844436baf2fb7b232b02e7.png{"width":"420px"} "3")
4. Search for **npc-5537** from **Preset List - NPC** and place it on the right of the map.
5. Change the entity's name to **Azalin**.

> <span style="color: #7cafc2">**Tip**
> By changing the entities' names, it wil be easier to identify them and call them through simplified paths in the script.</span>

# Moving the Taxi
The **Position** value under `TransformComponent` needs to be changed to move the taxi. All entities that are placed on the map have location information. This location information is displayed as Position x, y, z from TransformComponent. To move it to the right, the x value needs to be larger.
   
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1747902531299ee5bec9350ac458d9f255a76eba57191.gif "Example")

1. Create a new script component by selecting **![workspace](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_workspace.png "workspace") Workspace - ![MyDesk](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_user_folder_no.png "MyDesk") MyDesk - Create Scripts - Create Component**. Change the component name to **TaxiComponent**.

2. Select the **Taxi** entity that's placed on the map and add **TaxiComponent** in the property window.
 ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1747906917428bf8e9ca2f016463ca3ca1f839b59d363.png{"width":"420px"} "5")

3. Add a new property and change the name to **TaxiTransform**. Change the type to **Component**.

    ```lua
    Property:
    [Sync]
    Component TaxiTransform = nill
    ```

4. Add the `OnBeginPlay()` function from **Method**. This is a function that is called first once the script is executed.
    
    ```lua
    [server only]
    void OnBeginPlay()
    {
        local taxi = self.Entity
        self.TaxiTransform = taxi.TransformComponent
    }
    ```
    
5. Add the `OnUpdate()` function. Write the following so that the taxi's `Position` moves **0.02** at a time.

    ```lua
    [server only]
    void OnUpdate()
    {
        local pos = self.TaxiTransform.Position
        pos.x = pos.x + 0.02
        self.TaxiTransform.Position = pos
    }
    ```

> <span style="color: #585858">**Learn More**
>  You can also use the `Translate()` function instead of `Position` to move the taxi. Use with a method like `self.TaxiTransform:Translate(0.02, 0)`.</span>

6. Check if the taxi moves.

# Stopping the Taxi
The created taxi continues to move without stopping. The taxi needs to stop where Azalin is standing, so Azalin's location information needs to be used. Let's add a condition so that it stops when it gets closer to Azalin.

1. Add the new **AzalinTransform** property to the **Taxi** component and change the type to **Component**.

    ```lua
    Property:
    [Sync]
    Component AzallinTransform = nil
    ```
    
2. Write the following for the `OnBeginPlay()` function. Bring the Azalin entity to the entity path so its location information can be used.
 
    ```lua
    [server only]
    void OnBeginPlay()
   { 
        local azallin = _EntityService:GetEntityByPath("/maps/map01/Azallin") 
        self.AzallinTransform = azallin.TransformComponent
    }
    ```

3. Edit the `OnUpdate()` function as shown below. When the taxi gets close to Azalin, it will now stop.

    ```lua
    [server only]
    void OnUpdate() 
    {
        local pos = self.TaxiTransform.Position 
        local targetPos = self.AzallinTransform.Position 
        
        if pos.x < targetPos.x then
            pos.x = pos.x + 0.02 
            self.TaxiTransform.Position = pos 
        end
    }
    ```

5. Check if the taxi stops in front of Azallin. `OnUpdate()`**Taxi**