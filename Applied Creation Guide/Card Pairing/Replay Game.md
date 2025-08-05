It would be a shame if you could only play the game once. That's why we ensure players can play as many times as they want.
Let's make some very small modifications to the World we've created so that whenever you press the <span style="color: #dc9656">**Replay**</span> button, you can match the cards all over again. Very, very small modifications!
  
![example](https://mod-file.dn.nexoncdn.co.kr/bbs/167160907270904afd8986bd14369b01281d091212927.gif "example")
  
# Trim the Beginning of the Game
Congratulations on completing the matching game!
If you run the game you have created so far, you'll have to play the matching game once every time you enter the World.
What do we have to change so that users can play the game when they want to? To give you a little hint, you can modify `CardPairGameLogic` and `CardBoard`.

If `Enable` in <span style="color: #dc9656">**CardPairGameUI**</span> is inactive by default when the game starts, you don't need to play the game. 

1. Select the <span style="color: #dc9656">**CardBoard**</span> component to open the Script Editor window.
2. Write the following so that you can add an `OnBeginPlay()` function and make it inactive by default.

    ```lua
    [client only]
    void OnBeginPlay ()
    {
        _CardPairGameLogic.CardPairGameUI.Enable = false
    }
    ```

3. Add a condition in `self.CardPairGameUI.Enable = true` from the `StartGame()` function of the <span style="color: #dc9656">**CardPairGameLogic**</span> logic.
If the **Enable** property of **CardPairGameUI** is false, modify it as follows so that it can be changed to true.

    ```lua
        if self.CardPairGameUI.Enable == false then
            self.CardPairGameUI.Enable = true
        end
    ```

# Create a Start Game Button
1. Add the <span style="color: #dc9656">**Retry**</span> component to **ui - DefaultGroup - GameStartBtn**.
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1671607817367598e7d74f1d64c41a3da3d4acaa509f1.png "1")
2. Select the Retry component to open the Script Editor window, and write the following in <span style="color: #dc9656">**HandleButtonClickEvent**</span>.
When the Enable property of **CardPairGameUI** is false, call `StartGame()` of the **CardPairGameLogic** logic by utilizing `EntityService`.

    ```lua
    Event Handler:
    [self]
    HandleButtonClickEvent (ButtonClickEvent event)
    {
    -- Parameters
    local Entity = event.Entity
    --------------------------------------------------------
    local cardPairGameUI = _EntityService:GetEntityByPath("/ui/CardPairGameUI")
    
    if cardPairGameUI.Enable == false then
    	_CardPairGameLogic:StartGame()
    end
    }
    ```
   
3. Press Start to check if <span style="color: #dc9656">**CardPairGameUI**</span> appears when pressing the Game Start button.