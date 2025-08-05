# Creating a Timer
Let's create a timer that counts down by 1 second when the game starts.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/167115941466273a4cd2377584375aea624be4f22e70b.gif "1")
1. Add the <span style="color: #dc9656">**PlayTime**</span> property to the **CardPairGameLogic** logic to use as the game timeout and the <span style="color: #dc9656">**RemainingTime**</span> property to reduce time.
The initial value of PlayTime is set to **60**.
    ```lua
    Property:
    [None]
    number PlayTime = 60
    [None]
    number RemainingTime = 0
    ```

    > <span style="color: #7cafc2">**Tip**
    > The initial value of PlayTime is 60, but we recommend entering a smaller number for quick confirmation during production so that the game stops sooner.</span>

2. Write the following for **RemainingTime** to decrease every second in the `OnUpdate` function of the **CardPairGameLogic** logic. 
    ```lua
    Method:
    [client only]
    void OnUpdate (number delta)
    {
        self.RemainingTime = self.RemainingTime - delta
        log(self.RemainingTime)
    }
    ```

3. Add the following script to `StartGame()` from the CardPairGameLogic logic.
    ```lua
        self.RemainingTime = self.PlayTime
    ```

4. Click Start to see if the time-decreasing logs output to the Console. If the number continues to decrease from 60, you have successfully created the timer!
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16711645661701a4073540a4e428bb71dad4e6b3b5548.gif "3")

    > **Learn more**
    > After confirming that the time decreases, we recommend erasing or commenting in the log.

#### Reflecting in the Timer UI
Now that we've confirmed in the log that the RemainingTime time is decreasing, let's make the time appear in the UI.

1. Set the text component for the timer in the **TimeText** property of the **CardBoard** component.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/167419874423331076a5054e84717bc29a4657db88794.png "1")

    ```lua
    Property:
    [None]
    Entity TimeText = /ui/CardPairGameUI/UI_Board/Background/Title/Timer_Background/UI_Time/UI_Time_Text
    ``` 

2. Write the following to make it display the time in the `UpdateText` function.

    ```lua
    [client only]
    void UpdateText()
    {
        local remainingTime = _CardPairGameLogic.RemainingTime
        self.TimeText.TextComponent.Text = tostring(math.floor(remainingTime))
    }
    ```

3. Enter the following in OnUpdate to decrease time by calling the `UpdateText()` function for every update.

    ```lua
    [client only]
    void OnUpdate (number delta)
    {
        self:UpdateText()
    }
    ```


# Game Over
We've finished the timer, but we should add one more feature. The number continues to decrease to a negative number after the timer reaches 0. Generally, when decreasing the count, you would expect the game to stop at 0 seconds. Let's make the UI turn off at 0 seconds.

![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1671156559730cbdccffb65eb49d5a990657301bfcee8.gif "2")

1. Write the following in the `EndGame` function of the <span style="color: #dc9656">**CardPairGameLogic**</span> logic.

    ```lua
    [client only]
    void EndGame ()
    {
       self.CardPairGameUI.Enable = false
    }
    ```

2. Write the following to call `EndGame()` when **RemainingTime** turns 0 seconds or less than in `OnUpdate`.

    ```lua
    if self.RemainingTime <= 0 then
    	self:EndGame()
    end
    ```

3. Press Start to check that the game stops at 0 seconds.


# Full Script

####  CardBoard Component
```lua
Property:
[None]
Entity CardSpawnRoot = /ui/CardPairGameUI/UI_Board/UI_Blocks/UI_Block_Group
[None]
Card SelectedCard = nil
[None]
Entity TimeText = /ui/CardPairGameUI/UI_Board/Background/Title/Timer_Background/UI_Time/UI_Time_Text

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