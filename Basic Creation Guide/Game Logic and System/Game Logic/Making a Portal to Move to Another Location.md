# Course Introduction
You can add portals within the maps of your world to allow players to quickly travel from place to place. Let's explore how to use [PortalComponent](/apiReference/Components/PortalComponent{"target":"_self"}).

# Making Portals
[PortalComponent](/apiReference/Components/PortalComponent{"target":"_self"}) allows you to set an entity with the same PortalComponent as the destination, and enables the player avatar to move to the destination when performing a specific action (such as a key input) within the portal area. We recommend using visually prominent sprites when creating entities that serve as functional portals. For example, a pair of shining circles or two distinct images will help the user distinguish the portals from other objects.
#### Making Portals with a Portal Model
Select **Preset List - Portal** and place it in the **Scene**. Portal models automatically include **PortalComponent**.<br>![101](https://mod-file.dn.nexoncdn.co.kr/bbs/1663915370804d5440d19b5644734bd933e7778f83708.png "101")<br><br>
#### Making Portals from Scratch
1. Place an entity that uses a custom-made sprite or MSW resource sprite.
2. Open the Property Editor window and add **PortalComponent**.
![portal03](https://mod-file.dn.nexoncdn.co.kr/bbs/16877649298670292c6aaa1a24154979d84844e1418f3.png "portal03")
![portal04](https://mod-file.dn.nexoncdn.co.kr/bbs/1634802949512ccfae4badddf4a9b94e54c465e52fe6a.png "portal04")

# Setting Portal Destinations
You can target an entity with **PortalComponent** to set it as the destination. Only one destination can be set per component and the portal is one-way only.

1. Place 2 or more portals.<br>![portal05](https://mod-file.dn.nexoncdn.co.kr/bbs/1634802987275d1041c5e61784d01aa9e0f46471e9154.png "portal05")
2. Click **PortalComponent - PortalEntityRef** and select the destination portal.<br>![portal06](https://mod-file.dn.nexoncdn.co.kr/bbs/1634803045300b9122896120d423ea925ee9a2a3f89c2.png "portal06")
3. Press the Start ![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play") to check if it works.<br>![portal4_1](https://mod-file.dn.nexoncdn.co.kr/bbs/16348033108010ff14400fad74976a47ac11befbe43e3.gif "portal4_1")

> <span style="color: #7cafc2">**Tip**</span>
> You can refresh the <span style="color: #7cafc2">portal path by placing it in ![workspace_world](https://mod-file.dn.nexoncdn.co.kr/bbs/1634520188174b448bb50e5c64320bb3c882a5b438d6d.png "workspace_world")World, and then **Saving (Ctrl + S) or ![Common_SoundPlay](https://mod-file.dn.nexoncdn.co.kr/bbs/1635317657654c59c47ffc44d414db579b8d2fc0715a8.png "Common_SoundPlay") test playing**.</span>
> <span style="color: #7cafc2">Please save or test, as you need to refresh to see the set portal path.</span>
# Making Two-Way Portals
You can connect two portals so that they go both ways.
This can work on the same map, or between two different maps.

#### Two-Way Portals on the Same Map
1. Place portals in their positions.
2. Choose the destination for each **PortalComponent - PortalEntityRef**.<br>![portral11](https://mod-file.dn.nexoncdn.co.kr/bbs/163480347290834c9258780db465485f2abcf0bc3c64c.png "portral11")
3. Press Start ![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play") to see the movement.<br>![portalediting6](https://mod-file.dn.nexoncdn.co.kr/bbs/16348058145167aaa89d461ff4c1c8946f863262bd401.gif "portalediting6")

#### Two-Way Portal between Maps
1. Place portals in their positions in different maps.
2. Press **PortalComponent - PortalEntityRef** and select the destination.![portal12](https://mod-file.dn.nexoncdn.co.kr/bbs/1634804528834453c58fe14e24cc8adb4ab9e4a3e0a5b.png "portal12")
3. Press the Start ![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play") to see the movement.<br>![portal55](https://mod-file.dn.nexoncdn.co.kr/bbs/16348062106844c69bcbaa6df4e4e9f3a6cd65394b137.gif "portal55")

# Setting Portal Areas
You can set the portal area regardless of the Sprite size. Box Size and Box Offset are synchronized.
![portal07](https://mod-file.dn.nexoncdn.co.kr/bbs/1634804632410373ff4eeb66a406facf39bb888857a1d.png "portal07")
* BoxCollider: Activates the portal range editing function.
* BoxOffset: The area that's considered the portal.
* BoxSize: Changes the size of the portal box.

1. Press **Box Collider - Edit** to enable editing mode.<br>In editing mode, the area will turn red.<br>![portal10](https://mod-file.dn.nexoncdn.co.kr/bbs/16348046753553247448f7d1e4b83add5fac2e7b76e80.png "portal10")
2. You can change the X and Y values, or drag the box handler to adjust its size.<br>![portalboxsize](https://mod-file.dn.nexoncdn.co.kr/bbs/1634804819824c51759c6a62848348bb5f7f339bbf701.gif "portalboxsize")
3. Press Start ![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play") to see the changed area. <br>As the box is bigger than the portal sprite, you can also move from the side of the portal.

| Basic box setting | After changing box size |
| -------- | ----------- |
| ![portal33](https://mod-file.dn.nexoncdn.co.kr/bbs/16348050501643cf72b5f23084b3794b9a405f1cbeceb.gif "portal33") | ![portal22](https://mod-file.dn.nexoncdn.co.kr/bbs/1634805074080dd3861e6a9bc4ec89beff04deb974bf0.gif "portal22") |
