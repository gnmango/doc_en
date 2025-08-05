# Course Introduction 
Let's learn about component structure and how to access entities and components through scripts.
# Structure of Entities and Components
First, let's examine the structure of entities and components.
Each entity is composed of different components and connected to each other in a parent-child relationship. By utilizing this hierarchical structure, you can access other entities or components from a specific component.
The image below illustrates the hierarchical structure between entities, as well as the components that make up each entity. Please take a moment to understand this structure before proceeding.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16371471617051e7a31dbcf5742d8b8f61020bf6913db.png "1")
<br>
> For more details about the concepts of entities and components, please refer to the following guide:
> [Entities, Components, and Properties](https://mod-developers.nexon.com/docs?postId=54{"target":"_self"})
# Entity Reference
Let's take a look at how to reference other entities from within a script component.

#### self.Entity
First, let's explore how to reference the entity that includes the component itself.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1635291524024e210da552eaf462a8558eddda0c5b43c.png "2")
<br>
When writing scripts, it is common to encounter situations where Component A references Entity C, as depicted in the diagram above.
The method is very simple. Like the code below, the entity can be received through self.Entity.
```lua
void OnBeginPlay()
{
    local myEntity = self.Entity
    log(myEntity.Name)  -- Outputs entity name
}
```
#### Entity.Parent
Let's explore how to reference parent entities.
Below is an example where Component A references Entity A. You can reference Entity A, the parent of Entity C, after referencing Entity C itself.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1635291534007f6d3e36ca453424485607b7fda2dd06b.png "3")
<br>
The following is an example code.
```lua
void OnBeginPlay()
{
    local myEntity = self.Entity
    log(myEntity.Name)  -- Outputs name of Entity C
 
    local parentEntity = myEntity.Parent
    log(parentEntity.Name) -- Outputs name of Entity A
}
```
#### Entity.Children
Let's explore how to retrieve a child entity from the parent entity.
You can reference the children entities of a parent entity using Entity.Children.
However, Entity.Children retrieves all children entities in list format. To reference a specific entity, such as Entity D, from the children entities of Entity A as shown in the diagram below, you will need to iterate through the array and selectively reference the entities that meet the desired condition.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/163529154797085c3b3e542834bb89e508df36a58dd18.png "4")
<br>
Next is an example of code that refers Entity D from Component A, as shown above.
```lua
void OnBeginPlay()
{
    local myEntity = self.Entity
    log(myEntity.Name)    -- Outputs name of Entity C.
      
    local parentEntity = myEntity.Parent
     log(parentEntity.Name) -- Output name of Entity A.
    
    local children = parentEntity.Children  --Receives the children entities of Entity A in tabular format.
    local childrenEntityName = ""
          
    for index, child in pairs(children) do
        if child.Name == "EntityD" then
            childrenEntityName = child.Name
            break
        end
    end
         
    if childrenEntityName == "" then
        log("EmptyEntityD")
        return 
    end
     
    log(childrenEntityName)
}
```
#### Entity:GetChildByName()
We will learn about another method of referring to child entities like Entity.Children.
The GetChildByName() function returns a child entity with a name that matches the parameter value. It retrieves an entity with the same name from among the child entities of entity A. Therefore, there is no need to retrieve all child entities using Entity.Children and then iterate through them in a loop to find the desired entity. You can import child entities much more easily if you know the name of the entity you are searching for.
* **Parameter**
    * GetChildByName function can receive two parameters.
    
        | Parameter | Type | Description |
        | --- | --- | --- |
        | name | string | Passes on the name of the child entity you are searching for. |
        | recursive | bool | Set whether to search recursively for children entities within child entities.<br>Set it to "True" to search recursively for children entities within children entities.<br>Set it to "False" to search only for immediate children entities.<br>This is an optional parameter, so you can omit it.<br>If no value is provided, it will be set to False (do not search within children entities). |
<br>
* **Return value**
    * If there is an entity you are searching for, then return the entity,
    * and if not, return nil.
