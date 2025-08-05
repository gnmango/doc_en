# Course Introduction
RectTileMapComponent and KinematicbodyComponent are suitable for top-down view game production, but not for side-scrolling game production. SideViewRectTile supports side-scrolling-type movement, jumping, and collisions with tiles to produce side-scrolling games easily using RectTileMap.
We'll discuss how SideViewRectTile mode works, and create a map.

> <span style="color: #585858">**Learn more** <span style="color: #ab4642"></span>
> SideViewRectTile mode is based on RectTile mode. Before trying this course, please refer to [Utilizing RectTileMap](docs?postId=589{"target":"_self"}) first.</span>


# Change SideViewRectTile Mode
To use SideViewRectTile, you need to change the map tile mode.
The first way to change Map Tile mode is to select the current map entity from the Hierarchy ![hierarchy](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene_maker.png "hierarchy") and select <span style="color: #dc9656">**Switch To SideViewRectTileMap**</span> from the right-click menu.
![sideview1](https://mod-file.dn.nexoncdn.co.kr/bbs/1686212940229528039cf5e884de28be22ce0db30fcd2.png "sideview1")

The second way is to long-press the Tile Editor button on the top menu to open the Select Tile Mode menu, and then select <span style="color: #dc9656">**SideViewRectTile**</span>.
![sideview2](https://mod-file.dn.nexoncdn.co.kr/bbs/1658472803827bec010dc49a346efbc96c900cbe8ca0a.png "sideview2")

If you select SideViewRectTile mode, the Scene ![scene](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene.png "scene") will change as below.
![scene](https://mod-file.dn.nexoncdn.co.kr/bbs/16587495455372f3485bfe4304859a62f38fe743d002d.png{"width":"750px"} "scene")


# Create SideViewRectTile Map
Creating a map in the SideViewRectTile mode is generally the same as the RectTileMap production process.
We'll create a simple SideViewRectTile map as shown below.
![sideviewmap](https://mod-file.dn.nexoncdn.co.kr/bbs/1658474735348794bdba4e80f40338aefc482a15595e0.png "sideviewmap")

First, let's save the four images below.
![tundraRight](https://mod-file.dn.nexoncdn.co.kr/bbs/16584737735555de4cfef1e6846b881fb08dd974e18f1.png "tundraRight") ![tundraMid](https://mod-file.dn.nexoncdn.co.kr/bbs/1658473786750f09028c3d7164a2694e9f0a099456a1c.png "tundraMid") ![tundraLeft](https://mod-file.dn.nexoncdn.co.kr/bbs/1658473808386f01655943b5f452a842aaf7f7ba3c4a6.png "tundraLeft") ![ice](https://mod-file.dn.nexoncdn.co.kr/bbs/1658473825666f3080a29dece4ce685465b7018636cbe.png "ice")

1. Change the tile mode to <span style="color: #dc9656">**SideViewRectTile**</span>.
<br>
2. Add the four images you downloaded above to ![resourcestorage](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_resource_storage.png "resourcestorage") Resource Storage - My Resource - etc.
![rs](https://mod-file.dn.nexoncdn.co.kr/bbs/165874972066502a39c24bbd646c384b9486e3bb4881d.png{"width":"750px"} "rs")
<br>
3. Search back-4 on the Preset List ![preset](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_asset.png "preset") - Background and then select it to change the map's background.
![bg](https://mod-file.dn.nexoncdn.co.kr/bbs/1658475486669606fbfbe07024b8dad2697df10431fec.png "bg")
<br>
4. Press the "[+]" button on the upper left of the Tile Set Palette, and then select <span style="color: #dc9656">**Create Empty TileSet**</span>. Enter the name of the new Tile Set as <span style="color: #dc9656">**TileSet**</span>.
![newtileset](https://mod-file.dn.nexoncdn.co.kr/bbs/165874984152737132bbc9da841f7b3ba6690a8d9fefd.png "newtileset")
<br>
5. On the bottom of Tile Set Palette, click the ![add](https://mod-file.dn.nexoncdn.co.kr/storage/icons/UI/icon_addtile_rect.png "add") icon on the lower left of the Tile Set Palette, and then select <span style="color: #dc9656">**Add ResourceStorage Image**</span>.
![add](https://mod-file.dn.nexoncdn.co.kr/bbs/16587499138192f06f82cfafb4535a1b7431d757df55d.png "add")
<br>
6. Select the images you have added in advance from the Resource Picker.
![picker](https://mod-file.dn.nexoncdn.co.kr/bbs/1681090600455cf1b5be97cca41538edc1002fed32cee.png "picker")
<br>
7. Select a tile from Tile Set to decorate the map as desired. In addition, place suitable objects for the map.
![mapedit](https://mod-file.dn.nexoncdn.co.kr/bbs/16584776165678f4bf1e92d9845bcb7a0745d3802952d.png{"width":"750px"} "mapedit")
<br>
8. Set the tile as immovable to make the character stand on the tile.
![move](https://mod-file.dn.nexoncdn.co.kr/bbs/16587502264797ed1bf2dbc3447d39081743abf93e075.gif "move")
<br>
9. Press the **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") button  to check if the side-rolling movement works well.
![play](https://mod-file.dn.nexoncdn.co.kr/bbs/1658478611789db5a20547f4f40a5bec1c0b961ad97a7.gif "play")

> <span style="color: #585858">**Learn more**
> For the movement on SideViewRectTile, please refer to [Controling Character Movement from SideViewRectTileMap](docs?postId=759{"target":"_self"}).</span>