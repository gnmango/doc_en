# Course Introduction
You can use `WorldInstanceService` to check the current World InstanceId and the status of other World instances. You can also utilize `TeleportService` to warp the player to a specific World instance. 
##### Reference Guide
[World Instance]
[World Instance Communication](d/ocs/?postId=999{"target":"_self"})
[Warp World]
##### APIReference for Reference
[TeleportService](/apiReference?postId=315{"target":"_self"})   
[WorldInstanceService](/apiReference?postId=1034{"target":"_self"})
[WorldInstanceInfo](/apiReference?postId=1128{"target":"_self"})

# Learn WorldInstanceId
#### WorldInstanceId
<span style="color: #dc9656">**WorldInstanceId**</span> is a unique value issued when the [World Instance] is created. It is used to distinguish between World instances. For example, if 30 World instances of World A are created, a WorldInstanceId is created for each World instance, for a total of 30 WorldInstanceIds.

There are two ways to know the Id of the World instance the user is currently connected to.
1.  You can use the `WorldInstanceId` property of `WorldInstanceService` to know the `WorldInstanceId` of the World instance the user is currently connected to.
2. You can use `GetWorldInstanceInfoPagesAndWait()` and `GetWorldInstanceInfoPagesAsync()` of the WorldInstanceService to search the WorldInstanceInfo by page.

#### WorldInstanceInfo
<span style="color: #dc9656">**[WorldInstanceInfo](/apiReference/Misc/WorldInstanceInfo{"target":"_self"})**</span> has the information for each World instance. The properties of WorldInstanceInfo are as follows.

* <span style="color: #dc9656">**Id**</span>: A unique value that each World instance has.
* <span style="color: #dc9656">**MaxUserCount**</span>: The maximum number of users that can access World Instance.
* <span style="color: #dc9656">**CurrentUserCount**</span>: The number of users currently in the World instance. 

#### WorldInstanceInfoPages
<span style="color: #dc9656">**[WorldInstanceInfoPages](/apiReference/Misc/WorldInstanceInfoPages{"target":"_self"})**</span>Object for retrieving information about the World Instance. You can use the `GetCurrentPageDatas()` and `MoveToNextPageAndWait()` functions to get a list of data or move to the next page.

# Warping World Instance
You can use <span style="color: #dc9656">**TeleportService**</span>'s `WarpUserToWorldInstanceAndWait()` and `WorldUserToWorldInstanceAsync()` functions to warp the user to another World instance. 
 
To warp a player, you need the **UserId** of the user to warp and the **WorldInstanceId** of the World instance to warp to. You can warp to another World instance by retrieving the information of the most recent World instances at the time the player attempts to warp. For more information about the recent WorldInstanceInfo, you can use the WorldInstanceService's `GetWorldInstanceInfoPagesAndWait()` and `GetWorldInstanceInfoPagesAsync()` functions to search the WorldInstanceInfo by page. However, the World instances for warping must be within the World the user is currently connected to.

Use `WarpUserToWorldAndWait()` to pass the user's accumulated data when warping to another World instance. You can get the passed data by accessing it with `GetWarpRecord()`. For detailed information, please refer to [TeleportService](/apiReference?postId=315{"target":"_self"}).

><span style="color: #7cafc2">**Tip**
> World instance warp is only available in the **publicly released Worlds**. World instance warp is not available in the private Worlds.
> You cannot test the World instance warp **during production**. You need to make sure that World instance warp works in the released World.<span>

> <span style="color: #585858">**Learn More**
> `WarpUserToWorldAndWait` and `WarpUserToWorldAsync` warp the World by the WorldId. The destination World for the warp is the second or third World, not the World the user is in, and it is impossible to specify which World instance to warp to.
> In addition, if there is no World instance to warp to, a new World instance will be created to warp.</span>

#### Non-Warpable World Instance 
Sometimes you may not be able to move to the World instance you attempted to warp to. If the warp fails, the player will see a failure message screen and be returned to the World's lobby screen. You cannot return to the World instance from which you attempted to warp. 
The reason why World instance warps fail is because the state of the World instance is constantly changing. The World instance may have reached its full capacity, or the World instance may be preparing to retire and no longer accept new users. 


# Use Example
#### Warp
 ```lua
 [server]
 void WarpToAnotherWorldInstance (string userId, string worldInstanceId)
 {
    local warpData = {}
    local serializedData = _UtilLogic:TableToString(warpData)
    
    _TeleportService:WarpUserToWorldInstanceAndWait(userId, worldInstanceId, serializedData)
 }
 ```


#### Searching World Instance Information by Page
This is an example of how to search the World instance information by page using the `GetWorldInstanceInfoPagesAndWait()` function.
```lua
    local result, pages = _WorldInstanceService:GetWorldInstanceInfoPagesAndWait()
    
    while true do
    
       -- You can get a list of current pages with GetCurrentPageDatas().
    	local datas = pages:GetCurrentPageDatas() 
    	
    	for _, data in pair(datas) do
    		log(data.Id)
    	end
        
       --You can check if the current page is the last page by using the IsLastPage property.
    	if pages.IsLastPage == true then 
    		break
    	end
    	
    	--You can move to the next page with MoveToNextPageAndWait().
    	pages:MoveToNextPageAndWait() 
    	
    end
```


This is an example of how to view the World instance information by page using the `GetWorldInstanceInfoPagesAsync()` function.
```lua
    local callback = function (result, pages)
    
    	while true do
    		local datas = pages:GetCurrentPageDatas()
    		
    		for _, data in pair(datas) do
    			log(data.Id)
    		end
    
    		if pages.IsLastPage == true then
    			break
    		end
    
    		pages:MoveToNextPageAndWait()
    	end
    end
    
    _WorldInstanceService:GetWorldInstanceInfoPagesAsync(callback)
```