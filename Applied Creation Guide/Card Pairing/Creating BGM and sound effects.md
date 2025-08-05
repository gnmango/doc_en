You've made the whole game, but something's missing, isn't it? We should complete the game by adding background music and sound effects.

# Setting Music
1. Select **Preset List - Background Music**. 

2. Select the desired music and click **Hierarchy - maps - map01**. You can see <span style="color: #dc9656">**SoundComponent**</span> added in the property window. Press **Audio Play** to check the music.
Make sure the **BGM, PlayOnEnable** property is activated. If both properties are activated, the music will play when entering the world.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1686532850382c535b46148644216b06c69ac0e68bf02.png "2")
3. If you've chosen the music, press Start to check if it plays properly.


# Creating Sound Effects
You've set a fun atmosphere with the music. Now, let's add some sound effects to make it even more exciting.
Playing sound effects when tapping cards can provide an important sensory experience.

1. You can find plenty of sound resources in **Resource Storage**.
    Find an appropriate audio clip in **MSW Resources - audioclip**. Pressing Start lets you listen to the audio clip in advance. 
    Choose an audio clip you like and copy the RUID. If you have difficulty finding a sound effect, you can use the RUID below.
    
    * Card selection sound effect: 0c57ceb6897c4955b1a4e860c6895868
    * Card matching sound effect: 3b79d37626de4023a600e90d7e1595f8
    * New deck sound effect: 08d81d2f5b4942cbbdaa9932f63c500f
    
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16823032910996aff64b534304fad848bbd5a9d73c3c8.png{"width":"640px"} "3")
    
    > <span style="color: #7cafc2">**Tip**
    > You can add found resources to favorites and manage them by categorizing them into folders.</span>

2. Utilize [SoundService](/apiReference?postId=316{"target":"_self"}) to play an audio clip in a specific situation. Enter the RUID value as the first parameter of the `PlaySound` function and the volume as the second parameter.
You need to add the code to play the sound effect when selecting cards into the `OnCardClicked()` function of the **CardPairGameLogic** logic.
Enter the following in the `cardboard:SelectCard(newCard)` underline.

    ```lua
    _SoundService:PlaySound("0c57ceb6897c4955b1a4e860c6895868", 0.7)
    ```

3. Enter the following in the first `cardboard:DeselectCard()` underline.

    ```lua
    _SoundService:PlaySound("0c57ceb6897c4955b1a4e860c6895868", 0.7)
    ```
		
4. Enter the following in the `newCard:SetDestroyed()` underline.

    ```lua
    _SoundService:PlaySound("3b79d37626de4023a600e90d7e1595f8", 0.7)
    ```

5. Enter the following in the `cardboard:SetCards()` underline.

    ```lua
    _SoundService:PlaySound("08d81d2f5b4942cbbdaa9932f63c500f", 1)
    ```

6. Press Start to test playing the sound effect. 


# Full Script

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
    	_SoundService:PlaySound("0c57ceb6897c4955b1a4e860c6895868", 0.7)
    
    elseif oldCard:IsPairOf(newCard) then
    	cardboard:DeselectCard()
    	
    	_SoundService:PlaySound("0c57ceb6897c4955b1a4e860c6895868", 0.7)
    	oldCard:SetDestroyed() 
    	newCard:SetDestroyed()
    	_SoundService:PlaySound("3b79d37626de4023a600e90d7e1595f8", 0.7)
    	
    	local me = _UserService.LocalPlayer
    	me.PlayerStats:AddScore(10)
    	
    	if cardboard:IsEmpty() then
    		cardboard:SetCards()
    		_SoundService:PlaySound("08d81d2f5b4942cbbdaa9932f63c500f", 1)
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

