# Implementing Scoring
Add a colliding body to the parent entity of Model_Obstacle. Let's make the player score when the player entity collides with that colliding body.
1. Add the **TriggerComponent** to **Workspace - MyDesk - Model_Obstacle**.

2. Press the **[Edit]** button, then add a collider. Create a long, vertical collider so that it goes through the center of the obstacle.
 
    * ColiderType: Box
    * BoxSize: X 0.05, Y 20
    
    ![Model_Obstacle_Collider](https://mod-file.dn.nexoncdn.co.kr/bbs/1707114688801b7dda01f91ba4739b2b0ba9c25873f71.png "Model_Obstacle_Collider")
    
    > <span style="color: #7cafc2">**Tip**
    > Both DefaultPlayer and ModelObstacle should have CollisionGroup set to **TriggerBox**.</span> 

2. Add **TriggerEnterEvent** in **ObstacleComponent**.
    
    ```lua
    Event Handler:
    [client only] [self]
    HandleTriggerEnterEvent (TriggerEnterEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: TriggerComponent
        -- Space: Server, Client
        ---------------------------------------------------------
     
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        ---------------------------------------------------------
        if isvalid(TriggerBodyEntity.FishPlayerComponent) then
            TriggerBodyEntity.FishPlayerComponent:AddScore()
        end   
    }
    ```

3. Add the new **score** property to the **FishPlayerComponent**.  
    
    ```lua
    Property:
    [None]
    integer score = 0
    ```

4. Initialize the score to 0 in the line below the wait(1) so that the Die() function can initialize the score.

    ```lua 
    self.score = 0
    ```

5. Add the new AddScore() function to the **FishPlayerComponent** and write to increment the score by 1.
Write additional logs to check that the logs increment properly whenever something passes an obstacle.

    ```lua
    [client]
    void AddScore()
    {
        self.score += 1
        log(self.score)
    }
    ```

#### Showing in the Score UI
1. Add the **ScoreText** UI entity in the UI. 
2. Add the new property in the **FishPlayerComponent**.

    ```lua
    [None]
    Entity scoreText = /ui/DefaultGroup/ScoreText
    ```

3. Delete the log part of the AddScore().
In the same location, write the following so that the score is visible in TextComponent.Text of **ScoreText**.

    ```lua
    self.scoreText.TextComponent.Text = string.format("Score:%d", self.score)
    ```

4. If the player hits an obstacle and the game ends, you must initialize the score. Add the same contents to the Die() function.
Write it under the self.score = 0.
 
    ```lua
    self.scoreText.TextComponent.Text = string.format("Score:%d", self.score)
    ```