<br>
* **Example of composing script**
    * Next is a script in which you receive the child EntityD from the parent EntityA.
    ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/163529158705102eb762c67b14c3cbe9c07c0ef87e8f4.png "5")
    <br>
        ```lua
        void OnBeginPlay()
        {
            local entityA = self.Entity -- get EntityA
            local entityD = entityA:GetChildByName("EntityD")    --  Get EntityD, a child entity
            log(entityD.Name)    -- Output name of EntityD to the Console window
         
            --If there is no child entity with the specified name, nil is returned
            local entityF = entityA:GetChildByName("EntityF")
            log(entityF)    -- Output nil to the Console window
        }
        ```
    <br>
* The following script allows you to reference the grandchild entity named EntityJ from the parent entity EntityA.
When importing a child entity, set the recursive value, the second parameter, as true.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1635291597805292fc8bfa543454eb370f7861ad928e3.png "6")
<br>
    ```lua
    void OnBeginPlay()
    {   
        local entityA = self.Entity    -- Get EntityA
        local entityJ = entityA:GetChildByName("EntityJ", true)
        -- Search for the grandchild entity with "EntityJ", a name of EntityJ. Set the second parameter value as true to search for the grandchild entity.     
        log(entityJ.Name)    -- Output the name of EntityJ to the Console window
    }
    ```
# Referencing an Entity through _EntityService
So far, we have explored how to reference entities through the hierarchical structure of high and children entities in components.
This approach is efficient when finding entities close to the component, but it becomes cumbersome when we need to find the great grandparent, then the child entities of that parent.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/16352916095449bc3ee34f64745c8856c713169446ff5.png "7")
<br>
By utilizing _EntityService, you can use functions that directly find and return the desired entity, making it convenient for your needs.
#### GetEntityByPath()
This function takes the desired entity path as a parameter and returns the entity at that path.
This is one of the most commonly used functions.
* **Parameter**
    
    | Parameter | Type | Description |
    | --- | --- | --- |
    | worldPath | string | Enter the desired entity path.<br>The path starts from /maps/ and includes the subsequent children entities.<br>For example, in the following hierarchy, the path for "object_1" would be as follows.<br>![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1635483238562b41e1da8c00d4eeaab15d6db6080a4a3.png "8")<br>"/maps/map01/object_1" |
    <br>
    > <span style="color: #7cafc2">**Tip**
    > Manually entering entity paths can be cumbersome and is error-prone, making it inefficient.
    > By using the entity path copy feature, you can easily and accurately input entity paths.
    > By clicking **Copy Entity Path** in the context menu of a specific entity, the corresponding entity path will be stored to the clipboard.</span>
    > ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1686210532005a4a5c0075ed74dfd84667cb938b58532.png "9")

* **Return value**
    * If there is an entity identical to the input path in the parameters, it will be returned.
    * If there are multiple entities that are identical to the path value, only the first entity found will be returned.
    * If there is no entity identical to the path value, returns nil.
<br>
* **Example of composing script**
    * Here's an example of code that references Entity E from Component A.
    ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1635291664607739291779c984748ad56f8bb91fddb83.png "10")
    <br>
        ```lua
        void OnBeginPlay()
        {  
            -- Enter the path of EntityE as a parameter to return EntityE
            local EntityE = _EntityService:GetEntityByPath("/maps/map01/EntityO/EntityB/EntityE")   
            if isvalid(EntityE) == false then return end   
         
            log(EntityE.Name)
        }
        ```
#### GetEntitiesByPath()
This function returns all entities that are identical to the path sent as parameter as a list format.
It is useful when there are different entities with the same name and same path.
For example, it can be useful when controlling a large number of entities, such as projectiles or bullets, that are created with the same name.

* **Parameter**

    | Parameter | Type | Description |
    | --- | --- | --- |
    | worldPath | string | Input the entity path you want to obtain.<br>The path starts from /maps/ and enters the children entities. |

