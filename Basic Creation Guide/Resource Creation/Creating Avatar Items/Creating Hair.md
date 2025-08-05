# Course Introduction
Let's learn how to make hair. You can change the color of existing hairstyles or draw a new one.
Hair has a complex structure, so we recommend practicing with pants or shoes before trying hair.
Please refer to [Basic Concept of Creating Avatar Items](/docs?postId=588%7B%22target%22:%22_self%22%7D) to familiarize yourself with the basic concept of avatars first.

# Preparation
Adobe Photoshop is required to create items. After installing the software, download and open the file below.
$$attachFile
{"name":"Avatar_Hair.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version5_230602/Avatar_Hair.psd"}
$$

><span style="color: #7cafc2"> **Tip**
> We recommend making hair within **120 x 120 pixels** based on the avatar's feet.</span>
> ![example](https://mod-file.dn.nexoncdn.co.kr/bbs/1685684754306c9db40323e1a487d8d01f6401b8a2ba4.png "example")
# Hair Type
Hair is either short or long.

| Short hair |  Long hair | 
| --- | --- | 
| ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/168568956575557a3b5c4532c4f31ba37bdfa798338fb.png "3") | ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/168568957903110d3a4890ef74bd58109ed7890cd741a.png "4") | 

# Production Sheet Description
The hair production sheet is divided into gray and yellow layers.
The gray layer has a data folder for image datafication, an avatar body layer to help with image creation, and a line layer for guidance. All gray layers are locked, and no modifications are required while working.
The creator can draw the actual item in the yellow layer.

> <span style="color: #7cafc2">**Tip**
> If you edit or delete the names of layers, you will not be able to register the item.
> **Never change the layer names.**</span>

### Gray Layer
1. <span style="color: #dc9656">**data folder**</span>: data folder is required for datafication of items. It does not affect image creation.
    * <span style="color: #dc9656">**data:origin**</span>: Marks the center of the data with a red dot.
    * <span style="color: #dc9656">**data:type:Hair**</span>: Indicates data type.
    * <span style="color: #dc9656">**data:vslot:H1H2H3H4H5H6HfHsHb**</span>:  Only hats use the concept of vslot.
    * <span style="color: #dc9656">**data:use_zmap_preset1**</span>: The images which form an avatar are piled up in layers. An avatar structure involves stacking images of clothes, shields, and weapons over each other to create various body movements.<br>Zmaps shop the height of each sprite separated by item in order.
2. <span style="color: #dc9656">**guide_layer**</span>: This is an avatar's sample body and hats. As the internal rule, the one with the bigger number attached after the layer name is placed below.

![01](https://mod-file.dn.nexoncdn.co.kr/bbs/1685684844020a9b9fe1ac2e748eba8846bd952a2f77e.png "01")
# Yellow Layer
On the yellow layer, the creator can draw the desired hair, and it is divided into 3 folders.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1685685193823a97012a2929d430ea43007325272ecf4.png "2")
1. <span style="color: #dc9656">**case1.front**</span>: Includes body and various hat samples for hair front work.
2. <span style="color: #dc9656">**case2.front_proneStab**</span>: Includes body types for hair when lying face down and various hat samples. Below is a purple layer.
3. <span style="color: #dc9656">**case3.back**</span>: Includes body and various hat samples for hair back work.


