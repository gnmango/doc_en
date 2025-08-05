Let's create a Model_Scroll_Item and import the contents of SkillDatas to the model.

![0](https://mod-file.dn.nexoncdn.co.kr/bbs/17086507096040d905f2bebc94b59a3cf1ae7fa353d07.gif "0")

# Creating a Scroll_Item model
Let's create a button to be displayed in the Scroll_Layout UI. 
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/17085888271740460ae3fc12542069d89190e56c2ec45.png{"width":"540px"} "1")
1. Add the **Button** in the Scene and rename it to **Scroll_Item**.
    * ImageRUID: 5813efbad2403444786f6955f97649fb
    * RaycastTarget: enable
2. Add the new **UIWeaponItemComponent** to **Scroll_Item**.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1708589370688dd9415df51ed4bc88331cf4f58562963.png "3")
3. Add **UISprite** as a child entity and rename it to **UIImageWeapon**.
4. Add **UIText** as a child entity and rename it to **UITextName**. This UI entity is used to show the name of the weapon.
5. Press **Create Model From Entity** to model the UI.

# Create UIAvatarPreviewComponent
Let's retrieve the weapon information and import it to **Model_Scroll_Item**.
1. Add **UIAvatarPreviewComponent** in **UIAvatarPreview**.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1708596278635c5fbeb35265648bc9f50b7b1f1b805b5.png "7")

2. Add new **UIRoot, UIScrollRoot, UIItemModelID** properties.

    ```lua
    [None]
    EntityRef UIRoot = /ui/UIGroup/UIAvatarPreview
    [None]
    ScrollLayoutGroupComponentRef UIScrollRoot = /ui/UIGroup/UIAvatarPreview/Scroll_Layout
    [None]
    string UIItemModelID = "model://Model_Scroll_Item Entry Id"
    [None]
    any SkillDatas = nil
    ```
    ><span style="color: #7cafc2">**Tip.**
    > You can use the Copy Entry ID to copy the ID.</span>


3. Add the new AddUIScrollWeaponItem() function. Add **number-type index, any-type row** parameters.
Write as below to use SpawnService to import the **UIItemModel** entity and place it.

    ```lua
    Method:
    [client only]
    void AddUIScrollWeaponItem (number index, any row)
    {
        local id = row:GetItem("dataID")
        local weaponRUID = row:GetItem("weaponRUID")
    
        local spawnPos = Vector3.zero
        spawnPos.y += index * -10
    
        local spawn = _SpawnService:SpawnByModelId(self.UIItemModelID, id, spawnPos, self.UIScrollRoot.Entity)
    }
    ```
    
4. Add the new LoadData() function. Write as below to use DataService to import the information of **SkillData** dataset.

    ```lua
    [client only]
    void LoadData()
    {
        self.SkillDatas = _DataService:GetTable("SkillDatas")
        
        if self.SkillDatas ==nil then log_error("failed to load table") return end
        
        for i = 1,self.SkillDatas:GetRowCount() do
        	local row = self.SkillDatas:GetRow(i)
        	self:AddUIScrollWeaponItem(i, row)
        end
    }
    ```

5. Add a OnBeginPlay() function. Load the LoadData function, and then write as follows to change the UIRoot state as well.

    ```lua
    [client only]
    void OnBeginPlay()
    {
        self.UIRoot.Enable  = false
        self:LoadData()
    }
    ```
   
# Entering SkillDatas
Let's create a dataset that contains the Weapon RUID, Skill RUID, location to place the skill, and the delay time.
1. Select **Workspace - MyDesk - Create DataSet** to create a new **SkillDatas**.
2. Enter the data as follows.

| dataID | weaponRUID | is2handWeapon | skillRUID | skillOffsetX |skillOffsetY  | skillDelay | 
| --- | --- | --- | --- | --- | --- | --- |
| sword | 257d484f10664c6<br>ab77b431545738dec | false | 04f2bea640ee48da<br>b4a8e9a7924a3aab | 0 | -50 | 0.5 | 
| dagger |  1489702f013a4ba<br>4a1ec080da4df17c3| false | 0d7381204f9b47<br>869a705a16cf0344d1 | 0 | -50 | 0.3 | 
| bow | 09facca645ee4e83<br>aea62a1da2c41347 | true | 387ec9be0573409<br>da315e5b3392f41e4 | 0 | 50 | 0.8 | 
| staff | 11c2b4cd6ab34cebb<br>e4b583e14ae6d0f | false | 1109be74331d4d<br>07946a978ada5b6311 | 0 | 0 | 0.5 | 
| wand | 061712eadf7d4357<br>a6c306dfd5368860 | false | 00613b24f0c045d<br>eab14ba24cdb90187 | 0 | -100 | 0.8 | 
| hammer | 2c61687c8325422<br>1af22e8cbdd0afd2b | true | fed5e1c5959b49e<br>eb43a04fa802a3ce9 | 0 | 100 | 0.5 | 

# Importing Data 

#### Create UIWeaponItemComponent
1. Select **Workspace - MyDesk - Create Script - Create Component** to add a new **UIWeaponItemComponent**.
2. Add new **DataID, WeaponImage, NameText, AvatarPreviewRef** properties in **UIWeaponItemComponent**.

    ```lua
    Property:
    [None]
    string DataID = ""
    [None]
    SpriteGUIRendererComponentRef WeaponImage = nil
    [None]
    TextComponent NameText = nil
    [None]
    UIAvatarPreviewComponentRef AvatarPreviewRef = nil
    ```
    
3. Write a new SetData() function. Add **dataID, weaponRUID** parameters and write as follows to import the data.

    ```lua
    Method:
    [client only]
    void SetData (string dataID, string weaponRUID)
    {
        self.DataID = dataID
        self.WeaponImage.ImageRUID = "thumbnail://"..weaponRUID
        self.NameText.Text = tostring(self.DataID)
    }
    ```
    
4. Add a SetAvatarPreviewComponentRef() function.
    ```lua
    [client only]
    void SetAvatarPreviewComponentRef (UIAvatarPreviewComponentRef mainUI)
    {
        self.AvatarPreviewRef = mainUI
    }
    ```

#### Modify UIAvatarPreviewComponent
At the end, write as follows to call the functions of **UIWeaponItemComponent** from AddUIScrollWeaponItem().

```lua
spawn.UIWeaponItemComponent:SetAvatarPreviewComponentRef(self)
spawn.UIWeaponItemComponent:SetData(id, weaponRUID)
```
    
#### Modifying Model_Scroll_Item
1. Select **Model_Scroll_Item** and place it in the Scene. In the Property Editor window, add **UIWeaponItemComponent**.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/17085932952390ac68af6c65345708c0bcf3a4894a280.png "5")
2. Set a path for each property of **UIWeaponItemComponent** in the Property Editor window as follows.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1713953204775053035c869ea46b9b843307f0f838f05.png{"width":"640px"} "6")
3. Press **Property - Model Extension Info - Apply**.
![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1713949120143ec54309645e04d619a0144e6464d918c.png{"width":"430px"} "13")
4. In the **Apply to Model** window, press **UIWeaponItemComponent - UIWeaponComponent - Apply** to apply it.
![14](https://mod-file.dn.nexoncdn.co.kr/bbs/17139491430101c6cf1827b36415386fc395979b66515.png{"width":"740px"} "14")
5. Press the **[Start]** button to check if the UI can be turned on and off.