Remade Worlds contain the UI, resources, and empty components required for further creation. We'll discuss how to complete the game together by filling in each of these elements.

# Activate CardPairGameUI

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1670403831746fd0717f80a5345a9a5e90b947c4bdbc5.png%7B%22width%22:%22640px%22%7D)

We'll start by changing **CardPairGameUI** to **Enable** when the user enters the World. Use Logic to create rules or logic that apply directly to the World.
In the **CardPairGameLogic** logic, we will dictate overall rules such as turning on/off the basic UI for matching and card input.

First, we'll make the game UI appear to players who enter the World. Before adding the script below, the UI will not turn on even though there is a game board in the UI group. To display the UI, the Enable property must be set to True.

```lua
Property:
[None]
Entity CardPairGameUI = /ui/CardPairGameUI

Method:
[client only]
void StartGame()
{
    self.CardPairGameUI.Enable = true
}

[client only]
void OnBeginPlay()
{
    self:StartGame()
}
```

# Create Cards
Now that the UI shows up, we should make a card. 
We'll need to display multiple cards on the screen at the same time. In this case, it is better to create a template for all cards than to create each card one at a time. If you need an entity with the same properties multiple times, you can use a model. A model is a version that contains information about the components and properties of an entity. It is also used by specifying a sprite in the model to create a monster with the same behavior, or by simply placing the same object. But instead of creating a model and placing it in the world, let's make it summon to the UI according to a specific logic when the world executes. 
Please refer to [Utilizing Models] for the details of a model.

So, you know what we need to do first, right? First, we'll create a card model.

#### Checking the Model_Card Size

1. Select **Workspace - MyDesk - Models - Model_Card**.
2. Sets the default image for a model.  Use the white image in **ImageRUID** of **SpriteGUIRendererComponent** or the transparent RUID below.

* Transparent RUID: 98c34caab88ee34459cb3e5807ac4219

#### Creating a Card Data Set

Select **Workspace - MyDesk - DataSets - CardSet**. You can load various images to the **Model_Card** made above with this data set. Twenty-nine SpriteRUID are already saved. We need 30 total, so let's find the last one ourselves.

1. Open **Resource Storage** and select **MSW Resources**.
2. Find square-shaped sprites to be used in **Sprite - Item**.
3. Select **Copy RUID**.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16704048938740f43af6881ca4b73a9110554ed0ecc8f.png "3")
4. Paste the copied RUID into <span style="color: #dc9656">**CardSet**</span>.
    ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16704113017760020d152c0be407a86c703472bcfe157.png)

> <span style="color: #7cafc2">**Tip**
> If you want to see two windows on one screen at the same time, you can adjust the position of the CardSet panel. </span>

> <span style="color: #7cafc2">**Tip**
> After creating the DataSet using Google Spreadsheet or Excel, you can load it into CardSet.
> Please refer to the [Edit Data] guide.</span>

# Creating a Deck

Now it's time to bring the card to the screen. Utilize <span style="color: #dc9656">**/ui/CardPairGameUI/UI_Board/UI_Blocks/UI_Block_Group**</span> to bring the card.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16704128182139ae9ca7ae6324a899544f763688b7506.png%7B%22width%22:%22740px%22%7D)
<span style="color: #dc9656">**UI_Block_Group**</span> utilized [ScrollLayoutGroupComponent](/apiReference?postId=382%7B%22target%22:%22_self%22%7D). When you need to expand multiple entities, use **ScrollLayoutGroupComponent**. It is mainly useful for creating a UI with many images that are seen together, such as for a shop or inventory. The size and placement of child UI Entities can be applied collectively.
Find **UI\_Block\_Group** in the Hierarchy and open the Property Editor to check the property values. After confirming that the values are the same as below, let's write a script that summons a card.

![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1674110690186be01644f71954e2e8184dad5dbd2d07b.png "6")

