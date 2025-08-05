# Course Introduction
MapleStory Worlds supports many convenient features for easy coding. Typically, there is a function to set up a simple execution control. However, simple settings can become tangled and complicated, and if set incorrectly, they may not execute properly. Sometimes the World can run slowly because of this.

These issues are hard to spot when the World is small or in the early stages of creation. However, as the size and complexity of the World increases, it can become a problem. Therefore, you need to think about the structure from the beginning of production so that all systems can proceed smoothly and efficiently. In particular, always keep in mind control flow and optimization issues. 

This course guides you through concepts that easily go unnoticed during World production.

# Using isvalid() 
In Lua, <span style="color: #dc9656">**nil check**</span> is used to check whether the target exists or not. It is mainly used in Lua tables or MapleStory Worlds entities and components. However, when accessing Native entities, you might want to use some limitations.

Code like the following runs properly in most cases, but may not in certain circumstances. To understand why, you need to understand the operating principles of MapleStory Worlds.

```lua
local targetEntity = _EntityService:GetEntity("a22396e1-6365-42d4-9cef-5924c223f7fa")
if targetEntity ~= nil then
	-- Do Something
end
```

For example, let's look at the process of deleting an entity in MapleStory Worlds. 
If there is a lot of content to process in the code written above and processing delays occur, it will not work as the creator intended. This is because the actual motion consists of more steps than expected.

* Expected action: Request to delete → nil
* Actual action: <span style="color: #dc9656">**Request to delete → Waiting to be processed → Being deleted → Deleted → nil**</span>

The reason this is not deleted right away is that the entity is deleted after **what to process** and **what to do** in the process. Therefore, you should use the `isvalid()` function to check whether an entity or component is valid.
The `isvalid()` function is safe because it checks the state of a series of actual actions. It also guarantees the effectiveness of unknown behavior. Therefore, please actively use `isvalid()` when writing scripts.

Let's modify the code we wrote above using the `isvalid()` function.

```lua
local targetEntity = _EntityService:GetEntity("a22396e1-6365-42d4-9cef-5924c223f7fa")
if isvalid(targetEntity) then
	-- Do Something
end
```


# Using Wait Series Functions
Functions with attached **Wait** such as `wait()`, `_DataStorage:SetAndWait()`, `_DataStorage:GetAndWait()` are called **Wait-type functions**. **Wait-type functions** are very convenient. This is because the execution sequence can be grasped sequentially, and it is easy to use. However, overusing it for the sake of convenience can cause problems.  
                                                                                      
####  Using Wait
Using `wait()` is as simple as shown in the example code below.

```lua
_UIToast:ShowMessage("Show Message")
wait(1)
_UIToast:ShowMessage("Show Delay Message")
```

However, be careful when using `wait()`. `wait()` literally "waits" for an action, so if you use it all over the place, you may lose track of the overall flow. Also, since the function waits, accompanying processes may not run properly. 

Let's look at the problem that occurs due to waiting through the example below.

1. This does not guarantee the effectiveness of actions that follow the wait.
    
    ```lua
    local targetEntity = _EntityService:GetEntity("a22396e1-6365-42d4-9cef-5924c223f7fa")
    targetEntity.TestComponent:ShowMessage()
    wait(3)
    targetEntity.TestComponent:ShowMessage()
    ```
    
    After `wait(3)`, the effectiveness of **targetEntity** and **targetEntity**'s **TestComponent** cannot be guaranteed. This is because entities or components may have been deleted in the meantime.
    <br>
