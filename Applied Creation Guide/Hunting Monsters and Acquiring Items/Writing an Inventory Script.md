# Course Introduction
> <span style="color: #585858">This guide follows the order below.
> Part 1. [Let's Spawn Some Monsters](/docs?postId=204{"target":"_self"})
> Part 2. [Creating Monster Drop Items and Inventory UI](/docs?postId=943{"target":"_self"})
> **▶ Part 3.** [**Writing an Inventory Script**](/docs?postId=942{"target":"_self"})
> Part 4. [Writing an Inventory UI Script](/docs?postId=946{"target":"_self"})
> Part 5. [Writing an Item Script](/docs?postId=947{"target":"_self"})</span>

Continuing from last time, in this lesson we will write an inventory script.
In the inventory script, we will control the operation of the inventory, deliver the changed item information to the inventory UI, and save the item information to the DB.

# Inventory Script
Write an inventory script to control the behavior of the inventory.

1. Create a new script component and name it <span style="color: #dc9656">**LiteInventory**</span>.
<br>
2. In the context menu of **Workspace - DefaultPlayer**, click **Add Component** to add the **LiteInventory** component.
<br>
3. Open the **LiteInventory** script and add the property in the Script Editor as follows.
    ```lua
    Property:
    [Sync]
    boolean IsInitialized = false
    ```
    <br>
4. Add the `OnBeginPlay()` function and set the execution space to **server only**.
First, define that you want to import and use **UserItemDataSet** in **inventoryData**. 
Then call the data saved in **userDataStorage** with the key **ItemData**. **nil** is returned if no data has been saved yet.
    ```lua
    [server only]
    void OnBeginPlay()
    {
        -- Load UserItemDataSet into inventoryData
        self._T.inventoryData = {}
        for i = 1, _DataService:GetRowCount("UserItemDataSet") do
        	self._T.inventoryData[i] = 0
        end
    
        local userDataStorage = _DataStorageService:GetUserDataStorage(self.Entity.PlayerComponent.UserId)
        local errorCode, itemData = userDataStorage:GetAndWait("ItemData")
        
        -- Output error code if data load fails
        if errorCode ~= 0 then
        	log(errorCode)
        	return
        end
            
        -- If there is loaded data, apply it
        if itemData ~= nil then
        	local itemArr = _UtilLogic:Split(itemData, ",")
        	for i, v in ipairs(itemArr) do
        		self._T.inventoryData[i] = tonumber(v)
        	end
        end
    }
    ```
    > <span style="color: #585858">**Learn More**
    > You can also use [ItemService](/apiReference/Services/ItemService{"target":"_self"}) to create, save, and manage items used in the World. However, [DataStorageService](/apiReference/Services/DataStorageService{"target":"_self"}) can be enough to implement a lightweight inventory where only the client and server of character A need to know the item information of character A in the World.
    > This example introduces how to implement inventory using `DataStorageService`.</span>
5. If there is any data loaded, we need to deliver that information to the UI as well. 
Add an event type to deliver this.
Click **MyDesk - Create Scripts - Create EventType** and set the name of the new event as <span style="color: #dc9656">**OnInventoryInitialized**</span>.
<br>
6. Open the **OnInventoryInitialized** script and add the property as follows. 
Select SyncTable\<v> as the property type and set v to **number**.
    ```lua
    Property:
    SyncTable<number> AllItemCounts
    ```
    <br>
7. Return to the **LiteInventory** script.
Now we need to deliver the loaded data to the inventory UI. Add the following to the `OnBeginPlay()` function written above. 
    ```lua
    local initEvent = OnInventoryInitialized()
    for i, v in ipairs(self._T.inventoryData) do
        initEvent.AllItemCounts[i] = v
    end
    
    self._T.userDataStorage = userDataStorage
    ```
    <br>
8. Add the `SendInitializedEvent()` function and set the execution space to **multicast**.  Click **Add Parameter** and add a parameter.
Select **EventType** as the parameter type and click **OnInventoryInitialized**. Enter **event** into **arg1**. Write as below.
    ```lua
    [multicast]
    void SendInitializedEvent (OnInventoryInitialized event)
    {
        self.Entity:SendEvent(event)
    }
    ```
    <br>
9. Let's go back to the `OnBeginPlay` function.
Continuing from the previous part, write the following.
    ```lua
    -- Existing content
    self._T.userDataStorage = userDataStorage
    
    -- Continue writing the following.
    self:SendInitializedEvent(initEvent)
    self.IsInitialized = true
    ```
    <br>
10. Let's save inventory information periodically using `_TimerService`.
Continuing with the `OnBeginPlay()` function above, write the content as below.
    * Use **ItemData** as the key. 
    * The format of the data to be saved is "a string in which the number of possessions per item index is separated by commas". 
        * For example, if you have 3 red potions of index 1 and 5 blue potions of index 2, the key **ItemData** will save a **"3, 5"** format string.
    <br>
    ```lua
    -- Set to save inventory information
    local saveInventory = function()
    	local dataConcatString = table.concat(self._T.inventoryData, ",")
    	self._T.userDataStorage:SetAsync("ItemData", dataConcatString, function() log("Saved") end)
    	log("Saving...")
    end
    
    -- Set saving period
    local saveInterval = 300
    _TimerService:SetTimerRepeat(saveInventory, saveInterval, saveInterval)
    ```
    <br>
    In this example, the saving period is set to 300 seconds. 
    If you set the saving period shorter, the risk of inventory data information loss will be reduced, but if you call `_DataStorageService` too frequently, load may occur and DB saving may be restricted in the future, so you should set it to an appropriate cycle.
    <br>
11. Add UserLeaveEvent to `Event Handler` and set the execution space to **server only**.
    We'll save the character's inventory information every time you stop playing.
    ```lua
    Event Handler:
    [server only] [service UserService]
    HandleUserLeaveEvent(UserLeaveEvent event)
    {
        -- Parameters
        local UserId = event.UserId
        ---------------------------------------------------------
     
        local thisUserId = self.Entity.PlayerComponent.UserId
     
        if thisUserId ~= UserId then
            return
        end
     
        -- Save inventory data info when play is over.
        if self._T.userDataStorage then
            local dataConcatString = table.concat(self._T.inventoryData, ",")
            self._T.userDataStorage:SetAndWait("ItemData", dataConcatString)
        end
    }
    ```
    <br>
12. As the character acquires new items, the number of items owned changes. Let's create a function to process this changed item information. 
Add the `ModifyItem()` function and set the execution space to **server only**. 
Add the **number** type of **itemIndex** and **deltaAmount** as a parameter. Write the content as below.
Passing a positive number to**deltaAmount** increases the number of items, and passing a negative number decreases it.

    ```lua
    [server only]
    void ModifyItem(number itemIndex, number deltaAmount)
    {
        -- Check if the item index is normal.
        if itemIndex < 1 or itemIndex > #self._T.inventoryData then
        	log_error("itemIndex error")
        	return
        end
        
        -- Check the number of items before the change.
        local prevAmount = self._T.inventoryData[itemIndex]
        -- Calculate the changed value
        local amountResult = prevAmount + deltaAmount
        
        -- Check if amountResult is normal. If it is less than 0, it is abnormal, so change it to 0.
        if amountResult < 0 then
        	log_error("Item amount is less then zero")
        	amountResult = 0
        end
        
        --If amountResult is OK, change inventoryData amountResult
        self._T.inventoryData[itemIndex] = amountResult
    }
    ```
    <br>

13. Adds an event type that delivers changed item information. 
Click **MyDesk - Create Scripts - Create EventType** and set the name of a new event type as <span style="color: #dc9656">**OnInventoryModified**</span>.
    <br>
14. Open **OnInventoryModified** and add the property in the Script Editor.
    ```lua
    Property: 
    number ItemIndex = 0
    number ItemCount = 0
    ```
    <br> 
15. Return to the **LiteInventory** script. 
Add the `SendModifiedEvent()` function and set the execution space to **multicast**. 
Then, after adding the parameters as shown below, call the function to deliver the item change information to the client as well.
    ```lua
    [multicast]
    void SendModifiedEvent (OnInventoryModified event)
    {
        self.Entity:SendEvent(event)
    }
    ```
    <br>
16. Return to the `ModifyItem()` function. 
Continuing from the previous part, write the following.
    ```lua
    -- Existing content
    self._T.inventoryData[itemIndex] = amountResult
    
    -- Continue writing the following.
    local modifiedEvent = OnInventoryModified()
    modifiedEvent.ItemIndex = itemIndex
    modifiedEvent.ItemCount = amountResult
    -- Deliver SendModifiedEvent to UI
    self:SendModifiedEvent(modifiedEvent)
    ```
 
# Empty Inventory
For testing purposes, let's set the character's inventory to be cleared each time they connect to the game.

1. Add the `RemoveItem()` function to **LiteInventory** to process item deletion.
Set the execution space to **server only**.
Add the **number** type of **itemIndex** as a parameter.
Then write the following.
    ```lua
    [server only]
    void RemoveItem(number itemIndex)
    {
        if itemIndex < 1 or itemIndex > #self._T.inventoryData then
        	log_error("itemIndex error")
        	return
        end
        
        self._T.inventoryData[itemIndex] = 0
        
        local modifiedEvent = OnInventoryModified()
        modifiedEvent.ItemIndex = itemIndex
        modifiedEvent.ItemCount = 0
        self:SendModifiedEvent(modifiedEvent)
    }
    ```
    <br>
2. Create a new script component and name it <span style="color: #dc9656">**ClearInventoryOnBeginPlay**</span>.
    <br>
3. Add the **ClearInventoryOnBeginPlay** component to **DefaultPlayer**.
    <br>
4. Open **ClearInventoryOnBeginPlay** and add the `OnBeginPlay()` function. 
Set the execution space to **server only**.
Then write the content as below. 
    ```lua
    [server only]
    void OnBeginPlay()
    {
        if self.Enable == false then
        	return
        end
        
        local inven = self.Entity.LiteInventory
        
        while not inven.IsInitialized do
        	wait(0.5)
        end
        
        for i = 1, _DataService:GetTable("UserItemDataSet"):GetRowCount() do
        	inven:RemoveItem(i)
        end
    }
    ```
    <br>
5. In **DefaultPlayer**, if you Enable **ClearInventoryOnBeginPlay** as **true** during the test, it will empty your character's inventory each time you start a new game. 
If you Enable **ClearInventoryOnBeginPlay** as **false**, it will not clear the inventory even if you start a new game, so you can see item information previously saved in the DB in the inventory.

# Full Script
Let's check the script we've written so far.
## LiteInventory Component
```lua
Property:
[Sync]
boolean IsInitialized = false

Function:
[server only]
void OnBeginPlay()
{
    -- Load UserItemDataSet into inventoryData.
    self._T.inventoryData = {}
    for i = 1, _DataService:GetRowCount("UserItemDataSet") do
    	self._T.inventoryData[i] = 0
    end

    local userDataStorage = _DataStorageService:GetUserDataStorage(self.Entity.PlayerComponent.UserId)
    local errorCode, itemData = userDataStorage:GetAndWait("ItemData")
    
    if errorCode ~= 0 then
    	log(errorCode)
    	return
    end
    
    if itemData ~= nil then
    	local itemArr = _UtilLogic:Split(itemData, ",")
    	for i, v in ipairs(itemArr) do
    		self._T.inventoryData[i] = tonumber(v)
    	end
    end
    
    local initEvent = OnInventoryInitialized()
    for i, v in ipairs(self._T.inventoryData) do
		initEvent.AllItemCounts[i] = v
    end

    self._T.userDataStorage = userDataStorage
    self:SendInitializedEvent(initEvent)
    self.IsInitialized = true
    
    local saveInventory = function()
    	local dataConcatString = table.concat(self._T.inventoryData, ",")
    	self._T.userDataStorage:SetAsync("ItemData", dataConcatString, function() log("Saved") end)
    	log("Saving...")
    end
    
    local saveInterval = 300
    _TimerService:SetTimerRepeat(saveInventory, saveInterval, saveInterval)
}

[multicast]
void SendInitializedEvent(OnInventoryInitialized event)
{
    self.Entity:SendEvent(event)
}
  
[server only]
void ModifyItem(number itemIndex, number deltaAmount)
{
    if itemIndex < 1 or itemIndex > #self._T.inventoryData then
    	log_error("itemIndex error")
    	return
    end
    
    local prevAmount = self._T.inventoryData[itemIndex]
    local amountResult = prevAmount + deltaAmount
    
    if amountResult < 0 then
    	log_error("Item amount is less then zero")
    	amountResult = 0
    end
    
    self._T.inventoryData[itemIndex] = amountResult
    
    local modifiedEvent = OnInventoryModified()
    modifiedEvent.ItemIndex = itemIndex
    modifiedEvent.ItemCount = amountResult
    self:SendModifiedEvent(modifiedEvent)
}

[multicast]
void SendModifiedEvent(OnInventoryModified event)
{
    self.Entity:SendEvent(event)
}

[server only]
void RemoveItem(number itemIndex)
{
    if itemIndex < 1 or itemIndex > #self._T.inventoryData then
    	log_error("itemIndex error")
    	return
    end
    
    self._T.inventoryData[itemIndex] = 0
    
    local modifiedEvent = OnInventoryModified()
    modifiedEvent.ItemIndex = itemIndex
    modifiedEvent.ItemCount = 0
    self:SendModifiedEvent(modifiedEvent)
}
    
Event Handler:
[server only] [service UserService]
HandleUserLeaveEvent (UserLeaveEvent event)
{
 -- Parameters
local UserId = event.UserId
---------------------------------------------------------

local thisUserId = self.Entity.PlayerComponent.UserId

if thisUserId ~= UserId then
    return
end

-- Save inventory data info when play is over.
if self._T.userDataStorage then
    local dataConcatString = table.concat(self._T.inventoryData, ",")
    self._T.userDataStorage:SetAndWait("ItemData", dataConcatString)
end
}
```

## OnInventoryInitialized Event
```lua
Property:
SyncTable<number> AllItemCounts
```

## OnInventoryModified Event
```lua
Property : 
number ItemIndex = 0
number ItemCount = 0
```