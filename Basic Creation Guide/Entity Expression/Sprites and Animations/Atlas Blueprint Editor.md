# Course Introduction

You can efficiently load images by packing the sprites used in the World you are creating into an Atlas.

# Atlas

An (Atlas) is a single large image made up of several sprites. Atlas has RUID information for bound sprites, so when calling Atlas, RUIDs included in the Atlas are called together to help improve World performance.

# Introduce AtlasBlueprint

AtlasBlueprint is an entry used to bind sprites to an Atlas. To create an Atlas, AtlasBlueprint must be created first, and sprites must be added to the corresponding AtlasBlueprint in the AtlasBlueprint Editor.
The AtlasBlueprint can be created in 3 ways.

1. **Create - Create AtlasBlueprint**
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1686213349231380059f652bd4f4cad87d2486766311f.png "1")
2. **Workspace - [+] - Create AtlasBlueprint**
    ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16813720723871d71d34343e4463e9bba1ef8de5e0919.png)
3. **Workspace - MyDesk - Create AtlasBlueprint**
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/168137208490909091639575b490084b0ced4b4033f9b.png)

After creating AtlasBlueprint and selecting it in **Workspace**, you can see it in the **Property** window as below. Click the **[Open AtlasBlueprintEditor]** button to open the AtlasBlueprint Editor window.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/17258656426015abcefd7d38446dd99abe03fe8026c95.png "8")

# Introduce AtlasBlueprint Editor

