# Course Introduction
Let's learn how to manage the resources registered by the user in the maker.

# Resource Classification
Resources in MapleStory Worlds can be used in the Property Editor or scripts. There is also a RUID for each resource. Resources can be divided into <span style="color: #dc9656">**MapleStory Worlds Resources**</span> and <span style="color: #dc9656">**User Resources**</span> according to the provider.
![resource](https://mod-file.dn.nexoncdn.co.kr/bbs/1663573480762b03715161bb34fed8cc0827095d2eecb.png "resource")

## MapleStory Worlds Resources
MapleStory Worlds resources literally refer to the resources provided by MapleStory Worlds.
You can check in Maker's **Resource Storage** and on the [MapleStory Worlds Official Website](https://maplestoryworlds.nexon.com/{"target":"_self"})'s **MSW Resource** page.

| MAKER | Official Website |
| :---: | :---: |
| ![01](https://mod-file.dn.nexoncdn.co.kr/bbs/1663572349099eb3cd0d27a594ba7a20a42d42b8b7556.png "01")| ![02](https://mod-file.dn.nexoncdn.co.kr/bbs/1675133031455e24cf07840e34bceb6f213e7b0263b1e.png "02") |

## User Resource
It refers to the registration of images and audio files owned by the user to the maker.
User resources can be categorized as **Workspace Resources** and **Resource Storage Resources** depending on the location registered on the Maker.

### Workspace Resource
Users can register (Import) image files (Sprite) and sound files (Audio Clip) in **Workspace**.
![03](https://mod-file.dn.nexoncdn.co.kr/bbs/1687485897578727af760113a444395d417a01539f4c5.png "03")
The resources registered in **Workspace**</span> <span style="color: #dc9656">**belong to**</span> the World to use freely within that World. If you plan to use a resource only in the World you are currently creating, register and use it in **Workspace**</span>.

### Resource Storage Resource
Users can directly register resources in **Resource Storage** or copy MSW resources to their own resources and edit meta information to use them.

#### Direct registration
In **Resource Storage**, <span style="color: #dc9656">**Sprite, Animation Clip, Audio Clip, and Avatar Item**</span> can be registered.
![04](https://mod-file.dn.nexoncdn.co.kr/bbs/1678782648494d9b3ec872bc64123891a3fd1a1559312.png "04")

**Sprite and Audio Clip** can be registered as files owned by users. **Animation Clip and Avatar Item** can be created and registered with an exclusive editor. 

> <span style="color: #585858">**Learn more**
> To learn more about how to register an Animation Clip, please refer to the guide below!
> [Making Animations](/docs/?postId=595{"target":"_self"})
> To learn more about how to register an Avatar Item, check out the guide below!
> [Registering Avatar Items](/docs/?postId=590{"target":"_self"})</span>

#### Duplicate MSW resource
After duplicating the MSW resource, you can use it by editing the meta information (name, description, pivot, etc.).
Selecting **Duplicate** from the context menu of the desired MSW resource creates a duplicate in the **duplicated sprite** folder at the bottom of the My Resources tab.
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/1678782789197b58ddbfda81346a7a7411c4f44b86624.png "12")

You can edit information such as name and description in the duplicate's details page. You can also change information such as pivots by pressing the **[Edit]** button on the sprite slice.
![34](https://mod-file.dn.nexoncdn.co.kr/bbs/1678782834334f32fee7df67249ebb27af62bf01c0af6.png "34")
<br>

The resources registered in the <span style="color: #dc9656">**individual World**</span>'s **Resource Storage** <span style="color: #dc9656">**belong to the account**</span>. The resources registered by an account can be used freely in multiple Worlds created with that account. The resources registered in the <span style="color: #dc9656">**group World**</span>'s **Resource Storage** belong to the <span style="color: #dc9656">**group**</span> to freely use in multiple Worlds created with that group. Therefore, if it is a resource that is commonly used in multiple Worlds, , it is convenient to register it in **Resource Storage** and use it.

# Resource Permissions
Permissions vary depending on the resource type.

| Resource Classification | Permissions |
| :---: | --- |
| MapleStory Worlds Resources | All creators are free to use it. |
| Workspace Resource | It can only be used in that World. |
| Resource Storage Resource | The World creator and the Resource Storage owner must be the same to use it. <br>However, setting the sharing function in Resource Storage makes it freely available to all users. |

## Personal Resources - Group Resources Migration
Resource Storage resources can be moved and used as group resources.

| Migrating from details | Migrating to the context menu |
| :---: | --- |
| ![transfer](https://mod-file.dn.nexoncdn.co.kr/bbs/1663573050112304b69a370bc49819c51aed9ab3a960e.png "transfer") |![transfer2](https://mod-file.dn.nexoncdn.co.kr/bbs/1663573160211616209bae24b41e592cb2fe8c002ac33.png "transfer2") |
By making the above selections, you can decide to which group to transfer ownership.
![transfer3](https://mod-file.dn.nexoncdn.co.kr/bbs/166009761623858710a6f359944a08c2e8083f1b9b6a4.png "transfer3")
> <span style="color: #585858">**Learn more**
> To learn more about how to coproduce as a group, check out the guide below! 
> [Coproduction](/docs/?postId=670{"target":"_self"})</span>

## Resource Sharing
You can set the sharing function for Resource Storage resources as shown below.
![resourcesharing](https://mod-file.dn.nexoncdn.co.kr/bbs/1663573280416035106d46d18409e86f429c841a8189e.png "resourcesharing")
Let's say that **World A** that the original author allowed someone else to remake was launched. For **World A**, **the Resource a** was used. If another creator remakes **World A**, they will be able to use **Resource a** freely in the Remake World as well. However, it is important to note that if the original creator disables sharing of the original **Resource a** in the future, **Resource a** will not be available in the Remake World.

# Resource Version
Depending on whether it is a Workspace resource or a Resource Storage resource, the extent to which resource updates and remakes are affected will vary. So, if you're thinking about allowing remakes when releasing your World, you should also consider where to add resources to, **Workspace** or **Resource Storage**.

## Workspace Resource Version
If you import a file with the same name into **Workspace**, the version of the resource will be upgraded. Since the Workspace resources for the derived Worlds are copies of the source, they will be updated per each World. So, even if the resource of the original World changes, the Workspace resource for the derived Worlds will not be affected.
> <span style="color: #585858">**Learn more**
> Derived Worlds refer to the Worlds like the ones listed below. 
> * Remake World
> * A World newly created by Importing after Exporting the source World</span>

## Resource Storage Resource Version
When the Resource Storage resource is replaced, their version will be upgraded. 
![details](https://mod-file.dn.nexoncdn.co.kr/bbs/16602681940820f541f49e7924e6d81a5d81f7eac38f3.png "details")
When a Resource Storage resource is replaced, the derived Worlds that are using that resource will also be affected by the resource replacement. As such, it doesn't matter where you add the resource to when you allow remakes, if you don't plan to change the original resource. But if there's a chance of changing it, we recommend adding the resource only to **Workspace**.