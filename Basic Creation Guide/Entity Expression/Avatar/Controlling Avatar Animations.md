# About the Course
Let's look at three ways to control avatar animations. In this course, we'll try to control the avatar animation with the function you want.

# Control with ActionStateChangedEvent
An avatar consists of a **Body** and **Face** within **AvatarRoot**. **Body** is the avatar's torso and **Face** is its face. You can set what animation to play in the avatar's **Body** and **Face** with [ActionStateChangedEvent](/apiReference/Events/ActionStateChangedEvent{"target":"_self"}). 
* **Face** controls the avatar's face animation, the facial expression.
* **Body** controls the avatar's torso action and parts action.

<br>
Let's take a look at the properties of **ActionStateChangedEvent**.

| Property | Description |
| :---: | --- |
| CoreActionName | The name of the avatar's torso action. |
| PartsActionName | The name of each part action. <br>Actions of avatar's equipped items except for face accessories. |
| PlayRate | It's the play speed. |
| PlayType | It's the play type. |
| StartFrameIndex | It's the starting frame of the animation. |
| EndFrameIndex | It's the ending frame of the animation. |

## ActionName
**ActionStateChangedEvent** determines which animation to play with **ActionName**.
Below is the list of **ActionName**.

|@cols=4:@rows=1:ActionName|
|:---:|:---:|:---:|:---:|
|	stand1	|	swingO1	|	stabO1	|	shoot1	|
|	stand2	|	swingO2	|	stabO2	|	shoot2	|
|	walk1	|	swingO3	|	stabOF	|	shootF	|
|	walk2	|	swingOF	|	stabT1	|		|
|	alert	|	swingT1	|	stabT2	|		|
|	prone	|	swingT2	|	stabTF	|		|
|	proneStab	|	swingT3	|		|		|
|	jump	|	swingTF	|		|		|
|	fly	|	swingP1	|		|		|
|	sit	|	swingP2	|		|		|
|	ladder	|	swingPF	|		|		|
|	rope	|		|		|		|
|	dead	|		|		|		|
|	heal	|		|		|		|
|	blink	|		|		|		|

You can preview the animations of each action in **AvatarItem Editor** Maker as follows.
1. Download **Avatar_Cap_A1.psd** in the [Open Back Cap] guide. 
    > **<span style="color: #7cafc2">Tip.</span>**
    > You can use any example resource file (psd) under the sub-documents in the <span style="color: #7cafc2">[Creating Avatar Item](/ko/docs/?postId=582{"target":"_self"}) guide. You don't have to use **Avatar_Cap_A1.psd**.</span>