* **Return value**
    * Returns all entities identical to the input path value provided as a parameter.
    * If there is no entity identical to the path value, returns nil.
<br>
* **Example of composing script**
    * Below is a code for importing an entity with the name EntityE among the child entities of EntityB from ComponentA.
    ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/16352916806362d0bfb38644b44e78e8ab10b2ce762e8.png "11")
    <br>
        ```lua
        void OnBeginPlay()
        {
            --Return all entities identical to the input path value provided as a parameter as a list format.
            local entityTable = _EntityService:GetEntitiesByPath("/maps/map01/EntityO/EntityB/EntityE")
            if entityTable == nil then return end
         
            -- Traverse all returned entities and send the name of the entities. (Output all to EntityE)
            for index, entity in pairs(entityTable) do
                log(entity.Name)
            end
        }
        ```
#### GetEntity()
Return the entity with the specified entity ID when passed as a parameter.
Using the GetEntity function, which allows you to retrieve entities based on their unique ID, is more efficient when the entity hierarchy is dynamically changed or when there is a possibility of entity names being changed, resulting in changes to the paths.
* **Parameter**

    | Parameter | Type | Description |
    | --- | --- | --- |
    | ID | string | Enter the Entity ID you want to retrieve.<br>In the entity's context menu, click **Copy Entity ID** to copy<br>the ID of the entity to the clipboard.<br>![12](https://mod-file.dn.nexoncdn.co.kr/bbs/16862106228295b326090f635424abcc59cf1e2980a19.png "12")<br>Alternatively, you can obtain the entity's ID in a script as follows.<br>local id = Entity.ID |

* **Return value**
    * Returns the entity of the ID entered as the parameter.
    * If there is no entity with the ID, returns nil.
 <br>
* **Example of composing script**
    * Here's an example of retrieving EntityE under EntityB from ComponentA.
    ![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1635291706386333eb05e82114f21a88219ec5980a571.png "13")
    <br>
        ```lua
        void OnBeginPlay()
        {
            -- If you pass the ID of the entity you want to find as a parameter, the entity of that ID will be returned.
            local entityE = _EntityService:GetEntity("eb9c3412-9730-4e32-a180-69f4d1596652")
            if entityE == nil then
                return nil
            end
            log(entityE.Name)
        }
        ```
