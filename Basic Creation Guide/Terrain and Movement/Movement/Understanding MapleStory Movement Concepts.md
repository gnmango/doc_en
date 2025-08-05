# Course Introduction
Let's examine the RigidbodyComponent characteristics that follow the MapleStory Movement Method among the essential movement methods for world production. We recommend studying movement with [Create MapleStory Tile Map].
# MapleStory Movement Method
RigidbodyComponent is used when you want to move as characters do in MapleStory. The movement method is closely related to the landscape type, so you can create a MapleStory-type movement method with TileMapComponent. At this time, the movement on the tile is managed by RigidbodyComponent.

> <span style="color: #7cafc2">**Tip**
> The player character has acceleration while moving, so the movement properties such as WalkAcceleration, WalkDrag, and WalkSlant are involved in both RigidbodyComponent and landscape's movement properties.</span>

# Default Movement
Let's learn about the default movement method of RigidbodyComponent.

#### Move on Foothold
The player moves while on the foothold where the player is currently stepping unless there is no particular movement change. If the player gets separated from the foothold, the relation to the foothold will be disconnected, and then it follows the foothold movement rule as to which the player steps on next. As shown in the picture below, the player, who is stepping on the red foothold, does not move while moving to the green foothold despite passing the crossing point with the green foothold. However, if the character jumps at the point where the red and green footholds overlap, the player will step on the green foothold and move following the green foothold movement rule.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1658193798420c9b8127ab85542ac8c25008d67ca62e6.png{"width":"740px"} "1")

![1-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16581938215047ccafa3686d54c30a7ed3194be7c793a.gif{"width":"740px"} "1-1")
#### Vertical Foothold Movement Restriction
The vertical foothold connected with the floor blocks the player character's movement.

