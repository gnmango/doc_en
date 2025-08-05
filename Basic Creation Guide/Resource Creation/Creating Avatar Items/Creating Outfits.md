# Course Introduction
Let's learn how to make jumpsuits. A jumpsuit is structured similarly to a coat, and it allows for all clothing items with connected coats and bottoms.
First, please refer to the [Basics of Creating Avatar Items](docs/?postId=588{"target":"_self"}) to understand the basic concept of avatars.

# Preparation
Adobe Photoshop is required to produce items. Install the software and download it to open the file below.

$$attachFile
{"name":"Avatar_Longcoat.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version5_230602/Avatar_Longcoat.psd"}
$$

> <span style="color: #7cafc2">**Tip**
> We recommend making items within 120 x 120 pixels based on the avatar's feet.</span>
> ![Examples](https://mod-file.dn.nexoncdn.co.kr/bbs/1677032255514a026c711634c4196ab0b96bc5424e7f9.png "Examples")
 
# Production Sheet Description
The production sheet is divided into gray and yellow layers. 
In the three gray layers, there are data folders for image datafication, avatar body layer for image creation, and line layer for guidance. All gray layers are locked, and no modifications are required while working.
The creator can draw the actual item in the yellow layer.

> <span style="color: #7cafc2">**Tip**
> If you edit or delete the names of layers, you will not be able to register the item.
> **Never change the layer names.**</span>

#### Gray Layer
When you open the sheet in Photoshop, the gray layer is locked as denoted by the padlock symbol. The layer cannot be edited because its contents are used in the item registration process.

1. <span style="color: #dc9656">**data folder**</span>
You need the data folder for item datafication. It does not affect image production.
    * <span style="color: #dc9656">**data:origin**</span>: Indicates the center of the data with a red dot.
    * <span style="color: #dc9656">**data:type:Longcoat**</span>: Indicates data type.
    * <span style="color: #dc9656">**data:vslot:MaPn**</span>: The concept of vslots is used only in caps.
    * <span style="color: #dc9656">**data:use_zmap_preset1**</span>: The images that make up an avatar are edited by stacking layers upon layers. To create various body movements for the avatar, the images are stacked layer by layer, starting with the body followed by clothes, shields, and weapons.
zmap file is a compilation of separate sprites arranged in sequential order by height depending on the item.
2. <span style="color: #dc9656">**guide_character_layer**</span>: This is an avatar's sample body. As the internal rule, the one with the bigger number attached after the layer name is placed below.
3. <span style="color: #dc9656">**guide_grid layer **</span>: This layer represents the workspace area. The creator has to work within the green line.
4. <span style="color: #dc9656">**guide_background layer**</span>: It has two options for background colors. gray_blue and gray are used to differentiate between different actions or operations. In the operation sheet with a gray_blue background, the arm is drawn in an exceptional position.
![0](https://mod-file.dn.nexoncdn.co.kr/bbs/168560569152022a0080afa5945e6a338edc40ecf2495.png "0")
#### Yellow Layer
Creators can draw the outfit on the yellow layer.

* guide_character_backBody:_113: The backside and arms of the avatar
    * <span style="color: #dc9656">**edithere:mail_backMailChest_103**</span>: Clothes drawn on the backside and arms of the avatar
* guide_character_summary_78,100: Avatar body, arms, backside, and head
    * <span style="color: #dc9656">**edithere:mail_mailChest_68**</span>: Clothes drawn on the top of the avatar body and arms
* guide_character_summary_60,62: Avatar arms
    * <span style="color: #dc9656">**edithere:mailArm_mailArmBelowHead_58,61**</span>: Clothes drawn on the top of the avatar arms
* guide_character_summary_52,53: Avatar arms
    * <span style="color: #dc9656">**edithere:mailArm_mailArm_50**</span>: Clothes drawn on the top of the avatar arms
* guide_character_summary_26,47: Avatar head and arms
    * <span style="color: #dc9656">**edithere:mailArm_mailArmOverHairBelowWeapon_2**5</span>: Clothes drawn on the top of the avatar head and arms
* guide_character_armOverHair_24: Avatar's arms
    * <span style="color: #dc9656">**edithere:mailArm_mailArmOverHair_22**</span>: Clothes drawn on the top of the avatar arms
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16856057153095341c6103f9b42a4992a1feee99bbe88.png "1")
# Creation
Let's create an adorable sleeveless outfit by starting with a spacesuit and removing the patterns.

| Basic Outfit | Sleeveless Outfit |
| --- | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/168560341854841c7039d6fff4e2bb8c4656c77001cff.png "1") | ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/168560345801161262e10c2b64268b4637b52cc34aac6.png "3") |

#### Creating a Sleeveless Design
1. Remove the sleeves from layer 3 to layer 5 of the yellow layer. 
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16856045068945b529e99248f4295afc9dbe1a8249689.png "3")
    
    > <span style="color: #585858">**Learn more**
    > You can delete all the base images on the selected layer by pressing **ctrl + a** to select everything and pressing **Delete** key to delete the selected content.</span>

2. Select yellow layer 1 and 2 for image editing.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16856049808171f93176089ab4fa68328b6cc295eb013.png "4")
3. Use the Pencil Tool to erase the pattern on layer 1 and refine the neck and arm areas by drawing.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1685604996639af0f8d46c1d54e80a4fe8937830f890c.png "5")
4. Select the Pencil Tool on layer 2 and erase the patterns on each operation, and refine the neck and arm areas by drawing.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/16856050073062819ed930f434646888db96f17e2c75d.png{"width":"1017px"} "6")

> <span style="color: #7cafc2">**Tip**
> When modifying the sleeves, there are specific exceptions to the default rules. For details, please refer to [Creating Coats](docs/?postId=586{"target":"_self"}).
> Please note the exceptions when drawing **Swing O3, Swing OF, Stab TF, Swim-Fly, Sit** in the sheet.
> Draw the sleeves in layer 4, although the arms of the **Swing O3, Swing OF** are in layer 3.
> Draw the sleeves in layer 6, although the arms of the **Stab TF, Swim-Fly, Sit** are in layer 4.</span>

> <span style="color: #7cafc2">**Tip**
> When drawing the sleeves, it may be easier to reference the default sleeves provided with the spacesuit.</span>