# Course Introduction
Logic generally progresses in order, but there are exceptions.
Let's learn the difference between the synchronous system, where the operations are dealt with sequentially, and the asynchronous system, where they are not. We'll also have a look at Callback, which you need to utilize asynchronous systems.

# Synchronous, Asynchronous
There are two ways of processing logic.

1. Synchronous System
2. Asynchronous System

<span style="color: #dc9656">**The synchronous system**</span> processes logic sequentially. Let's look at it with a real-life example. Today is Tom's cleaning day. He needs to do three things; cleaning, washing dishes, and laundry. Cleaning takes 10 minutes, washing dishes, 30 minutes, and laundry, 50 minutes. If he works in a synchronous system, he will start one task, finish it, and start another task. Since he works sequentially, it takes 90 minutes to finish everything.
Like this, the synchronous system waits until the previous operation finishes and starts a new operation when it is over. As it processes operations sequentially, it is easy to know when the operations will be done.

![Synchronous](https://mod-file.dn.nexoncdn.co.kr/bbs/16594006716892a53bc7e23524cd18d6c24d550b20dee.png "Synchronous")

<span style="color: #dc9656">**The asynchronous system**</span> doesn't process all operations sequentially. Some operations are processed separately. 
Let's go back to Tom's example. If Tom works in an asynchronous system, he uses the washing machine for the laundry so that he can clean and do the dishes in the meantime. Since Tom works in parallel, it takes 50 minutes to complete all operations.
Sometimes, while processing logic, it takes longer than the expected working time due to external factors. This occurs when transferring external data or processing other IO (Input/Output). In MapleStory Worlds, DataStorage is an API related to this. In this case, using the asynchronous system will save you some unnecessary time.

![Asynchronous](https://mod-file.dn.nexoncdn.co.kr/bbs/165940133424639cb2fcd1d0744d4903f5d9340e896f6.png "Asynchronous")
#### Learn about the Function Synchronous Method
You can easily find out with the ending of the function name whether the function the creator wants to use has been executed synchronously or asynchronously. The synchronous system shows Wait at the end and the asynchronous system will show Async.

# Callback
Before we learn about Callback, let's return to Tom. For Tom to finish cleaning entirely, he must finish all three tasks and hang up the washing.
If Tom works in a synchronous system, he will hang the washing after the laundry is done. But, even if Tom works in an asynchronous system, he still has to hang the washing after the laundry is done. However, the washing machine is doing the laundry for him and he's doing other tasks. He can't know when the laundry will finish. The washing machine must ring an alarm that the laundry is done to let Tom know.
Like this, ringing an alarm to indicate when the operation outside the main flow is completed is called Callback. 
The asynchronous operations are not done sequentially, so we must know when they are done. This is when you use Callback.

Callback can be used for process completion, and also other purposes. You can define what related operations to run after asynchronous work and call the Callback at a certain timing. For instance, TimerService SetTimerRepeat is a function that makes certain operations run at certain hours. If you set the certain operations to be Callback, you can execute a Callback function periodically.

![callback](https://mod-file.dn.nexoncdn.co.kr/bbs/1659403272785463a83a6affa428a8d835cd347e9e1fd.png "callback")

#### Callback Function Use
This is an example of executing the Callback function when the process is completed.

```lua
Property:
[Sync]
number Power = 0

Method:
[server only]
void OnBeginPlay ()
{
    local data = _DataStorageService:GetGlobalDataStorage("data")
    
    self.Power = 20
    
    local callBack = function (errorcode, key)
        log("Power is saved")
    end
    
    data:SetAsync("Power", tostring(self.Power), callBack)
}
```

><span style="color: #7cafc2">**Tip**
> When you use Function of Component as Callback, you need to use `self.function()`, not `self:function()`.</span>