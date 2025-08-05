# Course Introduction
In this course, we will create Maple GYM together.
Maple GYM is a simple game of exercising and building muscles. If you attack, exercising will increase your strength, and your body size increases according to strength. If you approach Elwin in the Caravan Refuge, you can exchange strength for money. The purpose of this game is to build your muscular body by increasing the maximum exercise capacity or increase exercise skills with the money you earn.
<br>
Here's an example play video:
$$youtube
QEMBFxES1UE
$$
<br>
Just like a general side-scrolling action game, you can run up and down.
When playing together with other users, you can show off your bigger body.
In this course, you will experience the general flow of game production using scripts.
<br>
In Chapter 1, we'll implement the following features.

* Basic map creation
* Attack increases strength
* As strength increases, body size increases
* Body size is checked via UI
* Acquire game money by selling increased strength
# #1. Basic Map Creation
1. Create a new game with Create New.<br>![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16564040065699e1e89301057403a944d91c843aafd0b.png "2")<br>Follow the below steps once a new game is created.<br>
2. In Window menu at the top of Maker, click the MapleStory Map menu.<br>![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16352108965824ed0df1a6a6d4737829ddb3326a29660.png "3")<br>
3. Enter caravan refuge in the pop-up search box.<br>![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1656404053875fce567028677430ca4bc85c7296a6005.png "4")<br>Select Caravan Refuge, and then click Import Map button.<br>
4. Proceed with saving when the map is imported. You can enter any name for the game.<br>![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1635213318018ea417c794897423daaa1ade29b923244.png "5")<br>Now we have the beautiful caravan refuge. However, the current version needs some adjustments. Placement of NPCs or portals marked in red look a bit sloppy. We'll delete those elements.<br> Place them as shown below.<br>![6](https://mod-file.dn.nexoncdn.co.kr/bbs/163521333466752cfbc210c5d47c2aee8f4be26ad06a8.png "6")
# #2. Processing to Move in Quarter-view
1. Let's click the Start button to play.
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/16352109729188dc724cfb00846a4af5d0fef455c4f19.png "7")
    <br>
    Player can go around the caravan refuge, but cannot move up or down.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/16564047383643596c6c7858d4422bf480d15262c82fb.png "8")
    <br>
    Let's exit the game and change it so that we can move up or down.
    <br>
2. In the Workspace on the top right, select DefaultPlayer.
    You'll see the properties of Player in the Property window.
    ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/163581573692942c05fb7ab2c450f9e85d6732f24c2fb.png "9")
    <br>
