# Course Introduction
While producing the world, you often have to search and receive a particular entity, or receive multiple entities that are related at the same time. This is when you can utilize [**EntityService**](/apiReference/Services/EntityService{"target":"_self"}). **EntityService** provides functions such as entity discovery, deletion, and effectiveness validation.
We'll take a look into the features offered by **EntityService**.

**Test Environment**
![01](https://mod-file.dn.nexoncdn.co.kr/bbs/16862117142285e8a5ff6889342debf36acfcd7fdccf4.png "01")
Proceed with the test using the example code after creating a component in **common**.
# Search Entity Function
**EntityService** offers eight functions to search for entities.
Each function receives world path, entity ID, model ID, etc. as parameters and searches for and returns an entity through them.
Individual entities or multiple entities can be returned based on the function of each function's task.
#### GetEntityByPath()
The function returns a single entity that matches the path within the world.
If there are more than 2 entities with the same world path, it returns the one found first.
Therefore, you are advised to use **GetEntityByPath** to search for entities that have a unique world path.
**worldPath** can be imported as **Copy Entity Path** from each entity context menu in the **Hierarchy**.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1686210532005a4a5c0075ed74dfd84667cb938b58532.png "1")
<br>
```lua
void GetEntityByPathExample()
{
    local entity = _EntityService:GetEntityByPath("/maps/map01/npc-320_1")
    local uiEntity = _EntityService:GetEntityByPath("/ui/DefaultGroup/Image_1")
    
    log(entity.Name)
    log(uiEntity.Name)
}
```
#### GetEntitiesByPath()
The function that returns all entities that match the world path that was delivered as a parameter.
This is used if there are multiple entities to find and they have the same world path. For example, using this function to get bullet entities in a shooting game would be efficient.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16862119603211b489e0a3c2e42dc91e46ea4837b80f8.png "2")
<br>
```lua
void GetEntitiesByPathExample()
{
    local entityArray = _EntityService:GetEntitiesByPath("/maps/map01/Bullet")
    
    for i, entity in pairs(entityArray) do
        log(entity.Name)
    end
}
```
#### GetEntity()
Returns all entities identical to the entity ID entered as a parameter.
This function is efficient when you're trying to get one entity among entities that have the same name, like above.
To get the Entity ID, go to **Hierarchy**, open the right-click menu, and click **Copy Entity ID**. You can also get an entity ID from the script.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16862106228295b326090f635424abcc59cf1e2980a19.png "3")
<br>
```lua
void GetEntityExample()
{
    local bulletEntity = _EntityService:GetEntityByPath("/maps/map01/Bullet")
    local bulletEntityId = bulletEntity.Id
    
    local bulletEntityById = _EntityService:GetEntity(bulletEntityId)
    
    if bulletEntity == bulletEntityById then
        log(bulletEntity.Id)
    end
}
```
#### GetEntitiesSpawnedByModelId()
Returns all entities that were created from the model with the model ID.
It's especially useful when you're getting all entities created from the same model when there are multiple entities placed.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1635310632309132b8a9defb34af18a8bd72812a72a56.png "4")
<br>
You can receive all entities created by a model that were recieved by accessing a certain entity.
```lua
void GetEntitiesSpawnedByModelIdExample()
{
    local modelId = _EntityService:GetEntityByPath("/maps/map02/npc-5396_1").Model.BaseModelId
    
    local entities = _EntityService:GetEntitiesSpawnedByModelId(modelId)
    
    for i, v in pairs(entities) do
        log(v.Name)
    end
}
```
#### GetEntitiesSpawnedByModel()
Returns all entities created from the model that were delivered from a parameter.
It's useful when you're getting all entities created from the same model when there are multiple entities placed.
```lua
void GetEntitiesSpawnedByModelExample()
{
    local model = _EntityService:GetEntityByPath("/maps/map02/npc-5396_1").Model
    
    local entities = _EntityService:GetEntitiesSpawnedByModel(model)
    
    for i, entity in pairs(entities) do
        log(entity.Name)
    end
}
```
#### GetSameModelEntities(Entity entity)
Returns all entities created from the circular model from the entity that was delivered as a parameter.
```lua
void GetSameModelEntitiesExample()
{
    local templateEntity = _EntityService:GetEntityByPath("/maps/map02/npc-5396_1")
    
    local entities = _EntityService:GetSameModelEntities(templateEntity)
    
    for i, entity in pairs(entities) do
        log(entity.Name)
    end
}
```
#### GetEntityByTag()
Returns all entities identical to the tag value delivered as a parameter.
You can set the tag with **TagComponent**. You can set multiple tags for a single entity. If any of the tags is a match, the function will return that entity.
If there are multiple entities with the same tag, it returns the entity found first. Therefore, you are advised to use **GetEntityByTag** to search for entities that have a unique tag.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/168621240536469fbf1de6ee5420083d94f7cf1b1a48c.png "5")
```lua
void GetEntityByTagExample()
{
    local entity = _EntityService:GetEntityByTag("FlayingMonster")
    
    log(entity.Name)
}
```
#### GetEntitiesByTag()
Returns all entities that match the tag value that was delivered as a parameter.
You can use it to get all entities with a specific tag value among numerous entities.
```lua
void GetEntitiesByTagExample()
{
    local monsterEntities = _EntityService:GetEntitiesByTag("Monster")
    
    for i, monsterEntity in pairs(monsterEntities) do
        log(monsterEntity.Name)
    end
}
```
# Deleting Entity and Validating Effectiveness
**EntityService** offers functions to delete Entities and to validate Entities. In other words, it sees if an Entity has been deleted and if it is currently valid.
#### Destroy()
Deletes the entity delivered as a parameter.
```lua
void DestroyExample()
{
    local entity = _EntityService:GetEntityByPath("/maps/map01/Monster1_1")
    _EntityService:Destroy(entity)
    
    if isvalid(entity) == false then
        log("Entity is destroyed")
    end
}
```
<br>
You can use Entity's `Destroy` function as well, aside from `EntityService:Destroy`.
```lua
void DestroyExample()
{
    local entity = _EntityService:GetEntityByPath("/maps/map01/Monster1_1")
    entity:Destroy()
    
    if isvalid(entity) == false then
        log("Entity is destroyed")
    end
}
```
#### IsValid()
The function checks if the entity delivered as a parameter has been deleted.
Of course, you can use nil to check if an entity exists. However, when you're checking if an entity that existed has been deleted, we recommend using the `IsValid` function rather than nil.
```lua
void IsValidExample()
{
    local entity = _EntityService:GetEntityByPath("/maps/map01/Monster1_1")
    _EntityService:Destroy(entity)
    
    if _EntityService:IsValid(entity) == false then
        log("Entity is destroyed")
    end
}
```
<br>
 `EntityService:IsValid` can be replaced with the `isvalid` function.
