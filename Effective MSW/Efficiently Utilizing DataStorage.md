# Course Introduction
DataStorage allows for easy storage and retrieval of data. However, using DataStorage can present various issues. In this guide, we will introduce some recommended practices for using DataStorage. Appropriate solutions are needed depending on the characteristics of the World created by the creator.

# Processing Error Codes in DataStorage
Data may not be loaded from DataStorage for a variety of reasons. For example, it could take too long (TimedOut) or the call count limit could be exhausted (ResourceExhausted). Implementing functions while keeping in mind the issues that prevent data from loading normally with the use of request function's error codes is recommended. Error codes can be seen from [Using DataStorage](/docs/?postId=692{"target":"_self"}). 

Let's consider a situation wherein the loading of Currency data fails, as shown in the diagram below. If changed Currency data is stored in this situation, there is a possibility that the accumulated Currency data of the user from before may be lost.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1688020475127ad60e0978dbf4a00a11806ee7343e3f2.png "1")
#### Expelling a User
In certain situations, a specific user may fail to load data for various reasons, while other users can successfully enter and load data. For example, if a user fails to load essential data for World play or initial user data, you can consider using the user expelling method. This is because the user will not be able to play the World properly, and handling the situation afterward could become very complex. 
Here is an example of how to expel a user who fails to load the Meso data upon entering the World.

```lua
Property:
[Sync]
integer meso = 0
 
Method:
[server only]
void OnBeginPlay()
{
    if self:LoadMesoData() == false then
        _UserService.KickUser(self.Entity.PlayerComponent.UserId, KickReason.WorldError) -- Expel User
        return
    end
}

[server only]
boolean LoadMesoData()
{
    local userDataStorage = _DataStorageService:GetUserDataStorage(self.Entity.PlayerComponent.UserId)
    if userDataStorage == nil then
        return false
    end

    local errorCode, mesoString = userDataStorage:GetAndWait("Meso")
    if errorCode ~= 0 then
        return false
    end

    if _UtilLogic:IsNilorEmptyString(mesoString) == false then
        self.meso = math.tointeger(tonumber(mesoString))
    end

    return true
}
```

> <span style="color: #585858">Learn More
> Depending on the cause of the data loading failure, users may succeed in retrieving the data upon reconnection or fail to reconnect to the World.</span>

#### Retry Mechanism
Creators can encounter temporary failures with unclear causes that result in data loading failures even though they made a valid request. To handle such situations, creators can implement a retry mechanism wherein data loading requests are attempted multiple times.

The following example code retries loading the data after a 2-second delay if it fails. 
If the data retrieval continues to fail after a certain number of retries, the user is expelled from the World. Additionally, if the error code ResourceExhausted: 1000005 occurs due to excessive requests, the user will be expelled without further retries.

```lua
Property:
[Sync]
integer meso = 0
[Sync]
boolean initialized = false
 
Method:
[server only]
void OnBeginPlay()
{
    local retryCount = 2    -- Set retry count to 2 for 3 total save attempts.

    while retryCount >= 0 do
        if self:LoadMesoData() == true then
            break
        end
    
        wait(1) -- Wait for the specified duration and retry.
        retryCount = retryCount - 1
    end

    if self.initialized == false then
        _UserService.KickUser(self.Entity.PlayerComponent.UserId, KickReason.WorldError)    -- Expel User
        return
    end
}
 
[server only]
boolean LoadMesoData()
{
        local userDataStorage = _DataStorageService:GetUserDataStorage(self.Entity.PlayerComponent.UserId)
        if userDataStorage == nil then
            return false
        end
         
        local errorCode, mesoString = userDataStorage:GetAndWait("Meso")
        if errorCode == 0 then
            self.meso = math.tointeger(tonumber(mesoString))
            self.initialized = true
            return true
        elseif errorCode == 1000005 then
            return true -- ResourceExhausted: Error occurs due to exceeding the limit of the number of calls. No retries will be performed.
        end
 
        return false
}
```

#### Not Storing Data
If the data is not essential for the World and it fails to load, you may consider not storing the data. 

