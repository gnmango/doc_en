# Course Introduction 
> Read the first part of Creating Maple GYM here.
> [Creating Maple GYM Ⅰ](https://mod-developers.nexon.com/docs?postId=126{"target":"_self"})

In the previous lesson, we looked at creating a default map, increasing a strength stat with exercise, changing character and UI to reflect increased strength, and selling strength.
In this course, we'll try the following:

* Moving to the strength sale area
* Simple shop UI
* Skill and DNA upgrades
# #1. Moving to the Strength Sale Area
It's inconvenient to move to an NPC every time you want to sell strength.
We'll add a button to teleport to the NPC.
1. Click the UI button on the top left of the Maker, and click the UI Editor.
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1657682976342e7641537e0b44db9961e5a5ce7632650.png "1")
    <br>
2. With DefaultGroup selected, click the button to place the button in the UI.
    ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1657683323722561138acd2044d0bba77f401a933a42f.png "2")
    <br>
3. Click the placed button and rename it to "ShopButton".
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/163524426112196b3d5d72d1644c7ba214b331a3482dc.png "3")
    <br>
    In the Property Editor, click Add Component to add a TextComponent.
    ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1635819509779d4ed9fde88eb4674aabee1e32605392e.png "4")
    <br>
    Edit the properties for each component as follows:

    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=4:UITransformComponent | PosX | -770 |
    | PosY | -130 |
    | Width | 120 |
    | Height | 70 |
    | SpriteGUIRendererComponent | ImageRUID | 869b38cc8e41f9649923b8877f0f22a5 |
    | @cols=1:@rows=2:TextComponent | FontSize | 30 |
	| Text | SHOP |
    <br>
    The outcome will be as follows:
    ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16352442882368f40585300fa4380b5b23950d7b82599.png "5")
    <br>
4. We'll also create a SELL button. Select the created SHOP button and right-click on it.
    Click Duplicate from the pop-up menu.
    ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/16352443038356025402f2c214c7c9528c5fdb2c0499e.png "6")
    <br>
    Rename the duplicated button "SellButton".
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/163524431559522d53269007147ec923e89d746d40500.png "7")
    <br>
    Click the duplicated button and edit the properties as follows:

    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=2:UITransformComponent | PosX | -630 |
    | PosY | -130 |
    | TextComponent | Text | SELL |
    <br>
    The outcome will be as follows:
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/16352443271625229640eeb4f4bdbb6eb5ad31867ef4a.png "8")
    <br>
5. Let's move the player character to the NPC's position upon clicking the SELL button.
    In the Workspace, double-click <span style="color: #dc9656"><span style="color: #dc9656">**UIManager**</span></span> to open Script Editor.
   Add the following property so that UIManager can refer to PlayerController since the change of location will be done from PlayerController.
    
    | Type | Property | Synchronization | Default value | Contents|
    | --- | --- | --- | --- | --- |
    | component | Controller | None | nil | Assign PlayerController component.<br>Set property Sync to None. |
    <br>
    Let's add a function to repeat the process of referring the button entity's component and linking the OnClickEvent handler.
    Add a new function within UIManager. Name the function "SetButtonHandler" and add a buttonPath parameter of type string.
    ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/163524434037247fe79d3f5bb4dde946227f748fc51df.png "9")
    <br>
    Add the following content in the function.
    ```lua
    void SetButtonHandler(string buttonPath)
    {
        local button = _EntityService:GetEntityByPath(buttonPath)
        button:ConnectEvent(ButtonClickEvent, self.OnClickButton)
    }
    ```
    <br>
    Now, we will call the SetButtonHandler function, which was added in the OnBeginPlay function.
    Add the below content in the OnBeginPlay function.
    ```lua
    self.UiPowerText = _EntityService:GetEntityByPath("/ui/DefaultGroup/UIPower").TextComponent
    self.UiMoneyText = _EntityService:GetEntityByPath("/ui/DefaultGroup/UIMoney").TextComponent
    self.Stats = _UserService.LocalPlayer.PlayerStats
         
    self:UISetPower()
    self:UISetMoney()
     
    --Added
    local buttonPaths = {
        "/ui/DefaultGroup/ShopButton",
        "/ui/DefaultGroup/SellButton",
    }
      
    for i, path in pairs(buttonPaths) do
        self:SetButtonHandler(path)
    end
     
    local localPlayer = _UserService.LocalPlayer
    self.Controller = localPlayer.PlayerController
    ```
    <br>
    In the buttonPaths table, enter the path for ButtonEntity.
    In the repeated statement afterward, we'll iterate over ButtonPath, use the SetButtonHandler function from above to bring in the Button component from the input Path, and then link events.
    Now every time you add a button, you can add the Path of the button to the buttonPaths table.
    <br>
    Moreover, we have assigned the PlayerController component information to the Controller property we added earlier.
    <br>
