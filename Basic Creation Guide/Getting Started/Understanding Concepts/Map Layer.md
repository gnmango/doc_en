# Course Introduction
You can use map layers to give your game a sense of depth.
Let's see how the Map Layer window looks, and how to use it.
You'll understand the order of layers and how to use them for your game.

# Map Layer Location
By the default panel settings, the Map Layer window is located at the bottom center of the screen.
When you have closed the tab, you can click <span style="color: #dc9656">**Panels - Map Layer**</span> to open it.

| Maker screen default location | Panels - Map Layer |
| ------------ | ------------------ |
| ![maplayer00](https://mod-file.dn.nexoncdn.co.kr/bbs/1661140579188d3b512992aad4a18bbc9411950f72d74.png "maplayer00") | ![maplayer](https://mod-file.dn.nexoncdn.co.kr/bbs/16595798309379692d0ca050848bea645debe9cb9286c.png "maplayer")|

> **<span style="color: #7cafc2">Tip</span>**
> Click <span style="color: #7cafc2"> **Panels - Reset Panels** to reset your panel status.</span>
> ![maplayer01](https://mod-file.dn.nexoncdn.co.kr/bbs/1682665165619440c6c5225ce4744b4e76a8245f1401a.png "maplayer01")

# Map Layer Window Configuration
![MapLayer01](https://mod-file.dn.nexoncdn.co.kr/bbs/1634540764998600230c000ac4c658b69d24a38131f32{"width":"500px"}.png)

| Number | Icon | Explanation |
| --- | --- | --- |
| ![NO01](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541272181b5c1a55fcf3d49b19734d25913c38583.jpg) | ![Common\_FocusOn](https://mod-file.dn.nexoncdn.co.kr/bbs/16345366745866d71bbf1f9f3454786e3f3477e268b01.png) | <span style="color: #181818">You can only check a single chosen layer. </span><ul><li><span style="color: #181818">None: All layers are activated.</span></li></ul><ul><li><span style="color: #181818">Focus: Only a single chosen layer is activated. Other layers will be dark and half-transparent, which tells you that they are deactivated. You cannot choose deactivated layers in the scene.|</span></li></ul> |
|  ![NO_02](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541300837cb541c2f44e046a79bb1901a885aa8ac.jpg "NO_02") | ![Common_show_black](https://mod-file.dn.nexoncdn.co.kr/bbs/1634538484615e92129425be84676a7af1ba82a8c1510.png "Common_show_black") | Deactivating a layer will hide its entities.<br>It's only hidden in the creation process. All deactivated layers are activated when running tests or releasing the world. |
|![NO_03](https://mod-file.dn.nexoncdn.co.kr/bbs/163454131465069e090278448490f965207e9a4a10348.jpg "NO_03") | Layer | You can create up to 10 layers. |
| ![NO_04](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541326353d8628c1473944497bf376acb7a65ca45.jpg "NO_04") | ![Common_lock](https://mod-file.dn.nexoncdn.co.kr/bbs/1634537017666e283f66030444da08eddef99819b2b07.png "Common_lock") | Lock the chosen layer. You cannot select or edit entities from a locked layer. |
| ![NO_05](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541338689678f574f21e54a6ca533737924124d7e.jpg "NO_05") | ![Common_layer_add](https://mod-file.dn.nexoncdn.co.kr/bbs/16345369150225f875c8c218444c58189ffac05201631.png "Common_layer_add") | You can add an empty layer. A new layer will be added on top of the entity that was last created, with the name LayerN (number). |
| ![NO_06](https://mod-file.dn.nexoncdn.co.kr/bbs/163454135201207284554a25b498380fff224cd767f6b.jpg "NO_06") | ![Common_LayerDelete](https://mod-file.dn.nexoncdn.co.kr/bbs/1634536995484055ff8f9ba99415486f815f599c0702d.png "Common_LayerDelete") | Delete a layer. Please be careful as deleted layers cannot be restored. |

# Understanding Map Layers
A layer is like a transparent board that you put on the map screen.
You can place multiple entities on top of it. Put another transparent on top of them to place new entities.
The bottom layer in the layer window is the furthest back in the map screen, and the top layer is at the front.
Entities placed on layers are all affected by the order. If you move the bottom layer to the top, the order of entities will change as well.
![maplayer01](https://mod-file.dn.nexoncdn.co.kr/bbs/1634601877817d0b37c6d8fbb4f668d37ea25d1783418.png "maplayer01")
![maplayer02](https://mod-file.dn.nexoncdn.co.kr/bbs/1634601899530a6e1a61208ba414ba15bb4a138e038de.png "maplayer02")

#### Layer Priority Rules
In the Maker, the order of entities is decided as follows:

1. **Map Layer** (SortingLayer of SpriteRendererComponent)
2. **OrderInLayer property of SpriteRendererComponent**
3. **Position Z value of TransformComponent** 
    Please refer to [Changing Location, Size, Rotation of Entities](docs?postId=82{"target":"_self"}).

#### Model's Layer Order
For placement convenience, each model type has default values for **OrderInLayer**.

![maplayer09](https://mod-file.dn.nexoncdn.co.kr/bbs/1682664093514566667bb5ccc473ca19732e72f275630.png "maplayer09")

<span style="color: #dc9656">**Default values for OrderInLayer in each model type**</span>

| OrderInLayer Value | Model Type |
| --- | --- |
| 0 | Object |
| 1 | Tile |
| 2 | Monster, NPC, foothold, ladder, rope, portal, trap, item |
| 3 | Other user's avatar |
| 4 | My avatar |
You can modify classified values to change the placement order, but please avoid entering a value that's 3 or higher.
Otherwise, your avatar may be covered when playing the world.

> <span style="color: #585858">**Learn more**</span>
> <span style="color: #585858">If you place multiple entities from the same classification in a single layer, the last entity you placed will be located at the front.</span>

# Relationship Between Map Layer and Player Avatar
A player avatar is an entity, but it has a few differences from other entities.
* A player avatar in editing state cannot be placed on the map, and it doesn't belong to a specific layer.
* Other entities belong only to the first map layer they are placed on, regardless of their movement. However, the player avatar will change which map layer it is on according to the entity it is currently stepping on (tile, foothold, etc.).
Take a look at these two examples to understand the relationship between map layers and player avatars.

#### Example 1
The dark green tile is placed on Layer1, and the yellowish green tile on Layer2.
The player avatar on the dark green tile belongs to Layer1 until it jumps in the air. However, as soon as it lands on the yellow tile, it belongs to Layer2.
![maplayer04](https://mod-file.dn.nexoncdn.co.kr/bbs/16346025247675038e0d2fd854f5e80a5359fe45d2df8.gif "maplayer04")

#### Example 2
Each of the 4 layers have tiles and objects. (OrderInLayer is set to default.)
Which layer the player avatar is on depends on the entity layer it is currently standing on. So it starts to move from the bottom Layer1. Tree objects on the green tile are placed on Layer4. Therefore, the avatar stepping on the green tile moves behind the tree objects.

When the player avatar moves to the yellowish green tile, it belongs to Layer3. Toy objects on the yellowish green tiles are placed on Layer2. Because the yellowish green tile belongs to a layer on top of Layer2, the avatar walks past the toy objects.

You can make good use of layers and objects to stage the avatar going through burning fire or thick bushes.
![maplayer05](https://mod-file.dn.nexoncdn.co.kr/bbs/1634602745986d158da5858dc44be9481dbd8b21ef50e.gif "maplayer05")
![maplayer07](https://mod-file.dn.nexoncdn.co.kr/bbs/1634620453053bba58d9571604df192a0ce5d56451b6d.png "maplayer07")

# Summary
You can use map layers to classify and manage entities by their characteristics. This is especially useful when you're making a vast, complex map. If this is your first world, you might find yourself making thing up as you go. Learning the rules of layers will help you improvise as you create your worlds.