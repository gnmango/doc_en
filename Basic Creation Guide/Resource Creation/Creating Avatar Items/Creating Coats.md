# Course Introduction
Let's learn how to make coats. You can change the color of the coat and add a drawing.
Coats have a complex structure, so we recommend that you work on them after getting used to making pants or shoes.
First, please refer to the [Basics of Creating Avatar Items](/docs?postId=588{"target":"_self"}) to understand the basic concept of avatars.

# Preparation
Adobe Photoshop is required to produce items. Install the software and then download and open the file below.

$$attachFile
{"name":"Avatar_Coat.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version5_230602/Avatar_Coat.psd"}
$$

><span style="color: #7CAFC2"> **Tip**
> > We recommend producing items within 120x120 based on the avatar's feet.</span>
> ![Examples](https://mod-file.dn.nexoncdn.co.kr/bbs/1677032255514a026c711634c4196ab0b96bc5424e7f9.png "Examples")

# Describe Production Sheet
The crafting sheet is divided into gray and yellow layers. 
The four gray layers consist of data folders for image datafication, avatar body layer for image production, line layer for guidance, and background layer. All gray layers are locked, and no modifications are required while working.
The creator can draw the actual items in the yellow layer.

> <span style="color: #7cafc2">**Tip**
> You cannot produce items if you edit or delete all layers' names.
> **Never change the name.**</span>
#### Gray Layer
When you open the sheet in Photoshop, the gray layer will be locked. The layer is uneditable because it contains content related to item registration.
1.  <span style="color: #dc9656">**Data Folder**</span>
You need the data folder for item datafication. It does not affect image production. 
    * <span style="color: #dc9656">**data:origin**</span>: Indicates the center of the data with a red dot.
    * <span style="color: #dc9656">**data:type:Coat**</span>: Indicates data type.
    * <span style="color: #dc9656">**data:vslot:Ma**</span>: The concept of vslots is used only in caps.
    * <span style="color: #dc9656">**data:use_zmap_preset1**</span>: The images that make up an avatar are edited by stacking layers upon layers. The structure is to pile the images of clothes, shields, and weapons in layers in a sequence based on the avatar's body to produce the avatar's various movements.<br>The height of each Sprite by item is organized in the file zmap.
2. <span style="color: #dc9656">**guide_character_layer**</span>: This is an avatar's sample body. The internal rule is that larger numbers attached to the end of the layer name are placed lower.
3. <span style="color: #dc9656">**guide_grid**</span>: Crafts item based on grid line. You have to work within the green line.
4. <span style="color: #dc9656">**guide_background**</span>: The background colors are gray_blue and gray. The arm located in gray_blue is marked, since its image is displayed in unusual places.

![coat01](https://mod-file.dn.nexoncdn.co.kr/bbs/165061743355448155ebe84c9405391d1de99cb7ca495.png "coat01")
#### Yellow Layer
The creator can draw a coat in the yellow layer.

1. guide_character_backBody:_113: Avatar Backside and Arms
    * <span style="color: #dc9656">**edithere:mail_backMailChest_103**</span>: Clothes drawn on the backside and arms of the avatar
2. guide_character_summary_78,100: Avatar Body, Arms, Backside, and Head
    * <span style="color: #dc9656">**edithere:mail_mailChest_68**</span>: Clothes drawn on the top of the avatar body and arms
  
3. guide_character_summary_60,62: Avatar Arms 
    * <span style="color: #dc9656">**edithere:mailArm_mailArmBelowHead_58,61**</span>: Clothes drawn on the top of the avatar arms
    
4. guide_character_summary_52,53: Avatar Arms
    * <span style="color: #dc9656">**edithere:mailArm_mailArm_50**</span>: Clothes drawn on the top of the avatar arms
    
5. guide_character_summary_26,47: Avatar Head and Arms
    * <span style="color: #dc9656">**edithere:mailArm_mailArmOverHairBelowWeapon_25**</span>: Clothes drawn on the top of the avatar head and arms
    
6. guide_character_armOverHair_24 : Avatar Arms
    * <span style="color: #dc9656">**edithere:mailArm_mailArmOverHair_22**</span>: Clothes drawn on the top of the avatar arms

![coat02](https://mod-file.dn.nexoncdn.co.kr/bbs/1645582927632f0e7790d36844043a8bbb992b2d24154.png "coat02")

# Create
Let's use the basic coat to make coats with patterns, another color, and long sleeves.

| Basic Coat | Draw Patterns | Change Colors | Long-Sleeve Coat |
| --- | --- | --- | --- |
| ![01](https://mod-file.dn.nexoncdn.co.kr/bbs/1645684463571e684de7e0cba4bdf8d78e2022a34c1a1.png "01") | ![02](https://mod-file.dn.nexoncdn.co.kr/bbs/16456845084177670d36c4b664d549d420a02542734dd.png "02") | ![03](https://mod-file.dn.nexoncdn.co.kr/bbs/16503348006950e9be3c69e664a34966cbc1fbdf57b0f.png "03")  |  ![04](https://mod-file.dn.nexoncdn.co.kr/bbs/1650334752955e04ab9761b2a43f8b724def5b499f658.png "04") |
#### Draw Patterns
1. Work on the image on yellow layer 2 (edithere:mail_mailChest_68). <br> ![coat03](https://mod-file.dn.nexoncdn.co.kr/bbs/16455830786944859a717c0be483bb5f59faa8651c14f.png "coat03")
2. Draw a small heart for all actions.<br>Leave the other layers untouched, as the heart will be drawn only on the body.<br>![coat04](https://mod-file.dn.nexoncdn.co.kr/bbs/1645583092780f3345eebaa4a4be5a9c23f91b74096cd.png "coat04")

#### Change Color
1. Click the yellow layer.
3. Select <span style="color: #dc9656">**Image - Adjustments - Hue/Saturation**</span> or use the shortcut <span style="color: #dc9656">**Ctrl+U**</span> to open the Hue/Saturation window.
4. Adjust values for <span style="color: #dc9656">**Hue**</span>. Change the color of the coat on all yellow layers, press the OK button, and save the file.<br>![coat16](https://mod-file.dn.nexoncdn.co.kr/bbs/1645591630688cb5180ce8d77408787f2decf3776e412.png "coat16")

#### Creating a Long-Sleeved Coat
1. Work on the images on yellow layers 1 through 6. <br>![coat16](https://mod-file.dn.nexoncdn.co.kr/bbs/165033411781314df1c56f85942fe8483569d7af5e76d.png "coat16")
2. Use the pencil tool to draw long-sleeves customized to the body.<br>![coat17](https://mod-file.dn.nexoncdn.co.kr/bbs/1650334169976ee38f585747246f596f2fb80b3e19d42.png "coat17")![coat18](https://mod-file.dn.nexoncdn.co.kr/bbs/1650334184379e86690f2a3f547eaa6bf6a44a445ae97.png "coat18")<br>![coat19](https://mod-file.dn.nexoncdn.co.kr/bbs/16503342030177f5460daeddc46f2971251a579dc4698.png "coat19")![coat20](https://mod-file.dn.nexoncdn.co.kr/bbs/16503342772725975591ca82d42a289930aff1428fc53.png "coat20")<br>![coat21](https://mod-file.dn.nexoncdn.co.kr/bbs/1650334295006e903a60bc3314a47b4c65c47c9a5aacf.png "coat21")![coat22](https://mod-file.dn.nexoncdn.co.kr/bbs/1650334309071f637ad711b474bf19c38c1aeacbfa18d.png "coat22")
<br>
3. When drawing the sleeves of a Coat, you should keep the basic rules and other exception sheets in mind.<br>During operating sheet, when drawing <span style="color: #dc9656">**Swing O3, Swing OF, Stab TF, Swim-Fly, Sit**</span>, you should follow some exceptional rules.<br><span style="color: #dc9656">**Swing O3, Swing OF**</span> is the guide based on the layer, the body (arm) is in position 3, but the sleeve is drawn on layer 4.<br><span style="color: #dc9656">**Stab TF, Swim-Fly, Sit**</span> is the guide based on the layer, the body (arm) is at the 4th position, but the sleeve is drawn on the 6th layer.
![coat22](https://mod-file.dn.nexoncdn.co.kr/bbs/16503343643288cb65f8b3cd847089f0e4a494bbed99c.png "coat22")<br>
 If you don't know the number of layers, you can draw sleeves on <span style="color: #dc9656">**the layer that has the basic white T-shirt**</span>.
![coat23](https://mod-file.dn.nexoncdn.co.kr/bbs/16503344581568f0d7baaca4d4c5ea17ddc395dad1716.png "coat23")![coat24](https://mod-file.dn.nexoncdn.co.kr/bbs/16503344730397780d272d77c48ba929bca725111a81b.png "coat24")<br>
![coat24](https://mod-file.dn.nexoncdn.co.kr/bbs/16503344933842ba8ee3242524637907486057d58ee84.png "coat24")![coat25](https://mod-file.dn.nexoncdn.co.kr/bbs/16503345071810a077a814e404a4dba975667e0261846.png "coat25")<br>
![coat26](https://mod-file.dn.nexoncdn.co.kr/bbs/16503345264358fbc43530b6e4822b41cfe99165d8f48.png "coat26")

##### Reference Guide
* [Creating Pants](/docs?postId=584{"target":"_self"})
* [Creating Capes](/docs?postId=585{"target":"_self"})
* [Creating Gloves](/docs?postId=587{"target":"_self"})
* [Creating Shoes](/docs?postId=583{"target":"_self"})