# Course Introduction
Let's learn about the default event function's life cycle and overall life cycle that takes place during play. 
Understanding and taking these life cycles into account will help you create more functional worlds.

# The Default Event Function's Life Cycle
You must make component scripts to produce the logic required for the world. You need to predict the time that will be called when you make the required component scripts, and then budget the time to make it operate, matching the logic you intended.
Once you've studied the concept of life cycles and understand the overall flow of scripts, you'll be able to create scripts more easily. 
There is a basic difference in the life cycle depending on the execution environment. Execution environments are broadly divided into **world in progress/released world** and **server/client**.

Function types can be divided into two varieties. 

* <span style="color: #dc9656">**Default event functions**</span>: MapleStory Worlds produces these functions, and a certain call time has been determined for each function. Creators cannot modify the call times of these functions, and thus need to make and call the user custom functions by setting the time of these functions as default.
* <span style="color: #dc9656">**User-defined functions**</span>: This is a function directly defined by users, and they can call the call time from the default event function and entity event function and then use it.
<br>
In essence, functions need to be called from somewhere. A default event function refers to a function that is automatically called upon a certain condition, even though the creator doesn't call it. These functions are called in server spaces by default. Press the ![plus](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_plus.png "plus") button next to Function on the Script Editor window, so you can add a default event function. The default event types are `OnInitialize`, `OnUpdate`, `OnBeginPlay`, `OnMapEnter`, `OnMapLeave`, `OnEndPlay`, `OnDestroy`, and `OnSyncProperty`. For the content of each function, please refer to [MSW Default Event Functions]. 

Logs let you see the flow of the functions, except for `OnUpdate` and `OnSyncProperty`. The flow of the default event function is as follows:
1. Create a new <span style="color: #dc9656">**LifeCycle**</span> component and add the `OnInitialize`, `OnBeginPlay`, `OnMapEnter`, `OnMapLeave`, `OnEndPlay`, and `OnDestroy` functions.
2. You write `log("execute function name")` to enable knowing what function was executed.
3. Add the **LifeCycle** component to **DefaultPlayer**.
4. Add a map and connect it using the portal to check `OnMapEnter` `OnMapLeave`.
5. Execute the world, move the map with the portal, and then terminate the execution.
6. The order of the remaining logs is as shown below.<br> 
![01](https://mod-file.dn.nexoncdn.co.kr/bbs/1683859157721a85006486490435ba29d7b5deea1e2f7.gif "01")

First, `OnInitialize`, `OnMapEnter`, `OnBeginPlay` logs are recorded. When you start the world, players are placed on the starting map and the two other functions upon entering the world. Once you move maps using the portal, you will leave the starting map and enter the new map so the `OnMapLeave` and `OnMapEnter` logs are noted in order. 

> <span style="color: #585858">**Learn more**
> **User-defined functions** cannot be called automatically from the system, so it needs to be called from the default event function to execute.
> If a creator made a MyNewFunction function to NewComponent, this function must be called from somewhere.
> This should be called at same time as when the creator wants the function to be executed.</span>

><span style="color: #585858">**Learn more**
> Entity Event can control the Event easily and call the user custom function. 
> Unlike a default event function, this is called only at the timing of the defined certain event and not included in the life cycle.</span>
# Overall Play Life Cycle
Let's learn about the overall flow wherein the server and client are called. The **world in progress** and the **released world** life cycles function slightly differently from one another.
The biggest difference is how each loads maps.
* When you play in a **world in progress**, it just loads the map with players, even though 10 maps are made. 
* All maps are loaded in a **released world**, regardless of whether or not a player is present. 

If you know the difference, you can figure out that when something doesn't work the way you intended in the **world in production** or **released world**, that the issue comes from an incorrect code or a call that didn't allow for the specific cycle.

#### Understanding the Cocept of Worlds
World is the highest level of entity, and it is created one by one depending on the execution environment.
Several worlds can exist simultaneously, but those worlds cannot interact with or affect each other.
When you play test in a **world in progress**, the world being edited and the world being play-tested will co-exist.

#### Playing Worlds in Progress
Only the client world being edited exists in the world in progress. `OnBeginPlay`, `OnUpdate`, and `OnEndPlay` are not called for the entity being editing, but `OnUpdate` is called for the component requiring rendering as an exception.

If you execute (or play) the world for a test, the world being edited will be inactive. After that, execute and connect the local server and create a world for test play in both server and client. At this time, the difference with the **released world** is that the server loads only the map being edited. However, the server will load that map if it acquires another map's entity using EntityService in the world in progress or if a player accesses other maps, such as moving to other maps through a portal.

A client loads only the map with the player's entity, and it is the same as the released world.
![maker_life_cycle](https://mod-file.dn.nexoncdn.co.kr/bbs/168386511019034f5d9ec657a46498ada3e73e4b80aa6.png "maker_life_cycle")
#### Playing a Released World
The released game's server loads the whole map when it starts, and the client loads only the map where the player entity always exists.

![publishing_life_cycle](https://mod-file.dn.nexoncdn.co.kr/bbs/1683865126694afeaa429658545449a876643960ffb68.png "publishing_life_cycle")

##### Reference Guide
* [Functions]
* [MSW Default Event Functions]
* [Effective MSW 1]


