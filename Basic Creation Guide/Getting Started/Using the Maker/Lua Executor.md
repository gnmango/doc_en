# Course Introduction
When testing Worlds made in Maker, you may want to check if everything functions as you intended in real time by changing component or property values. Or you may want to check if each created script functions without errors in the server or client environment. You can easily confirm the above using Lua Executor. Let's learn about Lua Executor with some practical examples.

# Lua Excutor
You can use Lua Executor to execute scripts in real time while playing in Maker and apply the changes immediately. Creators can execute various scripts as needed which can be confirmed in the World. Written scripts can be executed in the server, client, and instance room each, and the executed value can be viewed from the Console window.

#### Screen Guide
 ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17314026977102d66cc962e73441caee4eb403cf02d5b.png "1")
Number | Name | Desc
---------|----------|---------
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1")| Set Execution Space | Set the execution space. You can set the server, client, and instance room.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2")| Execute | A button used to execute the written script.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg "3")| Script Writing Space | A space to write scripts to execute. Only activates during test play in Maker.


# Use Example
The following are examples using Lua Executor. You can write your own script needed for a creator's World by referencing the example.
#### Confirm Tween Value
TweenLogic is often used in UI effects, so visualizing the final product will be difficult with just written script. You can use the Lua Executor to change the Tween value to confirm the result and search for the desired effect value.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1730979504422b4960734f4c449f9b2ddcbbd63f1cbf2.gif "UITweeen")

1. Add a new **TestGroup** to **Hierarchy - World - ui**.
2. To create a new UISprite, click on the context menu and select **Create Entity - Create UISprite**.
![UISprtie](https://mod-file.dn.nexoncdn.co.kr/bbs/1732151013045fb775efde16f4c0480caab166a5a357f.png{"width":"340px"} "UISprtie")
3. Write the following in Lua Executor to view the Tween results from the created UISprite.

    ```lua
    local entity = _EntityService:GetEntityByPath("/ui/TestGroup/UISprite")
    local startY = 90
    local goalY = 0
    _TweenLogic:PlayTween(startY, goalY, 2, EaseType.ElasticEaseOut, function(y) entity.UITransformComponent.Position.y = y end)
    ```

5. Click [Execute] after changing the Lua Executor execution space to **Client**.
6. Search for the UI effect of your choice by changing startY and goalY values and by changing EaseType.

#### Camera Movement
When making a large map, you will need to confirm whether the whole of the world has been made to the creator's specifications. You can see the entire map by adding a CameraComponent during a test play using Lua Executor.

![camera2](https://mod-file.dn.nexoncdn.co.kr/bbs/173098108808006c95dfa943e47a0ae3e8896635c4631.gif{"width":"840px"} "camera2")

1. Create a new **transformonly** model by clicking **Workspace - MyDesk - Create Model**.
2. Add **TransformComponent**.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17314992738388e253d7a07024b2eb33e070a74e78933.png "transformonly")
3. Enter the following into Lua Executor.

    ```lua
    local mapEntity = _EntityService:GetEntityByPath("/maps/map01")

    local debugCamera = _SpawnService:SpawnByModelId("model://transformonly", "DebugCamera", Vector3(0,0,0), mapEntity)

    local camera = debugCamera:AddComponent("CameraComponent")
    _CameraService:SwitchCameraTo(camera)
    ```
    
4. Change  the execution space for Lua Executor to **Client** after starting test play.
4. Select the **DebugCamera** that was added to **Hierarchy - World - Maps - Map01**.
![DebugCamera](https://mod-file.dn.nexoncdn.co.kr/bbs/1731499807402c81daf4e1a71486b827c3e25a48c88d0.png{"width":"340px"} "DebugCamera")
6. You can change the **CameraOffset** value under CameraComponent from the Property window to look at the whole map.
![CameraOffset](https://mod-file.dn.nexoncdn.co.kr/bbs/17314999391660155bbac855f4eda83d60e5568b67e54.png{"width":"340px"} "CameraOffset")
#### Confirm Server Value
You can find the cause of a bug by confirming it in a recreated server value. For example if a monster's HP is unchanged even after a player attacks it, you can confirm if the server value changes based on the monster's status.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17314825147714ec3aa415d38416d8b22e58fa4d56499.gif "spawnMonster")

1. Use 'EntityService' to load the path of the entity that needs confirmation. Write it out like the following.

    ```lua
    local monster = _EntityService:GetEntityByPath("/maps/map01/monster-2419_2")
    log(monster.Monster.Hp)
    ```

2. Click [Execute] and move close to the monster to confirm the value from the Console window.

#### Summon Monster
You can summon specific monsters to the server for testing.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17314820984397187de91e0444333991781e9ca6925e2.gif "spawnmonster")

1. Open the context menu after selecting the preset from **PresetList - Monster** and select **Add to Workspace**.
![AddToWorkspace](https://mod-file.dn.nexoncdn.co.kr/bbs/1731501190648856620b0912a40d481ef115741a2309c.png{"width":"340px"} "AddToWorkspace")
2. Copy the **Entry ID** of the monster model added to Workspace and write the following to summon that monster.

    ```lua
    local mapEntity = _EntityService:GetEntityByPath("/maps/map01")
    for i=1, 10 do
        _SpawnService:SpawnByModelId("model://000000", "TestMonster", Vector3(-7,0,0), mapEntity)
        wait(0.1)
    end
    ```
3. Change the execution space for Lua Executor to **Server** and click [Execute].
4. Confirm if 10 of the monster is summoned.

####  Creating and Testing Instance Rooms 
Create an instance room from Lua Executor and debug the position of certain entities in the instance room.
1. Create map02 and activate the **InstanceMap** under MapComponent in the property window.
2. Add a new TestTarget entity to the map that will be debugged.
3. Start play testing.
4. Use 'CreateInstanceRoom()' to create a temporary instance room. Write the following and click [Execute].   
    
    ```lua
    _RoomService:CreateInstanceRoom("Test", {"map02"}, 200)
    ```

5. Select **Server_Instance_Test** from execution environment.
![Instance Test](https://mod-file.dn.nexoncdn.co.kr/bbs/1731501337385b5fafa8f5d674b41afe2280eeef9215e.png{"width":"340px"} "Instance Test")
6. You can learn the position of the entity to debug by using 'EntityService'. Please write the following and click [Execute].

    ```lua
    local debug = _EntityService:GetEntityByPath("/maps/map02/TestTarget")
    log(debug.TransformComponent.Position)
    ```

7. You can view logs from the **Console window**.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17314885251115e78f866636b457f86abbc6b4ddf7460.png "4")