![AtlasBlueprintEditor](https://mod-file.dn.nexoncdn.co.kr/bbs/1688706053601b1d993cf2c8b49729f904b0a341f92c2.png{"width":"1080px"} "AtlasBlueprintEditor")
| Numbers | Description |
| --- | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg) | You can search sprites. |
| ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg) | <ul><li>**<span style="color: #dc9656">Resource in Use</span>**: Shows the sprites being used by the map being edited.</li><li>**<span style="color: #dc9656">Workspace</span>**: Shows the sprites added to the Workspace.</li><li>**<span style="color: #dc9656">Default UI</span>**: Shows only the default UI provided by MSW.</li><li>**<span style="color: #dc9656">My Resources</span>**: Shows only sprites added by the creator.</li><li>**<span style="color: #dc9656">MSW Resource</span>**: Shows only resources provided by MSW.</li></ul> |
| ![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg) | Resource categories. Resources are divided into <span style="color: #dc9656">**Sprite**</span> and <span style="color: #dc9656">**AnimationClip**</span>. |
| ![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg) | A thumbnail of the sprite. Allows you to check sprite information. If it is a sprite that has already been added to AtlasBlueprint, a warning![error](https://mod-file.dn.nexoncdn.co.kr/storage/icons/Atlas_Blueprint_Editor/icon_atlasblueprint_error.png) message appears. |
| ![5](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg) | This button adds the selected sprites to the designated AtlasBlueprint. |
| ![6](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_06.jpg) | Specifies the desired AtlasBlueprint to pack. The selected sprites are added to the specified AtlasBlueprint.<ul><li>You can use the [+] button to generate a new AtlasBlueprint.</li></ul> |
| ![7](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_07.jpg) | <ul><li>**<span style="color: #dc9656">Maximum Size</span>**: Sets the size of AtlasBlueprint. A size of **<span style="color: #dc9656">2048</span>** is recommended. 4096 is only recommended for PC-only releases of Worlds. </li><li>**<span style="color: #dc9656">Padding</span>**: Sets the spacing of the sprites that are packed into the Atlas. The recommended value is <span style="color: #dc9656">**4**</span>. </li><li><span style="color: #dc9656">**Filter Mode**</span>: Sets the filter mode of the Atlas. <ul><li>Point: Keeps colors vivid. When the sprite is enlarged, the color boundaries are clearly visible.<br>![point](https://mod-file.dn.nexoncdn.co.kr/bbs/1681376094059d24365daf6f3448a828a3c0570a4ca17.png%7B%22width%22:%22120px%22%7D)</li><li>Bilinear: Blends a specific color with surrounding colors to create a softer expression. When a sprite is enlarged, the color boundaries may appear more blurry than the original sprite.<br>![bilinear](https://mod-file.dn.nexoncdn.co.kr/bbs/1681376194233746640e900964e7288a13327aae2cbc9.png%7B%22width%22:%22120px%22%7D)</li><li>Trilinear: Blends a specific color with surrounding colors to create a softer expression. When a sprite is enlarged, the color boundaries may appear more blurry than the original sprite. </li></ul></li></ul> |
| ![8](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_08.jpg) | **<span style="color: #dc9656">Sprite RUIDs to Pack</span>**: A list of sprites that will be made into an Atlas. Only one RUID can be included per Atlas. Different icons appear depending on packing success/failure and abnormalities.<ul><li>![check](https://mod-file.dn.nexoncdn.co.kr/storage/icons/Atlas_Blueprint_Editor/icon_atlasblueprint_check.png): Can pack sprite</li><li>![warning](https://mod-file.dn.nexoncdn.co.kr/storage/icons/Atlas_Blueprint_Editor/icon_atlasblueprint_warning.png): There is something wrong with your sprite. Packing is possible with Atlas. Mostly when a My Resources sprite is replaced in Resource Storage, or an image of the same name is uploaded and replaced in Workspace.</li><li>![error](https://mod-file.dn.nexoncdn.co.kr/storage/icons/Atlas_Blueprint_Editor/icon_atlasblueprint_error.png): Unable to pack sprite. </li></ul> |
| ![9](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_09.jpg "9") | Pack the selected sprites into an Atlas. The button is not enabled if the list contains sprites that failed to pack. |

# Packing AtlasBlueprint 

The following describes how to pack multiple sprites into an Atlas. Sprites bound by an Atlas do not require any additional processing.
A sprite can only be applied to one Atlas. Therefore, even if you add a sprite to the Sprite RUIDs to Pack list of AtlasBlueprint to duplicate a packed sprite into another Atlas, it will not be packed into the Atlas and an error icon will appear in the corresponding sprite box. Also, you cannot pack RUIDs shared by other creators into your Atlas.
When grouping sprites into an Atlas, it's a good idea for creators to establish criteria and appropriately separate their intended uses. For example, grouping sprites that appear on the same screen or grouping sprites that overlap on the screen and packing them with an Atlas can help improve World performance. 

1. In the context menu of **Workspace - MyDesk, click Create AtlasBlueprint** to make a new **MyAtlasBlueprint**.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/168137208490909091639575b490084b0ced4b4033f9b.png)
    <br>
2. Select **Panels - AtlasBlueprint Editor**.
    <br>
3. Set **Current Blueprint** as MyAtlasBlueprint in AtlasBlueprint Editor.
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/168870792380611e986e3ce5547a29903b780da9c1a8d.png)
    <br>
4. Select the sprites you want to pack and press the **[Add to Blueprint]** button.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/16887064641292283efce8931413da235c6e49165b619.png{"width":"540px"} "8")

    > <span style="color: #7cafc2">**Tip**</span>
    > <span style="color: #7cafc2">When packing animation clips, you can preview the animation sprites by pressing the [>] button.</span>
    > ![4-1](https://mod-file.dn.nexoncdn.co.kr/bbs/172914774378304b3d4cf873e42d980aa2ece4aa41e02.png "4-1")

   <br>
5. Adjust max size, padding, and filter mode values in **Atlas Settings** as you wish. Press **[Pack]** to pack the sprites into the Atlas.
    ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/16887055695712cf0218ae48c457aa3a1b534549950e7{"width":"340px"}.png)
    <br>
6. You can check the packed Atlas information by pressing **Yes** in the Success Notification window. If you press **No**, the Details Info window does not open. Press the **[Open]** button to open the Details Info window.
    ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/16887069593473bbccf1d33a14c4faddf324c9dd9d884.png)
    <br>
    You can also check the packed AtlasBlueprint previews in the **Property** window.
    ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1689126815360b6a57da2448b4507b4daf444e26f3e16.png "9")

> <span style="color: #7cafc2">**Tip**</span>
> You can check the <span style="color: #7cafc2">**Sprite RUIDs to Pack** list for packing success/failure and sprite anomalies.</span>
> ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1688706020438d8d6eac716f24536a86c2efbfcc1c10f.png)