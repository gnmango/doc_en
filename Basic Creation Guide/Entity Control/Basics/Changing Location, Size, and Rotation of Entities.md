# Course Introduction
Let's examine the configuration of **TransformComponent** and how to use it.
You'll learn the concept of components, and use them to adjust entity movement, rotation, and size.

# Transforming Entities
There are two ways to transform entities. The creator can choose either of them.
* In the Property Editor window, change the property value of **TransformComponent**.
*In **Scene**, adjust entities by selecting, dragging, or using handlers.
# Understanding Coordinates
To use **TransformComponent**, first we'll look into the concept of coordinates.
Entities placed in **World** can be adjusted in terms of 'location, scale, and rotation' based on their coordinates.
The X axis indicates left and right, Y axis top and bottom, and Z axis front and back. Increases and decreases in value are represented with positive and negative numbers.
![Transform01](https://mod-file.dn.nexoncdn.co.kr/bbs/1637114783017401b498e2b214c02a549e756b19f3bb3.png "Transform01")
Axes X, Y, and Z are concepts most commonly used in 3D games. Since MapleStory Worlds uses a 2D model, we won't be using all three axes to change an entity's state.
**Position**and **Scale** mostly use X and Y values. In 2D entities, Z values are only used when necessary. 
When multiple entities overlap, larger Z values are placed in front, and smaller Z values are placed behind them, making it look like a layer.
**Rotation** only moves around the Z axis. Therefore, unlike **Position and Scale**, it is independently named **ZRotation/WorldZRotation** and can only have one value.
Below is a table that shows how often each axis is used.

|  | Position<br>WorldPosition | Scale | ZRotation<br>WorldZRotation |
| --- | --------------------- | ----- | ----------------------- |
| **X** | ● | ● | X |
| **Y** | ● | ● |  X|
| **Z** | ▲ | ▲ | ● |
#### Using Z Values
Z values in **Position** and **Scale** are rarely used because we are using 2D images.
2D is like a surface. We can only transform its surface area. Even if we change the Z value, it is not noticeable, as there is no volume moving back and forth.
However, when multiple entities are overlapping, the order of the layers can be affected by adjusting the Z value.
![transform02](https://mod-file.dn.nexoncdn.co.kr/bbs/1637114865603b3dbe59028114cbc95e80edb39e3d4aa.png "transform02")
**Rotation** uses only the Z value also because it is a 2D image. Imagine holding the top of a piece of paper in front of your eyes and rotating it. At first you see the image as a whole, but at some point, you can only see the thin side of the paper.
On-screen, the images rotates around the Y axis. The image will get thinner and thinner until it becomes distorted. It's hard to call this a proper rotation, which is why it isn't used. 
Rotating the image around the Z axis won't distort the image. It might seem as if it rotates horizontally on-screen, but likewise, it's because this is a 2D image.
![transform04](https://mod-file.dn.nexoncdn.co.kr/bbs/16371150089380390a89a833549279015e3acd4cd728d.png "transform04")
# Change Position
1. Select the entity you placed and activate Property Editor.
2. Change the **X and Y values of Position** in **TransformComponent**.
3. In the **Scene**, click and move the entity to your desired position.

#### Example of Changing X Value
Enter <span style="color: #dc9656">**4**</span> for the value of **Position X**, and the entity will move to the right.
![transform50](https://mod-file.dn.nexoncdn.co.kr/bbs/163711505255438094dc5f40847b980a0ec74bf336520.png "transform50")
#### Example of Changing Y Value
Enter <span style="color: #dc9656">**3**</span> for the value of **Position Y**, and the entity will move up.
![transform52](https://mod-file.dn.nexoncdn.co.kr/bbs/163711533385082094259354448c59298d1f60f084bad.png "transform52")
#### Example of Changing Z Value
If you enter <span style="color: #dc9656">**1**</span> and <span style="color: #dc9656">**2**</span> for the value of **Position Z**, the smaller number is placed in the front.
![50](https://mod-file.dn.nexoncdn.co.kr/bbs/1637129854895b7a5e5c04c754a389026df1017ff4c2f.png "50")

> <span style="color: #7cafc2">**Tip - Changing Order of Entities**</span>
>It is best to change the order using **OrderInLayer** of <span style="color: #7cafc2"> **SpriteRendererComponent**.</span>

# Adjusting Size
* In the Property Editor, change the **X and Y** values for **Scale**.
* In the **Scene**, move the activated handler.
#### Example of Changing X Value
Increasing the **X** value of **Scale** will increase the size left and right.
![53](https://mod-file.dn.nexoncdn.co.kr/bbs/163712981670835563d7a4c624c498e242240e3b8a9c4.png "53")
#### Example of Changing Y Value
Increasing the **Y** value of **Scale** will increase the size up and down.
![54](https://mod-file.dn.nexoncdn.co.kr/bbs/1637129906868ea6b4f61f3b34d25835aa40b8cd54d67.png "54")
#### Example of Changing X and Y Values
Entering <span style="color: #dc9656">**4**</span> for the **X and Y** values of **Scale** will make the image 4 times bigger proportionally.
![55](https://mod-file.dn.nexoncdn.co.kr/bbs/163712992630702c9e1c108bb41f1920d778f71388559.png "55")
# Rotation
**WorldZRotation** and **ZRotaion** interchange values.
#### Method 1
Change the **ZRotation** value of **TransformComponent** into <span style="color: #dc9656">**45**</span>.
See how the entity has been rotated 45 degrees counterclockwise.
![70](https://mod-file.dn.nexoncdn.co.kr/bbs/1637129956129332db21c233f45dba0e6741eff67ec52.png "70")
#### Method 2
1. In the **Scene**, click the entity to activate the handler.
2. Move the cursor towards the edge of the handler box.
3. When the cursor is changed into the ![CursorRotate](https://mod-file.dn.nexoncdn.co.kr/bbs/163452377985689268fa2aa5c412989c41cbb81e3e13f.jpg "CursorRotate") shape, rotate the entity to the desired direction.<br>![transformrotate](https://mod-file.dn.nexoncdn.co.kr/bbs/163538582711066d4943dd3814f5fa29565c837aa53a4.gif "transformrotate")
# Utilize TransformComponent Function
#### Translate Function
The `Translate` function is for moving the entity's location. It takes the entity position as the origin and moves it relative to the entered value.
The following code is an example of how the entity moves upon starting the game.

```lua
void OnBeginPlay()
{
    self.Entity.TransformComponent:Translate(4,5)  -- Moves the entity by 4 along the X-axis and 5 along the Y-axis from the current location.
    log("move on")  --  When the movement has been completed, Move On is printed to the console window.
}
```

Add the script to the entity and see if it works.
Once the game starts, the cat object placed near the tile **will move according to input coordinate values**.
![transformgif](https://mod-file.dn.nexoncdn.co.kr/bbs/16353203939604dd5bf2ec48549a2880226e944ec5021.gif "transformgif")
#### Rotate Function
`Rotate` function is used to rotate the entity counterclockwise. It rotates the entity to the left (counterclockwise) of it's placed position according to the input value.
The following code is an example of how the entity rotates when executing the game.

```lua
void OnBeginPlay()
{
    self.Entity.TransformComponent:Rotate(6) -- Rotates the entity counterclockwise by 6 from the current location.
    log("Move On") -- When completed, Move On is printed to the console window.
}
```

Add the script to the entity and see if it works.
Once the game starts, the cat object placed near the tile **rotates counterclockwise by 6**.
![transformgif02](https://mod-file.dn.nexoncdn.co.kr/bbs/163532046690887f78a7c6d9549fe8eb9b63f036d6060.gif "transformgif02")