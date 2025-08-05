# Course Introduction
> <span style="color: #585858">This guide follows the order below.
> Part 1. [Let's Spawn Some Monsters](/docs?postId=204{"target":"_self"})
> Part 2. [Creating Monster Drop Items and Inventory UI](/docs?postId=943{"target":"_self"})
> Part 3. [Writing an Inventory Script](/docs?postId=942{"target":"_self"})
> Part 4. [Writing an Inventory UI Script](/docs?postId=946{"target":"_self"})
> **▶ Part 5.** [**Writing an Item Script**](/docs?postId=947{"target":"_self"})</span>

Continuing from last time, in this lesson we'll write a script related to the items that the monsters will drop.

# Item Script
The item script will process the following:
* Set the information of items that monsters will drop
* When a character touches a dropped item, it puts the item in the character's inventory
* Creates the appearance of items dropping visually and naturally

<br>
Let's proceed as follows.
<br>
1. In the **Preset List**, select the desired item. It doesn't matter which item you choose. 
In the context menu of item, click **Add to Workspace**. 
Change the name to <span style="color: #dc9656">**Model_Item**</span>.
    <br>
2. Create a new script component with the name of <span style="color: #dc9656">**Item**</span>. 
Then, add the **Item** component to the **Model_Item** added above.
    <br>
3. Open the **Item** script and add the **number** type of **ItemIndex** to **Property**.
    ```lua
    Property:
    [Sync]
    number ItemIndex = 0
    ```
    <br>
4. Add the `InitItem()` function to set the dropped item's information. 
Add the **number** type of **itemIndex** as a parameter. After that, write the content as below.
    ```lua
    void InitItem(number itemIndex)
    {
        self.ItemIndex = itemIndex
        
        -- Set the sprites of what the dropped item will look like
        local sprite = self.Entity.SpriteRendererComponent
        local itemDataSet = _DataService:GetTable("UserItemDataSet")
        local ruid = itemDataSet:GetRow(self.ItemIndex):GetItem("IconRUID")
        sprite.SpriteRUID = ruid
    }
    ```
    <br>
5. In the Property Editor of **Model_Item**, add **TriggerComponent**.
    <br>
6. Add **TriggerEnterEvent** to the event handler of the **Item** script, and set the execution space to **server only**. Take the inventory of the entity (character) that collided with the item entity. Return if there's no inventory.
After calling the **LiteInventory** script - `ModifyItem()` function to process changes in the number of items, destroy the collided item entity so that it is no longer visible.

    ```lua
    [server only] [self]
    HandleTriggerEnterEvent(TriggerEnterEvent event)
    {
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        --------------------------------------------------------
        --Takes inventory of the entity that collided with the item entity. Returns if there's no inventory.
        local inventory = TriggerBodyEntity.LiteInventory
        if inventory == nil then return end
        
        -- Call the ModifyItem() function to process the change in the number of items
        inventory:ModifyItem(self.ItemIndex, 1)
        
        --Destroy the collided item entity
        self.Entity:Destroy()
    }
    ```

# Monster Item Drops
This will make monsters drop items when they die.

1. Open the **Monster** script.
    <br>
2. Continuing with the current content of the `Dead()` function, add the following content.
At this time, copy and paste the **Entry ID** of **Model_Item** in **Entry ID**. In the context menu of **Model_Item**, click **Copy Entry ID**, and paste it to **Entry ID**.

    ```lua
    -- Process monsters to randomly drop items set in UserItemDataSet.
    local itemCount = _DataService:GetTable("UserItemDataSet"):GetRowCount()
    
    local index = _UtilLogic:RandomIntegerRange(1,itemCount)
    -- Copy Entry ID of Model_Item and paste it to "Entry ID"
    local itemEntity = _SpawnService:SpawnByModelId("Entry ID", "Item", self.Entity.TransformComponent.WorldPosition, self.Entity.Parent)
    itemEntity.Item:InitItem(index)
    ```
    <br>
 
# Create Item Drop
1. Add **TweenFloatingComponent** to **Model_Item**. Each property setting is as follows.
    ![tween](https://mod-file.dn.nexoncdn.co.kr/bbs/167211880171701aca471f6134260a22ab98b6c4a145c.png "tween")
    <br>
2. Open the **Item** script and add the **number** type of the new **DropTweenTime** property.
    ```lua
    Property:
    [Sync]
    number ItemIndex = 0
    
    -- Add
    [Sync]
    number DropTweenTime = 0.8
    ```
    <br>
3. Add the `OnBeginPlay()` function and select the execution space as **Not used**. 
Call `OnBeginPlayClient()` and `OnBeginPlayServer()` to process necessary work separately in the client and the server.
    ```lua
    void OnBeginPlay()
    {
        if self:IsClient() then
          	self:OnBeginPlayClient()
        else
        	self:OnBeginPlayServer()
        end
    }
    ```
    <br>
4. Add the `OnBeginPlayClient()` function and enter its content as follows. 
When an item drops, set it so that it floats up once and then comes down.
    ```lua
    void OnBeginPlayClient()
    {
        -- Tween
        local totalTweenTime = self.DropTweenTime
        local jumpHeight = 1.1
        -- When an item is dropped, it floats up as much as jumpHeight once
        local tween = _TweenLogic:MoveOffset(self.Entity, Vector2(0, jumpHeight), totalTweenTime / 2, EaseType.QuadEaseOut)
        -- Let it go down as much as it goes up
        tween:SetOnEndCallback(function() 
        	local tween2 = _TweenLogic:MoveOffset(self.Entity, Vector2(0, -jumpHeight), totalTweenTime / 2, EaseType.QuadEaseIn) 
        	tween2:SetOnEndCallback(function() self.Entity.TweenFloatingComponent.Enable = true end)
        end)
    }
    ```
    <br>
5. Add the `OnBeginPlayServer()` function and compose its content as follows. 
As long as the item is floating and falling, make sure that it does not enter the inventory even if it touches the character. When the effect is done, activate TriggerComponent so that it goes into your inventory when it touches your character.
    ```lua
    void OnBeginPlayServer()
    {
        -- Enable TriggerComponent after Tween finishes
        _TimerService:SetTimerOnce(function() self.Entity.TriggerComponent.Enable = true end, self.DropTweenTime + 0.1) 
    }
    ```
    <br>
6. Let's press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test. 
Make sure the sequence of spawning monsters, dropping items on kill, acquiring items, and saving them in your inventory proceeds as planned.
![game](https://mod-file.dn.nexoncdn.co.kr/bbs/1672128051320c8efeb0f27e4436bb7cc30156f965407.gif "game")

# Full Script
Let's review the script we've written so far.

## Item Script
```lua
Property:
[Sync]
number ItemIndex = 0
[Sync]
number DropTweenTime = 0.8

Method:
void InitItem(number itemIndex)
{
    self.ItemIndex = itemIndex

    local sprite = self.Entity.SpriteRendererComponent
    local itemDataSet = _DataService:GetTable("UserItemDataSet")
    local ruid = itemDataSet:GetRow(self.ItemIndex):GetItem("IconRUID")
    sprite.SpriteRUID = ruid
}

void OnBeginPlay()
{
    if self:IsClient() then
      	self:OnBeginPlayClient()
    else
    	self:OnBeginPlayServer()
    end
}

void OnBeginPlayClient()
{
    local totalTweenTime = self.DropTweenTime
    local jumpHeight = 1.1
    local tween = _TweenLogic:MoveOffset(self.Entity, Vector2(0, jumpHeight), totalTweenTime / 2, EaseType.QuadEaseOut)
    tween:SetOnEndCallback(function() 
    	local tween2 = _TweenLogic:MoveOffset(self.Entity, Vector2(0, -jumpHeight), totalTweenTime / 2, EaseType.QuadEaseIn) 
    	tween2:SetOnEndCallback(function() self.Entity.TweenFloatingComponent.Enable = true end)
    end)
}

void OnBeginPlayServer()
{
    _TimerService:SetTimerOnce(function() self.Entity.TriggerComponent.Enable = true end, self.DropTweenTime + 0.1) 
}

Event Handler:
[server only] [self]
HandleTriggerEnterEvent(TriggerEnterEvent event)
{
    -- Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    --------------------------------------------------------
    local inventory = TriggerBodyEntity.LiteInventory
    if inventory == nil then return end

    inventory:ModifyItem(self.ItemIndex, 1)

    self.Entity:Destroy()
}
```