![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16896700922911c1d3215ecb5499f93548d3cac593c63.png "2")

The following example demonstrates storing the success status of a request in a property within the component and using logic to avoid storing the data.

```lua
Property:
[Sync]
boolean initialized = false -- Use this property to prevent data storing.
[Sync]
integer meso = 0
 
Method:
[server only]
void OnBeginPlay()
{
    if self:LoadMesoData() == false then
        return
    end

    self.initialized = true
}

[server only]
boolean LoadMesoData()
{
    local userDataStorage = _DataStorageService:GetUserDataStorage(self.Entity.PlayerComponent.UserId)
    if userDataStorage == nil then
        return false
    end
        
    local errorCode, mesoString = userDataStorage:GetAndWait("Meso")
    if errorCode ~= 0 then
        return false
    end

    if _UtilLogic:IsNilorEmptyString(mesoString) == false then
        self.meso = math.tointeger(tonumber(mesoString))
    end

    return true
}

[server only]
void SaveMesoData()
{
		local userDataStorage = _DataStorageService:GetUserDataStorage(self.Entity.PlayerComponent.UserId)
		if userDataStorage == nil then
			return
		end
		
       -- You need to implement a handling mechanism to avoid accessing the related data when self.initialized is false.
		if self.initialized == false then
			log("info is not initialized") -- If Load fails, it does not perform Save.
			return
		end
		
		local mesoString = tostring(self.meso)
		userDataStorage:SetAndWait("Meso", mesoString)
}

Event Handler:
[server only] [service UserService]
HandleUserLeaveEvent(UserLeaveEvent event)
{
    self:SaveMesoData()
}
```

#### Handling Partial Loading of Associated Data
When creating a World, creators typically use multiple Keys to save associated data. If these data have associations with each other, it is important to consider blocking related functionalities or blocking data storing. If one of the associated data cannot be stored, it can lead to potential issues.

The diagram below depicts a scenario wherein one of the associated data fails to load successfully.
In this situation, the item data value cannot be loaded from DataStorage, resulting in the absence of Item 1, 2, 3 on the server. Since the retrieved data is missing, even if the user purchased Item 4, it should not be stored normally. Only Item 4 would be stored while the previously accumulated Item data 1, 2, and 3 would be lost.
On the other hand, the Currency data, which uses a different key, has decreased from 300 to 100.
The server fails to save the item data but successfully saves the Currency data. As a result, the user has spent money, but the purchased items are not available.

![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1688020503728c09b4e8ca56548f69ef07795ff58a501.png "3")

Likewise, if loading one of the associated data fails, we recommend blocking data storing altogether to prevent any values from being stored. We recommend allowing and storing the item purchase functionality only after confirming the successful loading of all associated data.

![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16880205373850f32e6ea80224776be8a64cbf6fcc299.png "4")

Here is an example that allows storing only if the loading of Cash and Item data is successful.