2. Click **Window - AvatarItem Editor** in the navigation menu of the Maker.
<br>
3. Select the downloaded **Avatar_Cap_A1.psd** after clicking the **[Select PSD File]** button.
<br>
4. After a while, the psd file is applied. Choose the desired name in the **ActionName** list. You can check the selected animation.
![action](https://mod-file.dn.nexoncdn.co.kr/bbs/16667653519861a35fd34874248b9a83f0b983525f3bc.gif "action")

You can utilize **ActionName** with **CoreActionName** and **PartsActionName**. As described above, **CoreActionName** is avatar's <span style="color: #dc9656">**bare body action**</span> and **PartsActionName** are <span style="color: #dc9656">**the actions of avatar's equipped items**</span> except for face accessories. If you look at the avatar's movements with different settings of **CoreActionName** and **PartsActionName** it's easy to understand. For example, look at the image below. The avatar's torso is walking as the **CoreActionName** is set to **walk1**, but the clothes and shoes stand still as the **PartsActionName** is set to **stand1**.
![walk](https://mod-file.dn.nexoncdn.co.kr/bbs/1666850718269278086006fbe4b198a9c492c952bd8c4.gif "walk")

In general, you should set **CoreActionName** and **PartsActionName** identically because it causes unnatural motion if they're different. Of course, sometimes they have to be set differently to appear natural, like the **Death** action, where you set the **CoreActionName** to **dead** and the **PartsActionName** to **stand1**. The creator can set **CoreActionName** and **PartsActionName** according to their own intentions.


## PlayRate
Control the play speed of the animation. The bigger the number is, the faster the play speed.

## PlayType
Set how to play the animation. **PlayType** includes **OneTime, Loop, and ZigzagLoop**.

| SpriteAnimClipPlayType | Description |
| :---: | --- |
| OneTime | Play once. |
| Loop | Play repeatedly in order. |
| ZigzagLoop | Play repeatedly in order - in reverse order. |

## Start/End Frame
You can check the total frames of each animation in **AvatarItem Editor**.

![01](https://mod-file.dn.nexoncdn.co.kr/bbs/16667657292657df3e93d54f8437cb7a709131c0ab531.png "01")

The creator can set the start frame (StartFrameIndex) and the end frame (EndFrameIndex) of the desired animation to play after checking the total frames.
Note the following when setting frames.
* The frame must be set to a positive number.
* The start frame must be less than or equal to the end frame.
* The end frame must be less than or equal to the total animation frame.
* You can correct the frame value if it's incorrect.
* The frame value is set with the default value if not entered.
* The normal output of animations through the input of **CoreActionName**(torso action) and **PartsActionName**(parts action).
<br>
## Example
You should send an event to the **GetBodyEntity** of the **AvatarRendererComponent** because **ActionStateChangedEvent** occurs through the avatar's **body**.
The default settings are as follows.
```lua
local body = self.Entity.AvatarRendererComponent:GetBodyEntity()
local event = ActionStateChangedEvent("walk1", "walk1", 1, SpriteAnimClipPlayType.Loop, 1, 2)
body:SendEvent(event)
```
<br>
Some properties operate normally even when omitted. 
However, you must input **CoreActionName** and **PartsActionName**.
```lua
local body = self.Entity.AvatarRendererComponent:GetBodyEntity()
local event = ActionStateChangedEvent("walk1", "walk1")
-- You can omit the playback speed, playback type, start frame, and end frame.
-- It's the same meaning as ActionStateChangedEvent("walk1", "walk1", 1, SpriteAnimClipPlayType.Loop, 0, End Frame).
body:SendEvent(event)
```
<br>
If you enter the wrong frame value, it's automatically corrected.
```lua 
local body = self.Entity.AvatarRendererComponent:GetBodyEntity()
local event = ActionStateChangedEvent("walk1", "walk1", 1, SpriteAnimClipPlayType.Loop, -1, 4) 
-- The frame value should be a positive number. If you enter a negative number as startFrameIndex above, it's corrected to "0".
-- When endFrame is "3", if entering "4" for endFrameIndex, it would be corrected to "3".
-- It would be finally corrected to ActionStateChangedEvent("walk1", "walk1", 1, SpriteAnimClipPlayType.Loop, 0, 3).
body:SendEvent(event)
```
<br>
You can fine-tune multiple actions like the different modification between torso and parts animations respectively using **ActionStateChangedEvent**. However, there is also the hassle of having to specify an action each time. If you set frequently used animations like 'Stand' or 'Walk' like this every time, you will increase the amount of unnecessary repetitive code. Also, you need different animations for when your avatar is holding a one-handed weapon versus a two-handed weapon.
You can get rid of these inconveniences by using **BodyActionStateChangeEvent**.

# Control with BodyActionStateChangeEvent
[BodyActionStateChangeEvent](/apiReference/Events/BodyActionStateChangeEvent{"target":"_self"}) is the event to literally change the bodyActionState. This event is convenient because you don't need to specify an animation for each weapon type separately. 
**BodyActionStateChangeEvent** controls animations with **MapleAvatarBodyActionState**. **MapleAvatarBodyActionState** predefines which torso action and parts action to use for each state and what the play speed and the play type are. It's easy to understand if you think of it as the animation presets used frequently are defined.
<br>
**MapleAvatarBodyActionState** matches **ActionStateChangedEvent** as shown in the table below.

|	MapleAvatarBodyActionState	|	CoreActionName	|	PartsActionName	|	PlayRate	|	PlayType	|
| :---: | :---: | :---: | :---: | :---: |
|@cols=1:@rows=2:Stand	|	stand1	|	stand1	|	1	|	ZigzagLoop	|
| stand2	|	stand2	|	1	|	ZigzagLoop	|
|@cols=1:@rows=2:Walk	|	walk1	|	walk1	|	1	|	Loop	|
|	walk2	|	walk2	|	1	|	Loop	|
|	Attack	|	alert	|	alert	|	1	|	Loop	|
|	Crouch	|	prone	|	prone	|	1	|	Loop	|
|	Fall	|	jump	|	jump	|	1	|	Loop	|
|	Sit	|	sit	|	sit	|	1	|	Loop	|
|	Rope	|	rope	|	rope	|	1	|	Loop	|
|	Ladder	|	ladder	|	ladder	|	1	|	Loop	|
|	Dead	|	dead	|	stand1	|	1	|	Loop	|
|	Blink	|	blink	|	blink	|	1	|	Loop	|
|	Fly	|	fly	|	fly	|	1	|	Loop	|
|	Hit	|	alert	|	alert	|	1	|	ZigzagLoop	|
|	Alert	|	alert	|	alert	|	1	|	ZigzagLoop	|
|	Heal |	heal |	heal |	1	|	Loop |
|	Invalid	|	-	|	-	|	-	|	-	|

## Example
Let's look at an example where **BodyActionStateChangeEvent** calls the **Fly** state with **MapleAvatarBodyActionState**.
```lua
local event = BodyActionStateChangeEvent()
event.ActionState = MapleAvatarBodyActionState.Fly
event.needResetAction = true
event.startFrameIndex = 1
event.endFrameIndex = 2
self.Entity:SendEvent(event)
```

The event finally calls **ActionStateChangedEvent** as seen in the matching table above.
```lua
ActionStateChangedEvent("fly", "fly", 1, SpriteAnimClipPlayType.Loop, 1, 2)
```

Even if the wrong Start/End frame is entered in **BodyActionStateChangeEvent**, it can be corrected in **ActionStateChangedEvent**.

# Control with ActionSheet and StateComponent
It can also be cumbersome to change the animation by sending an event every time the state of a predefined or newly defined avatar changes. It would be very convenient if the animation would also change whenever the avatar state changes without sending an event. You can easily change animations by changing your avatar state at any time you want with [StateComponent](/apiReference/Components/StateComponent{"target":"_self"}) after setting up an animation to be connected to the avatar state in **ActionSheet** of [AvatarStateAnimationComponent](/apiReference/Components/AvatarStateAnimationComponent{"target":"_self"}).

The default **ActionSheet** provided by **AvatarStateAnimationComponent** is as follows.
![02](https://mod-file.dn.nexoncdn.co.kr/bbs/1666838653260b4a969d3d22a438990ede8add75f3edb.png "02")
|	Key	|	Value	|
|:---:|:---:|
|	IDLE	|	stand	|
|	MOVE	|	walk	|
|	ATTACK	|	attack	|
|	HIT	|	hit	|
|	CROUCH	|	crouch	|
|	FALL	|	fall	|
|	JUMP	|	fall	|
|	CLIMB	|	rope	|
|	LADDER	|	ladder	|
|	DEAD	|	dead	|
|	SIT	|	sit	|

**ActionSheet**In ,
* **Key** means **State**.
* **Value** means **MapleAvatarBodyActionState**.
    |	ActionSheet Value	|	MapleAvatarBodyActionState	|
    |	:---:	|	:---:	|
    |	stand	|	Stand	|
    |	walk	|	Walk	|
    |	attack	|	Attack	|
    |	crouch	|	Crouch	|
    |	fall	|	Fall	|
    |	sit	|	Sit	|
    |	rope	|	Rope	|
    |	ladder	|	Ladder	|
    |	dead	|	Dead	|
    |	blink	|	Blink	|
    |	fly	|	Fly	|
    |	hit	|	Hit	|
    |	alert	|	Alert	|
 <br>
If an avatar reaches a specific **State**, it finds the **MapleAvatarBodyActionState** that matches the **Value** of the **State** in the **ActionSheet**. As you saw above, **MapleAvatarBodyActionState** predefines which torso and parts animation to play. Therefore, the avatar's animation will change according to the **State** changed with **StateComponent**. 
 <br>
As such, **ActionSheet** conveniently and easily changes animations along with the state. For example, if you want to play the **walk** animation in the **IDLE** state, you just change the value of **Value** to **walk**. It's also easy for creators to add new states manually.

> <span style="color: #585858">**Learn more**
> Refer to the [Easy Control of Avatar Animation with ActionSheet] guide to easily change animations using **ActionSheet** and **StateComponent**.</span>

<br>
Changing animations with **ActionSheet** and **StateComponent** is easy, but it also has its drawbacks.
* You cannot select the start and end frames of the animation.
* If there is a defined default/user state change condition, the state can be changed automatically by this condition. Therefore, the state may change immediately before the desired animation is output.

## Summary
A summary of the changing animation process is as follows.
![03](https://mod-file.dn.nexoncdn.co.kr/bbs/1666767739753193f598641f64746982ab39b9808c9b4.png "03")

The table below summarizes the pros and cons of the three methods for controlling avatar's animation as seen above.
Each method has clear pros and cons, so a creator just needs to choose the method that matches their intent.

| Controlling Avatar Animation | Pros | Cons |
| :---: | --- | --- |
| ActionStateChangedEvent  | <ul> <li>You can adjust the body and parts animations separately.</li> <li>You can set the play speed, play type, and animation start/end frame in detail.</li></ul> | <ul> <li>It's cumbersome to set up frequently used actions like this every time.</li><li>You need to specify a separate animation depending on the weapon.</li></ul> |
| BodyActionStateChangeEvent | <ul> <li>You don't need to specify an animation depending on the weapon.</li><li>You can set the start/end frame of animations.</li></ul> | <ul> <li>You cannot adjust the body and parts animations separately.</li> <li>You cannot change state to undefined in MapleAvatarBodyActionState.</li></ul> |
| ActionSheet and StateComponent | <ul> <li>You can simply change the animation for each avatar state in the ActionSheet of the property editor. </li><li>You can easily add your avatar state.</li><li>If you change the State at the desired timing, the animation changes accordingly with StateComponent.</li></ul>  | <ul> <li>You cannot set the start/end frame of animations.</li><li>If the state is automatically changed by another state change condition, the desired animation may not be output.</li></ul> |

# Animation Frame Event
Let's look at three events to tell the frame information of the playing animation. Let's check out an example to change to another animation with a specific frame using obtained frame information.

| Event | Description |
| :---: | --- |
| [SpriteAnimPlayerChangeFrameEvent](/apiReference/Events/SpriteAnimPlayerChangeFrameEvent{"target":"_self"}) | It occurs when changing the sprite animation's frame. |
| [SpriteAnimPlayerStartFrameEvent](/apiReference/Events/SpriteAnimPlayerStartFrameEvent{"target":"_self"}) | It occurs when playing the first frame of sprite animation. |
| [SpriteAnimPlayerEndFrameEvent](/apiReference/Events/SpriteAnimPlayerEndFrameEvent{"target":"_self"}) | It occurs when playing the last frame of sprite animation. |

Each event sends the frame number (FrameIndex), whether it's reverse playing (ReversePlaying), and total frame count (TotalFrameCount). The event is normally operated when connecting to avatar's **Body** or **Face** with **ConnectEvent**. 

## Example
Let's see how to use **FrameEvent** in the following example code.
1. Suppose you want to change to another animation when playing the second frame of an avatar's animation. 
If so, you can utilize **SpriteAnimPlayerChangeFrameEvent**. 
Add the `OnBeginPlay` function and connect the event to **Body** as described above. 
In case of cancellation of subscription, the handler of the connected event should have it as a property.
    ```lua
    Property:
    [None]
    any handler = nil

    Method:
    [client only]
    void OnBeginPlay()
    {
        local body = self.Entity.AvatarRendererComponent:GetBodyEntity()
        self.handler = body:ConnectEvent("SpriteAnimPlayerChangeFrameEvent", self.ChangeFrameEvent)
        -- Connect SpriteAnimPlayerChangeFrameEvent to the body. 
        -- If SpriteAnimPlayerChangeFrameEvent occurs, call the ChangeFrameEvent function.
    }
    ```
    Now, call the `ChangeFrameEvent` function when **SpriteAnimPlayerChangeFrameEvent** occurs.
    <br>
2. Write the `ChangeFrameEvent` function. 
Set the **swingOF** animation to **Play 2-3 frames repeatedly** with a play speed **0.5** through **ActionStateChangedEvent** when changing the animation frame to 2.
    ```lua
    [client]
    void ChangeFrameEvent(any event)
    {
        if event.FrameIndex == 2 then
            local body = self.Entity.AvatarRendererComponent:GetBodyEntity()
            local actionEvent = ActionStateChangedEvent("swingOF", "swingOF", 0.5, SpriteAnimClipPlayType.Loop, 2,3)
            body:SendEvent(actionEvent)
        end
    }
    ```
    The code above works normally until the animation plays. However, since we are still subscribing to **SpriteAnimPlayerChangeFrameEvent**, as soon as the animation starts, the process will repeat again sending a new **ActionStateChangedEvent**. You will need to unsubscribe from **SpriteAnimPlayerChangeFrameEvent** as this will not give you the desired result.
    <br>
3. Unsubscribe from **SpriteAnimPlayerChangeFrameEvent** via the saved **handler** earlier.
Let's modify the `ChangeFrameEvent` function.
    ```lua
    [client]
    void ChangeFrameEvent(any event)
    {
        if event.FrameIndex == 2 then
            local body = self.Entity.AvatarRendererComponent:GetBodyEntity()
            body:DisconnectEvent("SpriteAnimPlayerChangeFrameEvent", self.handler) 
            -- Unsubscribe from SpriteAnimPlayerChangeFrameEvent
            local actionEvent = ActionStateChangedEvent("swingOF", "swingOF", 0.5, SpriteAnimClipPlayType.Loop, 2,3)
            body:SendEvent(actionEvent)
        end
    }
    ```
    Now after play, when the second frame of the first animation plays, it changes to the **swingOF** animation.
<br>
**SpriteAnimPlayerStartFrameEvent** and **SpriteAnimPlayerEndFrameEvent** are the same. Let's create the desired animation using events according to the creator's implementation intention.

> **<span style="color: #7cafc2">Tip.</span>**
> <span style="color: #7cafc2">The avatar animation is played through the client, so you need to control it with the client.</span>