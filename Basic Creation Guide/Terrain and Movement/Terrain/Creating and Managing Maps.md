# Course Introduction
Different maps provide a variety of fun to users exploring the world.
Let's examine the basics of creating and managing maps in this course.

# Managing Maps
There are two ways to manage a map.
* Managing through the **Hierarchy - maps** context menu and sub-menus
* Managing through the **Window - Map List** window

# Creating Maps

| Hierarchy | Map List |
| ----------- | ------- |
| Click **Create New Map** from the **Hierarchy - maps** context menu<br>![maplist03](https://mod-file.dn.nexoncdn.co.kr/bbs/168619256953482671c21f29e4b3fa358e803384cd88a.png "maplist03") | <ul><li>Click the **[Make New Map]** button on the bottom of the **Map List** window.</li> <li>Click the **[Add]** button in the map's context menu.</li></ul> ![New nap](https://mod-file.dn.nexoncdn.co.kr/bbs/174530183147858c0c408c04f4192a36f0dc75f34e47d.png{"width":"420px"} "New map") |

> **<span style="color: #7cafc2">Tip</span>**
> <span style="color: #7cafc2">The numbers in the map name rise in ascending order.</span>
> <span style="color: #7cafc2">If there are any changes, you'll be asked to save.</span>

# Duplicating Maps

| Hierarchy | Map List |
| ----------- | ------- |
| Click **Duplicate** from the context menu of the map you're trying to copy <br>![maplist008](https://mod-file.dn.nexoncdn.co.kr/bbs/166547548197887e3c38de1bc42f9aa9c4444a464c110.png "maplist008") | Click **Duplicate Map** from the context menu of the map you want to copy<br>![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1745301866206fb94152dc75147caa85a1fc06918c7e5.png "duplicate") |

# Deleting Maps
| Hierarchy | Map List |
| ----------- | ------- |
| In the context menu of the map you want to delete, click **Delete**<br>![maplist007](https://mod-file.dn.nexoncdn.co.kr/bbs/16654754071719502fdc9e05a4db98079408b2c3dc57d.png "maplist007") | In the context menu of the map you want to delete, click **Delete Map**<br>![maplist011](https://mod-file.dn.nexoncdn.co.kr/bbs/1687510622017287506de587c442eae8f10a1f3fffa35.png "maplist011") |

> **<span style="color: #7cafc2">Tip</span>**
> <span style="color: #7cafc2"> You cannot delete the **starting map** and the **currently editing map in the Scene**.</span>

# Loading Another Map

| Hierarchy | Map List |
| ----------- | ------- |
| Double-click a map to load<br>![maplist005](https://mod-file.dn.nexoncdn.co.kr/bbs/1665475758380d0ae41686e544179a2f88336522d930c.png "maplist005") | Click the **[Load]** button after selecting a map <br>![loadingMap](https://mod-file.dn.nexoncdn.co.kr/bbs/1745301918755eeb7dc3d4b3d4e9ab453d7888e88f9f5.png{"width":"430px"} "loadingMap")|

# Setting Starting Map
The starting map is the first map the player avatar enters when executing the released world. By default, **map01** is the starting map, but you can change it as you need.

| Hierarchy | Map List |
| --- | --- |
|Click **Set Starting Map** from the context menu of the map that will be the starting map <br>![startingmap01](https://mod-file.dn.nexoncdn.co.kr/bbs/16654761105842d01f5d7edc04abcb2e4fa0d6fb3c16d.png "startingmap01") | Click **Starting Map** from the map that will be the starting map <br>![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1745302008909327f9e259ac6418f850b800fa9902885.png "startingmap")|

><span style="color: #585858">**Difference between "Starting Map" and "Game Starting Point"**</span>
> <span style="color: #585858">Setting a starting map and choosing a position for test are two different concepts.</span>
> <span style="color: #585858">During production, if you're playing the game for a test, you start at the map you're working on. </span>
> <span style="color: #585858">Please refer to [Setting Game Start Position](docs?postId=115{"target":"_self"}) for more details.</span>

# Choosing Whether to Release the Map
Maps you create are included in the release target by default. However, you can exclude some maps from release.

| Hierarchy | Map List |
| --- | --- |
| In the context menu of the map you want to exclude, click **Exclude from Released Map** <br>![exclude](https://mod-file.dn.nexoncdn.co.kr/bbs/1684214359320ddda193ee6d5445a89731c10103fe946.png "exclude") <br> If you exclude it from being released, the release icon will not be visible. <br>![exclude03](https://mod-file.dn.nexoncdn.co.kr/bbs/166554042257954731952e0df4c27bdbe47c466b052f7.png "exclude03") | In the context menu of the map you want to exclude, click **Exclude from Released Map** <br>![exclude01](https://mod-file.dn.nexoncdn.co.kr/bbs/168445486910307dd7acb82f944ff8fe7ceea8b39e53c.png "exclude01") <br> If you exclude it from being released, the release icon will not be visible. <br>![exclude02](https://mod-file.dn.nexoncdn.co.kr/bbs/16844549675036d34a9a31b9548acaa2567c5858198e0.png "exclude02") |

> <span style="color: #585858">**Learn more**
> - You cannot exclude the starting map from release. Also, if you change an off-target map to a starting map, that map is automatically included in the release target.
> - In a co-produced world, only the group leader can set up whether to release a map.
> - To reintroduce a map that has been excluded from release, click the context menu of the map and click **Add Map to Publish**.</span>


# Map Movement Using Portal
1. Place the portal on the map you want to connect to.
![maplist30](https://mod-file.dn.nexoncdn.co.kr/bbs/16377379799957f5bcea9eb1d44a899fedb83cd07f630.png "maplist30")
<br>
2. Click the **[◉]** button of the **TargetPortal** in the Property Editor of the portal. 
Then select the portal you want to move from the **Reference** pop-up window.
![maplist31](https://mod-file.dn.nexoncdn.co.kr/bbs/1665477149603091fc46daa8d480cb009d0d4fa2ea34e.png "maplist31")
<br>
3. After clicking the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button, check to move from the departure portal to the arrival portal.

# Summary
We can't really define the perfect number or size of maps.
 You can make multiple maps according to your plan, or you can make one large map.
 However, you may have delays between loading and playing if the map is too large or has many placed objects. Therefore, you need to adjust the size of the map and the number of objects appropriately.
