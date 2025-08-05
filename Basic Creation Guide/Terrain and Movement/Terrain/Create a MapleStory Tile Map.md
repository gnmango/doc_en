# Course Introduction
MapleStory Worlds provides many options for landscapes and traversing them.
Today, we'll learn about MapleStory-formed tile maps, the most basic landscape.
MapleStory-formed landscape is closely related to MapleStory-formed movement. 

> <span style="color: #585858">**Learn more** <span style="color: #ab4642"></span>
> Please refer to the [Understanding MapleStory Movement Concepts] lesson for MapleStory-formed movement.</span>


# MapleStory-Formed Terrain
You can create MapleStory-formed landscapes after selecting MapleTile mode in the Tile Editor.
![tileEditor](https://mod-file.dn.nexoncdn.co.kr/bbs/1658282762014c76e4b1dd653437aa0b54c0f45122fb0.png "tileEditor")
To create terrain, long-press the Tile Editor button in ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1"), select MapleTile mode, and then choose the desired tile from the **Preset List** ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2"). However, only one TileMap mode can be used on one map. For example, when you create terrain with MapleTile mode and then convert it to a different TileMap mode, the existing terrain will all be reset, so proceed with caution.
When you select TileMap Mode, a map component that corresponds to that TileMap mode will be created in the **Hierarchy**.
If you select MapleTile ![map](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/map.png "map") for TileMap mode, you can check the TileMap entity in the Hierarchy ![hierarchy](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene_maker.png "hierarchy") and change the properties, color, or form of MapleTile ![map](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/map.png "map") through the TileMapComponent included in this entity.
By selecting MapleTile in TileMap mode, you can see the **TileMap** entity in the **Hierarchy**. Within this entity, you can use **TileMapComponent** to modify the properties, color, shape, and other attributes of MapleTile.

# Creating Terrain
## Select Tile
You can freely create terrain across a very wide range using the Maker's range of convenient features. 

![tilepick](https://mod-file.dn.nexoncdn.co.kr/bbs/1657699332147b812f9a825ea47818c4bd63cfc4b6d68.png "tilepick")

In order to create terrain, select the **TileSet** you want from the **Preset List**. Only one **TileSet** can be used in each **Map Layer**.
![maplayer](https://mod-file.dn.nexoncdn.co.kr/bbs/16577003279588aa2927b92194ba99c8f3cfb929b5e1c.png "maplayer")
By pressing the ![addlayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_layer_add.png "addlayer") button in the **Map Layer** as shown above, you can add layers and use multiple **TileSet** in a single map.

> **<span style="color: #7cafc2">Tip 1</span>**
> <span style="color: #7cafc2">If you change TileSet, all the tiles you placed on the layer earlier will be changed to the new TileSet, so you must be careful.</span>

> **<span style="color: #7cafc2">Tip 2</span>**
> <span style="color: #7cafc2">You can even modify the TileSetRUID of `TileMapComponent` and change TileSet.</span>


## Indicate Tile Auxiliary Line
There is a ![gridinfo](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_grid2.png "gridinfo") grid information button on the top-right side of the **Scene**. 
![grid](https://mod-file.dn.nexoncdn.co.kr/bbs/1657710972697056c508aa0f047ea994612920898098c.png "grid")
If you press the grid information button (![gridinfo](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_grid2.png "gridinfo")), you will see an auxiliary line of a grid form. This auxiliary line helps craft landscapes.

## Draw Topography
You can select a method to draw landscapes from a Draw Topography Tool.
![tiletool](https://mod-file.dn.nexoncdn.co.kr/bbs/1657711565470477997a2f4b14bf093847b8c29899d77.png "tiletool")

| Numbers | Items |Description|
| :---: | --- |--- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1") | Paint Line | Draws tiles at the point where you clicked, and draws along a line when you drag. |
| ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") | Paint Box | Fills the inside of the square you dragged with tiles.  |

If you click the grid-formed auxiliary line in Paint Line mode, the landscape shape created by the click position will differ.

| Click Position | Created Landscapes | Description|
| :---: | --- | --- |
| Point |![line1](https://mod-file.dn.nexoncdn.co.kr/bbs/1657712926265695555a3dfba412796f23cbf1ca5b906.png "line1") | Click the point where the auxiliary lines cross to create a small area.|
| Line |![line2](https://mod-file.dn.nexoncdn.co.kr/bbs/1657713375917614def59a5674c0289c399b71c910bd3.png "line2")| Click the line to create an area including both crossing points. |
| Surface |![line3](https://mod-file.dn.nexoncdn.co.kr/bbs/16577598884962e68f73359ad41aa82bf4279e3f87f9d.png "line3") <br> ![line4](https://mod-file.dn.nexoncdn.co.kr/bbs/165776120779882e8cb42c179416bae4c8fd6fdeab856.png "line4")  | Click the surface to create an area including the surface's edge lines. <br><br> In addition, if you click the middle of the surface while there are areas below and at the sides, this will create a diagonal area. |

## Erase Topography
### Erase by Right-Clicking
In Draw mode, you can erase a landscape by right-clicking. This is the only function of right-clicking while landscape editing.
![rightClick](https://mod-file.dn.nexoncdn.co.kr/bbs/16582894594106cf5ad992ec04c6a9d79ccb1f3616aaa.gif "rightClick")

### Erase with an Eraser
You can erase landscapes using an eraser as well.
![erase](https://mod-file.dn.nexoncdn.co.kr/bbs/1657762131044a2e122fbd3f1482698e73370ac8d4be0.png "erase")
To use the eraser cursor, select the ![eraser](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_eraser.png "eraser") eraser cursor and then click the terrain in the **Scene** to erase it.
However, the whole eraser cursor deletes other entities, not just tiles, so you must be careful when using it. Want to erase only tiles or only entities? We can change the eraser cursor options for this.
![erase2](https://mod-file.dn.nexoncdn.co.kr/bbs/16577635580024e93d1c0c63b490a99cfbc548a3604c4.png "erase2")
By using **TileOnly**, you can erase only the tiles, or by using **TileExceptOnly**, you can erase entities other than tiles.

## Foothold
The biggest reason we create landscapes is that you need space where entities can move. The movable area in MapleStory Tile is called <span style="color: #dc9656">**Footholds**</span>. There are two movement methods of [RigidbodyComponent](docs?postId=750{"target":"_self"}) that pair with MapleStory tiles.

**<span style="color: #dc9656">1. Move on the foothold
2. Move outside of the foothold**</span></span>

Footholds are created by a landscape creation rule, and are mostly matched with landscape one-on-one. When you press the foothold information button (![footholdicon](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_foothold.png "footholdicon")), you will see the foothold information with a red line above the landscape.
![foothold](https://mod-file.dn.nexoncdn.co.kr/bbs/16577650286084cedcd11123845d9a418c15946fd025f.png "foothold")

> <span style="color: #585858">**Learn more**
> You can also add a Foothold to an entity using `CustomFootholdComponent`, not just landscapes.
> For details, please refer to [Making Footholds]. </span>