```lua
Property:
[Sync]
boolean initialized = false
[Sync]
integer meso = 0
[Sync]
integer cash = 0
[None]
table items = {}    -- ItemIDs
 
Method:
[server only]
void OnBeginPlay()
{
    self:LoadData()
}

[server only]
void LoadData()
{
    local userDataStorage = _DataStorageService:GetUserDataStorage(self.Entity.PlayerComponent.UserId)
    if userDataStorage == nil then
        return
    end

    if self:LoadCurrency() == false then
        return
    end

    if self:LoadItems() == false then
        return
    end

    local itemInitialitedEvent = OnUserDataItemInitialized()
    for i, id in pairs(self.items) do
        table.insert(itemInitialitedEvent.items, id)
    end

    self:SendItemInitializedEventToClient(itemInitialitedEvent)  -- Synchronize items with the client using events.
    self.initialized = true -- Successful Loading of both Currency and Items.
}

[server only]
boolean LoadCurrency()
{
    local userDataStorage = _DataStorageService:GetUserDataStorage(self.Entity.PlayerComponent.UserId)
    if userDataStorage == nil then
        return false
    end

    local errorCode, currencyString = userDataStorage:GetAndWait("Currency")
    if errorCode ~= 0 then
        return false
    end

    if _UtilLogic:IsNilorEmptyString(currencyString) == false then
        local currency = _HttpService:JSONDecode(currencyString)
    
        if currency.meso ~= nil then
            self.meso = currency.meso
        end
    
        if currency.cash ~= nil then
            self.cash = currency.cash
        end
    end

    return true
}

[server only]
boolean LoadItems()
{
    local userDataStorage = _DataStorageService:GetUserDataStorage(self.Entity.PlayerComponent.UserId)
    if userDataStorage == nil then
        return false
    end

    local errorCode, itemString = userDataStorage:GetAndWait("Items")
    if errorCode ~= 0 then
        return false
    end

    if _UtilLogic:IsNilorEmptyString(itemString) == false then
        self.items = _HttpService:JSONDecode(itemString)
    end
        
    return true
} 
 
[client]
void SendItemInitializedEventToClient(OnUserDataItemInitialized event)
{
    self.Entity:SendEvent(event)
}

[server only]
string GetCurrencyString()
{
    local currency = {}
    currency.meso = self.meso
    currency.cash = self.cash
        
    return _HttpService:JSONEncode(currency)  -- Combining properties into a single table and converting it to a Json String for storage.
}

[server only]
string GetItemsString()
{
    return _HttpService:JSONEncode(self.items)  -- Storing a table as a Json String.
}

[server only]
void SaveData()
{
    if self.initialized == false then
        log("info is not initialized") -- If load fails, it does not perform store.
        return
    end
    
    local userDataStorage = _DataStorageService:GetUserDataStorage(self.Entity.PlayerComponent.UserId)
    if userDataStorage == nil then
        return
    end
    
    local currencyString = self:GetCurrencyString()
    local itemsString = self:GetItemsString()
    
    local errorCode = userDataStorage:SetAndWait("Items", itemsString)
    if errorCode ~= 0 then
        log("ErrorCode : "..errorCode)
    end
        
    errorCode = userDataStorage:SetAndWait("Currency", currencyString)
    if errorCode ~= 0 then
        log("ErrorCode : "..errorCode)
    end
}

Event Handler:
[client only] [self]
HandleOnUserDataItemInitialized (OnUserDataItemInitialized event)
{
    local items = event.items

    for i, id in pairs(event.items) do
        table.insert(self.items, id)
    end
}
```
 
Here is an example of a Logic script that determines whether to proceed with the shop purchase logic based on the value of the Initialized Property.

```lua
Method:  
[server]
void RequestBuyItemFromClient(string userId, integer buyItemId)
{
    local userEntity = _UserService:GetUserEntityByUserId(userId)
        if isvalid(userEntity) == false then
            return
        end

    local userDataComponent = userEntity.UserDataComponent
    if isvalid(userDataComponent) == false then
        return
    end

    if userDataComponent.initialized == false then
        log("userDataComponent not initialized") -- If load fails, shop purchasing will not be executed.
        return
    end

    -- Proceed with the shop purchase...
}

Event Handler:
[server only] [service UserService]
HandleUserLeaveEvent(UserLeaveEvent event)
{
    self:SaveData()
}
```
# DataStorage Usage Limit and String

