# Course Introduction
Let's make a jumping platform that launches the character up high. Through this example, you can learn how to write extension scripts, how to create collisions between objects through scripts, how to process events when collisions occur, and how to apply physical force to objects. 
You can also learn how to set different property values for different objects by adding properties to components.
# Placing a Jumping Platform
1. From the left **Preset List**, find and select the spring-shaped model (object-4575), and place it on the map.
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/165604935001714611bdebb36466091d47d17fb16a24a.png "1")
# Utilizing Collision Function with TriggerComponent
To be able to do something when a collision with an object occurs, we have to make use of the collision detection function. MapleStory Worlds offers **TriggerComponent**, which detects collision to trigger a certain action upon collision. Let's extend **TriggerComponent** to create a **function that makes the character jump when a collision event occurs**.

1. In the **Workspace**, search for <span style="color: #dc9656">**TriggerComponent**</span>.
2. In the context menu of **TriggerComponent**, click **Extend**.
    ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/165604959095153cfca57050245b38749c711113ff227.png "2")
    <br>
3. Enter <span style="color: #dc9656">**"Spring"**</span> as the name of the new script component.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/163542033144502adadc09f744c09917af9217755b545.png "3")
    <br>
4. Double-click the **Spring** script to open the Script Editor.
    ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1635420361509dad37849139e4c5798eb5b68431f70dc.png "4")
    <br>
# Composing Script and Applying Component to Entity
We'll create a script to add a feature that makes a character jump up high when it touches the jumping platform.

1. **Press the **[+]** button next to **Method** and add the `OnEnterTriggerBody` function.
    ![[5]](https://mod-file.dn.nexoncdn.co.kr/bbs/173949930506311bf9917c12346bcb94c6a915d5244d7.png "5")
    <br>
2. Write the content of the `OnEnterTriggerBody` function as follows.
    ```lua
    override void OnEnterTriggerBody(TriggerEnterEvent enterEvent) 
    {
        __base:OnEnterTriggerBody(enterEvent)
        
        --Receive the target (player) that collided with the jumping platform.
        local player = enterEvent.TriggerBodyEntity
        log(player.Name)
        --Receive the player's MovementComponent component. If the target has no MovementComponent, the function ends.
        local rigidbody = player.RigidbodyComponent
        if rigidbody == nil then
        	return
        end
        
        --Receive the player's PlayerControllerComponent component. If the target has no PlayerControllerComponent, the function ends.
        local controller = player.PlayerControllerComponent
        if controller == nil then
        	return
        end
        
        --Push in the direction the player is facing. Right if LookDirectionX is a positive number, left for a negative number.
        if controller.LookDirectionX > 0 then
            rigidbody:SetForce(Vector2(10,10))    
        else
            rigidbody:SetForce(Vector2(-10,10))
        end
    }
    ```
    <br>
3. Save the script, and then select the spring entity you placed on the map earlier.
    ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1659682364643e700e1fcbcd549539f62f219cdc00b24.png "6")
    <br>
4. Click **[Add Component]** at the bottom of the Property Editor, and add a **Spring** component.
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1635317761426ae2b93a52a6f47cfa0fbb58e9c90c16f.png "7")
    <br>
5. Let's press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and test. You can see that the character flies far away upon touching the jumping platform.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1656049685585a94042b540df4073af62cc5fa37abf20.png "8")