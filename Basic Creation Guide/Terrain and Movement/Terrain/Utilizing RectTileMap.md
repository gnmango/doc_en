# Course Introduction
In MapleStory Worlds, you can create a top-down view world instead of side-scrolling using RectTileMap mode. Let's learn about RectTileMap in this course.

# RectTileMap and Player Movement
MapleStory Worlds allows you to create topography in MapleTile, RectTile, SideViewRectTile mod.
* <span style="color: #dc9656">**MapleTile**</span>
    * You can create a side-scrolling map in the form of MapleStory. [RigidbodyComponent](/apiReference/Components/RigidbodyComponent{"target":"_self"}) controls the player's movement.

* <span style="color: #dc9656">**RectTile**</span>
    * You can create a top-down view map. The player moves up, down, left, and right. [KinematicbodyComponent](/apiReference/Components/KinematicbodyComponent{"target":"_self"}) controls the player's movement.

* <span style="color: #dc9656">**SideViewRectTile**</span>
    * Based on RectTile, but the player moves side-scrolling. [SideviewbodyComponent](/apiReference/Components/SideviewbodyComponent{"target":"_self"}) controls the player's movement.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1658737408689f48e44ffdffb4cbd8a4934cd33fb2bbe.png "1")
Unlike MapleTileMap, in the RectTileMap environment, the player normally moves up, down, left, and right. [KinematicbodyComponent](/apiReference/Components/KinematicbodyComponent{"target":"_self"}) supports the player's movement up, down, left and right, jumping, and colliding with tiles. So, in a RectTileMap map, RectTileMap and [KinematicbodyComponent](/apiRefe	rence/Components/KinematicbodyComponent{"target":"_self"}) must be active on the player entity.
Because DefaultPlayer currently includes [RigidbodyComponent](/apiReference/Components/RigidbodyComponent{"target":"_self"}), [KinematicbodyComponent](/apiReference/Components/KinematicbodyComponent{"target":"_self"}), and [SideviewbodyComponent](/apiReference/Components/SideviewbodyComponent{"target":"_self"}), the movement control has a different subject depending on the map mode.

> <span style="color: #585858">**Learn more** <span style="color: #ab4642"></span>
> For the movement on RectTileMap, please refer to [Control Character Movement from RectTileMap](/docs?postId=748{"target":"_self"}).</span>

# RectTileMap Mode Change
You can change to RectTileMap mode using one of the two methods below.

