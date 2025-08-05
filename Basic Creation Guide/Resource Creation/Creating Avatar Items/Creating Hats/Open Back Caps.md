# Course Introduction
Let's learn how to create open-back caps. You can change the default color of the hat and add accessories.
We recommend drawing hats after you get used to creating pants or shoes, because the hat structure is complicated.
First, please refer to the [Basics of Creating Avatar Items](/docs?postId=588%7B%22target%22:%22_self%22%7D) to understand the basic concept of avatars.

# Preparation
Adobe Photoshop is required to produce items. Install the software and then download and open the file below.

$$attachFile
{"name":"Avatar_Cap_A1.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version4_230221/Avatar_Cap_A1.psd"}
$$
$$attachFile
{"name":"Avatar_Cap_A2.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version5_230602/Avatar_Cap_A2.psd"}
$$
$$attachFile
{"name":"Avatar_Cap_Ani.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version4_230221/Avatar_Cap_Ani.psd"}
$$

><span style="color: #7cafc2"> **Tip**
> We recommend making hats within 120 x 120 pixels based on the avatar's feet.</span>
> ![example](https://mod-file.dn.nexoncdn.co.kr/bbs/16769485559327fc72cf03aca4128998b423540c3e049.png{"width":"150px"} "example")

# Production Sheet Description
The hat production sheet is divided into gray, yellow, and red layers.
The gray layer has a data folder for image datafication, an avatar body layer to help with the image creation, and a line layer for guidance. All gray layers are locked, and no modifications are required while working.
The creator can draw the actual items in the yellow layer.
The red layer determines the priority between the hat and the hair.

> <span style="color: #7cafc2">**Tip**
> If you edit or delete the names of layers, you will not be able to register the item.
> **Never change the layer names.**</span>

### Gray Layer/Red Layer
1. <span style="color: #dc9656">**data folder**</span>: A data folder is required for the datafication of items. It does not affect image production.
    * <span style="color: #dc9656">**data:origin**</span>: This marks the center of the data with a red dot.
    * <span style="color: #dc9656">**data:type:Cap**</span>: This indicates the data type.
    * <span style="color: #dc9656">**data:use_zmap_preset1**</span>: The images that form an avatar are produced by being piled in layers. The structure is to pile the images of clothes, shields, and weapons in layers in a sequence based on the avatar's body to produce the avatar's various movements.<br>Zmap is a file wherein the heights of each sprite, separated by items, are organized in order.
2. <span style="color: #dc9656">**guide_layer**</span>: This is an avatar's sample body. The internal rule is that the bigger the number attached after the layer name, it places below.<br>
    * guide_backHairBelowCapWide_samples_94 and guide_backHairBelowCapNarrow_samples_95 are the samples for the wide and narrow hairs depending on the hat shape.<br> ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16457811937535becc03a37664135889302106c115bff.png "5")
3. <span style="color: #dc9656">**(Red layer) data:vslot:CpH1H5**</span>: The red layer is a concept only for hats. The vslot layer in a hat file plays a role in determining whether hair or hat takes priority when the two overlap.

![layer](https://mod-file.dn.nexoncdn.co.kr/bbs/1677032356769c4bb8003ae4447ab8b8f3f11c31e2428.png "layer")

### Yellow Layer
The creator draws the actual open-back hat on the yellow layer.
#### A hat with only one layer in the front of the hair of the front view (Avatar_Cap_A1)
* <span style="color: #dc9656">**case1.front**</span>: This contains a layer to draw the front of the hat and front-view samples of normal hair and pressed hair.
    * <span style="color: #dc9656">**edithere:cap_cap_34**</span>: This layer lets you draw the front of the hat.
* <span style="color: #dc9656">**case2.back**</span>: This contains a layer to draw the back of the hat and back-view samples of normal hair and pressed hair.
    * <span style="color: #dc9656">**edithere:cap_backCap_92**</span>: This layer lets you draw the back of the hat.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16770399860801689e03a56a847c6b3082835b716c0f9.png "3")

#### A hat with two layers in the front and back of the hairs on the front view (Avatar_Cap_A2)
* <span style="color: #dc9656">**case1.front**</span>: This contains a layer to draw the front of the hat and the samples of normal hairs and pressed hairs on the front view.
    * <span style="color: #dc9656">**edithere:cap_cap_34**</span>: This layer lets you draw the front of the hat.
    * <span style="color: #dc9656">**edithere:cap_frontBackCap_92**</span>: This layer is used to draw the front view of the hat behind the hair.
* <span style="color: #dc9656">**case2.back**</span>: This contains a layer to draw the back of the hat and the samples of normal hairs and pressed hairs on the back view.
    * <span style="color: #dc9656">**edithere:cap_backCap_92**</span>: This layer lets you draw the back of the hat.

![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16856818406522ccc7186c35648dd8beb320cfcc7a07b.png "4")

> <span style="color: #585858">**Learn more**
> Use the 001 samples wherein the Wide and Narrow images are the same from the hair sample of guide_backHairBelowCapWide_samples_94 and guide_backHairBelowCapNarrow_samples_95 when working on the open-back cap (Avatar_Cap_A).</span>


#### A File with Effects (Avatar_Cap_Ani.psd)
Avatar sheets have an animation rule of 4 frames for the front side and 2 frames for the back side. Apply the frame as a hat effect.
You need to draw the hat and effect each to be played in order of the animation layers.
* <span style="color: #dc9656">**case1.front**</span> : The animation folder includes the layers to draw the hat's front view and effects and the avatar sample.
    * <span style="color: #dc9656">**animation folder**</span>: Includes four layers to draw the hat's front view and effects. You need to draw the same hat in all layers.
* <span style="color: #dc9656">**case2.back**</span>: The animation folder includes the layers to draw the hat's back side and effects and the avatar sample.
    * <span style="color: #dc9656">**animation folder**</span>: Includes two layers to draw the hat's front view and effects. You need to draw the same hat on all layers.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/16770332160558c40d4d45b4349d29f54ace53eec586e.png "6")

><span style="color: #585858">**Learn more**
> Only the stand, walk, fly, rope, and ladder actions will play the effect. In other actions, only the first frame is visible.</span>