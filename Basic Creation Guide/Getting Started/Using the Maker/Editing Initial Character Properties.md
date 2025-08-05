# Course Introduction
If a game is about climbing stairs, it would be unfair if someone could climb four steps at a time when others could climb only two. In any game, it's important that all players start from even footing.
Let's learn how to edit properties using model properties.

# Editing the Player's Basic Properties

You can change basic properties in the **Player** model.

1. Select **Workspace - BaseEnvironment - NativeModel - Player**.
2. Change the property value of **Player**</span>.
3. Press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** to see the changes.

The properties of the **Player** model are shown below.
In cases where the original property names match, some model property names have been changed.

![player](https://mod-file.dn.nexoncdn.co.kr/bbs/16357503109659b1496c0a9e84d899b57ed7a46c12439.png "player")

| Model Property Name | Original Component <br> Original Property Name | Explanation |
| --------- | ---------------- | --- |
| <span style="color: #181818">speed</span> | MovementComponent <ul><li>InputSpeed</li></ul> | Sets the player's default movement speed. The bigger the value, the faster the movement.<br>The default value is 1.2. |
| jumpForce | MovementComponent<ul><li>JumpForce</li></ul> | Sets the player's default jump height. The bigger the value, the higher the jump.<br>The default value is 1. |
| walkAcceleration | RIgidbodyComponent<ul><li>WalkAcc</li></ul> | Sets movement acceleration and deceleration values. The higher the value, the faster the player will reach maximum speed.<br>Default value is 1. |
| <span style="color: #181818">gravity</span> | RIgidbodyComponent<ul><li>Gravity</li></ul> | Gravity value applied to the player. The higher the value, the faster the fall.<br>Default value is 1. |
| <span style="color: #181818">cameraDeadZone</span> | CameraComponent<ul><li>DeadZone</li></ul> | <span style="color: #181818">If the player avatar moves within the input value range, the camera stays still.</span><br><span style="color: #181818">If the value for X or Y is 0, the camera will move to match all movement of player avatar.</span> |
| cameraSoftZone | CameraComponent<ul><li>SoftZone</li></ul> | Sets SoftZone area. If target enters the frame area, the camera will shift direction to DeadZone. |
| <span style="color: #181818">cameraDamping</span> | CameraComponent<ul><li>Damping</li></ul> | <span style="color: #181818">Camera will follow player avatar moving out of the DeadZone.</span><br><span style="color: #181818">In that case, the camera's movement will be more natural and smooth.</span><br><span style="color: #181818">The higher the value, the smoother the movement.</span> |
| <span style="color: #181818">cameraScreen</span> | CameraComponent<ul><li>ScreenOffset</li></ul> | <span style="color: #181818">Sets the centerpoint location of the DeadZone on the screen.</span><br><span style="color: #181818">X and Y values can range from (0,0) to (1,1).</span><span style="color: rgb(23, 43, 77);">.</span> |
| <span style="color: #181818">cameraDutch</span> | CameraComponent<ul><li>DutchAngle</li></ul> | Sets the rotation angle of the camera.<br>The higher the value, the more the screen will rotate clockwise.<br>The camera can also rotate counterclockwise depending on the input value. |
| <span style="color: #181818">cameraOffset</span> | CameraComponent<ul><li>CameraOffset</li></ul> | <span style="color: #181818">Adjusts camera location according to input value.</span><br><span style="color: #181818">X and Y coordinates are absolute values. The camera will move according to the input value to track the player avatar.</span> |
| <span style="color: #181818">message</span> | ChatBalloonComponent<ul><li>Message</li></ul> | <span style="color: #181818">Contains the content of the chat balloon for the player avatar.</span> |
| <span style="color: #181818">chatModeEnabled</span> | <span style="color: #181818">ChatBalloonComponent</span><ul><li><span style="color: #181818">ChatModeEnaled</span></li></ul> | Text entered into the chat window is simultaneously displayed in a chat balloon. <br>The function is disabled by default. |
| <span style="color: #181818">nameTag</span> | NameTagComponent<ul><li>Name</li></ul> | You can attach or detach the player's name tag.<br>Disable by unticking the Enable checkbox of NameTagComponent. |
| <span style="color: #181818">damageSkinId</span> | DamageSkinSettingComponent<ul><li>DamageSkinld</li></ul> | <span style="color: #181818">Enter the ID value to determine the damage skin type.</span> |
| <span style="color: #181818">damageDelayPerAttack</span> | DamageSkinSettingComponent<ul><li>DelayPerAttack</li></ul> | <span style="color: #181818">Enter the delay speed (in seconds) between damage values when they are displayed multiple times.</span> |
| <span style="color: #181818">triggerBodyBoxsize</span> | TriggerComponent<ul><li>BoxSize</li></ul> | <span style="color: #181818">Set coordinates to indicate the size of the box where the player senses shock.</span> |
| <span style="color: #181818">triggerbodyBoxOffset</span> | TriggerComponent<ul><li>Offset</li></ul> | <span style="color: #181818">Set coordinates to indicate the location of the box where the player senses shock.</span> |
| <span style="color: #181818">maxHp</span> | PlayerComponent<ul><li>MaxHP</li></ul> | Sets player's maximum HP.<br>Default value is 1,000. |

# Deactivating Model Property
A creator can choose to deactivate unnecessary model properties.
For example, if you'd like to deactivate **NameTag**, find **NameTagComponent** from the Property Editor and uncheck **Enable**.

![player01](https://mod-file.dn.nexoncdn.co.kr/bbs/1634115527034a6cafbd8f68a4de49a827121df6f7a9c.png)

# Deleting DefaultPlayer's Basic Components
If you'd like to extend an existing component to use it, you need to delete the basic components of **DefaultPlayer**. It is recommended that you use a single component, as multiple components with the same foundation often lead to malfunctions.

><span style="color: #7cafc2">**Tip**</span>
> <span style="color: #7cafc2">You can use multiple extended components depending on the component property.</span>

> <span style="color: #7cafc2">**Tip**</span>
> <span style="color: #7cafc2">Please refrain from just deactivating it, as different components operate in different ways.</span>
> <span style="color: #7cafc2">Though there is a slim chance for malfunction, it may lead to confusion between components or errors when writing scripts.</span>

1. Select **Workspace - BaseEnvironment - NativeModel - Player**.
2. In the Model Property window, choose the components to delete.
3. Open the context menu and click **Remove Component**.

![player09](https://mod-file.dn.nexoncdn.co.kr/bbs/1659574418759c11cbf083bd3472893c15be435fbe1f5.png "player09")
# Summary

You can adjust model property values to create all kinds of fun.
Try comparing basic settings and your own settings to come up with the best player properties for your game.