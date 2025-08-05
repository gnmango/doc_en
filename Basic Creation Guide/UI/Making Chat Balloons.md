# Course Introduction
You can attach chat balloons to entities and player avatars. Here's how to use ChatBalloonComponent and some examples.
# Attaching Chat Balloons
1. Select an entity to attach the chat balloon, and add <span style="color: #dc9656">**ChatBalloonComponent**</span>.
2. You can change the property value to make the chat balloon to your preference.
![chatballoon01](https://mod-file.dn.nexoncdn.co.kr/bbs/1635730184988c95467b0dd9f4613abf9ba85d68914d9.png "chatballoon01")
The property values are as follows:

| Property Name | Description |
| --- | --- |
| ArrowChatEnabled | You can attach or detach the tail from the balloon. <br>By default, chat balloons have tail images. |
| AutoShowEnabled | Set whether to automatically display chat balloons.<br>Players' ChatBalloonComponent does not operate when set to true. |
| BalloonScale | You can specify the size of the chat balloon. The bigger the value, the bigger the size gets horizontally. |
| ChatBalloonRUID | You can use various chat balloon Sprites. |
| ChatModeEnabled | When activated, dialogue from the chat window is simultaneously displayed in the chat balloon.<br> It only works on player's ChatBalloonComponent, and it is activated by default. |
| FontColor | Changes the color of the letters in the chat balloon.<br>For more information on how to change colors, please refer to Sprite Color Adjustment. |
| FontSize | Changes the size of the letters in the chat balloon.<br>Default value is 1. The bigger the value, the bigger the letters. |
|HideDuration| Set the time (seconds) when chat balloons are not shown on screen.<br>When the value is 0, chat balloons will always be displayed. |
| Message | Enter the phrase to display in the chat balloon.<br>Chat balloons will not be visible without any phrase, even though other settings have been activated. |
| Offset | You can adjust the position of the chat balloon. Starting point is 0, and the axis is set to Y by default. <br>If the value is 0, the name tag will be located at the starting point. A negative number will move the tag downwards, and a positive number will move it upwards. |
| ShowDuration | Set the time (seconds) when chat balloons are shown on screen.<br>Enter 1 or greater number to display the balloon. |

#### Use Example
The example makes a chat balloon for the seal entity object.
![chatballoon03](https://mod-file.dn.nexoncdn.co.kr/bbs/1655963930366197ddcce272b435d9633958b4b3fdbff.png "chatballoon03")

Let's enter the value as follows:

| Property | value |
| :--: | -- |
| ArrowChatEnabled | true |
| AutoShowEnabled | true |
| BalloonScale | 3 |
| ChatBalloonRUID | 0548325d3d4d40649acad12fde2359d4 |
| ChatModeEnabled | false |
| FontColor | #000000 |
| FontSize | 1.5 |
| HideDuration | 5 |
| Message | Is it a seal? Or a ture seal? |
| Offset | 1 |
| ShowDuration | 5 |

# Summary
Using chat balloons allows the users to communicate more conveniently.
You can use various chat balloon Sprites for different entities. 