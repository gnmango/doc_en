# Course Introduction
In MapleStory Worlds, creators can create their own avatar items.
To make their own avatar items, creators have to go through **creation** and **registration** steps.
This guide will cover the registration process. Please refer to the guides below to learn about creation.
##### Reference Guide
* [Basic Concept of Creating Avatar Items](/docs?postId=588{"target":"_self"})
* [Creating Pants](/docs?postId=584{"target":"_self"})
* [Creating Capes](/docs?postId=585{"target":"_self"})
* [Creating Coats](/docs?postId=586{"target":"_self"})
* [Creating Gloves](/docs?postId=587{"target":"_self"})
* [Creating Shoes](/docs?postId=583{"target":"_self"})
* [Creating Hair](/docs?postId=657{"target":"_self"})
* [Creating Caps](/docs?postId=592{"target":"_self"})

You can register the following parts: hair, cap, cape, coat, gloves, pants, and shoes.
Each part shares the same registration process. In this guide, we'll register a coat as an example.
We're going to use the resource file below as an example.
$$attachFile
{"name":"Avatar_Coat.psd","url":"https://mod-file.dn.nexoncdn.co.kr/storage/Avatar_psd/version5_230602/Avatar_Coat.psd"}
$$

# Register Avatar Items
1. Click the **[Create]** menu followed by the **Avatar** tab at the top.
![01](https://mod-file.dn.nexoncdn.co.kr/bbs/1669168971262bb2d9a28128c4f18a5d0d84e037a14b7.png "01")
<br>
2. Download the file **Avatar_Coat.psd**, then in the **Avatar** tab, click the **[Create New]** button.
(If you have an avatar item's PSD file that you created, you can go with that.)
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/166916901725233a6cc83676346419a904c895afcd217.png "2")
<br>
3. Click the **[Select PSD File]** button and select the downloaded PSD file.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1669169072328da1b888ea66c43858e34f22e3f474986.png "3")
<br>
4. When the PSD file is uploaded, you can check the preview.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1678688819372ef6b45a344c44f42a946e87abc138cf2.png "4")
<br>
5. Check whether the item has been created as you intended.
    * Check each action of the avatar to see if the item is working properly. 
        * In the action list, click each **ActionName** to check. 
        * Click **AutoPlay** to select a certain frame and check the avatar preview.<br>![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1678687227625a70c35df565d4377a7962ab9c5a23649.png "5")
    * **[Apply to My Avatar]** and **[Apply World Background]**
        * Click the **[Apply to My Avatar]** button to preview how the item you're creating looks on your avatar.
        * Click the **[Apply World Background]** button, and you can easily see how it actually looks in the game.<br>![6](https://mod-file.dn.nexoncdn.co.kr/bbs/167868732738290f6f4c876e9423aac23d2df0d797a75.png "6")
<br>
 6. Write the name and description of the item.
![21](https://mod-file.dn.nexoncdn.co.kr/bbs/1678688926747f9fad8a560cf4820a6de99a623953c2c.png "21")
    ><span style="color: #7cafc2">**Tip**
    > The item's thumbnail image is auto-generated based on the uploaded PSD file. Thumbnails can be set based on the motion and frame of the currently set avatar animation.
    > ![22](https://mod-file.dn.nexoncdn.co.kr/bbs/1678688963672637088dd40754b9dbd69e39db142dbab.png "22")</span>
<br>
7. Now, in the **AvatarItem Editor**, click the **[Upload]** button.
Once the upload is finished, you can see the item registered on the avatar list.
![10](https://mod-file.dn.nexoncdn.co.kr/bbs/16691695226200e4e178bd6744a74bcfe539386c224cd.png "10")


# Registering in Resource Storage
You can also register avatar items in the Maker's Resource Storage.
From the top navigation menu, click **Panels - Resource Storage** to open the Resource Storage.
Go to **My Resource - avatarItem - coat** to see the item you've registered in advance.
You can register the avatar in Resource Storage by clicking the ![add](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_layer_add.png "add") button at the top.
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/1669169579147bf312a28743a4c00b5fe5b7ff74ef6ce.png "12")


# Editing and Deleting Avatar Items
Let's look into the avatar item context menu.
* <span style="color: #dc9656">**Rename**</span>: Renames resource.
* <span style="color: #dc9656">**Delete**</span> : Deletes resource.
* <span style="color: #dc9656">**Copy RUID**</span>: Copies resource RUID to clipboard.
* <span style="color: #dc9656">**Detail Info**</span>: Opens pop-up window to see detailed information about resource. You can also double-click the resource icon.
![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1656554906953f6d2eb6287874ecfa49a2132ad6e31af.png "13")

Double-click the resource icon or click **Detail Info** to see detailed information. You can change the name and description of resources as well.
![14](https://mod-file.dn.nexoncdn.co.kr/bbs/1669169655194e1fccb67bfb64991bce0622737eacd7a.png "14")

On the **Detail Info** window above, click the **[Open File]** button marked with an arrow to open the **AvatarItem Editor** window where you can do things like replacing the PSD file.
![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1678689137446fc9b00e8b4194bafab9ec6b0c147dc84.png "10")

# How to Equip Avatar Items
In a World made by Creator, you can equip avatar items on yourself.
1. Click the property of the avatar item you want to equip in **CostumeManagerComponent** of **DefaultPlayer**.
![16](https://mod-file.dn.nexoncdn.co.kr/bbs/16456053556420f1ccdd1896d45a2a25427d572cbfecf.png{"width":"450px"} "16")
<br>
2. Clicking the button will open **Resource Picker** and show you the registered avatar items in **My Resources**. Click the desired avatar item.
![17](https://mod-file.dn.nexoncdn.co.kr/bbs/1681088578867267de45e70f84e819a2684a10f6a65d6.png "17")
<br>
3. If you check the property, you'll find the RUID value entered as follows.
![18](https://mod-file.dn.nexoncdn.co.kr/bbs/16456054206924cdf9652c0614d239739b4b10e9b8c3a.png{"width":"450px"} "18")

> <span style="color: #585858">**Learn More**
> If you create and register an avatar item as a product, you as a creator can purchase and use it and sell it to other users. 
> If you'd like to sell your avatar item, please refer to the [Creating Avatar Products](/docs/?postId=830{"target":"_self"}) guide. </span>
