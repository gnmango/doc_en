# Course Introduction
In a game, the world space and the screen space exist, and different coordinates are used in each space.
This time, let's understand the concept of world coordinates and screen coordinates and find out what differences they have.

# World Coordinates
World coordinates literally refer to a certain location value within a space named game world. From the Property of the Entity placed on the map, the WorldPosition of TransformComponent is the world coordinate. For example, we can see that the <span style="color: #dc9656">**Fraction Entity**</span> below is located on X = 23.91, Y = -3.37, Z = 999.953 within the world through <span style="color: #dc9656">**WorldPosition**</span>.
![fountain](https://mod-file.dn.nexoncdn.co.kr/bbs/16560451065876f02eae1080d46618432600ef68e44ec.png "fountain")

Let's use a simple example to aid in understanding.

We live on a planet named Earth, and your current location can be indicated by longitude and latitude.
If a man named James lives in New York, the location will indicate around New York. If he travels to Seoul, it will indicate around Seoul.
The real world's longitude and latitude where we are living are the <span style="color: #dc9656">**world coordinates**</span> within a game.

# Screen Coordinates
Let's go back to James to understand how screen coordinates are different from world coordinates.
James, who lives in New York but came to Seoul for a trip, is wearing glasses due to poor eyesight. Let's imagine a small star is drawn on the upper right of these glasses. 
Then, regardless of whether James is in New York or Seoul, he will always see the small star on the upper right of his eyes. It is possible to indicate the small star's location by making the coordinate based on the glasses. The coordinate of this lens of glasses is the <span style="color: #dc9656">**screen coordinate**</span>.

Screen coordinates refer to the location value on the screen. 
Screens typically place UI elements such as buttons and images. The point to be placed is set at a certain distance from a specific reference point on the screen. 
The UI elements placed are always indicated at a certain location no matter where your characters move within the world. Like the star always visible in the upper right corner of James' eyeglasses.

UI elements placed on the screen may have different relative positions depending on the change of <span style="color: #dc9656">**screen resolution**</span>.

For example, imagine that a creator wants to place a button on the right top of the screen. 
The creator placed the button at the location as below, based on the centerpoint, with 800X600 resolution.
![800600](https://mod-file.dn.nexoncdn.co.kr/bbs/1652766400447c2680c3ec3f74b9bba4eef4eca2cc697.png "800600")

However, what do you think will be shown to the users playing on a 1600x1200 resolution screen?
![16001200](https://mod-file.dn.nexoncdn.co.kr/bbs/16527664161266e70f3f1cee0431982988e23c95abcd5.png "16001200")

With this resolution, the button will be located somewhere the creator didn't intend, not on the right top.
Regardless of the user device resolution, the <span style="color: #dc9656">**Anchor Presets**</span> feature should be used to display UI elements where the creator wants them to appear.

## Anchor Presets
In UITransformComponent, if UIMode is Screen, you can set up <span style="color: #dc9656">**Anchor**</span>.
![anchorPresets](https://mod-file.dn.nexoncdn.co.kr/bbs/1652754058498d1bc17b4c1d04e75bc8000fde8722de1.png "anchorPresets")

In case Anchor is one Point as below, you can place UI distanced from the standard point as much as X and Y if you enter the PosX and PosY values.

|Anchor| Description |
| :---: | --- |
|![anchor01](https://mod-file.dn.nexoncdn.co.kr/bbs/1652753675728c2aaf37b9f124af7893d195adcef175b.png "anchor01") |The UI is located at a location distanced as much as PosX and PosY based on the end of the upper left.  |
|![anchor02](https://mod-file.dn.nexoncdn.co.kr/bbs/165275370174172181339e3834ca086504f0c535151ff.png "anchor02") |The UI is located at a location distanced as much as PosX and PosY based on the upper center.  |
|![anchor03](https://mod-file.dn.nexoncdn.co.kr/bbs/1652753728860c87e3a1bb08b4b5580456a8cce00f406.png "anchor03") |The UI is located at a location distanced as much as PosX and PosY based on the end of the upper right.  |
|![anchor04](https://mod-file.dn.nexoncdn.co.kr/bbs/16527537526160827153fdd254b85883a0b2034f68230.png "anchor04") |The UI is located at a location distanced as much as PosX and PosY based on the left end of the center. |
|![anchor05](https://mod-file.dn.nexoncdn.co.kr/bbs/1652753775316eb1fa9ba8c1b45b29705a7728b78db74.png "anchor05") |The UI is located at a location distanced as much as PosX and PosY based on the center. |
|![anchor06](https://mod-file.dn.nexoncdn.co.kr/bbs/1652753795158b1e8a296bbf44fddb88e77f0d9dd67bf.png "anchor06") |The UI is located at a location distanced as much as PosX and PosY based on the right end of the center. |
|![anchor07](https://mod-file.dn.nexoncdn.co.kr/bbs/165275381909396f89e4e066c4a9392fb67db882319e1.png "anchor07") |The UI is located at a location distanced as much as PosX and PosY based on the left end of the bottom.  |
|![anchor08](https://mod-file.dn.nexoncdn.co.kr/bbs/16527538405956b6dc7cc784f492cad18daaf2f449f22.png "anchor08") |The UI is located at a location distanced as much as PosX and PosY based on the bottom center.  |
|![anchor09](https://mod-file.dn.nexoncdn.co.kr/bbs/1652753858060d23fdebc0f4b4f9293594b55d52eacb1.png "anchor09")  |The UI is located at a location distanced as much as PosX and PosY based on the right end of the bottom. |

<br>
For example, for chat UI, the Anchor Point is set up on the left end of the top. If this has been set, the chat UI will be located distanced as much as PosX and PosY based on the left end of the top of the screen no matter what the resolution is.
![anchorex01](https://mod-file.dn.nexoncdn.co.kr/bbs/165604514596759c47da1127745c2b9207457e3d9bbe9.png "anchorex01")

<br>
The Anchor can also be set in a form to be stretched long.
If the Anchor is stretched long, the Property that can be set depending on the type will be different.

|Anchor  | Explanation |
| :---: | --- |
|![anchor10](https://mod-file.dn.nexoncdn.co.kr/bbs/165276280240576b4802818d74cf095e74237cb57da69.png "anchor10") |The UI will be stretched horizontally after leaving the values as much as the Left and Right values based on the upper center. <br>It is located at a location distanced as much as PosY based on the top center.   |
|![anchor11](https://mod-file.dn.nexoncdn.co.kr/bbs/165276284856863a5c32a8f6c4e20b6261d44d2bda729.png "anchor11")  |The UI will be stretched horizontally after leaving the values as much as the Left and Right values based on the center. <br>It is located at a location distanced as much as PosY based on the center.  |
|![anchor12](https://mod-file.dn.nexoncdn.co.kr/bbs/16527628656822017ed5fcffc4ed7ac43bc57234b13f8.png "anchor12")  |The UI will be stretched horizontally after leaving the values as much as the Left and Right values based on the bottom center. <br>It is located at a location distanced as much as PosY based on the bottom center.  |
|![anchor13](https://mod-file.dn.nexoncdn.co.kr/bbs/165276288320119af691b47d54ee5a5f8f8cc841ba772.png "anchor13")  |The UI will be stretched vertically after leaving the values as much as the Top and Bottom values based on the left center. <br>It is located at a location distanced as much as PosX based on the left center.  |
|![anchor14](https://mod-file.dn.nexoncdn.co.kr/bbs/1652762899845dbdb414c754049a5b95a6e0d3f7426b8.png "anchor14")  |The UI will be stretched vertically after leaving the values as much as the Top and Bottom values based on the center. <br>It is located at a location distanced as much as PosX based on the center.  |
|![anchor15](https://mod-file.dn.nexoncdn.co.kr/bbs/1652762937587653a13ae666f45329c6afa9c618617e4.png "anchor15")  |The UI will be stretched vertically after leaving the values as much as the Top and Bottom values based on the right center. <br>It is located at a location distanced as much as PosX based on the right center.  |
|![anchor16](https://mod-file.dn.nexoncdn.co.kr/bbs/1652762962358809eb3ba031f47f491b2a12900b0b324.png "anchor16")|The UI will be stretched everywhere after leaving the values as much as the Top and Bottom values based on the center.|
<br>
For example, for the black translucent background image (PopupBack) of the pop-up message, the UI is set to be stretched everywhere based on the center, covering the whole screen. When a pop-up message appears at any resolution, a black background image overlays the colorful map, allowing the user to focus only on the pop-up message. 
![anchorex02](https://mod-file.dn.nexoncdn.co.kr/bbs/165604560743653ab60032b284c4cb1fa149610be2b93.png "anchorex02")
<br>

If you understand the Anchor's concept and use Anchor Presets, you can display the UI to show where the creator intended in various environments such as PC and mobile.