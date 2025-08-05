# Course Introduction
Here is the production sheet for face-covering masks. 
First, please refer to the [Basics of Creating Avatar Items] to understand the basic concept of avatars.

# Preparation
Adobe Photoshop is required to produce items. Install the software and then download and open the file below.
$$attachFile
{"name":"Avatar_Cap_E.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version4_230221/Avatar_Cap_E.psd"}
$$

><span style="color: #7CAFC2"> **Tip**
> We recommend making caps within 120 x 120 pixels based on the avatar's feet.</span>
> ![example](https://mod-file.dn.nexoncdn.co.kr/bbs/16769485559327fc72cf03aca4128998b423540c3e049.png{"width":"150px"} "example")

# Production Sheet Description
The cap sheet is divided into gray, red, and yellow layers.
The gray layer has a data folder for image datafication, an avatar body layer to help with the image creation, and a line layer for guidance. All gray layers are locked, and no modifications are required while working.
The creator can draw the actual items in the yellow layer.
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
3. <span style="color: #dc9656">**(Red layer) data:vslot:CpH1H2H3H4H5H6HfHsHbHcAfAyAsAe**</span>: The red layer is a concept only for the hat. The vslot layer in a hat file plays a role in determining the priority to show when the hair and hat overlap. 
![TypeE](https://mod-file.dn.nexoncdn.co.kr/bbs/167703389960214c1a08396d74ffeba19e07a5787b1eb.png "TypeE")
### Yellow Layer
The creator can draw a face-covering mask that the creator wants to produce in the yellow layer.
1. <span style="color: #dc9656">**case1.front**</span>: This contains a layer to draw the front view of the face-covering mask and front-view samples of normal hair and pressed hair.
    * <span style="color: #dc9656">**edithere:cap_cap_34**</span>: This layer lets you draw the front of the face-covering mask.
2. <span style="color: #dc9656">**case2.back**</span>: This contains a layer to draw the back view of the face-covering mask and back-view samples of normal hair and pressed hair.
    * <span style="color: #dc9656">**edithere:cap_backCap_92**</span>: This layer lets you draw the back view of the face-covering mask.
![[TypeE1]](https://mod-file.dn.nexoncdn.co.kr/bbs/1724918527383e3a3141a069e42d7a212bfaec518a4be.png "TypeE1")