# Unique Entity Reference
We have now explored how to reference entities that are placed in the map or dynamically spawned using _SpawnService.
Now let's explore how to reference unique entities, such as world, maps, or Player. While these entities can also be referenced using the methods mentioned earlier, since they are unique entities that exist only once in the game, there are easier ways to reference them.
#### Entity.CurrentMap
Entity.CurrentMap loads the information about the map where a specific entity exists.
While it is possible to read through the entity's parent hierarchy to access the map entity, using CurrentMap makes it easier to retrieve the map entity.
The following is an example code that retrieves the map01 entity where EntityH is located from ComponentA of EntityH.
![14](https://mod-file.dn.nexoncdn.co.kr/bbs/1635291720112228087ed833e456688e74f3153ac2670.png "14")
<br>
```lua
void OnBeginPlay()
{  
    local myEntity = self.Entity    -- Get EntityH that is contained within the current entity.
    local curMap = myEntity.CurrentMap    -- Get the map01 entity to which the current EntityH belongs.
    if isvalid(curMap) == false then
        return
    end
     
    log(curMap.Name)    -- Output "map01" to the Console window
}
```
#### Entity.CurrentWorld
This property can read world entity through an entity.
Just like maps, as it is unique within the game, world entity can be read from any entity with CurrentWorld.
Here's an example of referencing the world entity from ComponentA of EntityB.
![15](https://mod-file.dn.nexoncdn.co.kr/bbs/163714718037512f7b2e3d4114a4ca50976d3a41504a3.png "15")
<br>
```lua
void OnBeginPlay()
{
    local myEntity = self.Entity    -- Get EntityB that is contained within the current entity.
    local world = myEntity.CurrentWorld    -- Get World entity, the top entity.
    if isvalid(world) == false then
        return
    end
     
    log(world.Name)    -- Output "World" to the Console window
}
```
#### _UserService.UserEntities
A read-only property that can receive all player entities currently connected to the game in a dictionary type.
* Key: string UserId
* Value: Entity PlayerEntity

UserId, the key value, is the ID created when the user joined MapleStory Worlds, and uses the same name as UserId and the player Entity.
```lua
void PrintUserEntitiesName()
{
    log(_UserService:GetUserCount())
    local playerEntitiesDic = _UserService.UserEntities  
    
    -- Iterate through all players and print "Player key value/Player name" on the Console window
    for key, playerEntity in pairs(playerEntitiesDic) do
        log(key.."/"..playerEntity.Name)                        
    end
}
```
#### _UserService.LocalPlayer
A read-only property that can refer to my player entity in the client space.
Since there is no concept of my player in the server, if used on the server side, it will return nil.
```lua
[client Only]
void OnBeginPlay()
{
    local myPlayerEntity = _UserService.LocalPlayer
    log(myPlayerEntity.Name)    -- Outputs the entity name of my player
}
 
[server Only]
void OnBeginPlay()
{
    -- Since there is no concept of My Player in the server, it will return nil
    local myPlayerEntity = _UserService.LocalPlayer     
    log(myPlayerEntity)    -- Outputs nil
}
```
#### _UserService:GetUserEntityByUserID()
This function returns a player entity via the ID of the player passed as a parameter.
* **Parameter**

| Parameter | Type | Description |
| --- | --- | --- |
| UserId | string | Enter the UserId of the player entity you are looking for.<br>UserId is the ID created when the user joined MapleStory Worlds and is currently used with the same value as the name of the player entity. |

* **Return value**
    * When there is a player entity identical to the UserId sent as a parameter, returns the player entity.
    * If there is no player entity identical to the UserId, returns nil.

* **Example of composing script**
    ```lua
    Entity Event Handler:
        service UserService
        HandleUserEnterEventType (UserEnterEventType event) 
        {
            -- Parameters
            local UserId = event.UserId
            --------------------------------------------------------
            local GetUserId = _UserService:GetUserEntityByUserID(UserId)
            log(GetUserId)
        }
    ```
#### Approach and Movement of Component
A read-only property that can refer to my player entity in the client space.
Since you can change the property of each component in the Property Editor, you can also set the property of component in script. In the Property Editor, you can only set the default value of a property before running the game. However, in a script, you can dynamically change the property value while running the game. In addition to property settings, you can also invoke functions belonging to components, allowing you to activate the features developed by the creator during the game execution.
<br>
Now let's explore the approach to accessing components included in each entity. We will also explore how to change dynamically the property values of components and call functions through scripts during gameplay.
#### Approaching Component
Components are the building blocks that operate the an entity. Therefore, accessing components must be done through entities.
Generally, you can access a component by using the format **Entity.Component Name**.
The component names of native components are automatically completed in the Script Editor.
![17](https://mod-file.dn.nexoncdn.co.kr/bbs/163529175281398b0a801dd3e437cabba9f493741f9a1.png "17")
<br>
The following example accesses the **RigidbodyComponent** of my player entity.
```lua 
[client only]
void OnBeginPlay()
{
    local myPlayerEntity = _UserService.LocalPlayer

    -- Reference the component of my player entity to the variable rigidbody
    local rigidbody = myPlayerEntity.RigidbodyComponent 
    if rigidbody == nil then
        log("rigidbody is nil")
        return
    end
    log(rigidbody)
}
```
<br>
Script component can also be accessed in the same way as native components
In the case of creating a script component, you may access it through the composed component name.
Below is an example of a script that accesses a script component with the name **CharacterStatus**.
You can access the **CharacterStatus** after creating it.
```lua
[client only]
void OnBeginPlay()
{
    local myPlayerEntity = _UserService.LocalPlayer
    
    --Refer the script component of my player entity to status variable
    local status = myPlayerEntity.CharacterStatus   
    if status == nil then return nil end
    log(status)
}
```
<br>
> <span style="color: #585858">**Learn more**
> MSW provides various methods to access the component depending on the situation.
> 
> **Array Entity:GetChildComponentsByTypeName(string typename, bool recursive)**
> Returns all components that have names identical to typename among the children entities' components in Array type.
> 
> * **Parameter**
>     
>     | Parameter | Type | Description |
>     | --- | --- | --- |
>     | typename | string | Enter the name of the component you are searching for. |
>     | recursive | bool | Set whether to continue the search until reaching the grandchild entities.<br>If set as true, the search for the component continues until reaching the grandchild entities.<br>If set as false, the search stops at the child entity of Entity.<br>In the case of not entering any value, it will be set as false. |
> * **Example of composing script**
>     If you want to retrieve all instances of ComponentA in the children entities of map01, you can write the following script.
>     ![19](https://mod-file.dn.nexoncdn.co.kr/bbs/163529177940448f5ac49614d4075979b2c3dc83dda6d.png "19")
>     ```lua
>     void OnBeginPlay()
>     {
>         local map01Entity = self.Entity.CurrentMap
>         local ComponentsArr = map01Entity:GetChildComponentsByTypeName("ComponentA")         
>         log(ComponentsArr)
>     }
>     ```
>     <br>
>     If you want to retrieve ComponentA that exists in the children entities of map01, you can write the following code.
>     ![20](https://mod-file.dn.nexoncdn.co.kr/bbs/16352917921880070f662f0394704b49bd154c9903320.png "20")
>     ```lua
>     void OnBeginPlay()
>     {
>         local map01Entity = self.Entity.CurrentMap
> 
>         -- Enter true to the second parameter in the script above 
>         local ComponentsArr = map01Entity:GetChildComponentsByTypeName("ComponentA", true) 
>         log(ComponentsArr)
>     }
>     ```
>     <br>
> **Component Entity:GetFirstChildComponentByTypeName(string typename, bool recursive)**
> Returns the component if a component identical to typename is found among the children entities while iterating over them.
> * **Parameter**
>     
>     | Parameter | Type | Description |
>     | --- | --- | --- |
>     | typename | string | Enter the name of the component you are searching for. |
>     | recursive | bool | Set whether to continue the search until reaching the grandchild entities.<br>If set as true, the search for the component continues until reaching the grandchild entities.<br>If set as false, the search stops at the child entity of Entity.<br>In the case of not entering any value, it will be set as false. |
> * **Example of composing script**
>     If you want to retrieve ComponentA of the first entity that has ComponentA among all the entities under map01, you can write the script as follows.
>     (It assumes that it itinerates from EntityA to EntityD)
>     ![22](https://mod-file.dn.nexoncdn.co.kr/bbs/16352918189148ef0321fb0ad4e038085c66dd874e31a.png "22")
>     ```lua
>     void GetChildComponents()
>     {
>         local map01Entity = self.Entity.CurrentMap
> 
>         --Return ComponentA of EntityA
>         local ComponentsArr = map01Entity:GetFirstChildComponentByTypeName("ComponentA")   
>     }
>     ```
>     <br>
>     If you want to search not only the immediate sub of the entity but also the components in the sub's sub, you can set the second parameter to true.</span>
>     ![23](https://mod-file.dn.nexoncdn.co.kr/bbs/163529183127423266c0257414814a3a380f36d667ad0.png "23")
>     ```lua
>     void GetChildComponents()
>     {
>         local map01Entity = self.Entity.CurrentMap
> 
>         --Return ComponentA of EntityF
>         local ComponentsArr = map01Entity:GetFirstChildComponentByTypeName("ComponentA", true) 
>     }
>     ```