| 1. Convert from Hierarchy | 2. Press the Tile Editor button to convert |
| :---: | :---: |
| Select **Hierarchy - Context Menu of the desired map - Switch To RectTileMap** <br> ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/168620964686807e552bd474248cdae01ec5e9d51530a.png "2") | Click the Tile Editor button for more than 1 second - Select the mode **RectTile** <br> ![sideview2](https://mod-file.dn.nexoncdn.co.kr/bbs/1658472803827bec010dc49a346efbc96c900cbe8ca0a.png "sideview2") |

# Creating and Editing Tile Sets
#### Adding and Editing Tile Set
Before we place tiles, we have to first make tile sets.
You can create and edit new tile sets in the tile set palette.
1. Create a tile set by clicking the **[+]** button in the tile set palette. 
    ![newtileset](https://mod-file.dn.nexoncdn.co.kr/bbs/1658800683076813781b77e774442b459cbf4857c82eb.png "newtileset")
    <br>
2. Create **NewTileSet** in **Workspace - MyDesk**.
    Feel free to rename the tile set. (Ex: TileSetExample)
    ![001](https://mod-file.dn.nexoncdn.co.kr/bbs/16808618014123977d1ec0ccb4476bcb815083121136f.png "001")
    <br>
3. In the drop box of the tile set palette, select the new tile set. (The first tile set you create is automatically selected.)
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1645606029806f7ed3d33041e480f9ecf2c6712caf4f4.png "7")

#### Adding a Tile to a Tile Set
Let's add the tiles we want to our new tile set.

*Add tiles using **Resource Storage**

1. Click the ![addtile_rect](https://mod-file.dn.nexoncdn.co.kr/storage/icons/UI/icon_addtile_rect.png "addtile_rect") button at the bottom of the tile set palette - Clicking on **Add ResourceStorage Image** opens **Resource Picker**.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/16530468681401f0132f4322843df987e486e38236682.png{"width":"450px"} "8")
    <br>
2. Add the built-in sprite as a tile in the **Resource Picker** - **MSW Resources** tab.
    ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1680863994283826ac603214b4807bc9c3809e9f898d8.png "9")

* Add an image from **Workspace as a tile**

1. Click the ![addtile_rect](https://mod-file.dn.nexoncdn.co.kr/storage/icons/UI/icon_addtile_rect.png "addtile_rect") button at the bottom of the tile set palette - Clicking on **Add Workspace Image** opens **Reference**.
    ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1653047049074a99d1de3a0084236b89f4d37942e0852.png{"width":"450px"} "11")
    <br>
2. Select an image from **Reference** to add to the tile set.
    ![12](https://mod-file.dn.nexoncdn.co.kr/bbs/1652917565746a7f06f04492340e9b1057560e5c1497d.png{"width":"580px"} "12")

* **Add a Tile Using Atlas Unpacker**
You can use a cut-out atlas from Atlas Unpacker by clicking **Add Rect Tile Set**. Please refer to [Utilizing Atlases](/docs?postId=687{"target":"_self"}) on how to cut the atlas.
The cut-out sprites are added as individual tiles to the selected tile set. 
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/16528661656754c6d218fbbe040bfba3c0c83bfd730a9.png{"width":"340px"} "12")

#### Setting Tile Properties
Switching to tile editing mode allows you to set tile properties.
Click the Edit button on the top right side of the tile set palette to change to tile property edit mode.
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/1682057408178785006c7aeee4236b8fca2b9a709fd44.png "12")

**Changing Tile Names**
You can double-click the text below the tile to rename the tile (ex: Tile02).
Tile names are used when creating tiles dynamically via script.
![13](https://mod-file.dn.nexoncdn.co.kr/bbs/164689337602731f90bf9601147b8b66f5ffa207ef6bf.png "13")

**Editing Sprites**
You can change the sprite image of the added tile.
1. Click the checkbox of **Edit Sprite** - Click the **[Edit Tile]** button - Open **Resource Picker**.
    ![00](https://mod-file.dn.nexoncdn.co.kr/bbs/1682057645787f5443a39cd6443eaafe9c45d9b45cad5.png "00")
    <br>
2. Select a sprite you'd like to use from **Resource Picker** to change the tile image to that sprite image.
    ![001](https://mod-file.dn.nexoncdn.co.kr/bbs/168205768599810bd27335f7a47999ef427e8286321fa.png "001")
    If you change the sprite of a tile already placed on the map, the tile will also change.
    ![16](https://mod-file.dn.nexoncdn.co.kr/bbs/16456061645131caf59bf83324f438f862a5af508ba87.png{"width":"640px"} "16")

**Editing Movable Tiles**
You can set whether the player can move to the tile.
Select the **Edit Movable Tiles** checkbox to edit properties.
![17](https://mod-file.dn.nexoncdn.co.kr/bbs/1682054762110b1ae7dca27344762a6611eb131d6caa6.png "17")

Clicking the arrow button will change the movable property.
* ![green](https://mod-file.dn.nexoncdn.co.kr/bbs/16636525526508fb98d4ba8974da19fa2ae6118820054.png "green"): Tiles to which the player can move
* ![red](https://mod-file.dn.nexoncdn.co.kr/bbs/1663652575156a3a8a3d780ee400a83a0b283ff8be1b0.png "red"): Tiles to which the player cannot move
![18](https://mod-file.dn.nexoncdn.co.kr/bbs/16820547187997dc64bf379dc4d52ab47eb6c91a33d90.png "18")

#### Removing Tiles from a Tile Set
Select the tile you want to remove from the tile set palette and click the **[Delete]** button.
![19](https://mod-file.dn.nexoncdn.co.kr/bbs/1645606220639116d64b78c364589b890061155388a68.png "19")

# Drawing Tiles
#### Placing Tiles
Select the desired tile from the tile set palette and drag to place the tile.
![20](https://mod-file.dn.nexoncdn.co.kr/bbs/16456062306076fefdf58a8264dbea4b8fcacb255d982.png{"width":"640px"} "20")

#### Convenient Tile Placement
You can use four different features to easily place the tiles. The default location is the upper left corner of **Scene**.
| Icon | Description |
| --- | --- |
| ![brush](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_tile_draw_brush.png "brush") | Tiles are drawn depending on the drawing movement. |
| ![tilefill](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_tile_draw_fill.png "tilefill") | A tile is drawn with the specified rectangular size. |
| ![tilemarquee](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_tile_elliptical_marquee.png "tilemarquee")| A tile is drawn inside the specified circular shape.  |
| ![timepaint](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_tile_draw_paint.png "timepaint") | The tile selected is placed over the entire map. |

#### Delete Placed Tiles
You can right-click on a tile to delete it.
If you right-click and drag, all tiles in the dragged position will be deleted.
![21](https://mod-file.dn.nexoncdn.co.kr/bbs/16456062487051dd3a7b4ed62497a9dafc5f8e690919b.png{"width":"640px"} "21")

#### Setting Tile Grid Size
The tile grid size can be set in **RectTileMap entity - RectTileMapComponent - GridSize** under the map currently being edited.
| GridSize (x = 1, y = 1) | GridSize (x = 0.5, y = 0.5) |
| --- | --- |
| ![22](https://mod-file.dn.nexoncdn.co.kr/bbs/16456062694889c7e72b895c140ce93b353ea7cc3ba33.png "22") | ![23](https://mod-file.dn.nexoncdn.co.kr/bbs/1645606280377b40fec3292dd4738a54caf7fee26fd98.png "23") |
| ![24](https://mod-file.dn.nexoncdn.co.kr/bbs/1645606294167b95cdf311685440e9bae999e36d3dfcc.png "24") | ![25](https://mod-file.dn.nexoncdn.co.kr/bbs/164560630741654c491198180455da76d8629fe612227.png "25") |

# Layer Setting
Adding a layer allows you to place another tile on top of a tile you have already placed.

#### Adding a Layer
1. If you press the **[+]** button at the bottom of **Map Layer** panel, a new map layer is added. 
    A new RectTileMap entity is also added to the **Hierarchy - Map Entity**.
    ![26](https://mod-file.dn.nexoncdn.co.kr/bbs/1661148577594d85c8f9327f04b3289a718aa44a2daef.png "26")
    <br>
2. If you place a tile after selecting a new layer, you can place another tile on top of the previous tile map.
    ![27](https://mod-file.dn.nexoncdn.co.kr/bbs/1645606334153ac37196bd49c453cb90f2b72229eafcb.png "27")

#### Applying Tile Set by Layer
When you add a layer, it defaults to using the old tile set in common. As such, if you change the properties of a specific tile, you may unintentionally change the properties of other layers as well.
For example, if you create **layer 2** when **tile set 1** is applied to **layer 1**, **tile set 1** will apply to **layer 2** as well.
Then if you change the property of **tile set 1** to change the image of the tile placed in **layer 1**, the change also occurs to tile placed in **layer 2**.
To avoid such an unwanted change, we recommend applying different tile sets for different layers.
![28](https://mod-file.dn.nexoncdn.co.kr/bbs/1645606345066334e779be0bd4779890385dcae16387c.png "28")

Here's how to add a different tile set for each layer.
1. Add a new tile set aside from an existing tile set. (Ex: TileSetExample2)
    Add some tiles to the new tile set as well.
    ![29](https://mod-file.dn.nexoncdn.co.kr/bbs/164560635412061c42fedcd1a4f32a853fbc48ee0940d.png "29")
    <br>
2. Adds a new map layer. (Ex: Layer 2)
    ![30](https://mod-file.dn.nexoncdn.co.kr/bbs/16644394470198d98072e5bb44d7fa21e9a5c1b092fa7.png "30")
    <br>
3. With the layer you added still selected, select the new tile set (Ex: TileSetExample2). The new layer is then applied with a new tile set. Then try placing a tile on the new layer.
    ![32](https://mod-file.dn.nexoncdn.co.kr/bbs/166443979497645960ec72f1d435698bad0417102aced.png "32")
    <br>
 4. Try selecting the layers one after the other in **RectTileMap** or **Map Layer** in **Hierarchy**.
    You'll see that different tile sets are applied to each **RectTileMap** in tile set palette.
    ![33](https://mod-file.dn.nexoncdn.co.kr/bbs/164560640382826178fc21ac34e7d99d4c13f1cc0f215.png{"width":"640px"} "33")

# Dynamic Tile Creation Using Script
RectTileMap entity's [RectTileMapComponent](/apiReference/Components/RectTileMapComponent{"target":"_self"}) offers various convenient functions, including tile placement or World coordinate conversion.
Let's place tiles dynamically using [RectTileMapComponent](/apiReference/Components/RectTileMapComponent{"target":"_self"}).

**Example overview**
* Sand tiles are placed in the map.
* When the player goes around Sand tiles and presses a certain key, the tile on which the player is standing changes to another tile.
* z key : Places Grass tile
* x key : Places Rock tile
$$youtube
krGgTjaJDL8
$$

1. Convert map to RectTileMap mode.
    ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/168620964686807e552bd474248cdae01ec5e9d51530a.png "2")
    <br>
2. Set the name as **DefaultTileSet** after adding a new tile set.
    ![35](https://mod-file.dn.nexoncdn.co.kr/bbs/1662618952893515b736d717e4972b5fe07a3d3c0e5cd.png "35")
    <br>
3. Click the ![addtile_rect](https://mod-file.dn.nexoncdn.co.kr/storage/icons/UI/icon_addtile_rect.png "addtile_rect") button at the bottom of the tile set palette - **Clicking on Add ResourceStorage Image** opens **Resource Picker**.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/16530468681401f0132f4322843df987e486e38236682.png{"width":"450px"} "8")
    <br>
4. Save the image below.
    ![37-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1645606456025f9dcb42718e54d048a4ef4d68caeb5c3.png{"width":"100px"} "37-1") ![37-2](https://mod-file.dn.nexoncdn.co.kr/bbs/164560648032304c0fb4f292b4738a29f50d683c16a74.png{"width":"100px"} "37-2") ![37-3](https://mod-file.dn.nexoncdn.co.kr/bbs/1645606496185fa29d88d5fe74731b4086ee5b157af0f.png{"width":"100px"} "37-3")
    Add a sprite by dragging the image saved in the **Resource Picker - etc** folder.
    ![38](https://mod-file.dn.nexoncdn.co.kr/bbs/1680863931512b691da8053614befabb2a18744ec2fca.png "38")
    <br>
5. Tiles are added by selecting the added sprites one after the other.
    ![40](https://mod-file.dn.nexoncdn.co.kr/bbs/16636620067727a0b1d62198d4c88bd0adcc1557464e3.png "40")
    <br>
6. Click the Edit Tile button in the tile set palette.
    Set the name of added tiles like the following.
    ![41](https://mod-file.dn.nexoncdn.co.kr/bbs/168205585653143803f0612be47e7937b4e451de0f5ea.png "41")
    | Sprite | Name |
    |:---:|:---:|
    | ![37-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1645606456025f9dcb42718e54d048a4ef4d68caeb5c3.png{"width":"100px"} "37-1") | Sand |
    | ![37-2](https://mod-file.dn.nexoncdn.co.kr/bbs/164560648032304c0fb4f292b4738a29f50d683c16a74.png{"width":"100px"} "37-2") | Grass |
    | ![37-3](https://mod-file.dn.nexoncdn.co.kr/bbs/1645606496185fa29d88d5fe74731b4086ee5b157af0f.png{"width":"100px"} "37-3") | Rock |
    <br>
7. Click the Edit Tile Properties button again to exit edit mode. 
    Select the Sand tile and place it as follows.
    ![42](https://mod-file.dn.nexoncdn.co.kr/bbs/1645606624729d3b155764feb452b8a536c74a08fb8b8.png{"width":"640px"} "42")
    <br>
8. In **Workspace**, add a new script component, enter <span style="color: #dc9656">**EditRectTile**</span> as its name.
    ![43](https://mod-file.dn.nexoncdn.co.kr/bbs/16611488907978a25e171e4884b109464ee22559ebff5.png "43")
    <br>
9. Add the **EditRectTile** component to **DefaultPlayer**.
    ![44](https://mod-file.dn.nexoncdn.co.kr/bbs/16611489073306b5098a2ff0548cc87bb5c84adc1288e.png "44")
    <br>
10. Open the **EditRectTile** script component.
    Add the `ChangeCurrentTile()` function and a **string**-type parameter, **tileName**.
   Write the function as follows.
    ```lua
    [server]
    void ChangeCurrentTile(string tileName)
    {
        if  tileName == nil then 
            return 
        end
                    
        local pos = self.Entity.TransformComponent.Position
        local tilemap = _EntityService:GetEntityByPath("/maps/map01/RectTileMap").RectTileMapComponent
                  
        -- Convert the world space coordinates to tilemap space coordinates
        local cellPos = tilemap:ToCellPosition(pos)
                    
        -- Return the information of the tile where the player is currently located
        local tileInfo = tilemap:GetTile(cellPos)
                  
        if tileInfo ~= nil then
            tilemap:SetTile(tileName, cellPos.x, cellPos.y)
        end
    }
    ```
    <br>
11. Add **KeyDownEvent** in **Event Handler**, and compose its content as follows.
    ```lua
    Event Handler : 
    [service: InputService]
    HandleKeyDownEvent(KeyDownEvent event)
    {
        -- Parameters
        local key = event.key
        --------------------------------------------------------
        if key == KeyboardKey.Z then
            self:ChangeCurrentTile("Grass") -- Z key: Place Grass tile
        elseif key == KeyboardKey.X then
            self:ChangeCurrentTile("Rock") -- X key: Place Rock tile
        end
    }
    ```
    <br>
12. Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and test.
    Move the player and press Z or X.
    Confirm the change in tile where the player is located.
$$youtube
krGgTjaJDL8
$$