```lua
void IsvalidExample()
{
    local entity = _EntityService:GetEntityByPath("/maps/map01/Monster1_1")
    _EntityService:Destroy(entity)
    
    if isvalid(entity) == false then
        log("Entity is destroyed")
    end
}
```
# Entity Creation and Deletion Event
When an entity is created or deleted, a corresponding event occurs in **EntityService**.
A handler for each event can be added through **Entity Event Handler** of Script Editor.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/16564190006160cea97bbfec24f528b7b1a9cd5a920c5.png "6")
<br>
Add an event handler and then see if event sender is set to **EntityService**.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1637567490394c7e585c8bc1e4288a36d0cd99d7aa9dc.png "7")
<br>
#### EntityCreateEvent
This event occurs when an entity is created.
You can add a handler from **Entity Event Handler**.
The event is sent to Event Handler's parameter, which contains information about the created entity.
```lua
Method:
[server only]
void OnBeginPlay()
{
    local monsterTemplate = _EntityService:GetEntityByPath("/maps/map01/monster-1")

    -- Spawns monster entity named "spawnedMonster"
    _SpawnService:SpawnByEntityTemplate(monsterTemplate, "spawnedMonster", Vector3(0,0,0), true, true, true, true)
}

Event Handler:
[service: EntityService]
HandleEntityCreateEvent (EntityCreateEvent event)
{
    -- Parameters
    local Entity = event.Entity
    --------------------------------------------------------
    
    -- Prints entities only created from OnBeginPlay.
    if Entity.Name ~= "spawnedMonster" then
    	return	
    end
    
    log(Entity.Name) -- Check if 'spawnedMonster' is printed.
}
```
#### EntityDestroyEvent
This event occurs when an entity is deleted.
You can add a handler from **Entity Event Handler**.
The event is sent to Event Handler's parameter, which contains information about the entity that is waiting to be deleted.
```lua
Method:
[server only]
void OnBeginPlay()
{
    local monsterTemplate = _EntityService:GetEntityByPath("/maps/map01/monster-1")
    
    -- Spawns monster entity named "spawnedMonster".
    local spawnedEntity = _SpawnService:SpawnByEntityTemplate(monsterTemplate, "spawnedMonster", Vector3(0,0,0), true, true, true, true)
    
    local callBack = function()
    _EntityService:Destroy(spawnedEntity)
    end
    
    _TimerService:SetTimerOnce(callBack, 5) -- Deletes spawned entity after 5 seconds.
}
    
Event Handler:
[service: EntityService]
HandleEntityCreateEvent(EntityCreateEvent event)
{
    -- Parameters
    local Entity = event.Entity
    --------------------------------------------------------
    
    -- Prints entities only created from OnBeginPlay.
    if Entity.Name ~= "spawnedMonster" then
    	return	
    end
    
    log("CreatedEntity : "..Entity.Name) -- Check if 'CreatedEntity : spawnedMonster' is printed.
}

[service: EntityService]
HandleEntityDestroyEvent(EntityDestroyEvent event)
{
    -- Parameters
    local Entity = event.Entity
    --------------------------------------------------------
    
    -- Prints entities only created from OnBeginPlay.
    if Entity.Name ~= "spawnedMonster" then
    	return	
    end
    
    log("DestroyedEntity : "..Entity.Name) -- Check if 'DestroyedEntity : spawnedMonster' is printed.
}
```