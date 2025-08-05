# Course Introduction
Let's learn how to share data and send events between world instances using WorldInstanceService and WorldInstanceSharedMemory.
To learn the basics of world instances and rooms, please refer to [World Instances](/docs?postId=984{"target":"_self"}), [Creating Instance Maps](/docs/?postId=540{"target":"_self"}).

><span style="color: #7cafc2">**Tip**
> If you want to implement communication between rooms, try using the RoomService function.</span>

# WorldInstanceSharedMemory
Since world instances cannot directly communicate, you must use [WorldInstanceSharedMemory](/apiReference/Misc/WorldInstanceSharedMemory{"target":"_self"}) to share data between instances. 
WorldInstanceSharedMemory is a group of shared variables. Once specified, these variables can be read and written to in real time. The characteristics of WorldInstanceSharedMemory are <span style="color: #dc9656">**volatile**</span>. Meaning,  WorldInstanceSharedMemory will disappear if there are no active world instances.

><span style="color: #7cafc2">**Tip**
> If communication between rooms is required, use RoomService and RoomSharedMemory.</span>

#### Shared Variables
<span style="color: #dc9656">**Shared variables**</span> are values shared between world instances. If you change any data in WorldInstanceSharedMemory, it means that you have changed the value of a shared variable. 
Below is an example how one might implement shared variables using WorldInstanceService and WorldInstanceSharedMemory.

```lua
-- Acquires WorldInstanceSharedMemory.
local getMemResultCode, mem = _WorldInstanceService:GetSharedMemory("MemoryName")
 
-- Create the shared variable var1.
local createVarResult = mem:CreateVariableAndWait("var1", "value1")
log (createVarResult.Info.Name)        -- var1
log (createVarResult.Info.Value)       -- value1
 
-- Look up the shared variable var1.
local getVarResult = mem:GetVariableAndWait("var1")
log (getVarResult.Info.Name)           -- var1
log (getVarResult.Info.Value)          -- value1
 
-- Modify the value of the shared variable var1 to value2.
local keyInfo = SharedVariableKeyInfo("var1", nil)
local updateVarResult = mem:UpdateVariableAndWait(keyInfo, "value2")
log (updateVarResult.Info.Value)       -- value2
 
-- Delete the shared variable var1.
local deleteVarResultCode = mem:DeleteVariableAndWait("var1")
 
-- Delete WorldInstanceSharedMemory. Any remaining shared variables are also deleted.
local deleteMemResultCode = _WorldInstanceService:DeleteSharedMemoryAndWait("MemoryName")
```
#### Shared Variable References
If a world instance creates, views, or edits shared variables, those shared variables are referenced. A world instance that has referenced certain shared variables will maintain the reference status until the shared variable is deleted. If a certain shared variable referenced by multiple world instances is deleted from a certain world instance, the shared variable won't be able to be used by other world instances.
To prevent impacting all world instances by deleting data from a single place like this, MapleStory Worlds recommends disabling referencing instead of deleting WorldInstanceSharedMemory shared variables. If referencing the same shared variable is disabled for all world instances, then the shared variable is automatically deleted.

```lua
-- Remove the reference to the shared variable var1.
local releaseVarResult = mem:ReleaseVariableAndWait("var1")
 
-- Remove the reference to all shared variables in WorldInstanceSharedMemory.
local releaseMemResultCode = _WorldInstanceService:ReleaseSharedMemoryAndWait("MemoryName")
```

#### Batch Requesting WorldInstanceSharedMemory
Functions **prefixed with "Batch"** allow batch requests to create, inquire, or modify shared variables. This can be more efficient when you need to process multiple variables.

```lua
local code, mem = _WorldInstanceService:GetSharedMemory("MemoryName")
 
local nameValues = { ["b_var1"] = "b_value1", ["b_var2"] = "b_value2", ["b_var3"] = "b_value3" }
local results = mem:BatchCreateVariableAndWait(nameValues)
 
log (results["b_var1"].Info.Value)        -- b_value1
log (results["b_var2"].Info.Value)        -- b_value2
log (results["b_var3"].Info.Value)        -- b_value3
```

#### ETag
<span style="color: #dc9656">**ETag**</span> is an identifier which is required to ensure that the data in the shared variable is up to date. Issue a new ETag whenever data is created or modified, or records are looked up. The issued ETag is used to record and compare data versions.
It's important to use ETags because, when a variable is shared across multiple world instances, it becomes difficult to know which data is newer and which has been modified. Also, since multiple world instances access shared variables, sometimes data can be overwritten unexpectedly. 

A shared variable and an ETag are pairs that are used together. If you modify the value of a shared variable, you must send the ETag along with the shared variable name and value in the request. At this time, if the ETag included in the request and the latest ETag are different, the value of the shared variable is not modified. However, depending on the creator's intention, the ETag verification process can be omitted. In this case, enter nil or an empty string to bypass the verification process unconditionally and modify the value of the shared variable.

The code below is an example using an ETag.

```lua
local code, mem = _WorldInstanceService:GetSharedMemory("MemoryName")
  
-- current.Info.ETag contains the latest ETag.
local current = mem:GetVariableAndWait("varname")
  
-- The shared variable is modified only when the latest ETag is sent in keyInfo. If you put a different value, it will not be modified. However, if an empty string or nil is contained, the shared variable is modified unconditionally.
local keyInfo = SharedVariableKeyInfo(current.Info.Name, current.Info.ETag)
 
-- When a shared variable is successfully modified, updated.Info.ETag contains the newly issued latest ETag.
local updated = mem:UpdateVariableAndWait(keyInfo, "new value")
 
-- When a shared variable modification fails due to an ETag mismatch, a SharedMemoryResultCode.PreconditionFailed error code is returned.
if updated.Code == SharedMemoryResultCode.PreconditionFailed then
    error ("ETag Mismatch.")
end
```

