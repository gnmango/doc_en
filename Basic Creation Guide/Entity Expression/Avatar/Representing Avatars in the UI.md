# Course Introduction
You can use **AvatarGUIRendererComponent** to display character information in the UI. Let's learn how to use **AvatarGUIRendererComponent** through this course.
##### Reference Guide
[Controlling Avatar Animations](docs/?postId=820{"target":"_self"})
##### API Reference
[AvatarGUIRendererComponent](/apiReference/Components/AvatarGUIRendererComponent{"target":"_self"})

#### AvatarGUIRendererComponent Property
Let's look at the main properties of **AvatarGUIRendererComponent**.

| Property | Description |
| --- | --- |
| Color | Adjusts the color. |
| FlipX | Determines whether to invert based on the X axis. |
| FlipY | Determines whether to invert based on the Y axis. |
| PlayRate | You can specify the playback speed of avatar animations. <br> It supports a value of 0 or more. The higher the number, the faster it is. |
| PreserveAvatar | <ul> <li>None: Resize freely regardless of the original proportions.</li> <li>AspectOnly: Resize while maintaining original proportions.</li> <li>NativeSize: Maintain original size.</li></ul> |
| RaycastTarget | If it is set to true, it becomes the target of screen touches or mouse clicks. The UI hidden behind AvatarGUIRendererComponent can't receive the input of screen touches and mouse clicks. |

# Example
Let's adjust the properties of **AvatarGUIRendererComponent** in the script or use functions to change the appearance of the avatar shown in the UI.
#### Show Avatar in UI
Let's take a quick look at how to display an avatar in the UI.

1. After opening the UI editor, press the **[Image]** button to add an entity. In the example, we'll add it to **DefaultGroup**.
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16734983895969a0457ac51754fd2923c7d97e8360a34.png "1")
    
2. Rename the image entity you added to <span style="color: #dc9656">**MyInfoWindow**</span> and enter the following to **ImageRUID**. Then, enter the proper value in **RectSize**.
    * **ImageRUID** : <span style="color: #dc9656">**8122dd6f67f3d9b4db8a3152172f9063**</span>
    * **RectSize** : <span style="color: #dc9656">**X = 350, Y : 460**</span> 
    
3. Open the context menu in **Hierarchy**, click **Create Entity - Create UIEmpty** to create **UIEmpty**.
    
4. Rename **UIEmpty** as <span style="color: #dc9656">**MyInfo**</span> and add **CostumeManagerComponent** and **AvatarGUIRendererComponent** in the property editor. 
    ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16747841322955b99b98e5ecc4c569cf74afef544d15b.png "4")
    
5. Enter the proper value in **RectSize**. Then position it at an appropriate place above **MyInfoWindow**.
    * **RectSize** : <span style="color: #dc9656">**X = 270, Y : 400**</span> 
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16747852539050f91bb139c244f2190199e8436edaf5c.png "3")
    
6. Move the **MyInfo** position to **MyInfoWindow** in **Hierarchy**.
    ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16734988019776451e912fdc64b22947bb1f22db571c0.png "2")

#### Changing UI Avatar Clothes
Let's take a look at an example where the character's clothes change when they hit a specific object, and the avatar's clothes visible in the UI also change.

1. After placing any object in **Preset List**, enter the following values in **SpriteRUID**. Change the object' name to <span style="color: #dc9656">**Pinkbean**</span>.
    * **SpriteRUID**: <span style="color: #dc9656">**5a81ed24c1354d08b38f8db4b497b607**</span>
    ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1673503382686e463830b58984fe0a02931f0950220a3.png "5")
    
2. In the context menu of **BaseEnvironment - NativeScripts - Component - TriggerComponent**, click **Extend**.
    
3. Rename the script component you extended as <span style="color: #dc9656">**ChangeUIAvatar**</span>.
    
4. Add the **ChangeUIAvatar** component to the **Pinkbean** object placed above.
    
5. Open the **ChangeUIAvatar** script and add the property as follows.
    ```lua
    Property:
    [None]
    Entity myInfo = /ui/DefaultGroup/MyInfoWindow/MyInfo
    [None]
    string changeCap = "1e6e49a6ecf94cc6872926598dcf179c"
    [None]
    string changeLongcoat = "22ab95fb9c3c4539b04d6a540f464e79"
    [None]
    string changeOHWeapon = "bbe63d40682c4f8ebcdbc22e7d92a5eb"
    ``` 
   
