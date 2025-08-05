# Course Introduction 
Let's learn how to make capes. You can change the color of the cape and add an image to it.
You can even make balloons instead of capes.
First, please refer to [Basics of Creating Avatar Items] to understand the basic concept of avatars.
# Preparation
Adobe Photoshop is required to create items. After installing the software, download and open the file below.
$$attachFile
{"name":"Avatar_Cape.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version2/Avatar_Cape.psd"}
$$
$$attachFile
{"name":"Avatar_Cape_balloon.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version2/Avatar_Cape_balloon.psd"}
$$

><span style="color: #7CAFC2"> **Tip**
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
1.  <span style="color: #dc9656">**data folder**</span>
You need the data folder for item datafication. It does not affect image creation. 
    * <span style="color: #dc9656">**data:origin**</span>: Marks the center of the data with a red dot.
    * <span style="color: #dc9656">**data:type:Cape**</span>: Indicates data type.
    * <span style="color: #dc9656">**data:vslot:Sr**</span>: Only hats use the concept of vslot.
    * <span style="color: #dc9656">**data:use_zmap_preset1**</span>: Image layers pile up to create an avatar. An avatar's structure involves stacking images of clothes, shields, and weapons over each other to create various body movements.<br>The height of each sprite by item is organized in the file zmap.
2. <span style="color: #dc9656">**guide_character_summary layer**</span>: Avatar's sample body. Layer with a bigger number is placed below.
3. <span style="color: #dc9656">**guide_grid**</span>: Craft items using grid lines. You have to work within the green line.
![cape01](https://mod-file.dn.nexoncdn.co.kr/bbs/1645511035444e8349fcd53a8498eafb9e14c0a5e0fe0.png "cape01")

#### Yellow Layer
The creator can draw capes in the yellow layer.

* <span style="color: #dc9656">**edithere:cape_capeOverHead_10**</span>: Layer of the outside of the cape.
* <span style="color: #dc9656">**edithere:cape_cape_48**</span>: Layer of the part that's seen when character lies face down.
* <span style="color: #dc9656">**edithere:cape_capeBelowBody_83**</span>: Layer of the inside of the cape.

![cape02](https://mod-file.dn.nexoncdn.co.kr/bbs/16455110548840d829341a51d485097d7258e74e7b2ba.png "cape02")

# Creation
Let's create your own cape.
If you'd like to check what the cape looks like in the animation, you can use [Registering Avatar Items].

#### Drawing Patterns
1. Select <span style="color: #dc9656">**edithere:cape_capeOverHead_10**</span>. <br>![cape14](https://mod-file.dn.nexoncdn.co.kr/bbs/1646030772333162edae825294ac48cc1d5fbb96647c1.png "cape14")
2. Draw patterns on the outside of the cape. Use Pencil Tool to draw heart-shaped patterns on layer 3.<br>![cape05](https://mod-file.dn.nexoncdn.co.kr/bbs/16456004442401ea5defa579a4b6abf23766fae78e720.png "cape20")![coat10](https://mod-file.dn.nexoncdn.co.kr/bbs/1645601105790a0371e5caeb844c998d7e93f9cef9bdd.gif "coat10")
#### Changing the Color
1. Click the yellow layer.<br>
2. Select <span style="color: #dc9656">**Image - Adjustments - Hue/Saturation**</span> or use the shortcut <span style="color: #dc9656">**Ctrl+U**</span> to open Hue/Saturation window.
3. Adjust values for Hue, Saturation. Change the color of the cape.<br>![cape04](https://mod-file.dn.nexoncdn.co.kr/bbs/16455174179436b418feba80a437dbff18a16cb72c1d8.png "cape04")
4. Change colors in all yellow layers and save the file.<br>![coat11](https://mod-file.dn.nexoncdn.co.kr/bbs/1645601140643c559a7b9c69a42ebb92aa2ff968912a6.gif "coat11")
#### Making Balloons
Open the sheet to create a balloon (Avatar_Cape_balloon.psd).
 
1. Click the yellow layer.<br>![ballon](https://mod-file.dn.nexoncdn.co.kr/bbs/164551700483099e8c4210c404b7a86d0e8663daf82fc.png "ballon")
2. Select <span style="color: #dc9656">**Image - Adjustments - Hue/Saturation**</span> or use the shortcut <span style="color: #dc9656">**Ctrl+U**</span> to open Hue/Saturation window.
3. Adjust values for Hue, Saturation. Change the color of the balloon, and press 'OK' button.<br> ![ballon01](https://mod-file.dn.nexoncdn.co.kr/bbs/1645516927945b5a70e3c58994a7b99fb73ff90921c95.png "ballon01v")
5. Select the Pencil Tool and draw a ribbon on the balloon. Draw the ribbon for all actions. <br>![ballon02](https://mod-file.dn.nexoncdn.co.kr/bbs/16455169584853247e15f49954ad69dd7a2d1d12567d7.png "ballon02")
6. Change colors in all yellow layers and save the file.<br>![coat12](https://mod-file.dn.nexoncdn.co.kr/bbs/1645601233197520c1a81436a4d819c9273af39c3b9cb.gif "coat12")

> <span style="color: #86c1b9">**Tip**
> You can draw a ribbon and then copy and paste it to other actions.</span>

><span style="color: #585858">**Learn more**
> A balloon is another type of cape, so it cannot be worn together with a cape.</span>

##### Reference Guide
* [Creating Pants]
* [Creating Coats]
* [Creating Gloves]
* [Creating Shoes]