6. In the SetButtonHandler function, ButtonClickEvent is checked and the OnClickButton function is called.
    Add a new function and name it OnClickButton. 
    Add a parameter to the function so that it can receive an event argument of any type.
    ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/163524435776493b1eb9fb5b24bd093952a599ed7f477.png "10")
    <br>
    Add the following content in the function:
    ```lua
    --void OnClickButton(any event)
    local buttonName = event.Entity.Name
     
    if buttonName == "ShopButton" then
         
    elseif buttonName == "SellButton" then
        self.Controller:MoveSellPosition()
    end
    ```
    <br>
    Use the delivered argument to check the name of the pressed button and branch according to the name.
    Now, UIManager will call the MoveSellPosition function of the PlayerController component when the button is clicked.
    <br>
    The composed UIManager component will look like the following.
    ![11-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1657689013116b9c63210cb91476aa4e514608959dc35.png "11-1")
    <br>
7. In the Workspace, double-click the PlayerController component to open the Script Editor.
    Add a new function and name it MoveSellPosition.
    <br>
    Add the following content in the MoveSellPosition function.
    ```lua
    --void MoveSellPosition()
    local npcPos = _EntityService:GetEntityByPath("/maps/map01/SellNPC").TransformComponent.Position
    self.Movement:SetPosition(Vector2(npcPos.x, npcPos.y))
    ```
    ![12-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16352444092796739c3cec4c6404d8399226889705403.png "12-1")
    <br>
    Let's go back to the scene and play.
    If you click the SELL button, you can see the character moving to the placed NPC.
# #2. Shop and Upgrade UI Design
We will design a shop that pops up when clicking the SHOP button.
We will proceed with the following structure.
![13](https://mod-file.dn.nexoncdn.co.kr/bbs/16564068701520288e0f651a64f4e91efc49883d8df10.png "13")
<br>
You can change the design as you like as we proceed.
1. In the top left menu, move to the UI Edit mode by clicking the UI button.
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1657682976342e7641537e0b44db9961e5a5ce7632650.png "1")
    <br>