3. Check the IsQuarterViewMove value in the component of RigidBody of ![player](https://mod-file.dn.nexoncdn.co.kr/bbs/1636357430620486e701f5b2d4d1599e7c72705e23f50.png "player") DefaultPlayer, and enter in the QuarterViewAccelerationX, QuarterViewAccelerationY value of 3 for each.
    ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1637560421500cbfac5af72444a4a8d134ded5cad8565.png "10")
    <br>
    Let's play now. 
    Now you can move up and down as well. (You're free from gravity!)
    ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/165640476435197401b2872d64e0bbebe70512e36d9ee.png "11")
    <br>
    However, now, the character can move to literally anywhere.
    It would look more natural if the character could only walk on the ground.
    ![12](https://mod-file.dn.nexoncdn.co.kr/bbs/165640478533731103cce740a4443a36c76f035b6f66b.png "12")
    <br>
    Let's use the script so that the character cannot move up or down outside a certain range.
    <br>
4. Add a new script component in the Workspace. 
    ![12-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1635815747247c4aad51c0ce34d7aaec022ed80e29014.png "12-1")
    <br>
    Name the component PlayerController,
    and double-click PlayerController component to open the Script Editor.
    ![13](https://mod-file.dn.nexoncdn.co.kr/bbs/16358158244946509c1f100be48dcb4ac99f48525d17a.png "13")
    <br>
5. Add properties as follows.
    ![13-1](https://mod-file.dn.nexoncdn.co.kr/bbs/163521107362304aadbb7903e43b69de83faa5d41e7a8.png "13-1")
    <br>
6. Add the OnBeginPlay function and write the code like the following.
    ```lua
    --void OnBeginPlay()
    self.Transform = self.Entity.TransformComponent
    self.Movement = self.Entity.MovementComponent
    ```     
    <br>
7. Then add the OnUpdate function and write the code as follows.
    ```lua
    --void OnUpdate (number delta)
    self:MoveArea()
    ```
    <br>
    First, execution space is deactivated for the OnBeginPlay and OnUpdate functions.
    In this case, we can call them both from server and client.
    ![14](https://mod-file.dn.nexoncdn.co.kr/bbs/16607943872494ea521f5da494263a53aa610fd1c9e36.png "14")
    ![15](https://mod-file.dn.nexoncdn.co.kr/bbs/16564052922690038d8c892b848ed974adfbd2e0b3aef.png "15")
    <br>
8. Add a new function and name it "MoveArea".
    Set the execution space as client only, and write the code as follows.
    ```lua     
    --void MoveArea() [client only] 
    local pos = self.Transform.Position
    if pos.y > -1.23 then
        self.Movement:SetPosition(Vector2(pos.x, -1.23))
    elseif pos.y < -2.6 then
        self.Movement:SetPosition(Vector2(pos.x, -2.6))
    end
    ```    
    <br>
    The execution space of MoveArea function called from OnUpdate function is set as Client. 
    We'll be checking and correcting the location on each frame. Processing this on the server may look awkward.
    We didn't do anything for the left or right part, but the loaded map's foothold is determining the width and limiting movement.
    Below is the script composed up to now.
    ![16-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1635211203200d239d44e4daa4629a26aa6e855e4f577.png "16-1")<br>Let's add the composed PlayerController component to the player.<br>Right-click DefaultPlayer in Workspace, click Add Component to add PlayerController component.<br>![17](https://mod-file.dn.nexoncdn.co.kr/bbs/1635815833027bde0364ea3a24a7693531f00a1402a27.png "17")<br>
9. Let's play now. You can see the character moving within the set range.
# #3. Adding Stat Components
In Maple Gym, you need stat information such as strength and its maximum capacity, game money, and level.
We will add a component that will save and manage this information.
<br>
1. Add a new script component. Name the component "PlayerStats".
    ![18](https://mod-file.dn.nexoncdn.co.kr/bbs/1635815846858e64d7594543d4b058f4d3a3d1c9b1b3e.png "18")
    <br>
2. Add below properties to PlayerStats.

    | Type | Name | Default value | Explanation |
    | --- | --- | --- | --- |
    | number | CurrentPow | 0 | Strength of the current character |
    | number | MaxPow | 15 | Strength with maximum capacity |
    | number | CurrentMoney | 0 | Money of the current character |
    | number | CurrentSkillGrade | 1 | Exercise level of the current character |
    | number | CurrentDnaGrade | 1 | DNA level of the current character |
    | number | WeightBonus | 1 | Strength increased when exercising |
    <br>
    The outcome will be as follows:
    ![19-1](https://mod-file.dn.nexoncdn.co.kr/bbs/163521124487163ebbadaf21c4c14b37f25a1d0f0aea2.png "19-1")
# #4. Implementation of Exercise
Strength must increase whenever the character attacks by pressing the Ctrl key.
We will check Attack and implement the script in a way that increases strength.
1. Double-click the PlayerController component, which was added earlier, to open Script Editor.
    ![20](https://mod-file.dn.nexoncdn.co.kr/bbs/16358158582908e359d1dfe0b4963ba0102b75b4b3221.png "20")
    <br>
2. Now, let's have PlayerController component refer to PlayerStats component.
    Within PlayerController, add Stats property of Component type.
    ![21-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16352112763652d192865582344028234f5125b6cc06a.png "21-1")
    <br>
3. As shown below, add a code to the OnBeginPlay function so that it receives PlayerStats.

    ```lua
    --void OnBeginPlay()
    --Previous code
    self.Transform = self.Entity.TransformComponent
    self.Movement = self.Entity.MovementComponent
    
    --Added code
    self.Stats = self.Entity.PlayerStats
    ```

   Now you can access PlayerStats through the stats property from PlayerController.<br>

4. In Script Editor, click the [+] button on the right side of the Entity Event Handler menu. Search for PlayerActionEvent in the search box and add a handler to PlayerActionEvent.<br>PlayerActionEvent function senses and calls events in which characters' status changes.<br>![22](https://mod-file.dn.nexoncdn.co.kr/bbs/16564050334581114e90875344d9f8c90fba64b81889d.png "22")<br>
5. Edit HandlePlayerActionEvent function as follows.
    ```lua
    --HandlePlayerActionEvent(PlayerActionEvent event) [self]
    -- Parameters
    local ActionName = event.ActionName
    local PlayerEntity = event.PlayerEntity
    --------------------------------------------------------
    if ActionName == "Attack" then
        self:WorkOut()
    end
    ```
    <br>
    Whenever Attack state is detected, it confirms that exercising is done and then calls WorkOut function.
    Now, we will add the WorkOut function.
    <br>
6. Add a new function in the PlayerController component. We'll name the function WorkOut. 
    HandlePlayerActionEvent can be called from client and server. Likewise, the WorkOut function can be called from both client/server, unless otherwise specified. We'll set its execution space as ServerOnly so that it is only called from the server.
    <br>
7. Add the following content in the WorkOut function.
    ```lua
    --void WorkOut() [server only]
    local calePow = self.Stats.CurrentPow + self.Stats.WeightBonus
     
    if calePow > self.Stats.MaxPow then
        calePow = self.Stats.MaxPow
    end
     
    self.Stats:SetPow(calePow)
    ```
    Once the WorkOut function is called, it adds current strength from the PlayerStats component and strength obtained by exercising.
    In the if statement, we'll make sure that strength does not exceed MaxPow.
    Once you prompt strength to accumulate, pass the value to PlayerStats component's SetPow function.
    <br>
    The composed PlayerController component will look like the following.
    ![24-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1635213980013464eb64389aa42aebd31ec5bbfafcae2.png "24-1")
    <br>
8. Move to PlayerStats component.
    Add a new function in the PlayerStats component. Name the function "SetPow" and add a parameter to receive currentPow values in number type.
    ![25](https://mod-file.dn.nexoncdn.co.kr/bbs/1635211377864f84bc461a73d4630b614a2066a5e5fcb.png "25")
    <br>
    SetPow function is called from the WorkOut function within the PlayerController component. Since the WorkOut function is executed in Server, the SetPow function will be executed in Server as well, unless otherwise specified.
    However, we'll be calling a function to refresh UI when calling the SetPow function. Let's set its execution space as Multicast.
    <br>
    Add the following content in the SetPow function.
    ```lua
    --void SetPow(number currentPow) [multicast]
    self.CurrentPow = currentPow
    log(self.CurrentPow)
    ```
    <br>
    Assign the delivered argument to CurrentPow property, and output the current CurrentPow value as log.
    The composed PlayerStats component will look like the following.
    ![27-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1635211771145b12492e4032f4e6abb83265ac04fc45d.png "27-1")
    <br>
9. Now, let's add the PlayerStats and PlayerController components to the player.
    In the Workspace, right-click DefaultPlayer and select Add Component.
    Search and add the PlayerStats component and PlayerController component.
     ![29](https://mod-file.dn.nexoncdn.co.kr/bbs/163581587905950dd938b2b8a4ababa6bb9f62c4d819f.png "29")
    <br>
Let's play now. You can see the accumulation of strength in Client and Server whenever you Attack by pressing the Ctrl key.
![30](https://mod-file.dn.nexoncdn.co.kr/bbs/16352118271803971a92709f64c119b21457b0955af21.png "30")
# #5. Implementing an Increase in Body Size
We will increase the size of a character according to their strength.
Add a new function in the PlayerController component. Name the function "SizeSet".
![31](https://mod-file.dn.nexoncdn.co.kr/bbs/163521184304293281b5711994b1a92ea556698a95249.png "31")
<br>
1. Add the following content in the function.
    ```lua
    --void SizeSet()
    local size = 1 + (0.1 * self.Stats.CurrentPow)
    self.Transform.Scale = Vector3(size, size, 1)
    ```
    Increase the Scale by 0.1 per 1 strength.
    <br>
2. Edit it so that SizeSet function is called from the existing WorkOut function.
    ```lua
    --void WorkOut() [server only]
    -- Previous
    local calePow = self.Stats.CurrentPow + self.Stats.WeightBonus
     
    if calePow > self.Stats.MaxPow then
        calePow = self.Stats.MaxPow
    end
     
    self.Stats:SetPow(calePow)
     
    -- Added
    self:SizeSet()
    ```
    <br>
    Code added to PlayerController is as follows.
    ![32-1](https://mod-file.dn.nexoncdn.co.kr/bbs/163521186588796423f5143ad4e48b706751674180efa.png "32-1")
    <br>

3. We will now play after returning to the Scene.
You can see that the size of the character increases whenever they attack.
![33](https://mod-file.dn.nexoncdn.co.kr/bbs/1635211887402e75dad0c44ad45e1bd94ea51826e0cdf.png "33")
# #6. Adding Strength and Game Money UI
If strength increases, the size of the body gets bigger. This allows the player to see the increase in strength.
We will add a UI to mark the strength stat more clearly.
1. Click on the Scene window, click the UI button at the top of the Maker, and move to UI Editor.
    ![34](https://mod-file.dn.nexoncdn.co.kr/bbs/1635211899345d1a5602a16e94398b6b9c7ab362f63ab.png "34")
    <br>
2. With DefaultGroup selected, click the Text button to add new text.
    ![35](https://mod-file.dn.nexoncdn.co.kr/bbs/1656405540209756e997e58ab426d96694407ab9e6e41.png "35")
    <br>
    Click the placed text, and rename the entity "UIPower".
    ![36](https://mod-file.dn.nexoncdn.co.kr/bbs/163521192682993cc1035d0904860a7bfee2f8f4f0d8a.png "36")
    
    | Component | Property | Value |
    |---|---|---|
    | @cols=1:@rows=4:UITransform | PosX | -700 |
    | PosY | 65 |  
    | Width | 300 |
    | Height | 100 |
    | @cols=1:@rows=2:SpriteGUIRendererComponent | ImageRUID | 0c80d52cfe5626b4ca1bb5067c0c6938 |
    |  Color  | R 255, G 255, B 255, A 100%<br> White, disabled transparency |
    | Text | FontSize | 30 |
 
    ><span style="color: #7cafc2">**Tip**</span>
    > <span style="color: #7cafc2">Since this property uses UI entities by assigning them, Sync value must be None.</span>
    
    While we're at it, let's add a UI to display game money as well.<br>Click the completed UI from above, and right-click to find Duplicate menu.<br> ![37](https://mod-file.dn.nexoncdn.co.kr/bbs/1635211942934c9c755880b9e4916a45837a048cc2def.png "37") <br> Click the button.<br>

3. You'll see the Text UI you made has been duplicated.
    Click the duplicated UI and name it "UIMoney".
    ![38](https://mod-file.dn.nexoncdn.co.kr/bbs/1635211967579c040e2f4ccaf40e28b72fdfa8d4aefc1.png "38")
    <br>
    Edit the property of UIMoney as follows.
    
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=2:UITransform | PoxX | -700 |
    | PosY | -40 |
    
    Every task is completed as shown below. Click ![Tool_UI](https://mod-file.dn.nexoncdn.co.kr/bbs/163453120840744616a62243642e889159a68a78a56c2.png "Tool_UI")button to exit UI Editor.<br> ![39](https://mod-file.dn.nexoncdn.co.kr/bbs/163521198367072bb7f3213004a7dabdcea2d27373534.png "39")
# #7. Connecting Character Stat and UI
For overall UI management, we'll add UIManager script component and set it up so it calls UIManager's strength UI setting function whenever strength is increased by PlayerStats component.

1. Add a new script component. Name the component UIManager.
    ![40](https://mod-file.dn.nexoncdn.co.kr/bbs/1635815893958818b3d248f8f4b0f865635a668a69329.png "40")
    <br>
2. Click the UIManager component to activate Script Editor.
    Add 3 properties in Script Editor as follows.
    
    | Type | Name | Sync | Default value | Explanation |
    | --- | --- | --- | --- | --- |
    | Component | UiPowerText | None | nil | UIPower Entity's Text Component Information |
    | Component | UiMoneyText | None | nil | UIMoney Entity's Text Component Information |
    | Component | Stats | None | nil | Character's PlayerStats Component Information |
    <br>
    The outcome will be as follows.
    ![41-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16376317734643c515797da414eb3b7bfb815697ba6b6.png "41-1")
    <br>
3. Add OnBeginPlay() function. 
    The OnBeginPlay function will refer to UI-related information. Its execution space should be set to ClientOnly.
    <br>
    Add the following content in function.
    ```lua
    --void OnBeginPlay() [client only]
    self.UiPowerText = _EntityService:GetEntityByPath("/ui/DefaultGroup/UIPower").TextComponent
    self.UiMoneyText = _EntityService:GetEntityByPath("/ui/DefaultGroup/UIMoney").TextComponent
    self.Stats = _UserService.LocalPlayer.PlayerStats
     
    self:UISetPower()
    self:UISetMoney()
    ```
    <br>
    Find path of the text entity to refer to and the player entity and assign the path to each property.
    Now, UISetPower and UISetMoney functions, setting UI of power and money, are called at the beginning.
    <br>
    Now let's add UISetPower and UISetMoney functions.
<br>
4. Add a new function. Name the function UISetPower and add the below content.
    ```lua
    --void UISetPower()
    self.UiPowerText.Text = "Strength : "..math.floor(self.Stats.CurrentPow).." / "..math.floor(self.Stats.MaxPow)
    ```
    <br>
    When the above process is done, add another function. Name the function UISetMoney and add the below content.

    ```lua
    --void UISetMoney()
    self.UiMoneyText.Text = "Money : "..math.floor(self.Stats.CurrentMoney)
    ```
    <br>
    If you need to refresh strength and money, you can call each function respectively.
    We're done with UIManager.
    ![43-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1657690305986b0ef66b85dfa4ca6b5be74284cbb664d.png "43-1")
    <br>
5. Double-click PlayerStats component to open Script Editor.<br>Add below properties.

    | Type | Name | Default value | Explanation |
    | --- | --- | --- | --- |
    | component | UiManager | nil | UiManager Component Information |

    ><span style="color: #7cafc2">**Tip**</span>
    > <span style="color: #7cafc2">Since this property uses UI entities by assigning them, Sync value must be None.</span>
    
    After that, add the OnBeginPlay function. Deactivate the function's execution space as "unused" so that it can be used from both the client and server.
    <br>
    Add the following content in OnBeginPlay function. 
    ```lua
    --void OnBeginPlay()
    if self:IsClient() then
        self.UiManager = _EntityService:GetEntityByPath("/ui/DefaultGroup").UIManager  
    end
    ```    
    
    Add the RenewPow and RenewMoney functions. <br>Set the two functions' execution space as ClientOnly.  <br>Then add the following content in the RenewPow function.
    
    ```lua
    --void RenewPow() [client only]
    self.UiManager:UISetPower()
    ```
    
    Add the following content in RenewMoney function.
    
    ```lua
    --void RenewMoney() [client only]
    self.UiManager:UISetMoney()
    ```
    
    
    Since strength UI has to be renewed whenever strength increases, add the following content to SetPow function of PlayerStats component.
    
    ```lua
    --void SetPow() [multicast]
    self.CurrentPow = currentPow
    log(self.CurrentPow)
    --Added
    self:RenewPow()
    ```
    
    Below is the modified PlayerStats script up to now.<br> ![46-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1637564692911a2caecc0dbcf46f799065bec5e397ea0.png "46-1")
    
6. Let's add the completed UIManager component to the entity.
    In Hierarchy, select ui → DefaultGroup.
    Right-click DefaultGroup, select Add Component, and add UIManager component.
    ![48](https://mod-file.dn.nexoncdn.co.kr/bbs/1659507470565386887b295a74624aee895d62cf09d99.png "48")
    <br>
We will now play after returning to the Scene.
When you try attacking, you can confirm the strength increasing.
# #8. Selling Strength
We will now create a function for exchanging strength for Gold.
Place an NPC in charge of exchange. When you approach this NPC, set strength as 0 and increase the money to correspond with the strength reduced.
1. Firstly, we will place the NPC that will be in charge of sales.
    Place an NPC you want to use. In the example, the NPC will be Elwin.
    ![49](https://mod-file.dn.nexoncdn.co.kr/bbs/1656406220297589e40f880104749b25a2092cf30cccf.png "49")
    <br>
    Rename the NPC to "SellNPC".
    ![50](https://mod-file.dn.nexoncdn.co.kr/bbs/16352127476757bd4ffc4d7084241a62dab1f468764ab.png "50")
    <br>
    If the NPC is placed behind the objects on the map, modify the NPC's Sorting Layer value of SpriteRenderer component to Layer7.
    <br>
2. In quarter-view games, entities should not be falling because of RigidBody. 
    So click the placed NPC and change the value of Gravity property of RigidBody component to 0.
    ![51](https://mod-file.dn.nexoncdn.co.kr/bbs/1637631798121a1ebc47ce547412b80da9a9e02bf3fcb.png "51")
    <br>
    If the placed entity does not have RigidBody, you can skip this part.
    <br>
3. Let's create a component to sense the NPC and execute function.
    Find TriggerComponent in Component of Workspace, right-click on it, and click Extend to expand TriggerComponent.
    Name the extended component NPCSellPow and then press Enter.
    ![52](https://mod-file.dn.nexoncdn.co.kr/bbs/16607937626065c5ee04d632841ed829bb4e2e14e9fe3.png "52")
    <br>
4. Double-click the created NPCSellPow component to open the Script Editor.
   As this component was extended from TriggerComponent, you can check the extended function OnEnterTriggerBody from the list when creating the function. Click to add.
    ![53](https://mod-file.dn.nexoncdn.co.kr/bbs/1635815943418a9a064434b80473389a959a69704e51a.png "53")
    <br>
    OnEnterTriggerBody is called when entity with TriggerBody enters the Trigger area.<br>We'll use this function to check the user approaching the NPC.

    Add the following content in the function. Delete the existing __base:OnEnterTriggerBody(enterEvent) statement.
    ```lua
    --override void OnEnterTriggerBody(TriggerEnterEvent enterEvent)
    local entity = enterEvent.TriggerBodyEntity
    log(entity)
    if entity.PlayerStats == nil then
        return
    else
        entity.PlayerStats:SellPow()
    end
    ```
    <br>
    The outcome of the work will be as follows.
    ![54-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1635212799021abde75fe54814351a3e1eb96b4e991dd.png "54-1")
    <br>
    Target entering triggerBody may not be a player. Therefore, we'll check if the target has PlayerStats to identify players.
    If it is a player, it will call SellPow function from PlayerStats component.
    <br>
5. Double-click the PlayerStats component to open the Script Editor.
    Add a new function and name it "SellPow".
    Add the following content in SellPow function.
    ```lua
    --void SellPow() 
    self:SetMoney(self.CurrentMoney + self.CurrentPow)
    self:SetPow(0)
    self.Entity:GetComponent("PlayerController"):SizeSet()
    ```
    <br>
    Add the current strength to money and reset strength to 0. Now that strength has decreased, let's reset the character's size.
    Now we'll add SetMoney function called from the function above.
    Add a parameter in number type and name it currentMoney.
    Add the currentMoney parameter in number type to the added function and set its execution space as Multicast.
    <br>
    Add the following content in SetMoney function.
    ```lua
    --void SetMoney(number currentMoney) [multicast]
    self.CurrentMoney = currentMoney
    self:RenewMoney()
    ```
    <br>
    Set the delivered value to current money and call the previously created UI update function.
    Now, the composed PlayerStats component will look like the following.
    ![56-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1637570606890eb27d56adfcd4bb7bf85fa2e484da16b.png "56-1")
    <br>
    We're done with the script to sell strength to the store.
    <br>
6. Add the NPCSellPow component to the placed NPC. Let's play now.<br>Accumulate strength by attacking, and strength becomes 0 when the character approaches the NPC. Money increases to correspond with reduced strength.