# Matching
Now that we've changed the color of the cards, let's select two cards this time and make them disappear when they match.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1671105090872a6b52b1bb84b443ea0cab8a35df43850.gif "1")

1. Write the following in the `SetDestroyed` function from <span style="color: #dc9656">**Card component**</span>.
Disable by changing the Visible property of the card entity to false. This is used in the destroyed state.

    ```lua
    [client only]
    void SetDestroyed ()
    {
        self.Entity.Visible = false
    }
    ```

2. Write the `IsPairOf()` function of the **Card** component as below to check if it is matched.

    ```lua
    [client only]
    boolean IsPairOf (Card other)
    {
        return (self.SpriteRUID == other.SpriteRUID) and (self.Entity ~= other.Entity)
    }
    ```

4. Let's modify the `OnCardClicked()` function of the <span style="color: #dc9656">**CardPairGameLogic**</span> logic a bit. Find `if oldCard == nil then` and write the following on the bottom line.
If the two selected cards are of the same suit, clear them and give points.

    ```lua
    if oldCard == nil then
    	cardboard:SelectCard(newCard)
    	
    elseif oldCard:IsPairOf(newCard) then
    	 cardboard:DeselectCard()
    	 oldCard:SetDestroyed() 
    	 newCard:SetDestroyed()
    else
    	cardboard:DeselectCard()
    end
    ```

![2](https://mod-file.dn.nexoncdn.co.kr/bbs/167110525627248f8e6860c274cb7b09a400838010e05.gif "2")

# Reload
You've finished one deck, but there's no new deck. Let's make a new deck appear when all cards are matched.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/167110527578284fef61b2d424604997cdfe363edab6c.gif "3")

1. Enter the following to check whether all cards are cleared or not in the `IsEmpty()` function of the <span style="color: #dc9656">**CardBoard**</span> component.

    ```lua
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

2. Write the following in the `ClearCardEntities()` function to remove all cards.

    ```lua
    [client only]
    void ClearCardEntities ()
    {   
        local cardEntities = self.CardSpawnRoot.Children
    
        -- Destroy all children of CardSpawnRoot.
        for i = 1, #cardEntities do
        	cardEntities[i]:Destroy()
        end
    }
    ```
    
3.  Call the function by writing the code below in the line above `self:CreateCardEntities(cardTable)` of the `SetCards` function.

    ```
    self:ClearCardEntities()
    ```

4. Find `newCard:SetDestroyed()` in the `OnCardClicked()` function of the <span style="color: #dc9656">**CardPairGameLogic**</span> logic and enter the following on the bottom line.

    ```lua
    if cardboard:IsEmpty() then
        cardboard:SetCards()
    end
    ```

5. Press Start to match all the cards and see if new cards appear.

# Full Script
#### CardBoard Component
```lua
Property:
[None]
Entity CardSpawnRoot = /ui/CardPairGameUI/UI_Board/UI_Blocks/UI_Block_Group
[None]
Card SelectedCard = nil

Method:
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
    	table.insert(cardTable, deckTable[i]))
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

[Client Only]
void ClearCardEntities ()
{
    local cardEntities = self.CardSpawnRoot.Children
        
     for i = 1, #cardEntities do
         cardEntities[i]:Destroy()
     end
}

[client only]
void SelectCard (Card card)
{
    self.SelectedCard = card
    card:CardOn()
}

[client only]
void DeselectCard()
{
    self.SelectedCard:CardOff()
    self.selected card = nil
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

#### Card Component
```lua
Property:
[None]
string SpriteRUID = ""

Method:
void CardOn()
{
    self.Entity.SpriteGUIRendererComponent.Color = Color(81/255,81/255,81/255,1)
}

void CardOff()
{
    self.Entity.SpriteGUIRendererComponent.Color = Color(255/255,255/255,255/255,1)
}

void SetDestroyed()
{
    self.Entity.Visible = false
}

boolean IsPairOf (Card other)
{
    return (self.SpriteRUID == other.SpriteRUID) and (self.Entity ~= other.Entity)
}

Entity Event Handler:
[self]
{
    -- Parameters
    local Entity = event.Entity
    --------------------------------------------------------
    if self.Entity.Visible == false then
    	return
    end

    _CardPairGameLogic:OnCardClicked(self)
}
```

#### CardPairGameLogic Logic
```lua
Property:
[None]
Entity CardPairGameUI = /ui/CardPairGameUI

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

    local cardboard = self.CardPairGameUI.CardBoard
    cardboard:SetCards()
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
```