# Course Introduction
You can easily change the color of sprites. We'll examine the difference between sprites and web sprites, and how to change their colors.


# Sprites
2D images are called Sprites. You can adjust their location, size, rotation, and so on.
Each sprite has a unique RUID value. You can use the RUID value to place sprites in an empty model or change them.
If a creator uploads their own image file to Maker, the file will automatically obtain RUID.

> Tip
> RUID is a unique ID value of various resources, including sprite, animation, sound, etc.

# Example
#### Using Resources from MSW
1. Find a resource from the resource search window. <br>Example RUID: 214069132ceb4962b374c8080fe265ef
2. Copy RUID, click<span style="color: #dc9656"> **Workspace - NativeModel - MapObject**</span> to place it.<br>![sprite09](https://mod-file.dn.nexoncdn.co.kr/bbs/16353870233517f049b81ee534a72bd40584976a70c91.gif "sprite09")
3. Enter the copied RUID to SpriteRendererComponent's <span style="color: #dc9656">**SpriteRUID**</span> property. <br>![sprite](https://mod-file.dn.nexoncdn.co.kr/bbs/168775319866329eb7f3e76cb4d72a8729ab30e0cc030.png "sprite")

#### Using Another Creator's Resources
1. Select <span style="color: #dc9656">**Workspace - MyDesk - Import From - Import Image**</span>, and import the image you'd like.
2. Select <span style="color: #dc9656">**Workspace - BaseEnvironment - NativeModel - MapObject**</span>, and place it in ![TabScene](https://mod-file.dn.nexoncdn.co.kr/bbs/163452458863504f49c7a23aa4a41af56b5b4611a6daf.png "TabScene")Scene.
3. **Enter the copied RUID to <span style="color: #dc9656">SpriteRendererComponent's SpriteRUID</span>** property.

# Web Sprite
You can use an image url from the web and use it as a "Web Sprite." If you do not own the image or if the resource isn't from MapleStory Worlds, you must check if you can use it, and to what extent.

#### Example
You can use Lara's artwork from MapleStory as a sprite.

1. Copy the image url you'd like to use. <br>Example address: https://lwi.nexon.com/maplestory/common/media/artwork/artwork_108.jpg
2. Go to <span style="color: #dc9656">**Workspace - BaseEnvironment - NativeModel - WebSprite**</span>, select and place a model.
3. Select the model and go to Property Editor, and enter <span style="color: #dc9656">**value for WebSpriteComponent's Url**</span>.<br>![websprite02](https://mod-file.dn.nexoncdn.co.kr/bbs/1687753413264ea7d4470867c43b6862bb02387c52d00.png "websprite02")
4. See that the web sprite has been applied.<br> ![sprite04](https://mod-file.dn.nexoncdn.co.kr/bbs/16349585335040cdf2dc709064f778ce427cbc7742cde.png)

# Sprite Color Adjustment
You can adjust the color, brightness, saturation, and opacity of an image in the Color property of SpriteRendererComponent and WebSpriteComponent.
![sprite05](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401414821492ba9c3b86b4d518c6a13747c463285.png)

| Number | Explanation |
| --- | --- |
| ![NO01](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541272181b5c1a55fcf3d49b19734d25913c38583.jpg "NO01") | You can drag on top of the color window to find the value you want. |  
| ![NO_02](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541300837cb541c2f44e046a79bb1901a885aa8ac.jpg "NO_02") | Bar you can use to adjust color. |  
| ![NO_03](https://mod-file.dn.nexoncdn.co.kr/bbs/163454131465069e090278448490f965207e9a4a10348.jpg "NO_03") | Bar you can use to adjust transparency. Moving it to the bottom will make the sprite transparent. |  
| 	![NO_04](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541326353d8628c1473944497bf376acb7a65ca45.jpg "NO_04") | You can see the new color from new window, the current color from current window below.  |  
| 	![NO_05](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541338689678f574f21e54a6ca533737924124d7e.jpg "NO_05") |You can enter values for HSV, RGB, Lab Color Space to find the color you want. |  
| ![NO_06](https://mod-file.dn.nexoncdn.co.kr/bbs/163454135201207284554a25b498380fff224cd767f6b.jpg "NO_06") | You can adjust opacity between 0-100. The lower the value, the more transparent the sprite.|  
| ![NO_07](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541365429e8d9fdf4049149eb96f0ec7fdae4b3d1.jpg "NO_07") | HexCode values. You can copy the code to apply the same color on other sprites. |  
| ![NO_08](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541383202032ed6f9f38046119acf89fd019b41cf.jpg "NO_08") |  ![TabScene](https://mod-file.dn.nexoncdn.co.kr/bbs/163452458863504f49c7a23aa4a41af56b5b4611a6daf.png "TabScene")Click on a point from the Scene to pick out the color you like.|  
|![NO_09](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541395594e0bdbc1a69204d11aef5d9fbdf7f7ecd.jpg "NO_09") | You can save the color you picked from the palette. You can create a new box whenever you click the last one.|  
##### Example
Adjust the sprite's hexcode value to ff53a1, and it will be colored like the sprite on the right.
![sprite05](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401406338bb847992bc304ad18f3c75d38259b1f5.png "sprite05")
# Summary
Simply changing the color can give the sprite a new look. If your game takes place in a dark world, you can adjust brightness of all the entities for a dark mood. Adjusting the color to match your game will enhance the atmosphere and the level of completion.

##### Reference Guide
[Importing a Custom Image](docs?postId=99{"target":"_self"})
[Finding Resources](docs?postId=211{"target":"_self"})