2. Several waiting actions will require a lot of time.
    The code uses **wait-type functions** to get a character's ability value from DataStorage. Let's look at an example.
    
    ```lua
    local str = _DataStorageService:GetAndWait("MyStat", "STR")
    local dex = _DataStorageService:GetAndWait("MyStat", "DEX")
    local int = _DataStorageService:GetAndWait("MyStat", "INT")
    local luck = _DataStorageService:GetAndWait("MyStat", "LUCK")
    local hp = _DataStorageService:GetAndWait("MyStat", "HP")
    local mp = _DataStorageService:GetAndWait("MyStat", "MP")
    local sp = _DataStorageService:GetAndWait("MyStat", "SP")
    ```
    
    Getting **STR** for the first time might not have caused any problems. However, the problem occurs when the amount of information increases. In particular, retrieving values from the DB or loading resources takes a very long time. If it takes 0.5 seconds to receive each value, it will take 3.5 seconds to receive all 7 stats.
    
    This is because it will repeat the process <span style="color: #dc9656">**Request → Wait → Receive → Request → Wait → Receive ....**</span>.
    
    For the 3.5 seconds of waiting, if something important happens underneath, or if another component tries to access that part, things will get complicated. To solve this, you can use `_DataStorageService:GetAsync` first. You can follow the procedure sequentially as it requires no waiting after sending the request. Of course, you have to process the response that came in after the request. For detailed information, please refer to [DataStorageService](/apiReference?postId=309{"target":"_self"}).
    <br>
3. If used in `OnBeginPlay()`, `OnUpdate()` cannot be called. This is because `OnUpdate()` is not called until `OnBeginPlay()` ends.

Several of the actions mentioned above must be performed in parallel. For parallel performance, you can use `TimerService`.

#### Difference between Wait and TimerService
`wait` is to perform some action directly after waiting, and `TimerService` is to create a separate job to perform that action. `wait` operates in a serial format, and `TimerService` operates in parallel format.
That means the usage code of `wait` can be expressed as follows.

```lua
local targetEntity  = _EntityService:GetEntity("a22396e1-6365-42d4-9cef-5924c223f7fa")
targetEntity.TestComponent:ShowMessage()

function delayMessage()
	local targetEntity2 = _EntityService:GetEntity("a22396e1-6365-42d4-9cef-5924c223f7fa")
	targetEntity2.TestComponent:ShowMessage()
end
	
_TimerService:SetTimerOnce(delayMessage, 3)
```
 
After understanding the difference between `wait` and `TimerService`, you should be able to use them where appropriate while understanding what flow each action will follow.
If you understand the difference between `wait` and `TimerService`, you should be able to guess the execution sequence of the code below. 

```lua
log("Do A")
wait(1)
log("Do B")

log("Do C")
function DoD()
	log("Do D")
end
_TimerService:SetTimerOnce(DoD, 1)
log("Do E")
```

The execution sequence is <span style="color: #dc9656">**A → B → C → E → D**</span>. 

# Notice when Calling from OnUpdate
Since Update is called every frame, you need to pay close attention to <span style="color: #dc9656">**optimization**</span>. Inappropriate use of the function will cause a noticeable decrease in performance. You should always be careful when using Update statements, as most of the optimization problems that many creators of Worlds face occur due to the Update statement.
#### Synchronization Property (Used in Server Update)
Let's say we put a property named `period` to display the time elapsed after a certain event has begun.
If you put a logic like `self.period = self.period + delta` in the update statement, it will add time each frame, so the value will actually represent the time elapsed after the event has begun. However, if `period` is a synchronization property, a big problem occurs.

A synchronization property literally attempts to <span style="color: #dc9656">**"synchronize"**</span>. Synchronization will use a vast amount of network cost. Synchronization needs to happen each frame, and that information is sent out according to the number of connected clients. If the logic is a component in an entity, each entity has to send the information to each client, consuming the cost of <span style="color: #dc9656">**N * N**</span>. It might not be much when you test alone, but the increase in the number of players will drastically increase the cost. This causes new problems to occur when testing multiplayer. Therefore, you should always be careful when using synchronization properties in an Update statement. 

If you really need to use these functions, let's write something like this: 
Synchronization happens <span style="color: #dc9656">**only once**</span> when synchronizing `eventActiveTime`, which will save you a lot of cost.
<br>
**At start of Event (Server)**
``` self.eventActiveTime = _UtilLogic.ServerElapsedSeconds```
<br>
**To get time elapsed (Client)**
```local period = _UtilLogic.ServerElapsedSeconds - self.eventActiveTime```

#### Execution Space Control Function (Used in Server, Client Update)
This is the same principle as synchronization properties. If you call another execution space (Client → Server or Server → Client) within Update, it will result in great network cost.
When used in the Client, network cost is used by the Server every frame, and when used in the Server, it becomes a bigger problem because it is sent to all clients.
#### Wait 
Now, let's look at the case of using it in `OnUpdate()`. How will the code below operate in the `OnUpdate()` statement?