![blocked2](https://mod-file.dn.nexoncdn.co.kr/bbs/165822007557308684a3bc9b44795a843406f14f5b8b9.gif{"width":"740px"} "blocked2")
#### A Movement That Got Out of Foothold
* <span style="color: #dc9656">**When going down**</span>: When the player is not stepping on the foothold, going down is a default movement method by being influenced by gravity. The player character falls, and then stops falling and lands upon encountering a foothold. The landed character follows the movement rule on the foothold again.
* <span style="color: #dc9656">**When going up**</span>: Even though the player character encounters another foothold while going up, the character can ignore and pass it and go up.
![UP](https://mod-file.dn.nexoncdn.co.kr/bbs/1658883585804ef670a8a5f244cd49d1aaee440b64e30.gif "UP")


# Intensive Movement
#### Topography Layer
You can create up to 10 Map Layers, and use unique tiles for each layer. This means that you can draw the same patterns on a different layer. However, the layer where the tile belongs is different, so it will be created as a different foothold. Therefore, if the layer is different, then the footholds will not be connected as one. Even though some differently-shaped tiles are connected in some world, and the character walks smoothly across them, the foothold information for the character will change.

We drew some portions of the same tiles overlapping in Layers 1 and 2, as shown below. When the player character moves from Layer 1 to Layer 2, the character can pass the vertical foothold. However, when the player character moves from Layer 2 to Layer 1, the character cannot move because the vertical foothold is blocked. When it belongs to Layer 1, it ignores the vertical foothold belonging to Layer 2, so the movement is not possible.

![maplayer2](https://mod-file.dn.nexoncdn.co.kr/bbs/16593207875474437a8ce52584df9bd06b8487e5d5dec.png{"width":"740px"} "maplayer2")
![maplayerblocked](https://mod-file.dn.nexoncdn.co.kr/bbs/165823394560592e2768650e94a9fb15284bd030a570a.gif{"width":"740px"} "maplayerblocked")

#### SortingLayer
In general, SortingLayer is automatically set unless the creator changes it from the script, meaning the order shown in the scene will not be changed. However, when the entity has RigidbodyComponent, the order shown can be changed. For the entity with RigidbodyComponent, for example, the player character's SortingLayer changes according to the TileMap Layer that is currently holding the player. As shown below, we drew some portions of tiles to overlap to different MapLayers, and arranged forest objects in Layer 2. If the player character moves from Layer 1 to Layer 2, Layer 2 is in front of Layer 1. So, the player character is drawn behind the forest, and placed in front of the forest when stepping on Layer 2.
![SortingLayer](https://mod-file.dn.nexoncdn.co.kr/bbs/1658231806874225549d7abfd4083aa75e5de3ed69aaf.gif{"width":"740px"} "SortingLayer")

#### Vertical Line Topography
The horizontal ground foothold (ground) and the connected vertical foothold become a wall blocking the movement of RigidbodyComponent. However, RigidbodyComponent can pass through vertical footholds that are separate from ground footholds.
![notblocked](https://mod-file.dn.nexoncdn.co.kr/bbs/16588846011832681a46df73b40e09ecf4140c57c09ce.gif "notblocked")
# Create Different Movement from MapleStory
 #### Vertical Line Topography
If you want to make the character unable to pass the vertical foothold with a jump, whether it is attached to or separate from the ground foothold, you should enable the <span style="color: #dc9656">**IsBlockvertialLine**</span> of RigidbodyComponent.

| IsBlockvertialLine Enabling | IsBlockvertialLine Disabling |
| --- | --- |
|![BlockedTrue](https://mod-file.dn.nexoncdn.co.kr/bbs/165830397057921277d58d2954629b55e8db6b9ca6d99.gif "BlockedTrue")| ![BlockedFalse](https://mod-file.dn.nexoncdn.co.kr/bbs/1658304402181bdd5063510e848f090ae484b1467f48e.gif "BlockedFalse") |

##### Blocking Only the Topography of Vertical Lines on a Specific Layer
Enabling RigidbodyComponent's **IsBlockvertialLine** prevents it from going through all vertical topography inside the map. If you want to prevent RigidbodyComponent from passing through the vertical topography only on some map layers, enable **TileMapComponent - IsBlockvertialLine** on those map layers.

Let's look at an example. After creating two map layers as shown below, we'll set the grass tiles to pass through the vertical topography and the stone tiles to not pass through.
![v01](https://mod-file.dn.nexoncdn.co.kr/bbs/1681114270716dcd0db08af484f42987ea977046172d4.png "v01")

After selecting **MapleMapLayer_1** in **Hierarchy** and looking at the Property Editor, **MapLayerName** is set to **Layer2**, which is a stone tile.
![v02](https://mod-file.dn.nexoncdn.co.kr/bbs/16811145893012649f50aa9c84d239be7289931be4c5b.png "v02")

After selecting **TileMap_1**, enable **IsVerticalLine** in **TileMapComponent**.
![v03](https://mod-file.dn.nexoncdn.co.kr/bbs/1681114736635e34bcba0a8e84b7d9588196e14f563c8.png "v03")

Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and test.
Grass tiles can pass through vertical topography, but stone tiles will block vertical topography when jumping.
![v04](https://mod-file.dn.nexoncdn.co.kr/bbs/1681115054201040f1c2e91564fe4919afebd2bcd9b7d.gif "v04")

#### Map Boundary
To ignore the Map Boundary and move even outside the map topography, enable the <span style="color: #dc9656">**IgnoreMoveBoundary**</span> property of the RigidbodyComponent. 

#### LayerSettingType
Change the **RigidbodyComponent's LayerSettingType** type and set it to follow MapleStory movement. Alternatively, you can set it differently from MapleStory movement by changing the relationship between the SortingLayer of the ladder/rope/foothold and the SortingLayer of the Entity with RigidbodyComponent.

As shown below, let's look at how the relationship between the avatar and the ladder changes according to each type when a tile is drawn on Layer 1 and a ladder is placed on Layer 1, 2, 3, and 4 respectively.
DefaultPlayer includes both RigidbodyComponent and AvatarRendererComponent. Let's see how SortingLayer of AvatarRendererComponent changes when adjusting LayerSettingType of RigidbodyComponent.

![layer](https://mod-file.dn.nexoncdn.co.kr/bbs/16740182831894ef72030e7074c09ab7a37a5e34bc3e0.png "layer")

* **<span style="color: #dc9656">All</span>**: Follows MapleStory movement. When RigidbodyComponent is stepping on the foothold, it follows the **SortingLayer of the foothold**. When climbing a ladder/rope, the SortingLayer value of AvatarRendererComponent follows the **SortingLayer of climbing on** value.
![all](https://mod-file.dn.nexoncdn.co.kr/bbs/16739467083350b8e1b78aa7f4626a6aa3a10630ee873.gif "all")

* **<span style="color: #dc9656">Climbable</span>**: The SortingLayer value of AvatarRendererComponent changes based on **SortingLayer of ladder/rope that ** RigidbodyComponent got on last. Even if the avatar steps down from the ladder and steps on the foothold, it follows the ladder's SortingLayer value.
 ![climable](https://mod-file.dn.nexoncdn.co.kr/bbs/1673946686984683664c7c7484952b78440846bdcf879.gif "climable")

* **<span style="color: #dc9656">None</span>**: Follow SortingLayer** of Entity with **RigidbodyComponent set by the creator. In the following example, the SortingLayer value of AvatarRendererComponent is Default. Therefore, the SortingLayer value maintains Default no matter which ladder you climb.
![none](https://mod-file.dn.nexoncdn.co.kr/bbs/1673946729714d6916a24f5b1426ea7df4a97d8a71964.gif{"width":"520px"} "none")

##### Reference Guide
* [Making Footholds]
* [Making a Moving Foothold]
* [Map Layer]
* [Create MapleStory Tile Map]

##### API Reference
* [RigidbodyComponent](/apiReference?postId=378{"target":"_self"})
* [MapComponent](/apiReference?postId=358{"target":"_self"})
* [TileMapComponent](/apiReference?postId=379{"target":"_self"})