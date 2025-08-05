# Course Introduction
The character can climb on a ladder or rope to go upward. This can happen due to the **ClimbableComponent** associated with the motion of a ladder or rope. Using this component, the character can also climb on objects other than a ladder or rope. In addition, you can control the climbing area or climbing speed as the creator desires.
There is also **ClimbableSpriteRendererComponent** as a related component. This component is used to draw a head, body, and tail-shaped Sprite, as with a ladder or rope. 
Let's look at these two components today.

##### API Reference
[ClimbableComponent](/apiReference/Components/ClimbableComponent{"target":"_self"})
[ClimbableSpriteRendererComponent](/apiReference/Components/ClimbableSpriteRendererComponent{"target":"_self"})

# ClimbableComponent
[ClimbableComponent](/apiReference/Components/ClimbableComponent{"target":"_self"}) sets the climbing action-related matters.
Let's take a look at the properties of **ClimbableComponent**.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16626264079829a7791304acd40db8bcb63b170f65d22.png "1")
| Property Name | Description |
| :---: | --- |
| Collider | After pressing the Edit button, you can control the climbing area.  |
| AllowHorizontalMove | Set whether or not to use free climbing moves. If it's true, it can move on both the X and Y axes. |
| ClimbableAnimationType | Set the character's climbing animation. <br>You can select Ladder or Rope. |
| IsUseDefaultObjectBoxSize | Set whether to fit the climbing area settings to the sprite size. <br>It works only in Maker Edit mode. <br>If true, Boxsize and BoxOffset are automatically set to fit the sprite size. <br>If false, Boxsize and BoxOffset can be set arbitrarily. |
| SpeedFactor | When climbing, this value is multiplied to the speed traveled in the X or Y direction. The greater the value, the faster the movement.|

# ClimbableSpriteRendererComponent
[ClimbableSpriteRendererComponent](/apiReference/Components/ClimbableSpriteRendererComponent{"target":"_self"}) can set up the Sprite of a ladder or a rope and adjust the length.
A ladder or a rope consists of a ① head, ② body, and ③ tail, and if you increase their length, the Sprite of the body part is repeated and increased.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/166262931403925ecbf22b49a4917b916676ac2835e47.png "4")

Let's take a look at the properties of **ClimbableSpriteRendererComponent**.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/167568565117370e633f438a04a19b05285b2f9cc1902.png "5")
| Property Name | Description |
| :---: | --- |
| FlipX | Determines whether the Sprite is inverted relative to the X axis or not. |
| FlipY | Determines whether the Sprite is inverted relative to the Y axis or not. |
| IgnoreMapLayerCheck | Does not perform automatic replace when designating the Map Layer name to SortingLayer. |
| MaterialId | Specifies the ID of the material that the renderer will use. |
| OrderInLayer | Determines the priority within the same Layer. A greater number indicates higher priority. |
| SortingLayer | When two or more Entities overlap, the priority is determined according to the Sorting Layer. <br>If you specify it as a Map Layer name, it is changed to the corresponding Sorting Layer. |
| SpriteRUID | Set the Sprite RUID to use on the ladder's body. |
| TiledSize | You can set the vertical height of the ladder by changing the Y value. Changing the X value is not supported. |
| Color | Colors the Sprite. In the case of basic white, the original color is displayed. |

> **<span style="color: #7cafc2">Tip</span>**
> If the <span style="color: #7cafc2">Entity includes both **ClimbableComponent** and **ClimbableSpriteRendererComponent**, you cannot adjust the climbing area manually and must use the object's **BoxSize** as the climbing area. </span>

# Using Object as a Ladder
1. Let's add the object we will use like a ladder to the World. Go to the context menu of the map you want to add the object to, then select **Create Entity - Create Empty**.
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/168619968509130d481ace9ef49e19fb609ae4446ab5e.png "7")
    <br>
2. Change the created **EmptyEntity** name to <span style="color: #dc9656">**Net**</span>.
<br>
3. Press **Add Component** from the **Net**'s Property Editor and add **TransformComponent**.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/166303145507485459a1a3bf34f75be561c8837b6c2dd.png "8")
<br>
4. Add **SpriteRendererComponent** to the **Net**'s Property Editor, then enter the net SpriteRUID. You'll see a net object as below.
    * SpriteRUID : <span style="color: #dc9656">**3f5d1c1eddf9428e8ca1c5581f1453d4**</span>
    ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/16630319308118bab7fbc64d8412a843c3f7964e0b419.png "9")
<br>
5. In the property editor of **Net**, add **ClimbableComponent**.
<br>
6. Set **false** by unticking the checkbox of **IsUseDefaultObjectBoxSize**. After that, if you press the **Collider - [Edit]** button and adjust the climbing area, the **BoxOffset** and **BoxSize** of **IsUseDefaultObjectBoxSize** will be set up.
    ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/16630324973683251945ba48b41b1a2dfacdca51c87e6.png "10")
<br>
7. Set **AllowHorizontalMove** to **True** to allow movement on both the X and Y axes.
<br>
8. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. You can see the character climbing up and down the net.
    ![ladder](https://mod-file.dn.nexoncdn.co.kr/bbs/1663033197084cc25b9e0220d41339699fa7de794d565.gif "ladder")