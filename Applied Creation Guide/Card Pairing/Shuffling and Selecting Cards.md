# Shuffling Cards
Now that we've succeeded in showing the cards, let's make a pair of cards appear.

1. Find the third `for i = 1, deckCount do` in the `SetCards()` function of the <span style="color: #dc9656">**CardBoard**</span> component. 
Originally, this executed the number of deckCount, but for this situation, we'll change it to (deckCount / 2) to execute half the deckCount. Bring those halves up twice to make the paired stacks.
Please refer to the [table](/apiReference/Lua/table{"target":"_self"}) reference for `table.insert`.

    ```lua
    for i = 1, (deckCount / 2) do
    	table.insert(cardTable, deckTable[i])
    	table.insert(cardTable, deckTable[i])
    end
    ```

2. Once you've loaded the matching table, it's time to shuffle it again. Write the code below to shuffle the loaded cards in the same way as shuffling the deck table.
Enter this right below the code you wrote above.

    ```lua
    for i = 1, #cardTable do
    	local randomIndex = _UtilLogic:RandomIntegerRange(1, #cardTable)
    	local temp = cardTable[i]
    	cardTable[i] = cardTable[randomIndex]
    	cardTable[randomIndex] = temp
    end
    ```

# Change Color When Selected
Now that we've floated the cards the way we want, let's make them tappable. 
After confirming that the card is tapped, change the color of the tapped card to better highlight the tapped condition.

1. Select **Model_Card** and add [ButtonComponent](/apiReference/Components/ButtonComponent{"target":"_self"}). Unlike before, when you tap the card, you can see that it blinks.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16711036169973a9e5d0cbe6a4f2ba6f773cb9c35e285.png{"width":"420px"} "4")
2. Since you have to tap one card and then another card, it is easier to match pairs if you clearly recognize which card you tapped.
Find the <span style="color: #dc9656">**Card**</span> component to open the Script Editor window. 
3. Write as below in the `CardOn` function. Colors are added as RGB values, with the value below equaling a gray color. 
    ```lua
    void CardOn()
    {
        self.Entity.SpriteGUIRendererComponent.Color = Color(81/255,81/255,81/255,1)
    }
    ```

4. Write as below in the `CardOff` function. The RGB value below equals white. 
    ```lua
    void CardOff()
    {
        self.Entity.SpriteGUIRendererComponent.Color = Color(255/255,255/255,255/255,1)
    }
    ```

5. When you press the card, the **ButtonClickEvent** occurs. Let's make it possible to call the previous **CardPairGameLogic** whenever this event occurs.
Add <span style="color: #dc9656">**ButtonClickEvent**</span> into **Entity Event Handler** of the <span style="color: #dc9656">**Card**</span> component.

    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16711043132112cde79b920754572868c361fb1e7ed89.png "3")

6. Write the following:

    ```lua
    Event Handler:
    [self]
    HandleButtonClickEvent (ButtonClickEvent event)
    {
        -- Parameters
        local Entity = event.Entity
        --------------------------------------------------------
        if self.Entity.Visible == false then
        	return
        end
        
        _CardPairGameLogic:OnCardClicked()
    }
    ```
    
# Calling Functions
1. In the <span style="color: #dc9656">**CardBoard**</span> component, add the new property and set the property type to **Component**.
    ```lua
    Property:
    [None]
    Card SelectedCard = nil
    ```

2. Find the `SelectCard` function in the CardBoard component. Add a parameter and change the type to Component. 
Set the component to <span style="color: #dc9656">**Card**</span>.
![4-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1674190213175d894e621ce944635ba0296cb2013333c.png "4-1")
    ```lua
    [client only]
    void SelectCard (Card card)
    {
        self.SelectedCard = card
        card:CardOn()
    }
    ```

3. Find the `DeselectCard` function in the CardBoard component and then write as follows.
    ```lua
    [client only]
    void DeselectCard()
    {
        self.SelectedCard:CardOff()
        self.SelectedCard = nil
    }
    ```

4. Add a parameter to the `OnCardClicked` function in the <span style="color: #dc9656">**CardPairGameLogic**</span> logic. Change the type to Component and set it to <span style="color: #dc9656">**Card**</span>.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1671104951796ce0308ccebce4832826c7e4509a83a20.png "4")

5. Add the following in the `OnCardClicked` function.

    ```lua
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
        else	
            cardboard:DeselectCard()
        end
    }
    ```

6. Add a parameter to the `OnCardClicked` function written in the **ButtonClickEvent**</span> handler of the <span style="color: #dc9656">**Card**</span> component.

    ```lua         
        _CardPairGameLogic:OnCardClicked(self)   
    ```

5. Press Start to see if the card changes color when tapped, and returns to the original color when tapped again.

# Full Script
#### CardBoard component
```lua
Property:
[None]
Entity CardSpawnRoot = /ui/CardParirGameUi/UI_Board/UI_Blocks/UI_Block_Group
[None]
Card SelectedCard = nil

Method:
[client only]
Void SetCards()
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
        
    self:CreateCardEntities(cardTable)
}      

[client only]
void CreateCardEntities(table cardTable)
{
    local cardModelId = "Model_Card EntryID"
    local cardCount = #cardTable
        
    for i = 1, cardCount do
        local spriteRUID = cardTable[i]
        
        local card = _SpawnService:SpawnByModelId(cardModelId, "Card" .. tostring(i), Vector3(0,0,0), self.CardSpawnRoot)
        
        card.Card.SpriteRUID = spriteRUID
        card.SpriteGUIRendererComponent.ImageRUID = spriteRUID
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
    self.SelectedCard = nil
}
```

#### Card Component

```lua
Property:
[None]
string SpriteRUID = ""

Method:
void CardOn ()
{
    self.Entity.SpriteGUIRendererComponent.Color = Color(81/255,81/255,81/255,1)
}

void CardOff ()
{
    self.Entity.SpriteGUIRendererComponent.Color = Color(255/255,255/255,255/255,1)
}

Event Handler:
[self]
HandleButtonClickEvent (ButtonClickEvent event)
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

#### CardPairGameLogic logic

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
    else	
        cardboard:DeselectCard()
    end
}
```