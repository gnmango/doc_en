# Course Introduction
Let's learn about the Play window where you can test created worlds.

#### Reference Guide
* [Scene](/docs/?postId=1152{"target":"_blank"}) 
* [Workspace](/docs/?postId=121{"target":"_blank"})
* [Hierarchy](/docs/?postId=453{"target":"_blank"})
* [Property Editor](/docs/?postId=1087{"target":"_blank"})
* [Utilizing and Controlling the UI Editor](/docs/?postId=120{"target":"_blank"})
# Play
The Play window is used when testing created worlds. It only activates when you start the playtest by pressing the **[Start]** button. It operates with a camera separate from the Scene window where the world is edited, so screen movement during testing isn't synchronized. However, when the client is changed from Scene, it will be reflected on the Play window.
The test screen ratio can be set before testing, and a virtual client can be added during testing to initiate multi-testing.

#### Play's Function
You can use a variety of functions that suit the Play window's status. The icon is visible on the top.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17370970458165d802f305fde434d9020a4eac96ab265.png "Play")
| Icon | Name| Desc |
| --- | --- | --- |
| -  | Simulator | You can set the test screen ratio. The default value is PC. <ul><li>PC: Functions with FHD Resolution (1920x1080) as the default. The screen ratio is fixed to 16:9.</li><li>Mobile: The size of Scene and Play size can be adjusted for various mobile screen sizes. The screen ratio is kept even when testing. A Safe Area can be set.</li><li>Free: The simulator's screen ratio can be set freely. The screen ratrio is set based on the Play window size.</li></ul> |
| - |  Test Play Language | Set the language that will be tested. The language can't be changed during testing.|
| ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_latency_01.png "latency") | Function Stats |Shows the FPS and memory info of the world that is currently being played.  |
| ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_scene_maximize.png "Maximize") | Maximize Window | Converts the Play window to fullscreen. Revert to the previous size by pressing the **[Minimize]** button. |

#### Play when Testing
The function that can be used in the Play window when testing has been changed.
![play01](https://mod-file.dn.nexoncdn.co.kr/bbs/1737452078994c0fb1a6973394aa4b20d64ba20768447.png{"width":"960px"} "play01")

| Icon | Name | Desc |
| --- | --- | --- |
| -  | Simulator | Displays the screen ratio info that is being tested. |
| - |  Test Play Language | Displays the language setting currently tested. The language can't be changed while testing.|
| ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_latency_01.png "latency") | Function Stats |Shows the FPS and memory info of the world that is currently being played.  |
| ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_scene_maximize.png "Maximize") | Maximize Window | Converts the Play window into fullscreen. Revert to the previous size by pressing the **[Minimize]** button. |
|  ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_multiplayer.png "multiplayer") | Adding Virtual Players | Activated while testing. Multiple virtual clients can be open. |
|- |  Number of Virtual Players |  You can open multiple clients by setting the number of virtual players that will be added. You can open up to 10 virtual clients. All of them close when the test ends.
