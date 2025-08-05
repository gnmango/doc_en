# Course Introduction
Let's find out how to control animations easily through avatar's state in **StateToAvatarBodyActionSheet**. 

# Avatar Animation
In order for the user to recognize when the avatar is walking, sitting, attacking, or jumping, an animation that matches the state is required. If there is **Key** and **AvatarBodyActionStateName** in **StateToAvatarBodyActionSheet** of [AvatarStateAnimationComponent](/apiReference/Components/AvatarStateAnimationComponent{"target":"_self"}), when the avatar enters a certain state, the corresponding animation is played. You can use this feature to delete the original animation value and create a new state and animation to connect with your avatar. For example, pressing the attack key will play the **attack** animation by default, but you can change it to the **dead** animation.

**StateToAvatarBodyActionSheet** defines 11 states (Key) of the avatar and animations (AvatarBodyActionStateName) associated with them by default. When the avatar enters a certain state, the **Key** sends a request to play an action and plays the animation corresponding to **AvatarBodyActionStateName**. If you only enter **AvatarBodyActionStateName** but do not enter a state (Key), the animation will not play because there's no connected state.
**AvatarStateAnimationComponent** is the default component of **Player** models. You can deactivate this component in **DefaultPlayer** and add properties to the component. To add/delete a **Key**, you can change the **StateToAvatarBodyActionSheet** property's **Size** value or click the **[+] and [-]** buttons at the bottom.

![animation105](https://mod-file.dn.nexoncdn.co.kr/bbs/167927196496004370d09cca342d0b0ae918cc40e24ac.png "animation105")

**StateToAvatarBodyActionSheet** has default values as shown below. If you change or delete the default, the corresponding animation will not play. For example, if you delete **ladder** from **AvatarBodyActionStateName**, when the avatar climbs the ladder, the ladder ascending animation (ladder) does not play, and the walking animation stops while climbing.
**PlayRate** allows you to adjust the default playback rate for avatar animations. The default values are different according to AvatarBodyActionStateName.
| Key | AvatarBodyActionStateName | PlayRate |
| --- | --- | --- |
| IDLE | stand| 1 |
| MOVE | walk | 1.68 |
| ATTACK | attack| 1.33 |
| HIT | hit | 1 |
| CROUCH| crouch | 1 |
| FALL | fall | 1 |
| JUMP | fall | 1 |
| CLIMB | rope | 1 |
| LADDER | ladder | 1 |
| DEAD | dead | 1 |
| SIT | sit | 1 |

><span style="color: #7cafc2">**Tip.**
> Key has to be in **Upper case**.</span>


# Connecting Different Animation to Existing State Key
You can modify the **AvatarBodyActionStateName** value of the **StateToAvatarBodyActionSheet** property to connect a different animation to an existing state.

1. Change **attack** to **dead**.
![animation104](https://mod-file.dn.nexoncdn.co.kr/bbs/16793645420245e8a2fcc25234f7391c1c2585fb08417.png "animation104")
<br>
2. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and test. Press the left **Ctrl** key to attack. Before modification, it plays the attack animation. However, after the modification, it plays the dead animation due to changing the connected animation to **Dead**.<br>![animation103](https://mod-file.dn.nexoncdn.co.kr/bbs/1656037601843938b5091f64c4a3499ff0940db8d3670.gif "animation103")

# Adding New State Key and Animation
You can also add new states (Keys) and assign animations. If you add new states and actions without using the existing one, the two states will operate together.
Let's play a certain animation upon pressing number keys 6, 7, 8, 9.

1. Change **Size** of **Workspace - DefaultPlayer - AvatarStateAnimationComponent** to <span style="color: #dc9656">**15**</span>.
<br>
2. Enter a new **Key** and **AvatarBodyActionStateName** in the added fields.
    
    | Key | AvatarBodyActionStateName |
    | --- | --- |
    | NEW_ATTACK | attack |
    | NEW_DEAD | dead |
    | NEW_JUMP | fall |
    | NEW_CROUCH | crouch |
    ![ActionSheet100](https://mod-file.dn.nexoncdn.co.kr/bbs/1679272025027f89505b96cf948aba9190b4a1bac83be.png "ActionSheet100")
    <br>
3. Create the new **NewAvatarAnimation** script component in **MyDesk** and add it to **DefaultPlayer**.
<br>
4. Open the **NewAvatarAnimation** script. Write it with the `AddState()` and `ChangeState()` function of **StateComponent** to play the new added **Key**.
    
    ```lua
    Property:
    [None]
    any stateComponent = nil
     
    Method:
    [client only]
    void OnBeginPlay()
    {
        self.stateComponent = self.Entity.StateComponent
        self.stateComponent:AddState("NEW_ATTACK") -- Adds a new state.
        self.stateComponent:AddState("NEW_DEAD")
        self.stateComponent:AddState("NEW_JUMP")
        self.stateComponent:AddState("NEW_CROUCH")
    }
    
    [client only]
    void Animate(number key) 
    {
        if key == 6 then
            self.stateComponent:ChangeState("NEW_ATTACK") 
        elseif key == 7 then
            self.stateComponent:ChangeState("NEW_DEAD")
        elseif key == 8 then
            self.stateComponent:ChangeState("NEW_JUMP")
        elseif key == 9 then
            self.stateComponent:ChangeState("NEW_CROUCH")
        end
    }
    
    Event Handler:
    [client only] [service: InputService] 
    HandleKeyDownEvent(KeyDownEvent event) 
    {
        -- Parameters
        local key = event.key
        --------------------------------------------------------
          
        if key == KeyboardKey.Alpha6 then
            self:Animate(6) 
        elseif key == KeyboardKey.Alpha7 then
            self:Animate(7)
        elseif key == KeyboardKey.Alpha8 then
            self:Animate(8)
        elseif key == KeyboardKey.Alpha9 then
            self:Animate(9)
        end
    }
    ```
       
    ><span style="color: #585858">**Learn more**</span>
    > We added new state and action but did not delete the existing one, so the two inputs operate together.

 5. Press Start followed by the number key to play the animation. 
![animation101](https://mod-file.dn.nexoncdn.co.kr/bbs/1656037637302737656324fc249a3a33872e34f4e54c4.gif "animation101")