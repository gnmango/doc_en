# Course Introduction
Let's learn how you can draw unique RectTiles that are perfect for your world.
You can use whichever image editing software you are most comfortable with, but for this exercise, we will be using Adobe Photoshop to create tiles.
Once you've created a sheet, you can refer to [Utilizing Atlases] to cut the sheet to the required size for your tiles.

##### Reference Guide
[Utilizing RectTileMap]
[Utilizing Atlases]
# Tile Type

To design a colorful world map, you need a variety of tiles. If you were to make a driveway, you'd also need to make intersections, dead-end streets, and so on. MapleStory Worlds' tiles can be divided into the following types:

1. **Default tiles**: There are two types of default tiles: 
    * **Unpatterned tiles**: Used to create maps. They lack the impession of height.<br>![tile01](https://mod-file.dn.nexoncdn.co.kr/bbs/1683172461007e5dd537aba3b4c36a7988bc6e051253b.png{"width":"140px"} "tile01")
    * **Patterned tiles**: Tiles with patterns that can be cleanly tesselated. They lack the impession of height.<br>![Tile02](https://mod-file.dn.nexoncdn.co.kr/bbs/1683172485011d380326979b94099928580e77910ef59.png{"width":"140px"} "Tile02")
2. **Connection tiles**: Can be used in conjuction with the two default tile types to create driveways, walkways, paths, and more.<br>![Tile03](https://mod-file.dn.nexoncdn.co.kr/bbs/1683172563731bbae861a1a4a4e159914605d974e095c.png{"width":"430px"} "Tile03")
3. **Height tiles**: Tiles used to give players the impression of height or imply that they can not travel any further.
    * **Wall tiles**: Tiles that give the impression of being blocked. Use two 64-pixel spaces for best results.
    * **Stair tiles**: When placed between wall tiles, these tiles look like a place where a player could move up or down.

# Preparation
Default tiles are 64x64 pixels in size. In some cases, multiple height tiles may be needed.

> <span style="color: #7cafc2">**Tip**
> To display 64 x 64 pixel grid lines on your canvas, follow the steps below.
> In **Edit - Preferences - Guidelines, Grids, and Split Zones - Grids**, set the grid spacing to **64px** and press OK.</span>
> ![Tile04](https://mod-file.dn.nexoncdn.co.kr/bbs/172722843000891d9a4b3ffa9426185b04dda2c7d7fc9.png{"width":"506px"} "Tile04")


# Drawing Default Tiles 
We recommend using the Pencil Tool. Setting the brush to a size of **1px** will allow you to draw fine detail.

#### Unpatterned Tiles
Let's draw a tile to be used as a lawn. To quickly apply texture, we'll use a noise effect.

1. Draw a base color for the tile on the canvas.
![tile05](https://mod-file.dn.nexoncdn.co.kr/bbs/1683177510180846b5f4a72854ca09cb73fe35a50bf25.png{"width":"240px"} "tile05")
2. Let's add some noise to give our grass texture.
Select **Filter - Add Noise**. Select the distribution as **Gaussian** to avoid banding.
![tile06](https://mod-file.dn.nexoncdn.co.kr/bbs/17272306437656297277c04494d64a14707117aba9edd.png{"width":"340px"} "tile06")
3. Add a layer, zoom in on the canvas, and use the **Dropper Tool** to extract the darkest color of our grass.
4. You can use this dark color to draw in details suited for your world. Feel free to add additional layers and add flowers, stones, bugs, or whatever you please.
![tile07](https://mod-file.dn.nexoncdn.co.kr/bbs/1683178056930bd6b1b6e43774f0bbad8cd074b5a997c.png{"width":"240px"} "tile07")

> <span style="color: #7cafc2">**Tip**
> You can use the **pattern preview feature** to see if the tiles will look natural when placed in a row.</span>
> ![tile9](https://mod-file.dn.nexoncdn.co.kr/bbs/1727230704597cda42c7095944568a3c7ecc22f38af8d.png{"width":"240px"} "tile9")

#### Patterned Tiles
For patterned tiles, it's best to draw the pattern yourself rather than using an automated effect.

1. Draw a base color on the canvas.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1684213100045b739955101124b2e9c94a4e1f790a164.png{"width":"240px"} "9")
2. Use the Pencil Tool to draw patterns. You can use patterns of different sizes to create a more natural pattern. Make sure the edges of your pattern will line up well when placed next to each other. When drawing patterned tiles, we recommend adding irregular shapes to prevent the pattern from being too simple.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16842125974123f276d4479a1433c9fb49407ad3ec8fa.png{"width":"240px"} "5")
3. Once the patterns naturally connect, add additional colors to liven it up.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1684212623125803ddf134ed84e8ba260cc08d9ab2d6b.png{"width":"240px"} "6")
4. If the lines look blurry, use a darker color than you used for the outline.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1684213063427fd9e3ff1e8c54d09b587c485d32ddc6e.png{"width":"240px"} "9")
#### Connection Tiles
A minimum of 12 tiles are required to create a path, such as a road or trail, and more tiles may be required depending on the creator's intent. Assemble your 12 tiles as a 3-by-3 square attached to a 2-by-2 square (see images below).
When drawing connection tiles, you should use multiple layers and increase the size of your canvas so you can better see how the tiles will look when placed together.

1. Transfer the default tiles you've created in all 12 spaces to their respective layers.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/16841253385017f35e462522443939954b1c45e9af49b.png{"width":"330px"} "11")
> <span style="color: #7cafc2">**Tip**
> The Quadrangle Tool makes it easy to add the tiles you've created.
> Quadrangle Selection Outline Tool (M) - Copy (Ctrl+C) - Paste (Ctrl+V) </span>

2. Add an empty layer and use the Quadrangle Tool to set the area you want to erase within the 9 tiles. 
![tile12](https://mod-file.dn.nexoncdn.co.kr/bbs/1683600217782267bb2460a3648d795ae8035dbfc6943.png{"width":"330px"} "tile12")
3. Use the Quadrangle Selection Outline Tool to select a small quadrangular area on the default tile layer. Tap the Delete key to clear the inside.
![tile14](https://mod-file.dn.nexoncdn.co.kr/bbs/16836003085094e6f0e1c4b384cd79fa4883cf8baf54a.png{"width":"330px"} "tile14")
4. Draw a shadow on the default tile layer so the border where the two tiles meet looks like a natural connection.
5. Within each space of the four tiles, use the Quadrangle tool to draw the area you want to erase. It's best to draw the area in a small size in the center of each tile.
![tile15](https://mod-file.dn.nexoncdn.co.kr/bbs/16837929814178a2fa1b9d3a84089811d9668aa1abd79.png{"width":"330px"} "tile15")
6. Erase the area outside the quadrangle from the layer of default tiles. Draw a shadow so the tiles have a natural transition where they overlap.
![tile15](https://mod-file.dn.nexoncdn.co.kr/bbs/1683792351851bdf013fb85324732ba72cacad615b344.png{"width":"330px"} "tile15")

> <span style="color: #b8b8b8">**Learn more**
> In the free space on the sheet, you can use the Quadrangle Selection Outline Tool to simulate the placement of the connection tiles you've created.
> You can try different combinations to see if the tiles you've drawn all flow together naturally, so you can modify it.</span>
> ![Tile03](https://mod-file.dn.nexoncdn.co.kr/bbs/1683172563731bbae861a1a4a4e159914605d974e095c.png{"width":"430px"} "Tile03")

# Drawing Height Tiles 
Depending on your intent, you can make height tiles to create a map that feels three-dimensional. Here's how to create wall tiles and stair tiles. You can apply and draw the tiles according to the intention of the height tile creator.
 
#### Wall Tiles
Draw the wall tiles using the following procedure:
![tile17](https://mod-file.dn.nexoncdn.co.kr/bbs/16837937290502fb16601298145539698b1e606f50ad4.png{"width":"440px"} "tile17")
1. Draw a base color on the canvas.
![19](https://mod-file.dn.nexoncdn.co.kr/bbs/168379507952663b08c218c404a698ed06d5d712c239d.png{"width":"180px"} "19")

For the height tiles, think of and draw 2 spaces as the basis for creating a tile. We recommend you draw the tiles with a 4-to-6 pixel margin above and below the space, rather than trying to fill the entire space. This is because filling the entire tile space creates an awkward sense of space when characters and tiles overlap.
![tile17](https://mod-file.dn.nexoncdn.co.kr/bbs/1683793076682f42c6e4d211249a085896ac4d968d131.png{"width":"340px"} "tile17")

2. Use the Brush and Pencil tools to draw tiles. Draw the part that you see from the front, leaving a remaining part that will be the top of the wall.
![18](https://mod-file.dn.nexoncdn.co.kr/bbs/1683799584550110a5602bd174daab81d06aefb44885a.png{"width":"180px"} "18")
3. You should think and draw things that are visible from above. For a stone wall, the top of the wall should also be visible. 
![22](https://mod-file.dn.nexoncdn.co.kr/bbs/168380235708851a9316b524646f78d3730eda72853bb.png{"width":"180px"} "22")
3. Enable the Pattern Preview feature. Check to see if there are any awkward spots when the tiles are placed together.
![19](https://mod-file.dn.nexoncdn.co.kr/bbs/1683799652780e101c88d4935443d8ffa868750dd2818.png{"width":"420px"} "19")
4. Once you've made sure the patterns flow naturally, color the tiles to create contrast. Paint the top with a lighter color and the bottom with a darker color to create contrast.
![20](https://mod-file.dn.nexoncdn.co.kr/bbs/16838017207928e91a3c7af2a4bfb9f90261110122226.png{"width":"180px"} "20")
6. Finish the tiles by painting the shadows and grass.
![23](https://mod-file.dn.nexoncdn.co.kr/bbs/16838027731038ea6a3aeb2684b818debeb2de2fe1dff.png{"width":"180px"} "23")

#### Stair Tiles
Stair tiles are often used in conjunction with other tiles, particularly wall tiles. So it's a good idea ensure your stair and wall tiles look good together while you work. Here, let's learn how to draw stair tiles between wall tiles.

1. Place the wall tiles you've created on either side and draw a quadrangle with a specific background color in the center. The quadrangles should be made inward against both wall tiles.
![24](https://mod-file.dn.nexoncdn.co.kr/bbs/16838048081190e727c53a86f461cb048285a5692503b.png{"width":"440px"} "24")
2. Use the Pencil Tool with a color slightly darker than the background color to draw the shape of the stairs.
![25](https://mod-file.dn.nexoncdn.co.kr/bbs/1683805578337318af7a249044751bcd3c360ffbccb91.png{"width":"440px"} "25")
3. Draw the top of the stairs lighter and the bottom darker to give the stairs some contrast.
![27](https://mod-file.dn.nexoncdn.co.kr/bbs/1683806374729a2adb6d3cbb14d02a8cc06ade600f063.png{"width":"440px"} "27")

> <span style="color: #585858">**Learn more**
> You can create different shapes of height tiles and use them as shown below.</span>
> ![28](https://mod-file.dn.nexoncdn.co.kr/bbs/16841254749616f0d753209764215945f389c15eac91c.png{"width":"440px"} "28")