#### case1.front
![front](https://mod-file.dn.nexoncdn.co.kr/bbs/168568527472950ccb61ca0e14cbe86f2644e579fd5d3.png "front")
1. NoCap folder
A folder containing a layer for drawing the front of the hair and a hat sample.
[Creating Caps](/docs?postId=592{"target":"_self"}) This is a hat sample corresponding to the type 3 of vslot.<br>
    * <span style="color: #dc9656">**guide_cap_samples**</span>: Includes a sample hat that does not change the shape of the head with the default hair.
    * <span style="color: #dc9656">**edithere:front_hairOverHead_31**</span>: This layer allows you to draw the front of the hair.

2. WithCap folder
Contains a layer to draw the front view of the pressed hair when wearing a hat and samples of the corresponding type of the hat.
[Creating Caps](/docs?postId=592{"target":"_self"}) This is a hat sample corresponding to types 1 and 2 of vslot.<br>
    * <span style="color: #dc9656">**guide_cap_samples**</span>: Include samples of the hair-pressing type hats.
    * <span style="color: #dc9656">**edithere:front_hair_35**</span>: This layer allows you to draw the front of the pressed hair.
    
3. <span style="color: #dc9656">**edithere:front_hairShade_46**</span>: This layer allows you to draw the shadow of the hair.
This layer allows you to draw the shadow of the hair.
Draw the shadow with <span style="color: #6d5031">**#6d5031**</span> color according to the hair shape and apply 30% transparency to match all skin tones.

4. <span style="color: #dc9656">**edithere:front_hairBelowBody_84**</span>
This layer allows you to draw hair placed behind the body. This is mainly for long hair.

#### case2.front_proneStab
![proneStab](https://mod-file.dn.nexoncdn.co.kr/bbs/168568532965909c3e58fa3c34781830758d9ea4b2c33.png "proneStab")

1. <span style="color: #dc9656">**NoCap(copy from 1-a.NoCap) folder**</span>
This is a folder that includes a layer for drawing the front view of hair in a prone position and samples of hats.
     * <span style="color: #dc9656">**edithere:proneStab_hairOverHead_31**</span>: This is a layer that can be used to draw the front view of hair in a prone position. To facilitate smooth workflow, you can paste the layer edithere:front_hairOverHead_31 from the WithCap folder and then draw it in a spread-out manner based on the prone position.

3. <span style="color: #dc9656">**WithCap(copy from 1-b.WithCap) folder**</span>
There is a layer for drawing the front view of the hair when wearing a hat in the prone position, along with samples of hats of that type.
     * <span style="color: #b8b8b8">**guide_cap_samples**</span>: Include samples of the hat types that press hair.
     * <span style="color: #dc9656">**edithere:proneStab_hair_35**</span>: This layer is for drawing the front view of the hair in the prone position. Copy the image on the edithere:front_hair_35 layer for reference and use it as a guide to draw the hair, making it spread wider based on the prone position.

    ><span style="color: #7cafc2">**Tip**
    > The front view of the hair drawn in **duplicated:front_hairOverHead_31, duplicated:front_hairOverHead_35** should be same as edithere:front_hairOverHead_31, edithere:front_hair_35.</span>

4. <span style="color: #dc9656">**edithere:proneStab_hairBelowBody_84**</span>
This layer allows you to draw hair that is placed behind the body while in the prone position. This is mainly for long hair.
#### case3.back
![back](https://mod-file.dn.nexoncdn.co.kr/bbs/1685685577670201094e012274eb19907cfcb18577e89.png "back")
1. NoCap
Include a layer and a hat sample for drawing the back of the hair.
[Creating Hats](/docs?postId=592{"target":"_self"}) This is a hat sample corresponding to type 3 of vslot.<br>
    * <span style="color: #dc9656">**guide_cap_samples**</span>: Includes a sample hat that does not change the shape of the head with the default hair.
    * <span style="color: #dc9656">**edithere:back_backHair_91**</span>: This layer allows you to draw the back of the hair. In the front view, you should draw the long hair on a separate layer together

2. WithCap
Contains a layer to draw the back view of the pressed hair when wearing a hat, and samples of the corresponding type of the hat.<br>
    * <span style="color: #dc9656">**guide_cap_samples**</span>: Include samples of the hair-pressing type cap.
If the same shape, you can keep to the two layers below, but you can also work differently with wide and narrow shapes. It is okay to draw at the same intervals as in the sample.
[Creating Hats](/docs?postId=592{"target":"_self"}) Check out the hair spacing on the back in types 1 and 2 of vslot.<br>![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1646015941633b8506858e9f44000aed41fdb5f5efe41.png "8")
    * <span style="color: #dc9656">**edithere:back_backHairBelowCapWide_94**</span>: This layer allows you to draw the back of pressed hair. Draw a wide gap for the long hair seen from behind.
    * <span style="color: #dc9656">**edithere:back_backHairBelowCapNarrow_95**</span>: This layer allows you to draw the back of pressed hair. Draw a narrow gap for the long hair seen from behind.


    ><span style="color: #585858"> **Learn more**
    > Depending on the type of hat, you can distinguish wide and narrow head shapes. This hair needs to be crafted when applying the type of hat covering the back of the head among the hat's vslot type.
    > Depending on the creator's plan, you can work differently with wide and narrow shapes.
    > Try on the provided hat sample, and if it looks natural, you can use Wide and Narrow equally.</span>
    > ![21](https://mod-file.dn.nexoncdn.co.kr/bbs/1646019003831f716ca8d04f043dc82dfb6646dd13d68.png{"width":"430px"} "21")

# Creation
Work only with the image on the yellow layer. 
After making the basic shape in black, you can work more efficiently by modifying the detailed hair shape and color. 
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/168568560935132a94259bdea4729a8de80f5f318ada0.png "11")
#### Drawing the Front
edithere:front_hairOverHead_31, edithere:front_hairBelowBody_84, edithere:front_hair_35, edithere:front_hairShade_46 Edit the image on the layers.<br>![17](https://mod-file.dn.nexoncdn.co.kr/bbs/164601737197069b5662eaca34060b8ee41ad221a610e.png "17")<br>
1. On the edithere:front_hairOverHead_31, edithere:front_hairBelowBody_84 layer, draw the basic shape in black with <span style="color: #dc9656">**'Pencil Tool'**</span> and change the color and draw detailed hair shapes.<br> For short hair, leave the <span style="color: #dc9656">**edithere:front_hairBelowBody_84**</span> layer blank, which draws the hair placed behind the body.<br>![12](https://mod-file.dn.nexoncdn.co.kr/bbs/168568563110469b6c925c14447018db3dde23d47ee9a.png "12")<br>
2. Erase the pressed hair on the <span style="color: #dc9656">**edithere:front_hair_35**</span> layer to fit the head shape.<br>![13](https://mod-file.dn.nexoncdn.co.kr/bbs/16856861702113e6be6f4c60f474cb20004a28f6faf56.png "13")<br>
3. The front hair has shadows. On <span style="color: #dc9656">**edithere:front_hairShade_46**</span>, draw a shadow with R:109, G:80, B:49 or #6D5031 color to match the shape of your hair. Apply 30% transparency to the layer to match all skin tones.<br>![14](https://mod-file.dn.nexoncdn.co.kr/bbs/16856862015273a94bd2c09b84f52aa3deaf45a6c8199.png{"width":"440px"} "14")
#### Drawing Hair in the Prone Position
It is helpful to use the same image as the hair in "case1.front", so duplicate the corresponding layers.
Rename the duplicated layer to continue working. For short hair, keep it as it is. For long hair, draw it in a widespread manner based on the appearance when the character is in a prone position.
![18](https://mod-file.dn.nexoncdn.co.kr/bbs/168568935395427b2788e8cd941e897fee75547b9970b.png "18")

1. Copy the edithere:front_hairOverHead_31 layer and paste it to the 2-a.NoCap(copy from 1-a.NoCap) folder.
2. Copy the edithere:front_hair_35 layer and paste it to the 2-a.NoCap(copy from 1-a.NoCap) folder.
3. On the <span style="color: #dc9656">**edithere:proneStab_hairBelowBody_84**</span> layer, draw a wide spread of hair placed behind the body in a prone position.<br>![19](https://mod-file.dn.nexoncdn.co.kr/bbs/1685689459920fd561d6ff4fc406498b6eb0ae6ea6190.png "19")

#### Drawing the Back
Work with an image on the layer of edithere:back_backHair_91, edithere:back_backHairBelowCapWide_94, edithere:back_backHairBelowCapNarrow_95
![20](https://mod-file.dn.nexoncdn.co.kr/bbs/1646018647414c1a3589d385249488341053dd0652423.png "20")

1. Draw the basic shape in black with the 'Pencil Tool' in edithere:back_backHair_91, change the color, and draw the detailed hair shape.<br>![22](https://mod-file.dn.nexoncdn.co.kr/bbs/1685689505332f4c4431c46954bc1b4e4f671835ec0ff.png "22")
2. Erase the pressed hair on the edithere:back_backHairBelowCapWide_94 layer to fit the head shape.<br>![23](https://mod-file.dn.nexoncdn.co.kr/bbs/16856895279327b7abfea089946e79eed6a94368c2980.png "23")
3. Copy number 2 and put it in edithere:back_backHairBelowCapNarrow_95.<br> If you want to close the gap in the back of the tied hair, erase the long hair and redraw it.<br>![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16457811937535becc03a37664135889302106c115bff.png "5")

##### Reference Guide
* [Creating Pants](/docs?postId=584{"target":"_self"})
* [Creating Capes](/docs?postId=585{"target":"_self"})
* [Creating Coats](/docs?postId=586{"target":"_self"})
* [Creating Gloves](/docs?postId=587{"target":"_self"})
* [Creating Shoes](/docs?postId=583{"target":"_self"})
* [Creating Hats](/docs?postId=592{"target":"_self"})
