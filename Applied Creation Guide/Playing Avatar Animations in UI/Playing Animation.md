# Course Introduction
Let's make the avatar play an attack skill when the player presses the Weapon button.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/17086569133259c569b2e82a6422db00fefac10a10c1c.gif "1")

# Create ActionNameStruct
1. In **Workspace - MyDesk - Create Scripts - Create StructType**, create a **ActionNameStruct**.
2. Add **CoreActionName, PartsActionName** properties.
    
    ```lua
    Property:
    string CoreActionName = ""
    string PartsActionName = ""
    ```
    
# Create ActionNameLogic
1. Add the new **ActionNameLogic**.
2. Add **AttackActionType_OneHand_Names, AttackActionType_TwoHand_Names, AttackActionType_StaffWand_Names, and AttackActionType_Bow_Names** properties.

    ```lua
    Property:
    [None]
    table AttackActionType_OneHand_Names = {}
    [None]
    table AttackActionType_TwoHand_Names = {}
    [None]
    table AttackActionType_StaffWand_Names = {}
    [None]
    table AttackActionType_Bow_Names = {}
    ```

3. Add `OnBeginPlay()` function and specify the required state for each weapon by writing as below.

    ```lua
    Method:
    [client only]
    void OnBeginPlay ()
    {
        self.AttackActionType_OneHand_Names = {"swingO1","swingO2","swingO3","stabO1","stabO2"}
        self.AttackActionType_TwoHand_Names = {"swingT1","swingT2","swingT3","stabO1","stabO2"}
        self.AttackActionType_StaffWand_Names = {"swingO2", "swingO1","swingO3"}
        self.AttackActionType_Bow_Names = {"swingT1", "swingT3","shoot1"}
    }
    ```

5. Create the `GetRandomButValidName()` function. Write the following to call the state randomly using `UtillLogic`.

    ```lua
    Method:
    [client only]
    string GetRandomButValidName (table randomNameTable)
    {
        local index = _UtilLogic:RandomIntegerRange(1, #randomNameTable)
        return randomNameTable[index]
    }
    ```

4. Create the `GetActionNameDatas()` function.

    ```lua
    [client only]
    ActionNameStruct GetActionNameDatas (string dataID)
    {
        local randomName =""
        
        if dataID == "sword" or dataID == "dagger"   then
        	randomName = self:GetRandomButValidName(self.AttackActionType_OneHand_Names)	
        	return ActionNameStruct(randomName, randomName)
        	
        elseif dataID == "bow" then
        	randomName = self:GetRandomButValidName(self.AttackActionType_Bow_Names)	
        	return ActionNameStruct(randomName, randomName)
        	
        elseif dataID == "staff" or dataID == "wand" then
        	randomName = self:GetRandomButValidName(self.AttackActionType_StaffWand_Names)	
        	return ActionNameStruct(randomName, randomName)
        
        elseif dataID == "hammer" then
        	randomName = self:GetRandomButValidName(self.AttackActionType_TwoHand_Names)	
        	return ActionNameStruct(randomName, randomName)
        else
        	log_warning("dataID invalid")
        	return nil
        end
    }
    ```

# Create UIWeaponSelectEvent
1. In **Workspace - MyDesk - Create Scripts - Create EventType**, create the **UIWeaponSelectEvent**.
2. Add a **EventSender** property.

    ```lua
    UIWeaponItemComponentRef EventSender = nil
    ```

# Creating the Button Feature
1. Write a new `OnSelectButton()` function in **UIAvatarPreviewComponent**. Add **weaponItemComp, dataID** parameters. Write the following to retrieve the information of SkillDatas.

    ```lua
    Method:
    [client only]
    void OnSelectButton (UIWeaponItemComponentRef weaponItemComp, string dataID)
    {
        self.Entity:SendEvent(UIWeaponSelectEvent(weaponItemComp))
        
        local row = self.SkillDatas:FindRow("dataID", dataID)
        
        if row == nil then log_error("invalid dataID") return end
        
        local id = row:GetItem("dataID")
        local weaponRUID = row:GetItem("weaponRUID")
        local is2handWeapon = row:GetItem("is2handWeapon") == "true"
    }
    ```

2. Add **ButtonClickEvent** in **UIWeaponItemComponent**. Write it to call the `OnSelectButton()` function when the button is pressed.

    ```lua
    Event Handler:
    [self]
    HandleButtonClickEvent (ButtonClickEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        local Entity = event.Entity
        ---------------------------------------------------------
        if Entity == self.Entity then
        	if self.AvatarPreviewRef ~= nil then	
        		self.AvatarPreviewRef:OnSelectButton(self, self.DataID)
        	end
        end
    }
    ```

# Playing Animation
1. Create the new **AvatarGUIRenderer** property in **UIAvatarPreviewComponent**.

    ```lua
    Property:
    [None]
    AvatarGUIRendererComponent AvatarGUIRenderer = /ui/UIGroup/UIAvatarPreview/UIAvatarGUIRenderer
    ```

2.  Add the new `SetAvatarAnimation()` function and add **dataID, weaponRUID, is2handWeapon** as parameters.
Change the state to **Attack** when the Weapon button is pressed and equip the avatar with the weapon. Write as below to play state-specific animations.

    ```lua
    Method:
    [client only]
    void SetAvatarAnimation (string dataID, string weaponRUID, boolean is2handWeapon)
    {
        --  Avatar weapon equip
        local costume = self.AvatarGUIRenderer.Entity.CostumeManagerComponent
        if costume ~=  nil then
        
        	local oneHandRUID = ""
        	local twoHandRUID = ""
        
        	if is2handWeapon then
        		twoHandRUID = weaponRUID
        	else
        		oneHandRUID = weaponRUID
        	end
        
        	costume.CustomOneHandedWeaponEquip = oneHandRUID
        	costume.CustomTwoHandedWeaponEquip = twoHandRUID
        	
        end
        
        -- Avatar animation
        local body = self.AvatarGUIRenderer:GetBodyEntity()
        local names = _ActionNameLogic:GetActionNameDatas(dataID)
        local event = ActionStateChangedEvent()
        
        event.CoreActionName = names.CoreActionName
        event.PartsActionName = names.PartsActionName
        
        event.PlayRate = 1
        event.PlayType = SpriteAnimClipPlayType.Onetime
        
        body:SendEvent(event)
    }
    ```
    
3. Write as below to call the `SetAvatarAnimation()` function at the end of **UIAvatarPreviewComponent**'s `OnSelectButton()`.

    ```lua
    self:SetAvatarAnimation(id, weaponRUID, is2handWeapon)
    ```


4. Press the **[Start]** button and turn on the UI. Confirm that the avatar equips the weapon and that a random attack animation plays when the Weapon button on the right is pressed.