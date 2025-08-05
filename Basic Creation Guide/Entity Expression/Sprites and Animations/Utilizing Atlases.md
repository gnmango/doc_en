# Course Introduction
You can slice an atlas into the required standard and utilize it in various ways. Let's learn about the composition of the Atlas unpacker as well as how to use it.

# The Atlas Unpacker
An "atlas" refers to an image which can be divided into several sprites. Atlases can be used in several ways, including slicing an atlas to get RectTile sprites.
The Atlas Unpacker can be used to slice atlases into separate sprites and edit it to save. After filling out the required fields, the sliced sprites can be saved in Resource Storage, or used as RectTiles in TileDataSet immediately.
The single atlas' maximum size to slice in the Atlas Unpacker is 2048 x 2048px, and you can slice it into a maximum of 256 pieces.
![AtlasUnpacker](https://mod-file.dn.nexoncdn.co.kr/bbs/168774586730023f9ad9f8ca645fbac5d4e64189423e1.png "AtlasUnpacker")

| Number | Explanation |
| --- | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg) | Select File: Load the image to be sliced. <ul><li>All: You can select only a certain sprite you want to save from the list of all sliced sprites, or select/deselect the entire list.</li><li> File Name Prefix: This is a required item to save the sliced images. The number will increment by one for each stored image, and they will be stored as 'name_number'.</li></ul> |
| ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg) | <ul><li>Slice: Set the number of cells or the size of the sprites created. </li><li>Scale: You can enlarge/reduce the image or return it to its original state.</li></ul> |
| ![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg) | <ul><li>Category: Select the category where the sliced images will be stored. The selected images will be stored in Resource Storage under the chosen category.</li><li>Add to RectTile Set: The sliced images will be added to the selected RectTile set.</li><li>Add to AnimationClip: When uploading the cut images with this option selected, the images will be immediately added to the AnimationClip Editor window.</li></ul> |
| ![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg) | A preview window where you can see the loaded atlas. Right-click and drag to pan the screen.<br>When you slice an atlas, a blue line is generated on top of the image. |
| ![5](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg) | The sliced images are stored according to the setting in number 3. |

# Slice Atlas
1. Select **Window - Atlas Unpacker** and open the Atlas Unpacker window<br> ![01](https://mod-file.dn.nexoncdn.co.kr/bbs/16528540865389d7c47d2f2b747c99924d748f6c18163.png{"width":"540px"} "01")
2. Press Select File and load the atlas you'd like to slice. Select **Slice** from the drop-down menu and enter your desired settings.<br>![02](https://mod-file.dn.nexoncdn.co.kr/bbs/16877460545951b7bb92f466947bea630a6698bdfec2b.png "02")
3. Deselect any unnecessary images among the sliced sprites, and then press the **[Upload]** button to save the images to Resource Storage.<br>![03](https://mod-file.dn.nexoncdn.co.kr/bbs/1687746080484b55f93689cd040bf82b01b4c40e0ee73.png "03")

#### Slice Cell Size and Number of Cells
For cell size, the same size of the square will be created and slice, matching the atlas size depending on the set X and Y values. The number of cells is different depending on the number of rows and columns that set the whole image.
Some of atlas' margins, which are not included when slicing the image following the slice setting, are not saved as sprites.

![04](https://mod-file.dn.nexoncdn.co.kr/bbs/1687746116339a207552520b348adb3bef6b862053083.png "04")

#### Adjust Offset
You can adjust the offset based on the X and Y axes. Adjust the position to sliced atlas following the offset values set.

![08](https://mod-file.dn.nexoncdn.co.kr/bbs/168774613832540e9fddfaea740bbbfc204cc82b984dc.png "08")
#### Adjust Padding
If you adjust padding values, there will be a margin following the X and Y values based on the divided cells, and it will slice the atlas except for the margin.
![09](https://mod-file.dn.nexoncdn.co.kr/bbs/1687746162369f04b87a0d37749bdafdb67f8de5ba878.png "09")

# Use with RectTile
If you want to use the sliced sprites directly as RectTiles, you can use the <span style="color: #dc9656">**Add to RectTile Set**</span> feature. 
Refer to [Utilizing RectTileMap](/docs/?postId=589{"target":"_self"}) for more information on how to use RectTiles.

1. Press **Workspace - MyDesk > Create TileDataSet** and create a new tile data set to add the sliced sprites.
2. Press the **[⋯]** button and select **TileDataSet** to add the sliced sprites from the **Reference**.<br>![7](https://mod-file.dn.nexoncdn.co.kr/bbs/168774618410097ac8ece190746b5a9300708eba55578.png "7")
3. After setting the file name prefix and details, press the **[Upload] button**.  You can see that all the images you selected have been added in **TileDataSet**.