2. Let's add a new UI Group from the bottom right. 
    In the UI Group list, press the [+] button to add a new UI Group.
    ![15](https://mod-file.dn.nexoncdn.co.kr/bbs/1635819521289dac8a8a7fea84a7abb4bdd5f30ecc85b.png "15")
    <br>
    Click the new UIGroup once it's added.
    In UIGroup, change the property value of UIGroupComponent's DefaultShow to true.
    ![16](https://mod-file.dn.nexoncdn.co.kr/bbs/1637720583796bf3d7baac2c6465ead9ea17be43cdf56.png "16")
    <br>
3. We'll add a panel that will become the UI background. Click the image button and place the image.
    Click the placed image and edit the properties as follows:
    
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=2:UITransformComponent | Width | 1200 |
    | Height | 700 |
    | SpriteGUIRendererComponent | ImageRUID | 8122dd6f67f3d9b4db8a3152172f9063 |
    <br>
4. We'll place a Close button. Click the button to place a button. 
    Click the placed button and rename it to "ShopCloseButton".
    ![17](https://mod-file.dn.nexoncdn.co.kr/bbs/1635244963786f9add6c451134bbebd130bf08fa36d3c.png "17")
    <br>
    Edit the properties of the button as follows:
    
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=4:UITransformComponent | PosX | 540 |
    | PosY | 290 |
    | Width | 70 |
    | Height | 70 |
    | SpriteGUIRendererComponent | ImageRUID | c25813e3ecb203146b24f48b2bebb891 |
    <br>
    
5. Click the text button to place text.
    Click the placed text and edit the properties as follows:
    
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=4:UITransformComponent | PosX | -300 |
    | PosY | 200 |
    | Width | 350 |
    | Height | 100 |
    | @cols=1:@rows=7:TextComponent | FontColor | RGB(255,0,0 / Red) |
	| FontSize | 60 |
    | OutLineColor | RGB(255,255,255 / White) |
    | OutLineDistance X | 2 |
    | OutLineDistance Y | -2 |
	| Text | Exercise Skills |
	| UseOutLine | True (check) |
    <br>
    
6. Right-click the text placed in step 5. 
    Click Duplicate and edit the properties as follows:

    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=4:UITransformComponent | PosX | 300 |
    | PosY | 200 |
    | Width | 350 |
    | Height | 100 |
    | @cols=1:@rows=7:TextComponent | FontColor | RGB(0,0,255 / Blue) |    
	| Text | DNA |
    <br>
    
7. Click the text button to place a new text. 
    The text shows the level of exercise skill. As the text will be different depending on level, it should refer to the entity's name.
    Therefore, click the entity's name to change it to SkillLevelText.
    ![19](https://mod-file.dn.nexoncdn.co.kr/bbs/163524499743527d6478794f34defad44ea7df016f9b2.png "19")
    <br>
    Then edit the properties as follows:

    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=4:UITransformComponent | PosX | -300 |
    | PosY | 135 |
    | Width | 350 |
    | Height | 60 |
    | @cols=1:@rows=7:TextComponent | FontColor | RGB(255,255,0 / Yellow) |
    | FontSize | 35 |    
    | OutLineDistance X | 2 |
    | OutLineDistance Y | -2 |
	| Text | (Skill Level: Level 1) |
	| UseOutLine | True (check) |
    <br>
    
8. Right-click the text from step 7, and click Duplicate from the pop-up menu.
    Since this text will show the DNA level, rename it to "DnaLevelText".
    ![20](https://mod-file.dn.nexoncdn.co.kr/bbs/16352450122335c48da47779946deb837091a9ba57a58.png "20")
    <br>
    Then edit the properties as follows:
    
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=2:UITransformComponent | PosX | 300 |
    | PosY | 135 |
    | TextComponent | Text | (DNA : Quark) |
    <br>
    
9. Right-click the text from step 8, and click Duplicate from the pop-up menu.
    Then edit the properties as follows:
    
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=2:UITransformComponent | PosX | -300 |
    | PosY | 80 |
    | @cols=1:@rows=2:TextComponent | FontColor | RGB(255,255,255 / White) |
	| Text | Growth of strength increases when exercising |
    <br>
    
10. Right-click the text from step 9, and click Duplicate from the pop-up menu.
    Then edit the properties as follows:
    
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=2:UITransformComponent | PosX | 300 |
    | PosY | 80 |
    | TextComponent | Text | Max capacity of strength increases |
    <br>
    
11. Click the image button to add a new image.
    Click the added image and edit the properties as follows:
    
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=4:UITransformComponent | PosX | -300 |
    | PosY | -30 |
    | Width | 300 |
    | Height | 300 |
    | SpriteGUIRendererComponent | ImageRUID | 37d80af03df343d59b575fa37f6140a9 |
    <br>
    However, the placed image covers the text because of the order of placement.
    ![21](https://mod-file.dn.nexoncdn.co.kr/bbs/16564069090061677edc19b9a4cbb93ebc992964fc83f.png "21")
    <br>
    You can drag the image from Hierarchy to change the rendering order.
    ![22](https://mod-file.dn.nexoncdn.co.kr/bbs/1637800364977c7253062e7bf41ab8737dcd9e80d852e.png "22")
    <br>
12. Click the image button to add a new image.
    Click the added image and edit the properties as follows:
    
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=4:UITransformComponent | PosX | 300 |
    | PosY | -30 |
    | Width | 300 |
    | Height | 300 |
    | SpriteGUIRendererComponent | ImageRUID | 28eab0401d8b43a1a2865841ddad0a8f |
    <br>
    
13. Add a new button entity.
    Since button entities should refer to the script, rename it as "SkillUpButton".
    ![23](https://mod-file.dn.nexoncdn.co.kr/bbs/16352450546579b0410cb21c04887aa77d406781105ba.png "23")
    <br>
    And in the Property Editor, click Add Component to add a TextComponent.
    Edit the properties as follows:
    
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=4:UITransformComponent | PosX | -300 |
    | PosY | -250 |
    | Width | 230 |
    | Height | 60 |
    | SpriteGUIRendererComponent | ImageRUID | 869b38cc8e41f9649923b8877f0f22a5 |
    | @cols=1:@rows=6:TextComponent | FontColor | RGB(255,255,255 / White) |
	| FontSize | 35 |
    | OutLineColor | RGB(0,0,0 / Black) |
    | OutLineDistance X | 2 |
    | OutLineDistance Y | -2 |
	| UseOutLine | True (check) |
    <br>
    The button should show the upgrade cost.
    As the content of the text will be edited in the script, you may enter whatever you want.
    <br>
14. Right-click the text from step 13, and click Duplicate from the pop-up menu.
    Since button entities should refer to the script, rename it to "DnaUpButton".
    ![24-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1637729504554e51308b651bc4869b59ee005514a1b51.png "24-1")
    <br>
    Edit the properties as follows:
    
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=2:UITransformComponent | PosX | 300 |
    | PosY | -250 |
    <br>

UI placement is complete!
Now, we'll add each function via scripting.
# #3. Connecting Shop and Close Buttons
In the UI process, we added 3 more buttons.
We'll connect the handler of these buttons.
<br>
1. Double-click the UIManager component to open the Script Editor.
2. Add a new property. 
    
    | Type | Property name | Synchronization | Default value | Contents |
    | --- | --- | --- | --- | --- |
    | Entity | ShopGroup | None | nil | We'll assign UIGroup's Entity and control from script whether to Enable it.<br>Set property Sync to None. |
    <br>

3. Let's look at how buttonPaths is defined within the OnBeginPlay function.
    ![24](https://mod-file.dn.nexoncdn.co.kr/bbs/1635245071039e81f10b5ce764b319b08a0c45701ff35.png "24")
    Let's insert the Path of the added buttons here, and assign the RectTransform component of UIGroup to the shopGroupTransform property assigned earlier.
    ```lua
    [client only]
    void OnBeginPlay()
    {
        self.UiPowerText = _EntityService:GetEntityByPath("/ui/DefaultGroup/UIPower").TextComponent
        self.UiMoneyText = _EntityService:GetEntityByPath("/ui/DefaultGroup/UIMoney").TextComponent
        self.Stats = _UserService.LocalPlayer.PlayerStats
             
        self:UISetPower()
        self:UISetMoney()
        
        local buttonPaths = {
        "/ui/DefaultGroup/ShopButton",
        "/ui/DefaultGroup/SellButton",
        "/ui/UIGroup/ShopCloseButton", --Add
        "/ui/UIGroup/SkillUpButton", --Add
        "/ui/UIGroup/DnaUpButton" --Add
        }
          
        for i, path in pairs(buttonPaths) do
            self:SetButtonHandler(path)
        end
         
        local localPlayer = _UserService.LocalPlayer
        self.Controller = localPlayer.PlayerController
        
        self.ShopGroup= _EntityService:GetEntityByPath("/ui/UIGroup") --Add
    }
    ```
    <br>
    
4. We'll add If statements for each button in the OnClickButton function of UIManager.
    We'll need to do some editing.
    ```lua
    void OnClickButton(any event)
    {
        local buttonName = event.Entity.Name
         
        if buttonName == "ShopButton" then
            self.ShopGroup:SetVisible(true, false)
        elseif buttonName == "SellButton" then
            self.Controller:MoveSellPosition()
        elseif buttonName == "ShopCloseButton" then --Change
            self.ShopGroup:SetVisible(false, false)
        elseif buttonName == "SkillUpButton" then -- Add
             --self.Stats:SkillUpGrade() -- Add
        elseif buttonName == "DnaUpButton" then -- Add
             --self.Stats:DnaUpGrade() -- Add
        end -- Add
    }
    ```
    ![26-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1635245101395612e1a88e7de4ac18f61f6589722bed0.png "26-1")
    <br>

We'll now play after returning to Scene.
By clicking the Shop button, you can see the shop pop up.
Also, if you click the X button in the Shop, the Shop closes.
![27](https://mod-file.dn.nexoncdn.co.kr/bbs/16564068701520288e0f651a64f4e91efc49883d8df10.png "27")
# #4. Exercise Skills and DNA Information Structure
Exercise and DNA can be upgraded with money. They increase the quantity of strength increased as well as the quantity of information of strength that can be accumulated.
This means you must hold information about the required amount of money, strength that can be accumulated, and the increased strength at each level.
You can use various methods (e.g. using a formula, data table or defining in a script), but here, we will define directly in the script.
<br>
1. Double-click the PlayerStats component to open the Script Editor.
    Add the two properties below.
    
    | Type | Property name | Default value | Contents |
    | --- | --- | --- | --- |
    | table | SkillGradeTable | {} | Saves the table information related to exercise skills. |
    | table | DnaGradeTable | {} | Saves the table related to DNA. |
    <br>

2. Add a new function in the PlayerStats component. We'll name the function SetTable.
    Add the following content to the SetTable function.
    ```lua
    void SetTable()
    {
        self.SkillGradeTable = {
            {["Cost"] = 0, ["WeightBonus"] = 1, ["GradeName"] = "Grade 1"},
            {["Cost"] = 15, ["WeightBonus"] = 2, ["GradeName"] = "Grade 2"},
            {["Cost"] = 45, ["WeightBonus"] = 3, ["GradeName"] = "Grade 3"},
            {["Cost"] = 90, ["WeightBonus"] = 4, ["GradeName"] = "Grade 4"},
            {["Cost"] = 187, ["WeightBonus"] = 5, ["GradeName"] = "Grade 5"},
            {["Cost"] = 315, ["WeightBonus"] = 6, ["GradeName"] = "Grade 6"},
            {["Cost"] = 600, ["WeightBonus"] = 7, ["GradeName"] = "Grade 7"},
            {["Cost"] = 1200, ["WeightBonus"] = 8, ["GradeName"] = "Grade 8"},
            {["Cost"] = 2150, ["WeightBonus"] = 9, ["GradeName"] = "Grade 9"},
            {["Cost"] = 3475, ["WeightBonus"] = 15, ["GradeName"] = "Grade 10"}
        }
          
        self.DnaGradeTable = {
            {["Cost"] = 0, ["MaxPow"] = 15, ["GradeName"] = "Quark"},
            {["Cost"] = 15, ["MaxPow"] = 45, ["GradeName"] = "Atom"},
            {["Cost"] = 45, ["MaxPow"] = 90, ["GradeName"] = "Particulate"},
            {["Cost"] = 90, ["MaxPow"] = 187, ["GradeName"] = "Worm"},
            {["Cost"] = 187, ["MaxPow"] = 315, ["GradeName"] = "Dog"},
            {["Cost"] = 315, ["MaxPow"] = 600, ["GradeName"] = "Human"},
            {["Cost"] = 600, ["MaxPow"] = 1200, ["GradeName"] = "Adventurer"},
            {["Cost"] = 1200, ["MaxPow"] = 2150, ["GradeName"] = "Hero"},
            {["Cost"] = 2150, ["MaxPow"] = 3475, ["GradeName"] = "Adversary"},
            {["Cost"] = 3475, ["MaxPow"] = 5200, ["GradeName"] = "MVP Red"}
        }
    }
    ```
    <br>

3. Allow calling the SetTable function from the OnBeginPlay function of the PlayerStats component.
    ```lua
    if self:IsClient() then
        self.UiManager = _EntityService:GetEntityByPath("/ui/DefaultGroup").UIManager  
    end
    self:SetTable() -- added
    ```
    <br>
    We have set the basic necessary information. 
    Next, we'll ensure that all information on the SHOP page is shown in a corresponding manner.
# #5. Information Notation on Shop Page
Let's refer to each text entity information marked in the shop, and set the appropriate value for the player's stat information.
1. Double-click the UIManager component to open the Script Editor.<br>Add the 4 properties below in the UIManager. All added <span style="color: #dc9656">**Property Sync is None**</span>.
    
    | Type | Property name | Synchronization | Default value | Contents |
    | --- | --- | --- | --- | --- |
    | Component | SkillLevelText | None | nil | Refer to the image below |
    | Component | DnaLevelText | None | nil | Refer to the image below |
    | Component | SkillUpButtonText | None | nil | Refer to the image below |
    | Component | DnaUpButtonText | None | nil | Refer to the image below |
    <br>
    These properties contain the TextComponent information of the entity of the image below.
    ![30](https://mod-file.dn.nexoncdn.co.kr/bbs/1656406952624d34d124df5924f0c8cbfc7a7cd2cd132.png "30")
    <br>
2. Open the OnBeginPlay function of the UIManager component to edit the content as follows.
    ```lua
    [client only]
    void OnBeginPlay()
    {
        self.UiPowerText = _EntityService:GetEntityByPath("/ui/DefaultGroup/UIPower").TextComponent
        self.UiMoneyText = _EntityService:GetEntityByPath("/ui/DefaultGroup/UIMoney").TextComponent
        self.Stats = _UserService.LocalPlayer.PlayerStats
             
        self:UISetPower()
        self:UISetMoney()
        
        local buttonPaths = {
            "/ui/DefaultGroup/ShopButton",
            "/ui/DefaultGroup/SellButton",
            "/ui/UIGroup/ShopCloseButton",
            "/ui/UIGroup/SkillUpButton",
            "/ui/UIGroup/DnaUpButton"
        }
          
        for i, path in pairs(buttonPaths) do
            self:SetButtonHandler(path)
        end
         
        local localPlayer = _UserService.LocalPlayer
        self.Controller = localPlayer.PlayerController
        
       self.ShopGroup= _EntityService:GetEntityByPath("/ui/UIGroup")
        
        selfkillLevelText = _EntityService:GetEntityByPath("/ui/UIGroup/SkillLevelText").TextComponent --Add
        self.DnaLevelText = _EntityService:GetEntityByPath("/ui/UIGroup/DnaLevelText").TextComponent --Add
        self.SkillUpButtonText = _EntityService:GetEntityByPath("/ui/UIGroup/SkillUpButton").TextComponent --Add
        self.DnaUpButtonText = _EntityService:GetEntityByPath("/ui/UIGroup/DnaUpButton").TextComponent --Add
         
        self.ShopGroup:SetVisible(false, false) --Add
    }
    ```
    ![30-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1637717326151973defc1d98d41bc9a1bc4b04b6d1576.png "30-1")
    <br>
3. Add a new function in the UIManager. Name the function "SetShopInfo".
    Add the following content to the function.
    ```lua
    void SetShopInfo()
    {
        local skillGrade = math.floor(self.Stats.CurrentSkillGrade)
        local dnaGrade = math.floor(self.Stats.CurrentDnaGrade)
          
        self.SkillLevelText.Text = ("(Skill Level : Lv. "..skillGrade..")")
        self.DnaLevelText.Text = ("(DNA : "..self.Stats.DnaGradeTable[dnaGrade]["GradeName"]..")")
          
        local hasMoney = math.floor(self.Stats.CurrentMoney)
          
        local skillFontColor = nil
        local dnaFontColor = nil
          
        local skillButtonMessage = ""
        local dnaButtonMessage = ""
          
        if skillGrade >= #self.Stats.SkillGradeTable then
            skillButtonMessage= "Final Grade Reached"
            skillFontColor = Color(1,0,0,1)
        else
            local nextSkillMoney = math.floor(self.Stats.SkillGradeTable[skillGrade+1]["Cost"])
            skillButtonMessage = "Amount Needed : ".. nextSkillMoney
            if hasMoney >= nextSkillMoney then
                skillFontColor = Color(1,1,1,1)
            else
                skillFontColor = Color(1,0,0,1)
            end
        end
                  
        if dnaGrade >= #self.Stats.DnaGradeTable then
            dnaButtonMessage= "Final Grade Reached"
            dnaFontColor = Color(1,0,0,1)
        else
            local nextDnaMoney = math.floor(self.Stats.DnaGradeTable[dnaGrade+1]["Cost"])
            dnaButtonMessage = "Amount Needed : ".. nextDnaMoney
            if hasMoney >= nextDnaMoney then
                dnaFontColor = Color(1,1,1,1)
            else
                dnaFontColor = Color(1,0,0,1)
            end
        end
          
        self.SkillUpButtonText.Text = skillButtonMessage
        self.SkillUpButtonText.FontColor = skillFontColor
          
        self.DnaUpButtonText.Text = dnaButtonMessage
        self.DnaUpButtonText.FontColor = dnaFontColor
    }
    ```
    <br>
    Based on the table information defined in PlayerStats, check the current amount of money and change the color of the letter accordingly.
    You can call this function whenever you are calling the shop information.
    <br>
4. Edit the OnClickButton function of the UIManager as follows:
    ```lua
    void OnClickButton(any event)
    {
        local buttonName = event.Entity.Name
          
        if buttonName == "ShopButton" then
            self:SetShopInfo() -- Add
            self.ShopGroup:SetVisible(true, false)
        elseif buttonName == "SellButton" then
            self.Controller:MoveSellPosition()
            self:SetShopInfo() -- Add
        elseif buttonName == "ShopCloseButton" then
            self.ShopGroup:SetVisible(false, false)
        elseif buttonName == "SkillUpButton" then
            --self.Stats:SkillUpGrade()
        elseif buttonName == "DnaUpButton" then
            --self.Stats:DnaUpGrade()
        end
    }
    ```
    <br>
    We'll now play after returning to the Scene.
    Overall purchase and UI renewal should be working correctly.
# #6. Managing Purchase
We'll make sure that, when you click the Cost Required button with enough money, your exercise skill and DNA are upgraded.
1. From the current UIManager's OnClickButton, there are comments in the SkillUpGrade and DnaUpGrade functions.
    Implement this function and deal with the comment.
    ![33-2](https://mod-file.dn.nexoncdn.co.kr/bbs/16576900229411c83b859e8404e3391d539f3d4b44fa6.png "33-2")
    <br>
1. In the Workspace, double-click the PlayerStats component to open the Script Editor.
    Add a new function and name it "SkillUpGrade".
    Add the following content in the function:
    ```lua
    void SkillUpGrade()
    {
        if self.CurrentSkillGrade >= #self.SkillGradeTable then
            return
        end
         
        local nextSkillMoney = self.SkillGradeTable[self.CurrentSkillGrade+1]["Cost"]
        if  self.CurrentMoney >= nextSkillMoney then
            self:SetMoney(self.CurrentMoney - nextSkillMoney)
            self.CurrentSkillGrade = self.CurrentSkillGrade + 1
            self.WeightBonus = self.SkillGradeTable[self.CurrentSkillGrade]["WeightBonus"]
        end
        self:RenewUI()
    }
    ```
    ![34-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1635245215063b566a0f2e5494b09b0b974815141eea8.png "34-1")
    <br>
    After increasing the exercise level, it sets the appropriate amount of exercise for the corresponding exercise level.
    <br>
    Add a new function. We'll name the function "DnaUpGrade".
    Add the following content in the function.
    ```lua
    void DnaUpGrade()
    {
        if self.CurrentDnaGrade >= #self.DnaGradeTable then
            return
        end
        local nextDnaMoney = self.DnaGradeTable[self.CurrentDnaGrade+1]["Cost"]
        if self.CurrentMoney >= nextDnaMoney then
            self:SetMoney(self.CurrentMoney - nextDnaMoney)
            self.CurrentDnaGrade = self.CurrentDnaGrade + 1
            self:SetMaxPow(self.DnaGradeTable[self.CurrentDnaGrade]["MaxPow"])
        end
        self:RenewUI()
    }
    ```
    ![35-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16352452269435022a1d1c5d04c2aad9cb953c2021d16.png "35-1")
    <br>
    The DnaUpGrade function also reduces the amount of money and levels up Dna.
    Maximum strength will be processed by calling a function to renew maximum strength, in order to deal with the strength UI with a single function.
    <br>
    Set the execution space of the two functions (SkillUpGrade, DnaUpGrade) as Server, so that they can be executed from the server.
    ![36](https://mod-file.dn.nexoncdn.co.kr/bbs/16564072164681557617b2ce34d66b798b476cb5c0725.png "36")
    <br>
2. Each upgrade function, upon completion, will call the RenewUI function to renew the store UI.
    Add a new function and name it "RenewUI".
    Edit the function as shown below.
    ```lua
    void RenewUI()
    {
        self.UiManager:SetShopInfo()
    }
    ```
    ![36-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16352452528725939a9040421400b95c77e0af00f6a08.png "36-1")
    <br>
    Also, change the execution space to Client so that it can be called from the server function.
    <br>
3. We're almost done. The DnaUpGrade function calls a function that increases maximum strength. We'll be adding that function.
    Add a new function. Name the function "SetMaxPow", and let it take maxPow as a number type parameter.
    ![38](https://mod-file.dn.nexoncdn.co.kr/bbs/1635245283834e671ad9ea141499b98ecde8a7e896a5c.png "38")
    <br>
    Since this will refresh the strength UI as well, set its execution space as Multicast, so that the two tasks can be handled together.
    <br>
    Add the following content in the function.
    ```lua
    void SetMaxPow(number maxPow)
    {
        self.MaxPow = maxPow
        self:RenewPow()
    }
    ```
    <br>
    ![40-1](https://mod-file.dn.nexoncdn.co.kr/bbs/163524531207450440ad7ccff43869d255bc253571998.png "40-1")
We're all done now.
If everything went right, upgrading and using game money at the shop will work perfectly.