# Course Introduction
Creation and deletion of entities are the most frequently used functions when creating a world.
In this course, we'll look at how to create and delete entities, as well as discuss an effectiveness validation function to check the existence of entities.

# Creating Entity
**SpawnService** provides a function to create an entity. We'll look into `SpawnByEntity` and `SpawnByModelId`, the main functions used when creating entities.

## SpawnByEntity
Creates a new entity based on an entity.

 #### Parameter
|Parameter   |Type  |Description  |
| :---: | :---: | --- |
| entity | Entity  | Specifies the entity to be duplicated. |
| name | string  | Set the name of the entity that will be created. |
| spawnPosition | Vector3  | Set the location coordinate where the entity will be created. |
| parent<br>(Optional) | Entity  | Enter the parent entity of the entity that will be created. |
| includeChild<br>(Optional) | boolean  | Decide whether you will also copy the child entity of the entity to be duplicated. |

#### Return Value
* In case of successful spawning, returns **Entity**.
* In case of failed spawning, returns **nil**.
    
#### Usage Example
Let's learn how to create an entity identical to the entity placed in a map.
 
1. In **Preset List**, place the entity to be duplicated on the map.
     ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16602756252223e544189296248be9e622a6f6aede534.png "1")
2. Generate the new script component **SpawnManager**. Double-click **SpawnManager** to open Script Editor, and create the `SpawnByEntity` function.

    ```lua
    void SpawnByEntity()
    {
        local entity = _EntityService:GetEntityByPath("/maps/map01/monster-8") -- Right click on placed entity - Select Copy Entity Path to get the Path.
        local name = entity.Name .. "Copy" -- Set the entity name to be created
        local spawnPosition = Vector3(0,0,0) -- Set the location to be created
        
        local spawnEntity = _SpawnService:SpawnByEntity(entity,name,spawnPosition)
        if isvalid(spawnEntity) == false then 
            log("Spawn Failed")
        end
    }
    ``` 
3. Upon completion, add the `OnBeginPlay` function and write as follows.
    ```lua
    [server only]
    void OnBeginPlay()
    {
        self:SpawnByEntity()
    }
    ``` 
4. Add the **SpawnManager** script component to **common**.
     ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1659601195286fbb9556ae01c4adb8b44b90ab8e68840.png "2")
5. ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") Click **[Start]** to check if an entity identical to the entity placed on the map is created.
     ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1655870326527ec7ce48fd9b74ea799d7072cf1ac7f5f.png "1")

## SpawnByModelId

#### Description
Assign one model from the models added to **Workspace** and create as an entity.

#### Parameter

| Parameter | Type | Description |
| :--: | :--: | ----------- |
| id | string | Among the models added to the workspace, enter the Entry ID that will serve as the template for the newly created entity.<br>You can import an Entry ID by selecting **Workspace - Model (to become the template for the entity to be created) - Copy Entry ID**. |
| name | string | Set the name of the entity that will be created. |
| spawnPosition | Vector3 | Set the location coordinate where the entity will be created. |
| parent | Entity | Enter the parent entity of the entity that will be created. |

#### Return Value
* In case of successful spawning, returns **Entity**.
* In case of failed spawning, returns **nil**.

#### Usage Example
1. In **Preset List**, open the context menu for the desired model, press **Add to Workspace** to add the model to **Workspace**.
        ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/165995860982314bcc1601e8c44d885b2067b34ea32f3.png "8")
2. In the context menu of the added model, press **Copy Entry ID** to copy the **Entry ID** to the clipboard.
        ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1687760366731d075f28ddc3b4f14ac0791f36257437a.png "9")
3. Open **SpawnManager** component and create the `SpawnByModelId` function.
    ```lua
    void SpawnByModelId()
    {
        local id = "maplestorymonster$0108737afb9548309c03795430b20c05" -- Click Copy Entry ID to paste the copied ID. Remove the preceding "model://".
        local name = "SpawnedEntity" -- Set the entity name to be created
        local spawnPosition = Vector3(0,0,0) -- Set the location to be created
        local parent = _EntityService:GetEntityByPath("/maps/map01") --The parent entity of the entity to be created
        
        local spawnedEntity = _SpawnService:SpawnByModelId(id, name, spawnPosition, parent)
        if isvalid(spawnedEntity) == false then
            log("Spawn Failed")
        end
    }
    ``` 
4. Modify the `OnBeginPlay` function as below.
    ```lua
    [server only]
    void OnBeginPlay()
    {
        -- self:SpawnByEntity()
        self:SpawnByModelId()
    }
    ``` 
4. ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") Click **[Start]** to check if the same entity as the model in **Workspace** is created.
    ![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1655816153693283ae21166b6476ea65044a4ffde1a21.png "13")

# Removing Entity
MapleStory Worlds provides two methods to remove entities, `_EntityService:Destroy` and `Entity:Destroy`.
`_EntityService:Destroy` deletes an entity that was passed as a parameter, while `Entity:Destroy` deletes the entity itself that called **Destroy**. Hence, the two functions share the same deletion movement with only a difference in the target.