DataStorage has usage limits. Excessive calls to functions of DataStorage types that consume Credits can lead to issues due to these limits. For detailed information on usage limits, refer to the document [Learning DataStorage Use Limits](docs?postId=1044{"target":"_self"} and [Utilizing DataStorage Use Limits](docs?postId=1045{"target":"_self"}.

#### Storing as a Single String
Using keys incurs Credit usage. When using many keys like the following example, it can consume a significant amount of Credits when loading or storing data. 
At the initial stage of World Creation, there may be a small number of keys, but as the World progresses, more Property may be added. It can result in higher Credit usage.
To optimize credit usage, you can consider bundling data into a single String and using a single Key for storage. We recommend grouping related data together. This approach simplifies data management. 

1. <span style="color: #dc9656">**Using Json String Conversion Functions**</span>
Use `_HttpService:JSONEncode()` and `_HttpService:JSONDecode()` for storing and loading.
Conversion between Json and table is possible. 

    ```lua
    void PrintTable()
    {
        local testTable = {}
        testTable.meso = 10
        testTable.money = 200
        testTable.innerTable = {}
        testTable.innerTable.val = "string value"
     
        log(_HttpService:JSONEncode(testTable))
    }
     
    -- Result of JsonEncode
    {"meso":10,"money":200,"innerTable":{"val":"string value"}}
    ```

2. Using Conversion Functions of <span style="color: #dc9656">**UtilLogic**</span>
You can use `_UtilLogic:TableToString()` and `_UtilLogic:StringToTable()` to combine data into a single String.
If a table contains another table, it may not be converted correctly.  Type of key and value will be output as strings, which can result in longer string lengths.

    ```lua
    void PrintTable()
    {
        local testTable = {}
        testTable.meso = 10
        testTable.money = 200
        testTable.innerTable = {}
        testTable.innerTable.val = "string value"
     
        log(_UtilLogic:TableToString(testTable))
    }
     
    -- Result of TableToString
    String  meso    Int64   10
    String  money   Int64   200
    String  innerTable  LuaTable    table :868
    ```

#### Concatenating and Splitting Strings Manually
Creators have the option of directly creating and using strings instead of using functions that follow pre-defined string formats. By creating strings manually, it is possible to save data in shorter string representations.
Using` _UtilLogic:Split()` and `string concatenation(..)`, strings can be split and combined as shown in the following example. Table<v> data can be combined using table.concat.

```lua
Property:
[None]
integer meso = 0
[None]
integer cash = 0
 
Method:
[server only]
boolean LoadCurrency()
{
    local userDataStorage = _DataStorageService:GetUserDataStorage(self.Entity.PlayerComponent.UserId)
    if userDataStorage == nil then
        return
    end

    local errorCode, currencyString = userDataStorage:GetAndWait("Currency")
    if errorCode ~= 0 then
        log(errorCode)
        return false
    end

    if _UtilLogic:IsNilorEmptyString(currencyString) == false then
        local split = _UtilLogic:Split(currencyString, ",")
        if split[1] ~= nil then
            self.meso = tonumber(split[1])
        end

        if split[2] ~= nil then
            self.cash = tonumber(split[2])
        end
    end

    return true
}

[server only]
string GetCurrencyString()
{
    local currencyString = tostring(self.meso) .. "," .. tostring(self.cash)
    return currencyString   -- Save this string as a value. SetAndWait("Currency", currencyString)
}
```

# DataStorage Usage Limit and Storage Frequency
Frequent storage of data in DataStorage with usage limits can lead to insufficient Credit. Therefore, we recommend storing only values that need to be stored when they are changed using property to reduce Credit consumption. It is advisable to save data in the following three scenarios.

1. <span style="color: #dc9656">**Saving on User Disconnection**</span>: When a user disconnects, the data is stored in DataStorage.
2. <span style="color: #dc9656">**Saving at Regular Intervals**</span>: User's updated information should be periodically stored. As there is a chance of failed saves due to the possibility of server issues. We recommend determining an appropriate storage frequency. Requesting frequent saves can improve the reliability of DataStorage, but it may consume more Credit.

    ```lua 
    Property:
    [None]
    integer saveDataTimer = 0
    
    Method:
    [server only]
    void OnBeginPlay()
    {
        local intervalSeconds = 300 -- Save data at 5-minute intervals.
        local startDelaySeconds = 300
        local userId = self.Entity.PlayerComponent.UserId
    
        local callback = function()
            _UserDataLogic:SaveData(userId) -- Call the save function in the logic.
        end
    
        self.saveDataTimer = _TimerService:SetTimerRepeat(callback, intervalSeconds, startDelaySeconds)
    }
    ```
3. <span style="color: #dc9656">**Saving during Exception Handling**</span>: When an exception situation occurs, data is stored to DataStorage. If there are specific values that need to be stored when data is updated in the World created by the creator, they can be handled as exceptions and stored only in those cases.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16880205552196aad4508ce5743c3a87df656da53dbf9.png "5")

#  Considerations for DataStorage Usage Limit
#### Users Directly Avoid Execution of DataStorage Type Functions
It is advisable not to allow users to use DataStorage functions directly when they press the data update button. This is because unexpected usage of Credit may occur due to the actions of other users, which can result in higher Credit consumption than intended. If it is necessary for users to use DataStorage functions due to their actions, setting a specific interval for using that feature can help reduce unintended Credit usage. 

Let's consider the scenario wherein a user writes code that retrieves data from DataStorage every time he or she presses a UI button. If a specific user ends up pressing that button more frequently than expected by the creator, it can lead to excessive Credit usage and waste.

The following script allows users to save data using the save button.
By adding a cooldown mechanism, it prevents users from repeatedly saving data within a short period; thus preventing excessive Credit usage.
```lua
Property:
[None]
number lastSaveTime = 0

Method:
[server]
void SaveData()
{
	local cooldown = 120	-- Saving is not allowed for 120 seconds.

	if _UtilLogic.ServerElapsedSeconds < self.lastSaveTime + cooldown then
		-- Using cooldown prevents users from attempting multiple saves within a short period of time.
		return
	end

	self.lastSaveTime = _UtilLogic.ServerElapsedSeconds

	-- Data is stored using DataStorageService...
}

Event Handler:
[client only] [entity] [Button_Save (/ui/DefaultGroup/Button_Save)]
HandleButtonClickEvent(ButtonClickEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: ButtonComponent
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    local Entity = event.Entity
    ---------------------------------------------------------
    
	self:SaveData()
}
```

#### Consideration of Credit Usage for Instance Room and DataStorage Functions
Worlds that utilize instance room need to consider its characteristics when using DataStorage functions in Logic. For example, let's imagine a scenario wherein the ranking data for the entire World is periodically retrieved from DataStorage, calculated, and displayed based on client requests. If a user enters an instance room, the ranking needs to be calculated for each instance room, so the usage of Credit must be taken into account. Setting an appropriate update interval can also prevent excessive Credit consumption in such cases.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1688020588749d8a2d007f695499c9b22ac0a0ae4f8b4.png "6")


#### Save Data in UserLeaveEvent Handler when a User Leaves
The implementation of the feature to save in DataStorage when a user leaves should be added to UserLeaveEvent Handler.
If you use the AndWait function in OnEndPlay to save data, the previously saved data may not be loaded properly when the users re-enter the World.
If you use SetAndWait or SetAsync in OnEndPlay to save data, the users may get the wrong data when they re-enter the World.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/169215377846091c995734f724ee9b07fbc0a4a025456.png "8")