```lua
log("TEST") -- No.1
wait(1)
log("TEST2") -- No.2
```

* Expected action: TEST - 1 second wait - TEST2 - TEST1 - 1 second wait - TEST2
* Actual action: <span style="color: #dc9656">**TEST - TEST - TEST - .... (1 second after first TEST was outputted) - TEST2**</span>

In logical terms, when `OnUpdate()` ends, it stops that part, and a new `OnUpdate()` is created in the next frame.

However, if you use wait, `OnUpdate()` will perform its role until it encounters wait, which will cause it to stop operating and wait for the entered time period. A new `OnUpdate()` is created on the next frame after the wait, then it executes from the beginning, and then waits again to reach the time entered in `wait()`. Not only is it something creators don't want, it also causes optimization issues.

><span style="color: #585858">**Learn more**
> Using `wait()` in `OnUpdate()` is bad design. We recommend modifying the code.</span>

In particular, if you use `wait()` together with **Update**, the combination will cause significant inefficiency.
As explained above, when execution of `OnUpdate()` stops, the **Update** entity has to stop as well. However, as **Wait** blocks the Update statement from stopping, Update entities will keep piling up. Piled Update entities cause slower World performance as they consume resources in speed and memory. On top of that, you will find it harder to debug and catch the flow of properties in each Update entity. 

Therefore, please refrain from using `wait()` in **Update** statements, except in very exceptional cases.

# Sending Responses to Specific Clients Only
If a function's execution control option is set to **Client**, that function will be executed on all clients.
However, on the server you may come across a situation where you want to run a function only on certain clients, not all clients.
This is a situation where a specific client sends a (Request) to the server and the server sends a (Response) to the client after processing the logic.

Let's look at an example.
1. If you click the purchase item button on client A (Request)
2. In the server, after deducting the money of A and increasing the number of items
3. A response will be sent to the client indicating that it has been processed (Response)

At this time, the server only needs to respond to A and does not need to respond to other clients. 
Let's implement this process of responding only to the client that sent the request in the first place in code.

```lua
[Server]
void RequestBuyItem(integer shopID, integer itemID)
{
    local itemCost = _ShopLogic:GetItemCost(shopID, itemID)
    if itemCost < self.money then
        self.money = self.money - itemCost
        self:BuyItem(itemID)
        self:ResponseBuyItem(itemID, self.Entity.PlayerComponent.UserId)
    end 
}

[Client]
void ResponseBuyItem(integer itemID, string userId)
{
    -- After checking whether it is the user who sent the request for the first time, this works only when the user sends the request from the client.
    if userId == _UserService.LocalPlayer.PlayerComponent.UserId then
        log("Success")
    end
}
```

Although writing code like this works, it is very inefficient.
Since the server sends the message to all clients, not just A, which sent the request in the first place, network costs go up and the whole World slows down.

To solve this problem, MapleStory Worlds provides "a feature to send functions whose execution space is set to Client only to a specific client". Using this feature is simple. Just write the <span style="color: #dc9656">**UserId**</span> you want to send in the last parameter of the function with execution space **Client**. However, this **UserId** can be omitted because it is automatically appended.
This way, you can send the function only to that user's client. It also simplifies the code and helps with World optimization.

The above explanation can be more easily understood by looking at the code below.
```lua
[Server]
void RequestBuyItem(integer shopID,integer itemID)
{
    local itemCost = _ShopLogic:GetItemCost(shopID, itemID)
    if itemCost < self.money then
        self.money = self.money - itemCost
        self:BuyItem(itemID)
        self:ResponseBuyItem(itemID, self.Entity.PlayerComponent.UserId)
    end 
}

[Client]
-- In fact, it is `ResponseBuyItem(integer itemID, string targetUserId = nil)`, but UserId is omitted because it is automatically attached.
void ResponseBuyItem(integer itemID)
{
    log("Success")
}
```

-- In the code above, `ResponseBuyItem(integer itemID)` is actually `ResponseBuyItem(integer itemID, string targetUserId = nil)`, but the last parameter `targetUserId` is omitted because it is automatically attached when writing the code. Therefore, if you write the code above, the function will be delivered only to the user. In addition, it can be used on the client without any user confirmation, saving network costs.