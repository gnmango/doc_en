# Course Introduction 
Let's learn how to make gloves. You can change the color of gloves and add images.
Gloves have a complex structure, so we recommend practicing with pants or shoes before trying gloves.
First, please refer to [Basics of Creating Avatar Items](/docs?postId=588{"target":"_self"}) to understand the basic concept of avatars.

# Preparation
Adobe Photoshop is required to create items. After installing the software, download and open the file below.
$$attachFile
{"name":"Avatar_Gloves.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version5_230602/Avatar_Gloves.psd"}
$$

><span style="color: #7CAFC2"> **Tip**
> We recommend making items within 120 x 120 pixels based on the avatar's feet.</span>
> ![Examples](https://mod-file.dn.nexoncdn.co.kr/bbs/1677032255514a026c711634c4196ab0b96bc5424e7f9.png "Examples")

# Production Sheet Description
The crafting sheet is divided into gray and yellow layers. 
In the three gray layers, there are data folders for image datafication, avatar body layer for image creation, and line layer for guidance. All gray layers are locked, and no modifications are required while working.
The creator draws the actual item on the yellow layer.

> <span style="color: #7cafc2">**Tip**
> If you edit or delete the names of layers, you will not be able to register the item.
> **Never change the layer names.**</span>
### Gray Layer
![glove00](https://mod-file.dn.nexoncdn.co.kr/bbs/16460333014592202c8cc50e94904959448d94d68d14d.png "glove00")

1.  <span style="color: #dc9656">**data folder**</span>
You need the data folder for item datafication. It does not affect image creation. 
    * <span style="color: #dc9656">**data:origin**</span>: Marks the center of the data with a red dot.
    * <span style="color: #dc9656">**data:type:Glove**</span>: Indicates data type.
    * <span style="color: #dc9656">**data:vslot:GlGw**</span>: Only hats use the concept of vslot.
    * <span style="color: #dc9656">**data:use_zmap_preset1**</span>: Image layers pile up to create an avatar. An avatar's structure involves stacking images of clothes, shields, and weapons over each other to create various body movements.<br>The height of each sprite by item is organized in the file zmap.
2. <span style="color: #dc9656">**guide_character_summary layer**</span>: Avatar's sample body. Layer with a bigger number is placed below.
3. <span style="color: #dc9656">**guide_grid**</span>: Craft items using grid lines. You have to work within the green line.
4. <span style="color: #dc9656">**guide_background**</span> : For background colors, there are gray_blue and gray. Hands in gray_blue will not be shown because of the face, so you don't need to draw gloves on them.<br> ![glove](https://mod-file.dn.nexoncdn.co.kr/bbs/164559428580573f3ac0478a74420915915fc5e57ab5f.png "02") ![glove06](https://mod-file.dn.nexoncdn.co.kr/bbs/164559429595558ea67d1a9b54db09324b060805a18fe.png "coat06")
### Yellow Layer
The creator can draw the gloves on the yellow layer.
#### Left and Right Hands
Left and right hands look different on avatars, and you'll be drawing the item separately as well.
From the screen, <span style="color: #dc9656">**lGlove**</span> is the left hand, and <span style="color: #dc9656">**rGlove**</span> is the right hand.
![coat09](https://mod-file.dn.nexoncdn.co.kr/bbs/16455945165795c6af89bbd624ecd8f515e2236212434.png "coat09")
![coat08](https://mod-file.dn.nexoncdn.co.kr/bbs/1645594498083853f96a383c14814b91cbc2e90c29d90.png "coat08")
#### Working Standards
1. <span style="color: #dc9656">**guide_character_Body_78**</span>: From the avatar sample, draw gloves in layers 1, 2, 3, 4, 5, 6. Refer to the location of the gloves sample and draw according to the avatar's hands.<br>![glove10](https://mod-file.dn.nexoncdn.co.kr/bbs/16455950125317caa13191faa4327be3e5c715ae2f8fa.png "glove10")
2. <span style="color: #dc9656">**guide_character_summary_52,53,62**</span>: From the avatar sample, draw gloves in layers 3, 4, 5, 6. Refer to the location of the gloves sample and draw according to the avatar's hands.<br>![glove11](https://mod-file.dn.nexoncdn.co.kr/bbs/1645595078631cca5a165684046189a9a240bcd976956.png "glove11")
3. <span style="color: #dc9656">**guide_character_summary_24,47**</span>: From the avatar sample, draw gloves in layers 5, 6. Refer to the location of the gloves sample and draw according to the avatar's hands.<br>![glove12](https://mod-file.dn.nexoncdn.co.kr/bbs/1645595114548b9b59cd3a86346f0a629d8cb2842a41f.png "glove12")

# Creation
#### Drawing Patterns
In the yellow layer, use <span style="color: #dc9656">**Pencil Tool**</span> to draw small hearts in all <span style="color: #dc9656">**lGlove, rGlove layers**</span>.

![glove13](https://mod-file.dn.nexoncdn.co.kr/bbs/1645595325199dbbd9655c991466abd72cfe5e814921a.png "glove13")![glove13](https://mod-file.dn.nexoncdn.co.kr/bbs/1645595338100bf3e9fc39a5e4f58b382ad8976540d1d.png "glove13")

> <span style="color: #7cafc2">**Tip**
> Draw bracelets or watches, usually worn on one arm, only in 'rGlove'.</span>

#### Changing the Color
1. Click the yellow layer.
2. Select <span style="color: #dc9656">**image - Adjustments - Hue/Saturation**</span> or use the shortcut <span style="color: #dc9656">**Ctrl+U**</span> to open Hue/Saturation window.
3. Set value for Lightness to <span style="color: #dc9656">**-40**</span> to change the color to gray.<br>![glove14](https://mod-file.dn.nexoncdn.co.kr/bbs/1645596666352e7cc00268c8a40eca6dbddc476691c10.png "glove16")
4.  Select <span style="color: #dc9656">**Image - Adjustments - Color Balance**</span> or use the shortcut <span style="color: #dc9656">**Ctrl+B**</span>. <br> Adjust the Color Balance to <span style="color: #dc9656">**Blue**</span>.<br>![glove15](https://mod-file.dn.nexoncdn.co.kr/bbs/1645596879737ee76f2425107404a9507095929b396d9.png "glove15")
5. Select <span style="color: #dc9656">**Image - Adjustments - Levels**</span> or use the shortcut <span style="color: #dc9656">**Ctrl+L**</span>.
6. Change the values of both ends to <span style="color: #dc9656">**53, 189**</span> to change the color to purple.<br>![glove16](https://mod-file.dn.nexoncdn.co.kr/bbs/16455975905835fb5e72dbb074a25b4179641537a66e1.png "glove16")

##### Reference Guide
* [Creating Pants](/docs?postId=584{"target":"_self"})
* [Creating Capes](/docs?postId=585{"target":"_self"})
* [Creating Coats](/docs?postId=586{"target":"_self"})
* [Creating Shoes](/docs?postId=583{"target":"_self"})