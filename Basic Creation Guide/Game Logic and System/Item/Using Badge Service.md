# Course Introduction
Let's register the badge on the web and make it possible to obtain a badge when touching a specific object in the World by utilizing `BadgeService`. Keep in mind that in an unreleased World, you cannot actually earn the badge even if you already released it.

# BadgeService
You can use the registered badge via the badge service. For users who have acquired the badge, the acquisition notification window will appear for eight seconds, and the badge information will be displayed.
If multiple badges are acquired consecutively, the notification window will appear in the order in which they were acquired.

# Use Example
Let's register three badges of different ranks and make it possible to get badges when you reach certain locations.
#### Preparation
Register a badge following [Badge Registration].

| Badge Information | Badge 1 | Badge 2 | Badge 3 |
| --- | --- | --- | --- |
| Badge Grade | Normal | Rare | Epic |
| Badge Name | Arrived at Forest Village | Arrived at the Clock Tower | Arrived at the entrance of the Royal Palace |
| Acquisition Period | Indefinite | Indefinite | Indefinite |
| Badge Thumbnail | ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1646351123383a964ad8465424f30b49639e7ca5c4593.png "1")| ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1646351139128655068f53362491b9a0f7fbcec64136c.png "2") | ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1646351152274a308bfb2c7b84d1bbd0114c87a9d716d.png "3") |
| Badge Description | You receive this badge upon arriving at the entrance of Forest Village. | You receive this badge upon reaching the Clock Tower at the center of town. | You receive this badge upon arriving at the entrance of the Royal Palace. |