## Destroy

#### Parameter

| Parameter | Description |
| :--: | -- |
| entity | Enter the entity, which is the target of removal. |

#### Usage Example
Create an entity via **SpawnService**'s spawn function, then remove it 3 seconds after creation.
For the example, use the **SpawnManager** script component, which was used in the creation of the entity above.
1. Add a property, and set the type and name as follows.
    ```lua
    Property:
    [none]
    any SpawnedEntity = nil
    ```
    
2. Modify the `SpawnByModelId` function as follows.

    ```lua
    void SpawnByModelId()
    {
        local id = "maplestorymonster$0108737afb9548309c03795430b20c05"
        local name = "SpawnedEntity"
        local spawnPosition = Vector3(0,0,0)
        local parent = _EntityService:GetEntityByPath("/maps/map01")
        
        -- Modify the part below.
        self.SpawnedEntity = _SpawnService:SpawnByModelId(id, name, spawnPosition, parent)
        if isvalid(self.SpawnedEntity) == false then
            log("Spawn Failed")
        end
    }
    ``` 
    
3. Add the `OnUpdate` function and compose its content as follows. Write a script that deletes the entity 3 seconds after its creation.

    ```lua
    [server only]
    void OnUpdate(number delta)
    {
        if isvalid(self.SpawnedEntity) == false then
            return
        end
        
        if self._T.time == nil then
            self._T.time = 0
        end
    
        self._T.time = self._T.time + delta
    
        if self._T.time >= 3 then
            _EntityService:Destroy(self.SpawnedEntity)
        end
    }
    ```
5. ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") Click **[Start]** to check that the entity disappears 3 seconds after its creation.

    ><span style="color: #7cafc2">**Tip**
    > You can delete entities using `Entity:Destroy`. The following is a script using `Entity:Destroy`.</span>

    ```lua
    [server only]
    void OnUpdate(number delta) 
    {
        if isvalid(self.SpawnedEntity) == false then
            return
        end
        
        if self._T.time == nil then
            self._T.time = 0
        end
    
        self._T.time = self._T.time + delta
    
        if self._T.time >= 3 then
        self.SpawnedEntity:Destroy() -- Replace _EntityService:Destroy with Entity:Destroy.
        end
    }
    ```
# Entity Effectiveness Validation
When creating or deleting an entity, check the state of the entity. Normally you would perform a **nil check of the entity**, but you often don't obtain the desired value, such as when the entity is **waiting to be deleted**, for instance. In this case, you can use the entity effectiveness check feature.
Entity effectiveness can be checked via `isvalid`, a global function. 

## isvalid
The `isvalid` function checks if an entity is **waiting to be deleted** or if it has been **already deleted**, then returns **true** or **false**.
#### Parameter
| Parameter | Description |
| :--: | ----------- |
| entity | Enter the entity, which is the target of effectiveness validation. |

#### Return Value
* If the entity passed as a parameter exists, returns **true**.
* If the entity is nil, waiting to be deleted, or has already been deleted, returns **false**.
#### Usage Example
In continuation of the above `Destroy` function example, let's see which value the `isvalid` function returns before and after the deletion of the entity, and how to use the method.

1. Edit the `OnUpdate` function of **SpawnManager** as follows.

    ```lua
    [server only]
    void OnUpdate(number delta)
    {
        if isvalid(self.SpawnedEntity) == false then
            return
        end
        
        if self._T.time == nil then
            self._T.time = 0
        end
    
        self._T.time = self._T.time + delta
    
        if self._T.time >= 3 then
            local isvalidValue = isvalid(self.SpawnedEntity)
            log("Before deletion : "..tostring(isvalidValue))
            self.SpawnedEntity:Destroy()
            isvalidValue = isvalid(self.SpawnedEntity)
            log("After deletion : "..tostring(isvalidValue))
        end
    }
    ```
        
2. ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") Click **[Start]** to test. Check the log for before and after entity deletion in the console window.
    ![19](https://mod-file.dn.nexoncdn.co.kr/bbs/1655869031532a1ede31bc9bd46d8b521d0e7b328442f.png "19")

    > <span style="color: #7cafc2">**Tip**
    > Although we usually use the global function `isvalid` for effectiveness check, `EntityService:IsValid(entity)` offers a similar function.
    > So if you modify the above script as follows, the result will be the same.</span>

    ```lua
     [server only]
     void OnUpdate(number delta)
     {
         if isvalid(self.SpawnedEntity) == false then
             return
         end
         
         if self._T.time == nil then
             self._T.time = 0
         end
      
         self._T.time = self._T.time + delta
      
         if self._T.time >= 3 then
             local isvalidValue = _EntityService:IsValid(self.SpawnedEntity)
             log("Before deletion : "..tostring(isvalidValue))
             self.SpawnedEntity:Destroy()
             isvalidValue = _EntityService:IsValid(self.SpawnedEntity)
             log("After deletion : "..tostring(isvalidValue))
         end
    }
    ```