# WorldInstanceService
You can use the function WorldInstanceService to share data between world instances or send events. 
You can get WorldInstanceSharedMemory with WorldInstanceService. You can create or modify a shared variable with the obtained WorldInstanceSharedMemory.
![WorldInstanceService01](https://mod-file.dn.nexoncdn.co.kr/bbs/1679450541490552c575873394cd49aedb25f7cf2f979.png{"width":"1500px"} "WorldInstanceService01")

# Sending and Receiving Events between World Instances
You can send events to all other world instances using the RequestSendEventToAllWorldInstancesAndWait() and RequestSendEventToAllWorldInstancesAsync() functions. If you want to send an event only to a specific world instance, use RequestSendEventToWorldInstanceAndWait() or RequestSendEventToWorldInstanceAsync().

#### Send Event
To send events to other world instances, you must use the target world's <span style="color: #dc9656">**world instance Id**</span>. The world instance Id can be found as WorldInstanceService.WorldInstanceId.

```lua
-- Send the MyWorldInstanceEvent event to all world instances.
local evt = MyWorldInstanceEvent()
local code = _WorldInstanceService:RequestSendEventToAllWorldInstancesAndWait(evt)
 
if code ~= SendEventRequestResultCode.OK then
     error ("_WorldInstanceService:RequestSendEventToAllWorldInstancesAndWait failed.")
end
```


#### Receiving Events
Creators must attach an event handler to WorldInstanceService to receive events. Events occur on the server.

```lua
Event Handler:
[service: WorldInstanceService]
HandleMyWorldInstanceEvent(MyWorldInstanceEvent event )
{
    log ("Receive MyWorldInstanceEvent.")
}
```

# Limitations of WorldInstanceService
**WorldInstanceService** should be used with the following limitations in mind.
 
#### Naming Rules
WorldInstanceMemory and shared variable names can only be named with the following characters. The name must be at least 1 character long and can be a maximum of 100 characters. 

* **Alphabet upper case, lower case**
* **Number**
* **Blank**
* **_, \\, -**

#### Quantity and Size Limit
* You can create up to **100** WorldInstanceSharedMemory variables per world.
* The maximum value that can be saved in one shared variable is **80,000 bytes**.
* The total capacity of one shared memory cannot exceed **80 MiB**.
* The total size of the event properties to be transmitted cannot exceed **80,000 bytes**.

#### Usage Limitations
* There can be up to **1,000** WorldInstanceSharedMemory function calls per minute across all generated world instances.
    * If you change the values of 100 shared variables with one batch function call, the number of function calls is 100.
    * 100 simultaneous calls from 10 world instances equals 1,000 calls.
 * The maximum number of event transfers between world instances is **1,000** per minute.

# Usage Example
This is an example of detecting events between world instances. 
1. Workspace - MyDesk - Create Scripts - Create EventType - create a new **MyUserEnterEvent**.
2. Add the property to **MyUserEnterEvent**. 

    ```lua
    Property:
    string WorldInstanceId = ""
    string Nickname = ""
    ```

3. Workspace - MyDesk - Create Scripts - Create Logic - create a new **MyUserEnterLogic**.
4. Press [+] in the Entity Event Handler of **MyUserEnterLogic** and add **UserEnterEvent**. 
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16794535826480d4537b7507743b7be90ce6f1e6666b7.png "1")
Write the following so that when a new user enters a world instance, an event is sent to all world instances.
    ```lua
    Event Handler:
    [Service: UserService]
    HandleUserEnterEvnet(UserEnterEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: UserService
        -- Space: Server
        ---------------------------------------------------------
        
        -- Parameters
        local UserId = event.UserId
        ---------------------------------------------------------
        
        local worldInstId = _WorldInstanceService.WorldInstanceId
        local user = _UserService:GetUserEntityByUserId(UserId)
        local nickname = user.PlayerComponent.Nickname
        
        local evt = MyUserEnterEvent(worldInstId, nickname)
        _WorldInstanceService:RequestSendEventToAllWorldInstancesAndWait(evt)
    }
    ```  
5. Press [+] in the Entity Event Handler and add **MyUserEnterEvent**.
Change self to **service**.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1679453622421b14075cdc1894563be692c22e59996f5.png "3")
6. Press ![open](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_open_folder.png "open") and specify **WorldInstanceService**.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16794536057958e520b24e60e46ad8b842e580a652971.png "2")
Write the following so that you'll know which world instance the user entered from.<br>
    ```lua
    [service: WorldInstanceService]
    HandleMyUserEnterEvent(MyUserEnterEvent event)
    {
        -- Parameters
        local WorldInstanceId = event.WorldInstanceId
        local Nickname = event.Nickname
        ---------------------------------------------------------
        
        local currWorldInstId = _WorldInstanceService.WorldInstanceId
         
        if currWorldInstId == WorldInstanceId then
        	log ("User '" .. Nickname .. "' has entered this world instance.")
        else
        	log ("User '" .. Nickname .. "' has entered another world instance.")
        end
    }
    ```
7. You can launch a world and check the logs to see if you are entering from different world instances. If you check in the Maker, they all enter the same world instance.