# Course Introduction
There are times when the timing of some events (actions) have to be controlled, such as a monster starting an attack a few seconds after it spawns or a monster dropping an item 1 second after it dies.
You can use TimerService to easily and efficiently control the timing of actions. Let’s learn what TimerService can do and how to use it.

# TimerService Introduction
**TimerService** provides a convenient scheduling function that can trigger an action at a desired time. Let’s see how to create a situation of a monster dropping reward items upon its death without the use of TimerService. 

![Timeservice01](https://mod-file.dn.nexoncdn.co.kr/bbs/165604735697978e39853991a4feea1e8253e5602185e.png "Timeservice01")

To make an item appear 3 seconds after a monster dies, you need to write a long script as follows. That's because you must check the time of a monster's death frame-by-frame and ensure the item appears after 3 seconds.

```lua
Property:
[None]
table DropItemList = {}

Method:
[server only]
void OnBeginPlay()
{
    self.DropItemList = {}
}

[server only]
void OnUpdate(number delta)
{
    for i, dropInfo in pairs(self.DropItemList) do
    	if dropInfo.dropTime < os.clock() then
    		continue
    	end
    	
    	self:DropItem(dropInfo.dropItemArr, dropInfo.itemDropPosition)
    end
}

[sever only]
void OnMonsterDestroy(Entity monsterEntity)
{
    local monsterInfo = monsterEntity.MonsterInfo
    local dropItemArr = monsterInfo.dropItemArr
    local itemDropPosition = monsterEntity.TransformComponent.Position
    
    table.insert(self.DropItemList, {["driopItemArr"] = dropItemArr, ["dropTime"] = os.clock() + 3, ["itemDropPosition"] = itemDropPosition})
    
    _EntityService:Destroy(monsterEntity)
}

[server only]
void DropItem(SyncTable<string> itemArr, Vector3 dropPosition)
{
    for i , itemId in pairs(itemArr) do
    	self:SpawnItem(itemId, dropPosition)
    end
}
```

With **TimerService**, you can write much simpler code. Pass the CallBack function for item drop to the `SetTimer()` function of TimerService, then set the number of seconds before it is executed. Instead of using several functions and writing dozens of lines of code, you can simplify the process by triggering the CallBack and `SetTimer()` functions.

```lua
Method:
[server only]
void OnMonsterDestroy(Entity monsterEntity)
{
    local monsterInfo = monsterEntity.MonsterInfo
    local dropItemArr = monsterInfo.dropItemArr
    local itemDropPosition = monsterEntity.TransformComponent.Position
    
    local dropItem = function()
    	self:DropItem(dropItemArr, itemDropPosition)
    end
    
    _TimerService:SetTimer(self, dropItem, 3, false)
    _EntityService:Destroy(monsterEntity)
}

[server only]
void DropItem(SyncTable<string> itemArr, Vector3 dropPosition)
{
    for i, itemId in pairs(itemArr) do
    	self:SpawnItem(itemId,dropPosition)
    end
}
```

# Using SetTimer
`SetTimer()` is a reserve function. This function can reserve an action to be executed at a designated time, cause it to repeat, and set the repetition interval. The parameter's meaning and usage vary depending on whether the reserved action is repeated. `SetTimer()` returns the integer-type timer's id if the reservation is successful; otherwise, it returns 0. Parameters are as follows.

| Type | Parameter Name | Explanation |
| --- | ------ | --- |
| IScriptable | scriptable | Passes the owner of the action to reserve. The action's owner can be logic or component, and normally the component that called `SetTimer()` is passed. |
| func | callback | Passes action to execute in a few seconds in the type of CallBack function. |
| float | intervalSeconds | <li>If **isRepeat value is true**: Sets seconds until the re-execution of the CallBack function after the initial execution.</li><li>If **isRepeat value is false**: Sets when to execute the CallBack function after calling `SetTimer()`. </li> |
| boolean | isRepeat | Sets if the CallBack function will be executed once or repeatedly. <li>**true**: Executes repeatedly</li><li>**false**: Executes once</li> |
| float | startDelaySeconds |  Sets how many seconds after an executed function the CallBack function will be executed. Valid if the isRepeat value is true.|

> <span style="color: #dc9656">**Caution!
> When delivering a component to a `scriptable` parameter, the execution of a callback is affected by the component’s EnableInHierarchy.**</span>

#### Executing the Action Once
When SetTimer is called, it outputs the string \"SetTimer!\" and the time to execute the code. After 3 seconds, when the CallBack function passed with `SetTimer()` is called, it outputs the string \"CallBack!\" and the time taken in executing the code.
![Timeservice04](https://mod-file.dn.nexoncdn.co.kr/bbs/1656047389794ca939882847b489ebf4ef39fb011ce3d.png "Timeservice04")

Example code will be as follows. Set the intervalSeconds to 3 seconds when passing the CallBack function. Since the execution will not be repeated, set isRepeat to false and ignore startDelaySeconds.

```lua
[server only]
void OnBeginPlay()
{
    local callBack = function()
        log("CallBack! : "..tostring(os.date()))
    end
 
    log("SetTimer! : "..tostring(os.date()))
    _TimerService:SetTimer(self, callBack, 3, false)
}
```

Start	![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play") for a test. Confirm each string and the time taken to execute the code from the console window to make sure the CallBack function is executed 3 seconds after calling `SetTimer()`. It was first called at 39s and then executed at 42s, showing the 3-second interval.

![TimeService06](https://mod-file.dn.nexoncdn.co.kr/bbs/1656047982444631a010e59c04cc8b5865653b19b0c15.png "TimeService06")

#### Executing the Action Repeatedly
Change isRepeat to true for the action to be executed repeatedly. When `SetTimer()` is called, it outputs the string \"SetTimer!\" and the time taken to execute the code.

![TImeService07](https://mod-file.dn.nexoncdn.co.kr/bbs/16560480360032fd64662b6f94dadb45de7c8f185d25a.png "TImeService07")

Example code will be as follows. Every 3 seconds, when the CallBack function passed with `SetTimer()` is called, it outputs the string \"CallBack!\" and the time taken to execute the code.

```lua
Method:
[server only]
void OnBeginPlay()
{
    local callBack = function()
        log("CallBack! : "..tostring(os.date()))
    end
     
    log("SetTimer! :"..tostring(os.date()))
    _TimerService:SetTimer(self, callBack, 3, true)
}
```

Test by clicking Start ![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play"). Confirm that the console window’s output time has increased by 3 seconds each time. Unlike a 1-time execution, the "SetTimer!" and "CallBack!" strings have the same output time thanks to the meaning of intervalSeconds when setting `SetTimer()` to repeat. For 1-time executions, intervalSeconds indicates how many seconds after `SetTimer()` the CallBack will be executed. For repeated executions, the CallBack function indicates how many seconds before it will execute again after the first execution. Therefore, `SetTimer()` and the CallBack function are executed at the same time, with the CallBack function being re-executed every 3 seconds. If the interval of time between each CallBack function execution is important, only adjust the intervalSeconds value.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1718961812726747d9d0148fd48bf9d9b923748e84c31.png "5")
If you'd like to set when the CallBack function will be first called after calling `SetTimer()`, enter a value for startDelaySeconds, the last parameter of `SetTimer()`. The CallBack function will be first executed 2 seconds after calling `SetTimer()`, and the CallBack function will be executed every 3 seconds afterward. Next is an example with startDelaySeconds value.

```lua
[server only]
void OnBeginPlay()
{
    local callBack = function()
        log("CallBack! : "..tostring(os.date()))
    end
 
    log("SetTimer! : "..tostring(os.date()))
    _TimerService:SetTimer(self, callBack, 3, true, 2)
}
```

Start	![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play") for a test. Confirm that the console outputs the time as the creator intended. Confirm that the CallBack function was first executed 2 seconds after calling `SetTimer()`, and then every 3 seconds after that.
![TimeService11](https://mod-file.dn.nexoncdn.co.kr/bbs/1656048115485a61aa280dea54528905c8da80da35fb7.png "TimeService11")

#### Canceling Action Reservation
Now that you've set up `SetTimer()` to specify the execution of a certain action, you might want to cancel the reservation under certain conditions.
For example, let's say you made a door that opens 3 seconds after turning the switch On. The door will automatically open 3 seconds after the user turns the switch On. That is because you would have used `SetTimer()` to reserve an action that is linked to opening the door. However, if the user turns the switch Off before the door opens, you must cancel the `SetTimer()` to prevent the door from opening when the switch is Off.

![TimerService12](https://mod-file.dn.nexoncdn.co.kr/bbs/1656048132717f15c590130b1464c8451382b95717a37.png "TimerService12")

The following script took our previous example and added code to cancel `SetTimer()` 10 seconds after `SetTimer()` is called.

```lua
Method:
[server only]
void OnBeginPlay()
{
    local callBack = function()
        log("CallBack! :"..tostring(os.date))
    end
    
    log("SetTimer! :"..tostring(os.date()))
    local timerId = _TimerService:SetTimer(self, callBack, 3, true, 2)
    if timerId == 0 then
        return
    end
    
    local cancelCallBack = function()
        _TimerService:ClearTimer(timerId)
    end
    
    _TimerService:SetTimer(self, cancelCallBack, 10, false)
}
```

Start	![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play") for a test. The log is no longer output 10 seconds after SetTimer! is first triggered.

> <span style="color: #7cafc2"> </span><span style="color: #7cafc2">**Tip**</span>
> <span style="color: #7cafc2"> The initial output was at 46s, and the final was at 54s. The log was only output for 8 seconds, since `ClearTimer()` was executed 10 seconds after `SetTimer()`.</span>"
> <span style="color: #7cafc2"> CallBack was first executed 2 seconds after `SetTimer()`, repeating every 3 seconds. First CallBack execution after 54s should be at 57s, but CallBack was canceled at 56s, leaving no CallBack to be executed at 57s. Therefore, no log was output 8 seconds after the initial execution.</span>

![TImeService14](https://mod-file.dn.nexoncdn.co.kr/bbs/1656048756087cf0360b0140447059661594e84ccd0da.png "TImeService14")

# SetTimer's Spin-Off Features
Depending on your needs, you can use `SetTimerOnce()` and `SetTimerRepeat()` as action reservation and action performance features, respectively. `SetTimerOnce()` is used for single action reservation, and `SetTimerRepeat()` is used for repeated action performance. Since they are separate, they require fewer parameters, making them easier to use as compared to `SetTimer()`.
#### SetTimerOnce
`SetTimerOnce()` works the same way as `SetTimer()` with its isRepeat set to false. It returns the integer-type timer's id if the reservation is successful; if not, 0 will be returned. The parameters are as follows.

| Type | Parameter Name | Explanation |
| --- | ------ | --- |
| func | callback | Passes action to execute in a few seconds in the type of CallBack function. |
| float | delaySeconds | Set when the action will be executed after reserving it. |

```lua
[server only]
void OnBeginPlay()
{
    local callBack = function()
        log("CallBack! : "..tostring(os.date()))
    end
 
    log("SetTimer! : "..tostring(os.date()))
    _TimerService:SetTimerOnce(callBack, 3)
}
```

Start	![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play") for a test. Confirm that the action was executed 3 seconds after `SetTimerOnce()` was called.
![TimeService16](https://mod-file.dn.nexoncdn.co.kr/bbs/1656048219588d8718bc60c154cab8e818e7561f37295.png "TimeService16")

> <span style="color: #dc9656">**Caution!
> Like ‘_TimerService:SetTimerOnce(self.Method,1)’, when directly delivering a component’s method to the ‘callback’ variable, the execution of the callback will be affected by the component’s EnableInHierarchy.**</span>

#### SetTimerRepeat
`SetTimerRepeat()` is used to reserve repeated actions. It works the same way as `SetTimer()` with its isRepeat set to true. It returns the integer-type timer's id if the reservation is successful; if not, 0 will be returned. The parameters are as follows.

| Type | Parameter Name | Explanation |
| --- | --- | --- |
| func | callback | Passes action to repeat in the type of CallBack function. |
| float | intervalSeconds | Set the repetition interval (sec).|
| float | startDelaySeconds | Set when the repetition will begin after reserving an action.|

```lua
[server only]
void OnBeginPlay()
{
    local callBack = function()
        log("CallBack! : "..tostring(os.date()))
    end
 
    log("SetTimer! : "..tostring(os.date()))
    _TimerService:SetTimerRepeat(callBack, 3, 2)
}
```

Start	![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play") for a test. Confirm that the action is first executed 2 seconds after calling `SetTimer()`, and then repeated every 3 seconds afterward.

![TimeService18](https://mod-file.dn.nexoncdn.co.kr/bbs/16560482841675f11475a77124fb58840efc33d198675.png "TimeService18")

> <span style="color: #dc9656"> **Caution!
> Like ‘_TimerService:SetTimerOnce(self.Method, 1, 1)’, when directly delivering a component’s method to the ‘callback’ variable, the execution of the callback will be affected by the component’s EnableInHierarchy.**</span>

#### Canceling Reservation
‘SetTimerOnce()’ and ‘SetTimerRepeat()’ can cancel scheduled actions. The ‘ClearTimer()’ function is used when canceling a scheduled action. 
You can cancel an action that is set to trigger in 10 seconds like in the example below. 

```lua
Method:
[server only]
void OnBeginPlay()
{
    local callBack = function()
         log("CallBack! :"..tostring(os.date()))
    end
    
    log("SetTimer! :"..tostring(os.date()))
    local timerId = _TimerService:SetTimerRepeat(callBack, 3, 2)
    if timerId == 0 then
        return
    end
    
    local cancelCallBack = function()
        _TimerService:ClearTimer(timerId)
    end
    
    _TimerService:SetTimerOnce(cancelCallBack, 10)
}
```

You can see that the log is not output after 10 seconds after the initial output.

![TimeService20](https://mod-file.dn.nexoncdn.co.kr/bbs/1656048305326b5db28fd79174da8bec8b26163ab0e74.png "TimeService20")

# Notice
When using the repeated action reservation, you must call the `ClearTimer()` function at the right time to cancel the reservation. We recommend canceling the reservation by calling the `ClearTimer()` function when the OnEndPlay of the component or logic or the owner of the reserved action is destroyed, since excessive accumulation of reserved actions can degrade the World performance. 
Even if the creator did not call the `ClearTimer()` function, it will automatically try the `ClearTimer()` function internally if the owner of the action reserved for optimization of the World is destroyed.

```lua
Property:
[None]
integer TimerId = 0

Method:
[server only]
void OnBeginPlay()
{
    local callBack = function()
        log("CallBack! :"..tostring(os.date()))
    end

    self.TimerId = _TimerService:SetTimerRepeat(callBack, 3)
}

[server only]
void OnEndPlay()
{
    if self.TimerId > 0 then
        _TimerService:ClearTimer(self.TimerId)
    end
}
```