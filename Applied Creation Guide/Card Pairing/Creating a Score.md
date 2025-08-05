# Creating a Score
Let's make matching cards increase the player's score.
![example](https://mod-file.dn.nexoncdn.co.kr/bbs/1671093611720833ddeaafc31418581ba3eca0c4a6e84.gif{"width":"740px"} "example")
1.  Add the <span style="color: #dc9656">**PlayerStats**</span> component to **DefaultPlayer**.  The <span style="color: #dc9656">**PlayerStats**</span> component increases the score. 
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1674715388906d70c2059ca454e0a8bc23f97ddb2edad.png "1")

2. Add a number type property in the <span style="color: #dc9656">**PlayerStats**</span> component.
    ```lua
    Property:
    [None]
    number Score = 0
    ```

3. Enter the following into the `AddScore` function to add the score to the **Score** property with the initial value of 0.
Add a parameter and set its type to number.

    ```lua
    Method:
    void AddScore (number add)
    {
        self.Score = self.Score + add
    }
    ```

4.  The `AddScore` function will be executed when matching cards during the game. Write the following in the line below `newCard:SetDestroyed()` in `OnCardClicked` of **CardPairGameLogic**. 

    ```lua
        local me = _UserService.LocalPlayer
        me.PlayerStats:AddScore(10)
    ```

#### Create a Timer in UI
1. Let's modify **UpdateText** a bit to display the incrementing score to the UI, as with the timer. Add and apply the **ScoreText** property in the <span style="color: #dc9656">**CardBoard**</span> component.
Change the **ScoreText** property type to Entity and specify **UI_Score_Text**.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1674715648067f7cd214b25ae4bc59424b3cd2aafadfd.png "1")
    ```lua
    Property:
    [None]
    Entity ScoreText = /ui/CardPairGameUI/UI_Board/Background/Title/Score_Backgrond/UI_Score/UI_Score_Text
    ```

2. In the `UpdateText()` function of the <span style="color: #dc9656">**CardBoard**</span> component, write the following in the line below `self.TimeText.TextComponent.Text = tostring(math.floor(remainingTime))`.

    ```lua
    local me = _UserService.LocalPlayer
    self.ScoreText.TextComponent.Text = tostring(math.floor(me.PlayerStats.Score))
    ```

3. Press Start to match the card pairs. Check if the incrementing score is reflected in the UI when the player matches card pairs.


# Full Script
#### PlayerStats Component
```lua
Property:
[None]
number Score = 0

Method:
void AddScore (number add)
{
    self.Score = self.Score + add
}
```


#### CardBoard Component
```lua
Property:
[None]
Entity CardSpawnRoot = /ui/CardPairGameUI/UI_Board/UI_Blocks/UI_Block_Group
[None]
Card SelectedCard = nil
[None]
Entity TimeText = /ui/CardPairGameUI/UI_Board/Background/Title/Timer_Background/UI_Time/UI_Time_Text
[None]
Entity ScoreText = /ui/CardPairGameUI/UI_Board/Background/Title/Score_Backgrond/UI_Score/UI_Score_Text

Method:
[client only]
void OnUpdate (number delta)
{
    self:UpdateText()
}

[client only]
void UpdateText()
{
    local remainingTime = _CardPairGameLogic.RemainingTime
    self.TimeText.TextComponent.Text = tostring(math.floor(remainingTime))
    
    local me = _UserService.LocalPlayer
    self.ScoreText.TextComponent.Text = tostring(math.floor(me.PlayerStats.Score))
}

void SetCards ()
{
    local deckTable = {}
    local cardTable = {}
    
    local deckCount = _DataService:GetRowCount("CardSet")
    
    for i = 1, deckCount do
    	local spriteRUID = _DataService:GetCell("CardSet", i, 1)
    	table.insert(deckTable, spriteRUID)
    end
    
    for i = 1, deckCount do
    	local randomIndex = _UtilLogic:RandomIntegerRange(1, deckCount)
    	local temp = deckTable[i]
    	deckTable[i] = deckTable[randomIndex]
    	deckTable[randomIndex] = temp
    end
    
    for i = 1, (deckCount / 2) do
    	table.insert(cardTable, deckTable[i])
    	table.insert(cardTable, deckTable[i])
    end
    
    for i = 1, #cardTable do
    	local randomIndex = _UtilLogic:RandomIntegerRange(1, #cardTable)
    	local temp = cardTable[i]
    	cardTable[i] = cardTable[randomIndex]
    	cardTable[randomIndex] = temp
    end
    
    self:ClearCardEntities()
    self:CreateCardEntities(cardTable)
}

[client only]
void CreateCardEntities (table cardTable)
{
    local cardModelId = "Model_Card EntryID"
    local cardCount = #cardTable
    
    for i = 1, cardCount do
    	local spriteRUID = cardTable[i]
    	
    	local card = _SpawnService:SpawnByModelId(cardModelId, "Card " .. tostring(i), Vector3(0,0,0), self.CardSpawnRoot)
   	
    	card.Card.SpriteRUID = spriteRUID
    	card.SpriteGUIRendererComponent.ImageRUID = spriteRUID
    end
}

[client Only]
void ClearCardEntities ()
{
    local cardEntities = self.CardSpawnRoot.Children
        
     for i = 1, #cardEntities do
         cardEntities[i]:Destroy()
     end
}

[client only]
void SelectCardCard (Card card)
{
    self.SelectedCard = card
    card:CardOn()
}

[client only]
void DeselectCard()
{
    self.SelectedCard:CardOff()
    self.SelectedCard = nil
}

[client only]
boolean IsEmpty ()
{
    local cards = self.CardSpawnRoot.Children
            
    for i = 1, #cards do
        if cards[i].Visible then
            return false
        end
    end
            
    return true
}
```

#### CardPairGameLogic logic

```lua
Property:
[None]
Entity CardPairGameUI = /ui/CardPairGameUI
[None]
number PlayTime = 60
[None]
number RemainingTime = 0

Method:
[client only]
void OnBeginPlay()
{
    self:StartGame()
}

[client only]
void StartGame()
{
    self.CardPairGameUI.Enable = true
    
    self.RemainingTime = self.PlayTime
    
    local cardboard = self.CardPairGameUI.CardBoard
    cardboard:SetCards()
}

[client only]
void OnUpdate (number delta)
{
    self.RemainingTime = self.RemainingTime - delta
    
    if self.RemainingTime <= 0 then
    self:EndGame()
    end
}

[client only]
void OnCardClicked (Card card)
{
    if isvalid(card) == false then
    	return
    end
    
    local cardboard = self.CardPairGameUI.CardBoard
    
    if isvalid(cardboard) == false then
    	return
    end
    
    local oldCard = cardboard.SelectedCard
    local newCard = card
    
    
    if oldCard == nil then
    	cardboard:SelectCard(newCard)
    	
    elseif oldCard:IsPairOf(newCard) then
    	cardboard:DeselectCard()
    	oldCard:SetDestroyed() 
    	newCard:SetDestroyed()
    	
       local me = _UserService.LocalPlayer
    	me.PlayerStats:AddScore(10)
    	
       if cardboard:IsEmpty() then
            cardboard:SetCards()
       end
    else
    	cardboard:DeselectCard()
    end
}

[client only]
void EndGame ()
{
    self.CardPairGameUI.Enable = false
}
```