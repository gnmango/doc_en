# Course Introduction
In this course, we will create a dialogue box to display an NPC conversation.
We will use a UI Editor to configure the dialogue box, set up data in the data table, and call the data with a script.

Follow the below steps:

1. UI Design
2. Adding Data Tables
3. Creating the Script

The example is as follows:
![example](https://mod-file.dn.nexoncdn.co.kr/bbs/1656920494193c72846a8d8f74844af6cfa91da77b3e8.gif "example")
# UI Design
1. Click the UI Editor button ![Tool_UI](https://mod-file.dn.nexoncdn.co.kr/bbs/163453120840744616a62243642e889159a68a78a56c2.png "Tool_UI"). <br> ![01](https://mod-file.dn.nexoncdn.co.kr/bbs/1660788007767080808545143493cb100805a6b9a61c2.png "01")
2. Add a new <span style="color: #dc9656">**UIGroup**</span> to proceed with UI design. If there is no UIGroup, it will be added.<br> ![02](https://mod-file.dn.nexoncdn.co.kr/bbs/1637231482321b6a1c3aa189b489cb747a38976838514.png "02")
3. Go to the Property Editor of <span style="color: #dc9656">**UIGroup**</span> and change the <span style="color: #dc9656">**DefaultShow value to true**</span>.<br>
![03](https://mod-file.dn.nexoncdn.co.kr/bbs/1659506015421476d2911ed06452493719b1af0dfceb8.png "03")
4. Add the image ![Ui_Image](https://mod-file.dn.nexoncdn.co.kr/bbs/16345251504813fd79a27d20049e9ac1e72847ba736fd.png "Ui_Image") entity to the created UIGroup. <br> ![04](https://mod-file.dn.nexoncdn.co.kr/bbs/1660788197640318f22d7b5134e31a667acb83318a7f6.png "04")
5. Select the new image entity and rename it <span style="color: #dc9656">**TalkPanel**</span>. <br>
![05](https://mod-file.dn.nexoncdn.co.kr/bbs/1659506127922faec4b37f0a24ccabbe88d817d365bbc.png "05")
6. Set the Anchor Presets of **TalkPanel** to <span style="color: #dc9656">**Bottom, Center**</span>. <br> ![06](https://mod-file.dn.nexoncdn.co.kr/bbs/163723199675809230c8976c94d44b278c796374982b1.png "06")
7. Modify the property of **TalkPanel** as follows.
 
    | Component | Property | Value |
    | --- | --- | --- |
    |@cols=1:@rows=4:UITransformComponent  | PosX | 0 |
    | PosY | 150 |
    | Width | 1600 |
    | Height | 200 |
    | SpriteGUIRendererComponent | Color | RGB 0, 0, 0 <br> A 60 <br> Black, transparency 60% |

8. Add a new image entity and rename it <span style="color: #dc9656">**Portrait**</span>.<br>![TabWorkspace](https://mod-file.dn.nexoncdn.co.kr/bbs/1634524705528846307a3a2ea422d817319d54a31b94c.png "TabWorkspace") From the **Hierarchy**, drag **Portrait** onto **TalkPanel** to set it as a <span style="color: #dc9656">**child of TalkPanel**</span>. <br> ![08](https://mod-file.dn.nexoncdn.co.kr/bbs/1659505222470e81a04d47eb54c02b4b3f5f731209a0b.png "08")
9. Change the property of <span style="color: #dc9656">**Portrait**</span> as follows:
    | Component | Property | Value |
    | --- | --- | --- |
    |@cols=1:@rows=4:UITransformComponent  | PosX | -600 |
    | PosY | 220 |
    | Width | 250 |
    | Height | 250 |
    | SpriteGUIRendererComponent | Color | RGB 255, 255, 255 <br> A 255 <br> White, transparency 100% |

    The outcome will be as follows:
    ![9-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1637296984764488870270fa0441e8ecb2ae066c401e3.png "9-1")
10. Add a new text entity. Click the new text entity and rename it <span style="color: #dc9656">**Name**</span>.
    ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/163729705658874b1b29f49f449059271ceffda3c02c3.png "10")
11. Drag the **Name** entity onto <span style="color: #dc9656">**TalkPanel**</span> to set it as a child of <span style="color: #dc9656">**TalkPanel**</span>.
12. Modify the properties of the **Name entity** as follows.
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=4:UITransformComponent  | PosX | -600 |
    | PosY | 110 |
    | Width | 300 |
    | Height | 100 |
    | SpriteGUIRendererComponent | Color | RGB 255, 255, 255 <br> A 0 <br> White, transparency 0% |
    | @cols=1:@rows=4:TextComponent  | FontColor | RGB 255, 255, 255 <br> A 100 <br> White, transparency 100% |
    | FontSize | 36
    | OutlineColor | RGB 0, 0, 0 <br> A 100 <br> Black, transparency 100% |
    | UseOutLine | true |
    The outcome will be as follows:
    ![12-1](https://mod-file.dn.nexoncdn.co.kr/bbs/163729711384407c9884921b44d4ab670c0e8f2407286.png "12-1")
13. Click to copy the **Duplicate** in the context menu of the **Name** entity. Rename the duplicated entity name <span style="color: #dc9656">**Text**</span>.
14. Edit the property of the **Text** entity as follows:
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=4:UITransformComponent  | PosX | 0 |
    | PosY | 0 |
    | Width | 1550 |
    | Height | 150 |
    | TextComponent | Alignment | UpperLeft |
    The outcome will be as follows:
    ![14-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16372973124240a1115e177034fcb9f90e436392ba93c.png "14-1")
15. Select the previously created **TalkPanel** entity and then **Disable Enable checkbox** to make it invisible.

# Adding Data Tables
1. Add new data. Rename the data <span style="color: #dc9656">**NPCTalk**</span>. <br>![14](https://mod-file.dn.nexoncdn.co.kr/bbs/163723344112648f1cde6a18545029a10e3a23634e26e.png "14")
2. Add 2 columns and give them the following names: <span style="color: #dc9656">**name, portrait, text**</span>.<br>![15](https://mod-file.dn.nexoncdn.co.kr/bbs/16372334787405ab202b8fd8f4b0398139e4c51bb652a.png "15")
3. Use names, text, or RUID to make up a dialogue. Values for the example are as follows:

    | name | portrait | text |
    | --- | --- | --- |
    | Will |641962d7a2db445c9f1c58cfbec790a3	  | I can't believe I've been given such a role. |
    | Will | 	641962d7a2db445c9f1c58cfbec790a3 | W-Welcome. This world was created for the sake of the Dialogue Template example.|
    | Will | 	641962d7a2db445c9f1c58cfbec790a3 | Likewise, you can uncover your own stories.  |
    | Lucid | f80b400198bc40dd947d7eb0c5935491	 | Will, you're quite an excellent guide. Haha. It suits your reputation as the ruler of the Mirror World. |
    | Will| 	641962d7a2db445c9f1c58cfbec790a3 | Well, I have no choice. In this world, our fate is in the hands of the our creator. |
    | Will | 	8263388ee54a4a90a0bf5ef8991eb623 | It might be the perfect place for you. You can only live out your wishes in your dreams.  |
    | Lucid|  dcbd5c0508b4471a836a806245dc7a3f| Hmph... |
    | Will |	641962d7a2db445c9f1c58cfbec790a3  | Likewise, you can uncover your own stories. |
    | Will | 641962d7a2db445c9f1c58cfbec790a3 | Let's see if your world will be greater than the Mirror World. I'll be delighted to keep an eye on you.  |
    
    This is from an actual process.
    ![16](https://mod-file.dn.nexoncdn.co.kr/bbs/16564840058389482b280201d4b3fb1ce2f8b5b465e39.png "16")

# Adding Scripts
Key items that need to be implemented in the script are as follows:

* Entering a certain key will output a dialogue box.
* Each time you enter a key, the data will be refreshed on the dialogue box.
* If there is no more data to use, the dialogue box closes.

First, let's check the key input to make sure the dialogue box is displayed.

1. Add a new script component. Rename the component <span style="color: #dc9656">**NPCTalk**</span>. <br>![17](https://mod-file.dn.nexoncdn.co.kr/bbs/16372943757898ccda6030c2a4c6da35f6549be0e91e6.png "17")
2. In the **Workspace**, double-click to open **NPCTalk** script, add **KeyDownEvent** into the Entity Event Handler, and enter the following:
    ```lua
    local key = event.key
    --------------------------------------------------------
    if key == KeyboardKey.Z then
        self:ShowNextText()
    end
    ```

3. Currently, there is an error because the `ShowNextText()` function does not exist.<br>Add the `ShowNextText()` function and log, and then test whether they are properly being called.
    ```lua
    log("Z key Down")
    ```
    
    The outcome will be as follows.
    
    ![18](https://mod-file.dn.nexoncdn.co.kr/bbs/16372341515894403c1232ac5431f97f98239a6f5cf76.png "18") <br>

4. ![TabWorkspace](https://mod-file.dn.nexoncdn.co.kr/bbs/1634524705528846307a3a2ea422d817319d54a31b94c.png "TabWorkspace") <span style="color: #dc9656">**Click ![workspace_world](https://mod-file.dn.nexoncdn.co.kr/bbs/1634520188174b448bb50e5c64320bb3c882a5b438d6d.png "workspace_world") World - ![icon_asset](https://mod-file.dn.nexoncdn.co.kr/bbs/163451962580666b43144ad504f8e9e4a7cc01771207a.png "icon_asset") common**</span> in the hierarchy and click **Add Component** to add the previously created script component **NPCTalk**. <br> ![19](https://mod-file.dn.nexoncdn.co.kr/bbs/16595058192181c0194d1789c49c89a3f6f1057333367.png "19")
5. Play, and then press Z to see if<span style="color: #dc9656"> **Z Key Down**</span> is successfully shown in the log box.
 <br>
![19](https://mod-file.dn.nexoncdn.co.kr/bbs/16372343956955ec785c4f783489fb11dded9ac234f9d.png "19")
6. If the log is correctly displayed, exit the play mode and reopen the **NPCTalk** script. Then, add the properties shown below.
    
    | type | name | value | Sync | Explanation |
    | --- | --- | --- | --- | --- |
    | number | count | 0 |none  | Used to determine rowIndex of the data that the script will refer to.<br>Increases the value by 1 each time the data is set, so that it can refer to the data in the next line.|
    | any | npcTalkData | nil | none |  Assigns a data table to refer to. The value will be set in OnBeginPlay.<br>Default value is nil.|
    | Entity | uiNameEntity | nil | none |  Used to edit value of Name entity or to control Enable.|
    | Entity | 	uiMessageEntity |nil  | none |  Used to edit value of Text entity or to control Enable.|
    | Entity |	uiTalkPanel|nil  |none  | 	Used to control Enable of TalkPanel entity.| |
    | Entity |  	uiPortraitEntity|nil  |none  | Used to edit value of Portrait entity or to control Enable.| |
    
7. click the **+** button to add the `OnBeginPlay()` function.
    ![7-1](https://mod-file.dn.nexoncdn.co.kr/bbs/163781675592619b16165adee45298636a2f972e97f68.png "7-1")
     Click the **[⋮] menu - Execution Space Setting** on the right side of the `OnBeginPlay()` function.
    ![7-2](https://mod-file.dn.nexoncdn.co.kr/bbs/166078883036715c2ef01a89349dd938635d783a48dda.png "7-2")
    Click **ClientOnly**.
    ![7-3](https://mod-file.dn.nexoncdn.co.kr/bbs/1656484055426cc33b555375b4d83a4d47def76621193.png "7-3")
   Add the following to the `OnBeginPlay()` function:
    
    ```lua
    [Client only]
    void OnBeginPlay() 
    {
        self.count = 1
        self.npcTalkData = _DataService:GetTable("NPCTalk")
        self.uiNameEntity = _EntityService:GetEntityByPath("/ui/UIGroup/TalkPanel/Name")
        self.uiMessageEntity = _EntityService:GetEntityByPath("/ui/UIGroup/TalkPanel/Text")
        self.uiTalkPanel = _EntityService:GetEntityByPath("/ui/UIGroup/TalkPanel")
        self.uiPortraitEntity = _EntityService:GetEntityByPath("/ui/UIGroup/TalkPanel/Portrait")
    }
    ```
    
9. Delete the logging statement from the previously added `ShowNextText()` function and add the following code:
    ```lua
    void ShowNextText()
    {
        local isNameEnable = false
        local isPortraitEnable = false
         
        local message = self.npcTalkData:GetCell(self.count, "text")
         
        if message == nil then
            self.uiTalkPanel.Enable = false
            return
        else
            self.uiTalkPanel.Enable = true
            self.uiMessageEntity.TextComponent.Text = message
        end
         
        local name = self.npcTalkData:GetCell(self.count, "name")
        local portrait = self.npcTalkData:GetCell(self.count, "portrait")
         
        if name ~= "" then
            isNameEnable = true
            self.uiNameEntity.TextComponent.Text = name
        end
         
        if portrait ~= "" then
            isPortraitEnable = true
            self.uiPortraitEntity.SpriteGUIRendererComponent.ImageRUID = portrait
        end
         
        self.uiNameEntity.Enable = isNameEnable
        self.uiPortraitEntity.Enable = isPortraitEnable
         
        self.count = self.count + 1
    }
    ```
    <br>
 The operation of the `ShowNextText()` function is as follows:
    <br>
    To determine if Name and Portrait will be enabled, it <span style="color: #dc9656">**declares local variable isNameEnable, isPortraitEnable and resets the default value to false**</span>. <br>
 The `GetCell()` function is used to retrieve the value corresponding to the "text" column in the self.count row from the data, assigning it to the local variable message. If it fails to retrieve the text value, nil is returned. In this case, it is determined that the text box cannot proceed, and the conversation window is closed, ending the operation of `ShowNextText()`. Afterward, it retrieves the name and portrait values from the data. <br>
    The values may not exist, but it will not affect the progress of the dialogue box. Text may be only used for the body content without any Portrait or Name.<br>
    Empty cells will return empty strings instead of nil. 
    If the value exists, name will assign Name entity's Text, portrait will assign it to Portrait entity's ImageRUID, and assign true to the local variable that will determine whether to enable the entity. 
    Assign the previous local variable's value to the Enable of Name entity and Portrait entity, increase count property value by 1 to bring value from the next row in the next call.
    <br>
10. Let's save and play. Whenever you press Z, the information in the dialogue box will change to proceed with the dialogue. When all dialogue have been displayed, the box will close.
    <br>
    The outcome will be as shown below:
    ![9-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16569204420795ce614194a30458681b3428319f65614.png "9-1")
