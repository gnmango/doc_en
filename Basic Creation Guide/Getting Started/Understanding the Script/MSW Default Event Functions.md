# Course Introduction
If you use scripts in MapleStory Worlds, you can freely generate creative Worlds unique to creators. When writing scripts, you will utilize the default event functions as needed. A **default event function** refers to **a function that is automatically called upon a certain condition**, even though the creator doesn't directly call it.
In this course, we will look at the types of default event functions and when to call them.

# Adding Default Event Function
Let's add the default event function.

1. In the **Workspace - MyDesk**, click **Create Scripts - Create Component** in the context menu to create a new script component and open the script editor.
<br>
2. Tap the **[+]** button next to **Function**. All except for **New** (user defined function) are default event functions. Add functions as needed.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16370612894139b23c38d3a884725832fe814dbe7be63.png "2")
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/163706129949446ade8e045fc4743a36f98f922e244da.png "3")

# Default Event Function
Event functions provided by MapleStory Worlds are basically called by the server.
The calling space can be set up differently depending on the creator's intent.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1665647190294905d077d3058476d8a437d915bde5933.png "4")

Now let's take a closer look at the function.

### OnInitialize
This is a function called once before the 'OnBeginPlay' function is called and after the entity and component are created.
However, the 'OnInitialize' function can be called before the entity's component is created. Therefore, when referring to other components or entities in the 'OnInitialize' function, there is a possibility of receiving **nil**.
```lua
void OnInitialize()
{
    local myEntity = self.Entity      
    --Can refer to entity applying itself.
    --Outputs myEntity's name and "Hello MapleStory Worlds!" on the console window
    log(myEntity.Name.."Hello MapleStory Worlds")

    --It is not recommended to refer to other entities or components within OnInitialize, as shown below.
    local otherComponent = myEntity.component name      --May be nil
    local anotherEntity = _EntityService:GetEntityByPath("entity path") --Refers to other entities. May be nil
}
```

### OnBeginPlay
This function is called once when the logic starts in earnest (after the `OnInitialize` function call, right before the `OnUpdate` call).
An entity's `OnBeginPlay` function is called after all other entities and components have been created. Even for dynamically created entities, the `OnBeginPlay` function is called after all the components of the entity are created. Therefore, unlike `OnInitialize`, the `OnBeginPlay` function guarantees that it refers to other entities and components.
```lua
void OnBeginPlay()
{
    local myEntity = self.Entity    -- Can refer to entity applying itself.
    log(myEntity.Name.."Hello MapleStory Worlds")    --Outputs myEntity's name and "Hello MapleStory Worlds!" on the console window
     
    --Unlike OnInitialize, it is guaranteed that it refers to other entities and components as follows.
    local otherComponent = myEntity.component name
    local otherEntity = _EntityService:GetEntityByPath("entity path")
    local otherEntityComponent = otherEntity :GetComponent("component name")
}
```
<br>
However, the order of calling the 'OnBeginPlay' function between each component is not guaranteed. Therefore, if a specific component receives the value set by the `OnBeginPlay` function, it may not work properly.
Let's looks at an example.
* In **Component A**, define **Property B** as <span style="color: #dc9656">**" "**</span>. In the `OnBeginPlay` function, set **Property B** as <span style="color: #dc9656">**"Hello"**</span>.
* In **Component C**'s OnBeginPlay function, refer to **Property B**.
* At this time, you may receive not <span style="color: #dc9656">**"Hello"**</span>, but <span style="color: #dc9656">**nil**</span>.
```lua
-- ComponentA
Property: 
[Sync]
string B = ""

Method: 
[server only]
void OnBeginPlay()
{
    self.B = "Hello"
    log(self.B)
}

-- ComponentC    
Property: 

Method: 
[Server Only]
void OnBeginPlay()
{
    local myEntity = self.Entity
    local componentA = myEntity.ComponentA
    log(componentA.B)   
    --Since we don't know if ComponentA or ComponentC's OnBeginPlay will be called first, nil may be printed in the console window.
}
```
<br>
Hence, if you have to refer to the property of **Component A** in the `OnBeginPlay` of another component, utilize the `OnInitialize` function as follows.
```lua
-- ComponentA
Property: 
[Sync]
string B = ""

Method: 
[server only]
void OnInitialize()
{
    self.B = "Hello"
    log(self.B)
}

-- ComponentC
Property: 

Method: 
[Server Only]
void OnBeginPlay()
{
    local myEntity = self.Entity
    local componentA = myEntity.ComponentA
    log(componentA.B)   
    -- Since the value was allocated via OnInitialize in ComponentA, "Hello" is printed to the console window.
}
```

### OnUpdate
This is called each frame after calling the `OnBeginPlay` function.
So if you want to change the entity's location, state, or action by frame, compose the script into `OnUpdate`.
Use **delta** as a parameter. It receives the time value (unit: seconds) spent on the previous frame.
Using **delta** allows you to control the implementation of each frame per unit as well as action for a certain time.
```lua
void OnUpdate(number delta)
{
    if self._T.Time == nil then self._T.Time = 0 end
    self._T.Time = self._T.Time + delta
     
    --Outputs Hello MapleStory Worlds on the console window every 3 seconds.
    if self._T.Time >= 3 then
        self._T.Time = 0
        log("Hello MapleStory Worlds")
    end
}
```

