# Course Introduction
Let's make footholds using different objects to make the game more interesting.
We'll discuss the concepts of the **FootholdComponent** and **CustomFootholdComponent** and how to edit the foothold.


# Making a Foothold with Foothold Model
Here's how to make footholds with the **Foothold** model. You can use **Foothold** models instead of normal tiles to make colorful streets.

1. In the **Preset List**, select **Foothold**.
2. Select the desired model and place it in the **Scene** panel.<br>A foothold matching the sprite of the **Foothold** model is saved with the model.
![foothold01](https://mod-file.dn.nexoncdn.co.kr/bbs/165995816559970800486aa594605aec8a12193083ba3.png{"width":"750px"} "foothold01")

><span style="color: #7cafc2">Tip - See Foothold Information</span>
>In the<span style="color: #7cafc2"> **Scene** panel, when clicking the foothold information icon ![Common_foothold](https://mod-file.dn.nexoncdn.co.kr/bbs/163453674810369fa34e7367d462bba709d528166c58d.png "Common_foothold"), the foothold is visible as a red line.</span>

# Making New Footholds
If there isn't a model you want in **Foothold**, you can make your own Foothold. Search for <span style="color: #dc9656">**Foothold**</span> in the **Workspace** panel, and you'll see two components. Two components are both related to **Foothold**, but they have different concepts and intended uses. You should use **CustomFootholdComponent** to create a new foothold.

* **FootholdComponent**: The component that saves and provides foothold information within the entire map. In Property Editor, you'll see the search results, but you can't use them. For more details, please refer to the [API Document](/apiReference?postId=336{"target":"_self"}) for each component.
* **CustomFootholdComponent**: The component to make and adjust footholds. Foothold models include this component. For more detail, please refer to the [API Document](/apiReference?postId=331{"target":"_self"}) of each component.
# Utilizing Custom Foothold Components
#### Drawing Foothold Paths
1. Place the sprite to use as foothold into the **Scene** panel.
<br>
2. Add **CustomFootholdComponent** in the Property Editor of the foothold entity.
![foothold02](https://mod-file.dn.nexoncdn.co.kr/bbs/1635312202910484e69092aac41a488ba45bc39c9edb2.png "foothold02")
<br>
3. Enter the **Size** (number of foothold's starting point) of **edgeLists** (foothold's starting point) and adjust the foothold's shape in the **Scene** panel.
![foothold03](https://mod-file.dn.nexoncdn.co.kr/bbs/1635312618765c3d8f742150b4a67bc2700d94d8f8379.png "foothold03")

| Name | Explanation |
| --- | --- |
| edgeLists | Indicates foothold's <span style="color: #dc9656">**starting point**</span>.<br>If you make multiple starting points, you can make individual footholds according to the value entered for a single entity. |
| Size | Indicates the number of points that make up a foothold.<br><span style="color: #dc9656">**Starting point**</span> will be created depending on the value you input for Size of edgeLists. <br>The value you entered in [ number ]'s Size will be the number of <span style="color: #dc9656">**connecting points**</span>. |
| [Number] | This refers to <span style="color: #dc9656">**the number of connecting points**</span> of footholds.<br>Starting from 0, connecting points are added equal to the value entered for Size.<br>Connecting points are created by selecting the corresponding entity in the scene, and in the context menu, you can add them with <span style="color: #dc9656">**Add Point**</span>. <br>Connecting points indicate the location with x, y values. |
#### Example
1. Enter <span style="color: #dc9656">**1**</span> in the **Size** of the **edgeLists** to create a starting point.
<br>
2. Enter <span style="color: #dc9656">**1**</span> in **[1] - Size**.
<br>
3. Click **Edit Foothold** in the context menu of that entity.
<br>
4. Move the starting point to your desired location.
<br>
5. Click **Add Point** in the context menu of that entity to add connecting points.
<br>
6. Repeat adding connecting points to create the desired movement line. 
<br>
7. Let's press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and test. Make sure the avatar moves along the line.
![foothold50](https://mod-file.dn.nexoncdn.co.kr/bbs/16419850592877b797c57a7064aa7af39d73058b42c22.gif "foothold50")

> <span style="color: #7cafc2">**Tip - Making Easily Climbable Footholds**</span>
> <span style="color: #7cafc2">Connecting the points at a gentle angle will help avatars move around easily.</span>
> <span style="color: #7cafc2">Test during production to see if the avatar can move around easily.</span>

#### Adding Properties to Footholds
You can change the properties of the foothold you created to make the foothold more unique. Adjusting each property value can make the foothold more interesting: footholds that go one-way only, footholds that make you slow, etc.

| Name | Explanation |
| --- | --- |
| FootholdDrag | Indicates friction force that applies when an Entity with the RigidbodyComponent is on the foothold. The bigger the value, the more the entity will slip on top of the foothold, then decelerate quickly to stop. <br> If an entity falls on top of the foothold with a value between 1 and 10, the entity will slip a shorter distance before it stops. The value also affects the player's basic movements, but it will be barely noticeable during play. |
| FootholdForce | A force is applied to the Entity containing a RigidbodyComponent on the foothold. Entity moves to the right if the value is positive, and to the left if negative. Try to move in the other direction, and you'll feel resistance.|
| FootholdWalkSpeedFactor|  The coefficient is multiplied by the movement speed when an Entity with the RigidbodyComponent is on the foothold. The higher the value, the faster the movement speed.  |

# Summary
It's up to the creator if there will be multiple starting points or a single starting point to the foothold. Either way, you'll be able to create a nice foothold that goes well with the Sprite you chose.
##### Reference Guide
[Adding and Deleting Components](/docs?postId=58{"target":"_self"})