6. Add the `OnEnterTriggerBody()` function and compose its content as follows in **Overridable Function**.
    ```lua
    override void OnEnterTriggerBody(TriggerEnterEvent enterEvent)
    {
        __base:OnEnterTriggerBody(enterEvent)
        
        -- Get a target (player) that collided with the object.
        local player = enterEvent.TriggerBodyEntity
         
        -- If the collided target with the object doesn't have AvatarRendererComponent, clothes do not change.
        local costumeManager = player.CostumeManagerComponent
        if costumeManager == nil then
            return
        end
        
        -- Change my character equipment.
        costumeManager.CustomCapEquip = self.changeCap
        costumeManager.CustomLongcoatEquip = self.changeLongcoat
        costumeManager.CustomOneHandedWeaponEquip = self.changeOHWeapon
        
        if self:IsServer() then 
            return 
        end
        
        -- Change UI avatar equipment.
        local myInfo = self.myInfo
        local myInfoCostumeManager = myInfo.CostumeManagerComponent
        
        myInfoCostumeManager.CustomCapEquip = self.changeCap
        myInfoCostumeManager.CustomLongcoatEquip = self.changeLongcoat
        myInfoCostumeManager.CustomOneHandedWeaponEquip = self.changeOHWeapon
    }
    ```
    
7. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. 
    Confirm that when the character touches the Pinkbean object, both the character's costume and the UI avatar's costume change.
    ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16735057387005fe75284eb82440fa0f179c088110d32.gif "5")


#### Change Avatar Colors
Let's change the **Color** property of **AvatarGUIRendererComponent** with script. Let's look at an example where the color value of the avatar shown in the UI changes randomly when pressing a button.

1. Add a button in the UI editor and enter it as <span style="color: #dc9656">**SetColorBtn**</span>.
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1673836836089d930239e5f034b8785e475aa09296d7c.png "7")
    
2. Add **TextComponent** to **SetColorBtn** and set the properties as follows.
    | Component | Property | Value |
    | :---: | :---: | :---: |
    | @cols=1:@rows=2:TextComponent | Text | SetColor |
    | FontColor | #FFFFFF |  
    
3. Add `SetColor()` function to the **ChangeUIAvatar** script component and write as follows. Set the execution space to **client only**.
    ```lua
    [client Only]
    void SetColor()
    {
        local r = _UtilLogic:RandomDouble()
        local g = _UtilLogic:RandomDouble()
        local b = _UtilLogic:RandomDouble()
        local a = _UtilLogic:RandomDouble()
        self.myInfo.AvatarGUIRendererComponent.Color = Color(r,g,b,a)
    }
    ```
    
4. Create a new script component <span style="color: #dc9656">**TestUIBtn**</span>. 
    Then add the **TestUIBtn** component to **SetColorBtn**.
    
5. Open the **TestUIBtn** script and add the property as follows.
    ```lua
    Property:
    [None]
    ChangeUIAvatar changeUIAvatar = /maps/map01/Pinkbean
    ```
    > **<span style="color: #7cafc2">Tip.</span>**
    > If you want to set the <span style="color: #7cafc2"> property type to ChangeUIAvatar, click Component in the property type and search to add the ChangeUIAvatar component.</span>
    
6. Add **ButtonClickEvent** to the event handler of the **TestUIBtn** script and write as follows.
    ```lua
    [self]
    HandleButtonClickEvent(ButtonClickEvent event)
    {
        --Parameters
        local Entity = event.Entity
        --------------------------------
        local name = Entity.Name
        local changeUIAvatar = self.changeUIAvatar
        
        --Property
        if name == "SetColorBtn" then
	        changeUIAvatar:SetColor()
        end
    }
    ```
    
7. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. 
    The color value of the UI avatar changes each time the **[SetColor]** button is pressed.
    ![color](https://mod-file.dn.nexoncdn.co.kr/bbs/1673935701110863dae7d69624a3bb8f1f747cd007fbf.gif "color")

#### Change Parts Colors
The following is an example where the color value of the avatar parts shown in the UI changes randomly when pressing a button.

1. Click to copy **Duplicate** in the context menu of the **SetColorBtn** button.
    
2. Move the buttons appropriately and rename it to <span style="color: #dc9656">**SetPartColorBtn**</span>.
    
3. Enter <span style="color: #dc9656">**SetPartColor**</span> to the **SetPartColorBtn - TextComponent - Text** property.
    
4. Add `SetPartColor()` function to the **ChangeUIAvatar** script component and write as follows. Set the execution space to **client only**.
    ```lua
    [client Only]
    void SetPartColor()
    {
        local parts = 
        {
        	MapleAvatarItemCategory.Body,
        	MapleAvatarItemCategory.Cap,
        	MapleAvatarItemCategory.Cape,
        	MapleAvatarItemCategory.Coat,
        	MapleAvatarItemCategory.Ear,
        	MapleAvatarItemCategory.EarAccessory,
        	MapleAvatarItemCategory.EyeAccessory,
        	MapleAvatarItemCategory.Face,
        	MapleAvatarItemCategory.FaceAccessory,
        	MapleAvatarItemCategory.Glove,
        	MapleAvatarItemCategory.Hair,
        	MapleAvatarItemCategory.Head,
        	MapleAvatarItemCategory.Longcoat,
        	MapleAvatarItemCategory.OneHandedWeapon,
        	MapleAvatarItemCategory.Pants,
        	MapleAvatarItemCategory.Shoes,
        	MapleAvatarItemCategory.SubWeapon,
        	MapleAvatarItemCategory.TwoHandedWeapon
        }
        
        local r = _UtilLogic:RandomDouble()
        local g = _UtilLogic:RandomDouble()
        local b = _UtilLogic:RandomDouble()
        local a = _UtilLogic:RandomDouble()
        
        local index = _UtilLogic:RandomIntegerRange(1,18)
        
        self.myInfo.AvatarGUIRendererComponent:SetAvatarPartColor(parts[index], r, g, b, a)
    }
    ```
    
5. Add the following to **ButtonClickEvent** of the **TestUIBtn** script.
    ```lua
    [self]
    HandleButtonClickEvent(ButtonClickEvent event)
    {
        --Parameters
        local Entity = event.Entity
        --------------------------------
        local name = Entity.Name
        local changeUIAvatar = self.changeUIAvatar
        
        --Property
        if name == "SetColorBtn" then
	        changeUIAvatar:SetColor()
        -- Add the following content
        elseif name == "SetPartColorBtn" then
        	changeUIAvatar:SetPartColor()
        end
    }
    ```
    
6. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. 
    The color value of the UI avatar's random parts changes each time the **[SetPartColorBtn]** button is pressed.
    ![partcolor](https://mod-file.dn.nexoncdn.co.kr/bbs/167393592286102105ba6e40f42b099350d3a70dfb1f4.gif "partcolor")

#### Change Emotion
The following is an example where the avatar's emotion shown in the UI changes randomly when pressing a button.

1. Click to copy **Duplicate** in the context menu of the **SetColorBtn** button.
    
2. Move the buttons appropriately and rename it to <span style="color: #dc9656">**SetEmotionBtn**</span>.
    
3. Enter <span style="color: #dc9656">**SetEmotion**</span> to the **SetEmotionBtn - TextComponent - Text** property.
    
4. Add the `SetEmotion()` function to the **ChangeUIAvatar** script component and write as follows. Set the execution space to **client only**.
    ```lua
    [client Only]
    void SetEmotion()
    {
        local emotions = 
        {
        	EmotionalType.Hit,
        	EmotionalType.Smile,
        	EmotionalType.Troubled,
        	EmotionalType.Cry,
        	EmotionalType.Angry,
        	EmotionalType.Bewildered,
        	EmotionalType.Stunned,
        	EmotionalType.Vomit,
        	EmotionalType.Oops,
        	EmotionalType.Cheers,
        	EmotionalType.Chu,
        	EmotionalType.Wink,
        	EmotionalType.Pain,
        	EmotionalType.Glitter,
        	EmotionalType.Despair,
        	EmotionalType.Love,
        	EmotionalType.Shine,
        	EmotionalType.Blaze,
        	EmotionalType.Hum,
        	EmotionalType.Bowing,
        	EmotionalType.Hot,
        	EmotionalType.Dam,
        	EmotionalType.qBlue
        }
        
        local index = _UtilLogic:RandomIntegerRange(1, 23)
        self.myInfo.AvatarGUIRendererComponent:PlayEmotion(emotions[index], 3)
    }
    ```
    
5. Add the following to **ButtonClickEvent** of the **TestUIBtn** script.
    ```lua
    [self]
    HandleButtonClickEvent(ButtonClickEvent event)
    {
        --Parameters
        local Entity = event.Entity
        --------------------------------
        local name = Entity.Name
        local changeUIAvatar = self.changeUIAvatar
        
        --Property
        if name == "SetColorBtn" then
	        changeUIAvatar:SetColor()
	    elseif name == "SetPartColorBtn" then
        	changeUIAvatar:SetPartColor()
        -- Add the following content
        elseif name == "SetEmotionBtn" then
	        changeUIAvatar:SetEmotion()
        end
    }
    ```
    
6. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. 
    The avatar's emotion changes each time the **[SetEmotion]** button is pressed.
    ![emotion](https://mod-file.dn.nexoncdn.co.kr/bbs/16739360881637f9ea067d7444efd8b7148aafc5cb01c.gif "emotion")