### OnEndPlay
The function is called once at the moment of removing an entity with the `OnDestroy` function.
However, the entity is in a valid state which hasn't been removed, even after completing the `OnEndPlay` function.
After the `OnEndPlay` function is called, the `OnDestroy` function is called.
```lua
vold OnEndPlay()
{
    log("OnEndPlay!!")
    -- Console Result
    -- OnEndPlay!!
}
```

### OnDestroy
The function is called once at the moment of removing an entity. It is called after the `OnEndPlay` function. 
Unlike the `OnEndPlay` function, the entity is destroyed after completing the `OnDestroy` function.
```lua
void OnDestroy()
{
    log("OnDestroy!!")
    -- Console Result
    -- OnDestroy!!
}
```
<br>
> <span style="color: #585858">**Learn More**
> The execution sequence from the `OnInitialize` function to the `OnDestroy` function described above is as follows.
> ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16352483627843dbf0389a0b64bf1beb7388ea89672e4.png{"width":"350px"} "5")</span>

### OnMapEnter
While `OnBeginPlay` is a function called whenever an entity is created, `OnMapEnter` is a function called whenever an entity enters a map or when it is created on a map. This makes it great for entities that need to be initialized every time the map is moved.

When calling the `OnMapEnter` function, the entity of the entered map is passed as a parameter.
Just like when a player enters the World, it is initialized using the `OnInitialize` function and the `OnBeginPlay` function. When a player enters a specific map, you can use the `OnMapEnter` function to do the necessary processing.

The following is an example of implementing the `OnMapEnter` function to increase the size of the player every time it enters map01.
```lua
void OnMapEnter(Entity enteredMap)
{
    --Player size increases whenever it enters map01.
    if enteredMap.Name == "map01" then
        local myPlayer = self.Entity
        local transform = myPlayer.TransformComponent
        local scale = transform.Scale
        scale.x = scale.x + 1
        scale.y = scale.y + 1
        transform.Scale = scale
    end
}
```


#### Difference between OnBeginPlay and OnMapEnter

For example, suppose my player entity **moves from map01 to map02**. At this time, if there was an `OnMapEnter` function in the script component of my player entity, the `OnMapEnter` function of my player entity is called when moving to map02. By the way, you can see that the `OnMapEnter` function of other players as well as my player is being called. From the client's perspective, it's like another player entity has been created on the map I'm currently on.

But on the server, even if my player entity moves from map01 to map02, it does not create another player entity that was originally on map02. The other player entity was already there from the moment my player entered the World. Creation in the server occurs when entering the World for the first time. So, the `OnBeginPlay` function in the server is called only once **when the player enters the World and entities are created**.

In contrast, the `OnMapEnter` function **is called every time an entity moves maps**. As this functions in the same way in the client as well as in the server, the `OnMapEnter` function is called every time a player entity wanders around the map. It is necessary to understand these differences and use the `OnBeginPlay` function and `OnMapEnter` function according to their intended use.

### OnMapLeave
In contrast to `OnMapEnter`, the function is called every time an entity exits the map or is removed from the map.
It is good to use it for entities that need initialization every time the map is moved, like the `OnMapEnter` function.

When calling the `OnMapLeave` function, the entity of the exited map is passed as a parameter.
The following is an example of implementing the `OnMapLeave` function to decrease the size of the player every time it exits map01.
```lua
void OnMapLeave(Entity leftMap)
{
    --Player size decreases whenever it exits map01.
    if leftMap.Name == "map01" then
        local myPlayer = self.Entity
        local transform = myPlayer.TransformComponent
        local scale = transform.Scale
        scale.x = scale.x - 1
        scale.y = scale.y - 1
        transform.Scale = scale
    end
}
```
<br>
### OnSyncProperty
This is a function called from client when the value changed in the server is synchronized to the client.
When calling the `OnSyncProperty` function, the synchronized property's name and value are passed as parameters.
If you set the property's synchronization setting to **None**, which will stop the sync, the `OnSyncProperty` function will not be called.
```lua
Property: 
[Sync]
number HP = 100
[Sync]
number MP = 100
 
Method: 
[Client Only]
void OnSyncProperty(string name, any value) --name : property name, value : changed property value
{
    if name == "HP" then
        if self.HP == value then 
            log("SInce it is called when the synchronization is complete, this log is printed to the console window.")
            return 
        end
        
        if value > 0 then 
            return 
        end
        
        -- Adds a process for when HP value is 0 or below.
        
    elseif name == "MP" then
        if self.MP == value then
            log("Since it is called when the synchronization is complete, this log is printed to the console window.")
        end
        
        if value > 0 then 
            return 
        end
        
        -- Adds process for when MP value is 0 or below.   
        
    end
}
```