If you call a DataStorage-type function in the UserLeaveEvent handler instead of OnEndPlay, the proper execution of previous SetAndWait and SetAsync functions is guaranteed when the users re-enter the World.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1692158087433fe8a72406fa04e0fbea3eaf4f1fc6ffa.png "8")
However, if you call the DataStorage saving function using Async Callback as below, the proper function execution is not guaranteed.

```lua
Method:
[server only]
void OnBeginPlay()
{
    local userDataStorage = _DataStorageService:GetUserDataStorage(userId)
    if userDataStorage == nil then
        return
    end

    local callBack = function()
        userDataStorage:SetAsync("DataKey2", "222", nil)    -- Saving of "222" before the users re-enter is not guaranteed.
    end

    userDataStorage:SetAsync("DataKey1", "111", callBack)    -- Saving of "111" before the users re-enter is guaranteed.
}
 
Event Handler:
[server only] [service UserService]
HandleUserLeaveEvent(UserLeaveEvent event)
{
    self:SaveData()
}
```

[UserLeaveEvent](/apiReference/Events/UserLeaveEvent{"target":"_self"}) The handler has the traits below.

* This event occurs in UserService when a user leaves.
* User Entity deletion will be executed after completing the execution of UserLeaveEvent handler.
* If executing UserLeaveEvent handler takes more than a certain amount of time, user Entity deletion will be executed first.