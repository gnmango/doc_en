# Course Introduction
Let's make the skill effect animation play in UISkillEffectRenderer when the avatar plays the attack animation.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/170865735856078f00d0158a941a2b7690fb497ec7e16.gif{"width":"640px"}  "1")
 
# Create SkillDataStruct
1. Press **Workspace - MyDesk - Create Scripts - Create StructType** to add a new **SkillDataStruct**.
2. Add **SkillRUID, SkillOffset, SkillDelay** properties.

    ```lua
    Property:
    string SkillRUID = ""
    Vector2 SkillOffset = Vector(0,0)
    number SkillDelay = 0
    ```
    
# Create UISkillEffectComponent
1. Select **Workspace - MyDesk - Create Script - Create Component** to create a new **UISkillEffectComponent**.
2. Add the component to **UISkillEffectRenderer**.
3.  Add **RendererComponentRef, OriginalPosition, SkillDataStruct, ActivatedTimerID** properties.

    ```lua
    Property:
    [None]
    SpriteGUIRendererComponentRef RendererComponentRef = nil
    [None]
    Vector2 OriginalPosition = Vector2(0,0)
    [None]
    any SkillDataStruct = nil
    [None]
    number ActivatedTimerID = 0
    ``` 

4. Add a `PlaySkill()` function. Write as below to use `TimerService` to play the skill effect.

    ```lua
    Method:
    [client only]
    void PlaySkill ()
    {
        if self.SkillDataStruct == nil then 
        	log_error("invalid data, set valid skill first") 
        	return 
        end
        
        if self.ActivatedTimerID ~= -1 then
        	_TimerService:ClearTimer(self.ActivatedTimerID)
        	self.ActivatedTimerID = -1
        end
    
        _TimerService:SetTimerOnce(function() 

		self.Entity.UITransformComponent.anchoredPosition = self.OriginalPosition + self.SkillDataStruct.SkillOffset     		
		self.RendererComponentRef.Enable = true
		self.RendererComponentRef.ImageRUID = self.SkillDataStruct.SkillRUID
		
		wait(0.1) 
		
		self.RendererComponentRef.LocalScale = Vector2.one * 3 

		self.ActivatedTimerID = _TimerService:SetTimerOnce(function () 
				self.RendererComponentRef.ImageRUID = nil
				self.RendererComponentRef.Enable = false 
			end, 1)
			
	    end, self.SkillDataStruct.SkillDelay)
    }
    ```


5. Add a `OnBeginPlay()` function. Write the following to determine whether there is **SpriteGUIRendererComponent** or not and to set **OriginalPosition**.

    ```lua
    [client only]
    void OnBeginPlay ()
    {
        if self.Entity.SpriteGUIRendererComponent == nil then 
        	log_error("skill dosen't have SpriteGUIRendererComponent")
        end
        
        self.RendererComponentRef = self.Entity.SpriteGUIRendererComponent
        
        if self.RendererComponentRef ~= nil then
        	self.RendererComponentRef.Enable = false
        end
        
        self.OriginalPosition = self.Entity.UITransformComponent.anchoredPosition
    }
    ```

# Calling a Skill
1. Add `SetSkill()` function to **UISkillEffectComponent**.

    ```lua
    [client only]
    void SetSkill (SkillDataStruct skillData)
    {
        self.SkillDataStruct = skillData
    }
    ```

2. Add **SkillEffect** property to **UIAvatarPreviewComponent**.
    ```lua
    Property:
    [None]
    UISkillEffectComponentRef SkillEffect = /ui/UIGroup/UIAvatarPreview/UISkillEffectRenderer
    ```

3. Write the following to call `SetSkill()`, `PlaySkill()` functions above `OnSelectButton()` function's `self:SetAvatarAnimation(id, weaponRUID, is2handWeapon)` in **UIAvatarPreviewComponent**.

    ```lua
    local skillData = SkillDataStruct()
    skillData.SkillRUID = row:GetItem("skillRUID")
    skillData.SkillOffset = Vector2(
        tonumber(row:GetItem("skillOffsetX")),
        tonumber(row:GetItem("skillOffsetY"))
    )
    skillData.SkillDelay = row:GetItem("skillDelay")
    
    self.SkillEffect:SetSkill(skillData)
    self.SkillEffect:PlaySkill()
    
    self:SetAvatarAnimation(id, weaponRUID, is2handWeapon)
    ```
    
3. Press the **[Start]** button to confirm the skill plays.