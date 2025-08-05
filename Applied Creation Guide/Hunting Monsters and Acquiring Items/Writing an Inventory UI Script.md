# Course Introduction
> <span style="color: #585858">This guide follows the order below.
> Part 1. [Let's Spawn Some Monsters]
> Part 2. [Creating Monster Drop Items and Inventory UI]
> Part 3. [Writing an Inventory Script]
> **▶ Part 4.** [**Writing an Inventory UI Script**]
> Part 5. [Writing an Item Script]</span>

Continuing from last time, in this lesson we will write an inventory UI script.

# Modify UIInventory
If you finished [Creating Monster Drop Items and Inventory UI], you can see the **UIInventory** script in the **Workspace**. Let's modify the **UIInventory** script so that when an item is acquired, a slot is created for the item to be placed in.

1. Open the **UIInventory** script.
    <br>
2. Let's create as many item slots as there are item types in advance. Add the following content in the `OnBeginPlay()` function.

    ```lua
    -- Create as many item slots as item types in advance.
    -- Manage slot list in inventory UI with slotList.
    self._T.slotList = {}
     
    for i = 1, _DataService:GetRowCount("UserItemDataSet") do
        self._T.slotList[i] = self:AddSlot(i, 0)
    end
    ```

3. Add the `AddSlot()` function. 
Set the return type as **Entity** and add the **number** type parameter of **itemIndex** and **itemCount**. 
If you write like this, the error will be displayed as a red underline in the `SetData()` function. This is because we have not yet modified the `SetData()` function of the **UIItemSlot** script as intended. We'll modify it in a later step, so leave it as it is for now and proceed to the next step.
    ```lua
    Entity AddSlot(number itemIndex, number itemCount)
    {
        local itemSlot =_SpawnService:SpawnByEntity(self.itemUI, "Item", Vector3.zero, self.scrollView)
        itemSlot.UIItemSlot:SetData(itemIndex, itemCount)
        return itemSlot
    }
    ```

4. Delete all the events below in the event handler.
    * InventoryItemInitEvent
    * InventoryItemAddedEvent
    * InventoryItemRemovedEvent
    * InventoryItemModifiedEvent
    <br>
5. In the previous guide [Writing an Inventory Script], you wrote the **LiteInventory** script. In this script, when a character connects to the game, if there is item information saved in the DB, it is set to be passed to the inventory UI through the `OnInventoryInitialized` event. 
<br>
In the **UIInventory** script, we will add the `OnInventoryInitialized` event to the event handler to receive this information. Set the event broadcaster at the top of the handler to **localplayer**. 
To show the items you have in the UI, write as below.
    ```lua
    [localplayer]
    HandleOnInventoryInitialized(OnInventoryInitialized event)
    {
        -- Parameters
        local AllItemCounts = event.AllItemCounts
        ---------------------------------------------------------
     
        for i, itemCount in pairs(AllItemCounts) do
            self._T.slotList[i].UIItemSlot:SetData(i, itemCount)
        end
    }
    ```
    <br>
6. Also, if the character picks up an item that drops after defeating a monster, the number of items owned will change. **UIInventory** should also receive the changed information. To do this, add an `OnInventoryModified` event to the event handler and set the event broadcaster at the top of the handler to **localplayer**. 

    ```lua
    [localplayer]
    HandleOnInventoryModified(OnInventoryModified event)
    {
        -- Parameters
        local ItemIndex = event.ItemIndex
        local ItemCount = event.ItemCount
        ---------------------------------------------------------
     
        -- Check if the slot list is normal.
        if self._T.slotList == nil then
            return
        end
     
        local slot = self._T.slotList[ItemIndex]
     
        if slot == nil then
            self._T.slotList[ItemIndex] = self:AddSlot(ItemIndex, ItemCount)
        end
     
        -- Change the number of items that appear in the slot.
        slot.UIItemSlot:SetCount(ItemCount)
    }
    ```
     <br>

# Modify UIItemSlot
We will modify the **UIItemSlot** script to show items in individual slots based on the character's item holding information.

1. Open the **UIItemSlot** script and remove all existing properties. Then add the **number** type of the **ItemIndex** and **ItemCount** property as follows.
    ```lua
    Property:
    [None]
    number ItemIndex = 0
    [None]
    number ItemCount = 0
    ```
    <br>
2. Modify the `SetData()` function. 
Delete existing parameters and add the **number** type of **itemIndex** and **itemCount**. After deleting all the content of the existing function, write the following content.
Acquiring an item activates the slot and shows the item and its number. Losing an item deactivates the slot and makes the item invisible.
    ```lua
    void SetData(number itemIndex, number itemCount)
    {
        -- Set the item information in the slot.
     
        self.ItemIndex = itemIndex
        self.ItemCount = itemCount
     
        local imageEntity = self.Entity:GetChildByName("img_slot")
        local iconRUID = _DataService:GetTable("UserItemDataSet"):GetCell(itemIndex, "IconRUID")
        imageEntity.SpriteGUIRendererComponent.ImageRUID = iconRUID
     
        imageEntity:GetChildByName("item_count").TextComponent.Text = tostring(math.floor(itemCount))
     
        -- If the number of items is greater than 0, it shows the slot; otherwise it hides it.
        if itemCount > 0 then
            self.Entity:SetEnable(true)
        else
            self.Entity:SetEnable(false)
        end
    }
    ```
    <br>
3. Add the `SetCount()` function. Add the **number** type of **count** as a parameter.
    ```lua
    void SetCount(number count)
    {
        -- Change Item Quantity
        self.ItemCount = count
     
        local textEntity = self.Entity:GetChildByName("item_count", true)
        textEntity.TextComponent.Text = tostring(math.floor(count))
     
        -- If the number of items is greater than 0, it shows the slot; otherwise it hides it.
        if count > 0 then
            self.Entity:SetEnable(true)
        else
            self.Entity:SetEnable(false)
        end
    }
    ```
    <br>
4. Modify **ButtonClickEvent** of the event handler as shown below.
    ```lua
    [self]
    HandleButtonClickEvent(ButtonClickEvent event)
    {
        -- Parameters
        local Entity = event.Entity
        --------------------------------------------------------
        
        if self.ItemIndex == 0 then
        	return
        end
        
        -- TODO: item logic
    }
    ```

# Full Script
Let's check the script we've written so far.

## UIInventory Script
```lua
Property:
[None]
Entity itemUI = nil
[None]
Entity scrollView = nil
[None]
SyncTable<string, Entity> SlotItems
[None]
string inventoryBtnPath = "/ui/DefaultGroup/RPGButtons/btn_inventory"
    
Method:
[client only]
void OnBeginPlay
{
    self.itemUI =_EntityService:GetEntityByPath(self.Entity.Path .. "/InventoryPanel/Inventory_ScrollView/item_slot")
    self.itemUI:SetEnable(false)
    self.scrollView = _EntityService:GetEntityByPath(self.Entity.Path .. "/InventoryPanel/Inventory_ScrollView")
    
    local inventoryButton = _EntityService:GetEntityByPath(self.inventoryBtnPath)
    if inventoryButton ~= nil then
    	local openFunc = function()
    		self.Entity.Enable = true
    	end
    	if inventoryButton.ButtonComponent == nil then
    		inventoryButton:AddComponent("MOD.Core.ButtonComponent")
    	end
    	inventoryButton:ConnectEvent(ButtonClickEvent, openFunc)
    end
    
    
    local closeButton = _EntityService:GetEntityByPath(self.Entity.Path .. "/InventoryPanel/CloseButton")
    local closeFunc = function()
    	self.Entity.Enable = false
    end
    
    closeButton:ConnectEvent(ButtonClickEvent, closeFunc)
    
    -- Create as many item slots as item types in advance.
    -- Manage slot list in inventory UI with slotList.
    self._T.slotList = {}
     
    for i = 1, _DataService:GetRowCount("UserItemDataSet") do
        self._T.slotList[i] = self:AddSlot(i, 0)
    end
}

Entity AddSlot(number itemIndex, number itemCount)
{
    local itemSlot =_SpawnService:SpawnByEntity(self.itemUI, "Item", Vector3.zero, self.scrollView)
    itemSlot.UIItemSlot:SetData(itemIndex, itemCount)
    return itemSlot
}

Event Handler:
[localplayer]
HandleOnInventoryInitialized(OnInventoryInitialized event)
{
    -- Parameters
    local AllItemCounts = event.AllItemCounts
    ---------------------------------------------------------
 
    for i, itemCount in pairs(AllItemCounts) do
        self._T.slotList[i].UIItemSlot:SetData(i, itemCount)
    end
}

[localplayer]
HandleOnInventoryModified(OnInventoryModified event)
{
    -- Parameters
    local ItemIndex = event.ItemIndex
    local ItemCount = event.ItemCount
    ---------------------------------------------------------
 
    -- Check if the slot list is normal.
    if self._T.slotList == nil then
        return
    end
 
    local slot = self._T.slotList[ItemIndex]
 
    if slot == nil then
        self._T.slotList[ItemIndex] = self:AddSlot(ItemIndex, ItemCount)
    end
 
    -- Change the number of items that appear in the slot.
    slot.UIItemSlot:SetCount(ItemCount)
}
```

## UIItemSlot Script
```lua
Property:
[None]
number ItemIndex = 0
[None]
number ItemCount = 0

Method:
void SetData(number itemIndex, number itemCount)
{
    -- Set the item information in the slot.
 
    self.ItemIndex = itemIndex
    self.ItemCount = itemCount
 
    local imageEntity = self.Entity:GetChildByName("img_slot")
    local iconRUID = _DataService:GetTable("UserItemDataSet"):GetCell(itemIndex, "IconRUID")
    imageEntity.SpriteGUIRendererComponent.ImageRUID = iconRUID
 
    imageEntity:GetChildByName("item_count").TextComponent.Text = tostring(math.floor(itemCount))
 
    -- If the number of items is greater than 0, it shows the slot; otherwise it hides it.
    if itemCount > 0 then
        self.Entity:SetEnable(true)
    else
        self.Entity:SetEnable(false)
    end
}
    
void SetCount(number count)
{
    -- Change Item Quantity
    self.ItemCount = count
 
    local textEntity = self.Entity:GetChildByName("item_count", true)
    textEntity.TextComponent.Text = tostring(math.floor(count))
 
    -- If the number of items is greater than 0, it shows the slot; otherwise it hides it.
    if count > 0 then
        self.Entity:SetEnable(true)
    else
        self.Entity:SetEnable(false)
    end
}
    
Event Handler:
[self]
HandleButtonClickEvent(ButtonClickEvent event)
{
    -- Parameters
    local Entity = event.Entity
    --------------------------------------------------------
    if self.ItemIndex == 0 then
        return
    end
    
    -- TODO: item logic
}
```