| Property | Value | Description |
| ---- | --- | --- |
| Padding | top:15, right, left, bottom:0 | Sets the free space at the top to 15. |
| CellSize | 90, 90 | Set the size of the unfolded card to 90, 90. |
| Constraint | FixedColumnCount | Set ScorollLayoutGroupComponent. Since you set it to FixedColumnCount, the number entered in ConstraintCount is applied to the board. |
| ConstraintCount | 6 | Fix the number of child UI entities to be displayed horizontally at 6. |
| Spacing | 95, 50 | Sets the spacing between cards. |


# Loading Model_Card

Let's make a script to load **Model_Card** into UI_Block_Group.

#### CardBoard component
1. Add the **CardBoard** component to the **CardPairGameUI** group.
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1674170611269041acb09868d4821a7cb09b910b252e6.png "7")
2. Click the **CardBoard** component to open the Script Editor.
3. Add the **CardSpawnRoot** property and change the return type to Entity. 
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/1671521982130c05a89c1ff844e5a87c22611a8bd4d0f.png "12")

4. Save the Entity as **UI_Block_Group**.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1671522029484e4d94cd892d540c28d9c500c497b2e79.png{"width":"420px"} "11")
    ```lua
    Property:
    [None]
    Entity CardSpawnRoot = /ui/CardParirGameUi/UI_Board/UI_Blocks/UI_Block_Group
    ```
    
4. Find the `CreateCardEntities` function in Function. This function serves to summon and place Model_Card. 

5. Click **Workspace - MyDesk - Models - Model_Card**, open the context menu, and select **Copy Entry ID**.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1671518293286d5743eb04f424a0d9bdb4405962b6230.png{"width":"420px"} "8")

6. You can use the copied model ID in the `CreateCardEntities` function. Assign the copied Entry ID to local cardModelId as shown below.

    ```
    local cardModelId = "Model_Card Entry ID"
    ```

7.  Specify where to load the card model using `SpawnService:SpawnByModelId()` in the `CreateCardEntities` function. In turn, you must enter the model ID, name, import location, and parent, which is the standard for the import location. Please refer to [SpawnService](/apiReference/Services/SpawnService{"target":"_self"}).

    ```lua
    Method:
    [client only]
    void CreateCardEntities()
    {
        local cardModelId = "Model_Card EntryID"
        
        local card = _SpawnService:SpawnByModelId(cardModelId, "Card", Vector3(0,0,0), self.CardSpawnRoot)
        }
    ```

8. Find the new `SetCards` function in Function. This function is responsible for placing and shuffling the cards in the future. Call the `CreateCardEntities` function created earlier.

    ```lua
    Method:
    [client only]
    void SetCards()
    {
        self:CreateCardEntities()
    }
    ```

#### CardPairGameLogic Logic

1. Click the **WorkSpace - Logics - CardPairGameLogic** logic to open the Script Editor window.
    Add the following content to `StartGame()` to make `SetCards` execute.

    ```lua
    local cardboard = self.CardPairGameUI.CardBoard
    cardboard:SetCards()
    ```
2. Press Start to see one card appear on the screen.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1671415272545b3de4b80996643dd8ffa241a870dfaee.png%7B%22width%22:%22640px%22%7D)

# Full Script

#### CardPairGameLogic

```lua
Property:
[None]
Entity CardPairGameUI = /ui/CardPairGameUI

Method:
[client only]
void StartGame()
{
    self.CardPairGameUI.Enable = true
    
    local cardboard = self.CardPairGameUI.CardBoard
    cardboard:SetCards()
}

[client only]
void OnBeginPlay()
{
    self:StartGame()
}
```

#### CardBoard

```lua
Property:
[None]
Entity CardSpawnRoot = /ui/CardParirGameUi/UI_Board/UI_Blocks/UI_Block_Group

Method:
[client only]
void CreateCardEntities()
{
    local cardModelId = "Model_Card EntiryID"
    
    local card = _SpawnService:SpawnByModelId(cardModelId, "Card", Vector3(0,0,0), self.CardSpawnRoot)
}

[client only]
void SetCards()
{
    self:CreateCardEntities()
}
```