Now that you've succeeded in revealing one card, it's time to do the same with all the cards.
Let's make all the cards appear as shown in the picture below.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16710997421625b07c67d26324ca3bbe7830e31a6699e.gif "1")

# Show Multiple Cards
Now that you've made sure that one card shows up just fine, it's time to shuffle all the cards and show everything. In this process, we'll use several different functions and the previously completed <span style="color: #dc9656">**CardSet**</span> data set. Please refer to [DataService](/apiReference/Services/DataService{"target":"_self"}).

1. Add a property to retrieve multiple RUID values using the <span style="color: #dc9656">**Card**</span> component.

    ```lua
    Property:
    [None]
    string SpriteRUID = ""
    ```

2. `SetCards()` of the <span style="color: #dc9656">**CardBoard**</span> component is a function that serves to place cards. After deleting the previously written content, enter the following.
The cards are called onto the table, shuffled, and two identical cards are placed in the shuffle.
    
    ```lua
    [client only]
    void SetCards()
    {
        local deckTable = {}
        local cardTable = {}
        
        local deckCount = _DataService:GetRowCount("CardSet")
        
        for i = 1, deckCount do
            local spriteRUID = _DataService:GetCell("CardSet", i, 1)
            table.insert(deckTable, spriteRUID)
        end
        
        for i = 1, deckCount do
        
            -- Randomly shuffle deckCount
            local randomIndex = _UtilLogic:RandomIntegerRange(1, deckCount)
    
            -- Swap places of first card and randomly selected card
            local temp = deckTable[i] 
            deckTable[i] = deckTable[randomIndex] 
            deckTable[randomIndex] = temp 
        
        end
        
        for i = 1, deckCount do
            table.insert(cardTable, deckTable[i])
        end 
    }
    ```

3. Modify `CreateCardEntities()` so that card entities can be created based on the information in cardTable. 


4. Add a new parameter to the `CreateCardEntities()` function and change it to a table type parameter.

    ```lua
    void CreateCardEntities (table cardTable)
    ```

5. Find `local cardModelId = "Model_Card Entry ID"` and write the following on the bottom line.

    ```lua
   local cardCount = #cardTable
   for i = 1, cardCount do
        local spriteRUID = cardTable[i]
   ```
  
6. Find `local card = _SpawnService:SpawnByModelId(cardModelId, "Card", Vector3(0,0,0), self.CardSpawnRoot)` in the `CreateCardEntities()` function. Change `"Card"` to `"Card " .. tostring(i)` and write as follows.
	
    ```lua
        local card = _SpawnService:SpawnByModelId(cardModelId, "Card " .. tostring(i), Vector3(0,0,0), self.CardSpawnRoot)
        card.Card.SpriteRUID = spriteRUID
        card.SpriteGUIRendererComponent.ImageRUID = spriteRUID
    end 
    ```

7. Since you added parameters to the `CreateCardEntities()` function, you also need to enter parameters to `CreateCardEntities()`, which you previously called from the `SetCards()` function of the CardBoard component.

    ```lua
    self:CreateCardEntities(cardTable)
    ```

7. Press Start to see all cards appear.

# Full Script

#### CardBoard Component
```lua
Property:
[None]
Entity CardSpawnRoot = /ui/CardParirGameUi/UI_Board/UI_Blocks/UI_Block_Group

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
    
    for i = 1, deckCount do
    	table.insert(cardTable, deckTable[i])
    end
    
    self:CreateCardEntities(cardTable)
}

[client only]
void CreateCardEntities(table cardTable)
{
    local cardModelId = "Model_Card Entry ID"
    local cardCount = #cardTable
    
    for i = 1, cardCount do
    	local spriteRUID = cardTable[i]
    
        local card = _SpawnService:SpawnByModelId(cardModelId, "Card " .. tostring(i), Vector3(0,0,0), self.CardSpawnRoot)
        
        card.Card.SpriteRUID = spriteRUID
        card.SpriteGUIRendererComponent.ImageRUID = spriteRUID
    end 
}
```

#### Card Component
```lua
Property:
[None]
string SpriteRUID = ""
```