# Course Introduction
Here is the production sheet for closed-back caps. 
First, please refer to the [Basics of Creating Avatar Items] to understand the basic concept of avatars.

# Preparation
Adobe Photoshop is required to produce items. Install the software and then download and open the file below.
$$attachFile
{"name":"Avatar_Cap_B.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version4_230221/Avatar_Cap_B.psd"}
$$

><span style="color: #7cafc2"> **Tip**
> We recommend making caps within 120 x 120 pixels based on the avatar's feet.</span>
> ![example](https://mod-file.dn.nexoncdn.co.kr/bbs/16769485559327fc72cf03aca4128998b423540c3e049.png{"width":"150px"} "example")

# Production Sheet Description
The cap sheet is divided into gray, yellow, and red layers.
The gray layer has a data folder for image datafication, an avatar body layer to help with the image creation, and a line layer for guidance. All gray layers are locked, and no modifications are required while working.
The creator draws the actual item on the yellow layer.
The red layer determines the priority between the cap and the hair.

> <span style="color: #7cafc2">**Tip**
> If you edit or delete the names of layers, you will not be able to register the item.
> **Never change the layer names.**</span>

### Gray Layer/Red Layer
1. <span style="color: #dc9656">**data folder**</span>: A data folder is required for the datafication of items. It does not affect image production.
    * <span style="color: #dc9656">**data:origin**</span>: This marks the center of the data with a red dot.
    * <span style="color: #dc9656">**data:type:Cap**</span>: This indicates the data type.
    * <span style="color: #dc9656">**data:use_zmap_preset1**</span>: The images that form an avatar are produced by being piled in layers. The structure is to pile the images of clothes, shields, and weapons in layers in a sequence based on the avatar's body to produce the avatar's various movements.<br>Zmap is a file wherein the heights of each sprite, separated by items, are organized in order.
2. <span style="color: #dc9656">**guide_layer**</span>: This is an avatar's sample body. The internal rule is that the bigger the number attached after the layer name, it places below.<br>
    * guide_backHairBelowCapWide_samples_94 and guide_backHairBelowCapNarrow_samples_95 are the samples for the wide and narrow hairs depending on the cap shape.<br> ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16457811937535becc03a37664135889302106c115bff.png "5")
3. <span style="color: #dc9656">**(Red layer) data:vslot:CpH1H4**</span>: The red layer is a concept only for the hats. The vslot layer in a hat file plays a role in determining whether hair or hat takes priority when the two overlap. 
![TypeB](https://mod-file.dn.nexoncdn.co.kr/bbs/1677032543121310bc61208a34d33b830aba8862f51c4.png "TypeB")
### Yellow Layer
The creator draws the actual item on the yellow layer.

1. <span style="color: #dc9656">**case1.front**</span>: This contains a layer to draw the front of the hat and front-view samples of normal hair and pressed hair.
    * <span style="color: #dc9656">**edithere:cap_cap_34**</span>: This layer lets you draw the front of the hat.
2. <span style="color: #dc9656">**case2.back**</span>: This contains a layer to draw the back of the cap and back-view samples of normal hair and pressed hair.
    * <span style="color: #dc9656">**edithere:cap_backCap_92**</span>: This layer lets you draw the back of the hat.

![TypeB01](https://mod-file.dn.nexoncdn.co.kr/bbs/1677033338202aa1833624a264a098601f346edc6fc3f.png "TypeB01")
> <span style="color: #585858">**Learn more**
> Use the 002 samples wherein the Wide and Narrow images are different from the hair sample of guide_backHairBelowCapWide_samples_94 and guide_backHairBelowCapNarrow_samples_95 when you work on a closed-back hat (Avatar_Cap_B).</span>