#### Giving Badges
Let's give badges to players when they pass certain places.
![01](https://mod-file.dn.nexoncdn.co.kr/bbs/16656411326436988d380fbbe4f44ac5cf34416a38aaa.gif "01")

1. Pick an object that will serve as a place for your player character to earn badges and place them in the World.
![objet](https://mod-file.dn.nexoncdn.co.kr/bbs/1665631051376747462ce6e7946dd9287caebe3462ee5.png{"width":"640px"} "objet")

2. Add **TriggerComponent** to an object.
3. Create a new **SendBadgeInfo component** into Workspace - MyDesk and add it to the place object to earn badges.
![SendBadgeInfo](https://mod-file.dn.nexoncdn.co.kr/bbs/16661642796074bf718f61bf849b99b9abace8e97342e.png "SendBadgeInfo")

4. Open Script Editor and add a property to check the badge ID in **SendBadgeInfo component**.

    ```lua
    Property:
    [None]
    string BadgeInfo = ""
    ```

5. In **Badge Viewer**, select **Copy Badge ID** and paste the copied ID into the **BadgeInfo** property of the **SendBadgeInfo** component of the entity that can obtain the badge.
![CopyBadgeId](https://mod-file.dn.nexoncdn.co.kr/bbs/16696880466919cdc990b2864460c9ecf41e1dfcc7ad1.png "CopyBadgeId")

6. Create a new **PlayerGetBadge component** and add it to **DefaultPlayer**.  Write the following using `BadgeService` to check that the user does not have a badge and to make the user receive a badge.
![Player get badge component](https://mod-file.dn.nexoncdn.co.kr/bbs/1665631087798d08c54dcac3a413ca25de89824cfc59d.gif "Player get badge component")

    ```lua
    Method:
    [server only]
    void GetBadge(string BadgeID)
    {
        local awardingUserId = self.Entity.PlayerComponent.UserId
        if _BadgeService:AwardBadgeAndWait(awardingUserId, badgeId) then
            self:SendEvent(awardingUserId)
        end        
    }
    
    [client]
    void SendEvent()
    {
        local badgeOpenBtn = _EntityService:GetEntityByPath("/ui/DefaultGroup/BadgeOpenBtn")
        local event = GetBadge()
        badgeOpenBtn:SendEvent(event)
    }
    
    Event Handler:
    [server only] [self]
    HandleTriggerEnterEvent(TriggerEnterEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: TriggerComponent
        -- Space: Server, Client
        ---------------------------------------------------------
     
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        ---------------------------------------------------------
     
        if TriggerBodyEntity.SendBadgeInfo then
            self:GetBadge(TriggerBodyEntity.SendBadgeInfo.BadgeInfo)
        end
    }
    ```

7. Click Start and check if the Badge Acquisition window appears when you hit an object.

    ><span style="color: #7cafc2">**Tip**
    > Since badges should only be awarded to users who have fulfilled the conditions for obtaining them, it is essential to verify that only users who meet the conditions can obtain badges through a multiplayer test.</span>

#### Create Badge Confirmation Window UI
Let's make a button that opens the badge acquisition confirmation window and, when pressed, changes from View Badge to Close.
![Badge Open Button](https://mod-file.dn.nexoncdn.co.kr/bbs/1669688833513f6d79eb764654c75ad5a287eed5b1a28.gif "Badge Open Button")

1. Place a button of **UI Editor - DefaultGroup - BadgeOpenBtn**. 
![Button](https://mod-file.dn.nexoncdn.co.kr/bbs/1675143487240a34f5c76c84f4cfaaf1f447c3a9a31cf.png "Button")

2. In the property editor, add TextComponent and enter <span style="color: #dc9656">**View Badge**</span> in **Text**.
3. Create a new **BadgeOpenBtn component** and add it to **BadgeOpenBtn**.
4. Receive ButtonClickEvent upon clicking a button, Enable the BadgeLayer entity, and write the following to change the button's Text.

    ```lua
    [self]
    HandleButtonClickEvent(ButtonClickEvent event)
    {      
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
     
        -- Parameters
        -- local Entity = event.Entity
        ---------------------------------------------------------
     
        local BadgeLayerPanel = _EntityService:GetEntityByPath("/ui/BadgeLayer")
     
        if BadgeLayerPanel.Enable == false then
            BadgeLayerPanel:SetEnable(true)
            self.Entity.TextComponent.Text = "Close"
        else
            BadgeLayerPanel:SetEnable(false)
            self.Entity.TextComponent.Text = "View Badge"
        end
    }
    ```
5. Check if View Badge changes to Close when the button is pressed.

#### Create Badge Confirmation Window UI
1. Add **UI Editor - BadgeLayer**.
2. Create a window to check badge information using images and text.
![badge ui](https://mod-file.dn.nexoncdn.co.kr/bbs/1665631306060d1951767a9be4695ac57542a369e7af1.png "badge ui")

    ><span style="color: #7cafc2">**Tip**
    > You need to put another UI entity under BadgeSlot_01.</span>

3. In **BadgeViewer**, select the <span style="color: #dc9656">**Arrived at Forest Village**</span> badge, open the context menu, and select **Copy Badge Thumbnail RUID**.
![copy badge thumbnail RUID](https://mod-file.dn.nexoncdn.co.kr/bbs/16696880920712cdc8dc41b314cc9a84ea5768b372110.png "copy badge thumbnail RUID")

4. Select the image entity to show the badge image and paste the RUID into ImageRUID** of **SpriteGUIRendererComponent.
![Image RUID](https://mod-file.dn.nexoncdn.co.kr/bbs/1665632185530c700ddc5625f484994f1c06880cbc08b.png "Image RUID")

#### Get Badge Information in the Badge Acquisition Confirmation Window
1. Create a new **BadgeSlot** component and add it to **BadgeSlot_01**.
![BadgeSlot](https://mod-file.dn.nexoncdn.co.kr/bbs/16656324011016d410ea46d8b488285c79a14cf05ccec.png "BadgeSlot")
2. Add properties to get the name, ownership information, and description of the acquired badge.

    ```lua
    Property:
    [None]
    string BadgeId = "" -- ID of badge to inquire
    [None]
    TextComponent BadgeTitle = nil -- Badge Name
    [None]
    TextComponent Hasbadge = nil -- Badge Ownership Information
    [None]
    TextComponent BadgDesc = nil -- Badge Description
    ```

    > <span style="color: #7cafc2">**Tip**
    > Property's TextComponent can be set to select TextComponent by pressing the ![open](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_open_folder.png "open") button.</span>
    > ![Text Component](https://mod-file.dn.nexoncdn.co.kr/bbs/1665639334907a1cca5eef1e04d67adad71c5632d75cc.png{"width":"240px"} "Text Component")

3.  `Use GetBadgeInfoAsync()` to retrieve badge information and write the following, so badge-related information is displayed in **TextComponent** when the window is opened.

    ```lua
    Method:
    [client only]
    void OnBeginPlay()
    {
        self.BadgeTitle = self.Entity:GetChildByName("BadgeTitle").TextComponent
        self.HasBadge = self.Entity:GetChildByName("HasBadge").TextComponent
        self.BadgeDesc = self.Entity:GetChildByName("BadgeDesc").TextComponent
     
        self:BadgeCheck()
     
        local EventHandler = _EntityService:GetEntityByPath("/ui/DefaultGroup/BadgeOpenBtn")
     
        EventHandler:ConnectEvent(GetBadge, function()
            self:BadgeCheck()   
        end)
    }
    
    [client only]
    void BadgeCheck()
    {
        local callBack = function(badgeId, badgeInfo)
            if badgeInfo ~= nil then
                self.BadgeTitle.Text = badgeInfo.Name
                self.BadgeDesc.Text = badgeInfo.Description
            end
        end
     
        _BadgeService:GetBadgeInfoAsync(self.BadgeId, callBack)
     
        local localUserID = _UserService.LocalPlayer.Name
     
        if _BadgeService:UserHasBadgeAndWait(localUserID, self.BadgeId) then
            self.HasBadge.Text = "Owned"
        else
            self.HasBadge.Text = "Not Owned"
        end
    }
    ```

5. In **Badge Viewer**, select the <span style="color: #dc9656">**Arrived at Forest Village**</span> badge, open the context menu, and select **Copy Badge ID**.
![Copy Badge ID](https://mod-file.dn.nexoncdn.co.kr/bbs/1669688215591d0cb0710db7848a19f5b8f2e25ac25ce.png "Copy Badge ID")

6. Paste the copied ID into **BadgeSlot - BadgeId** on the **BadgeSlot_01** property window.
6. To create the event we wrote in the above script, select **MyDesk - Create Script - Create EventType** to create a new **GetBadge** event.
![Get Badge Event](https://mod-file.dn.nexoncdn.co.kr/bbs/1665632709258ffdbc73fd6d84d45a2ff7c48dde1f0e9.png{"width":"120px"} "Get Badge Event")

7. Press Start to earn the badge and see if the badge status changes.