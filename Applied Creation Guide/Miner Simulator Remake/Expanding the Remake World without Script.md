# Making Portals
#### Portal from Town to Town
Let's place the Portal_TownToTown model and change its property value to make the player travel from town to town.

1. For the portal model that moves the player to town, place the **Workspace - MyDesk - Models - Portal - Portal_TownToTown** in the Scene.
![PortalTownToTown](https://mod-file.dn.nexoncdn.co.kr/bbs/169407345576958fcb84b535b476a9deaedaa9ea14e18.png{"width":"340px"} "PortalTownToTown")
2. Change the SortingLayer value of SpriteRendererComponent to **Layer1**.
![SortingLayer](https://mod-file.dn.nexoncdn.co.kr/bbs/1694073479466ef8f252e06344befadd2ee708c83327c.png{"width":"340px"} "SortingLayer")
3. In the Property Editor window, change the **DestinationTownIndex** property value of the Portal_TownToTown component to the index value of the destination town.
![DestinationTownIndex](https://mod-file.dn.nexoncdn.co.kr/bbs/1694073499868138d82902ac64200b8f654dcd9c1e864.png{"width":"340px"} "DestinationTownIndex")
#### Portal from Town to Mine
Let's place the Portal_TownToMine model and change its property value to make the player travel from town to mine. 
The mine portal is designed to move the player to the right mine based on the location of town. For example, in Town1, the player moves to the Mine1-1, and in Town2, the player moves to the Mine2-1. Therefore, when adding new towns and mines, they should be named according to the naming conventions for existing maps.

1. Place the **Workspace - Models - Portal - Portal_TownToMine** in the Scene.
![PortalTownToMine](https://mod-file.dn.nexoncdn.co.kr/bbs/16940741976881e5bf9d0e3b842859f49ce086b1339e4.png{"width":"340px"} "PortalTownToMine")
2.  Change the SortingLayer value of SpriteRendererComponent to **Layer1**.
![SortingLayer](https://mod-file.dn.nexoncdn.co.kr/bbs/1694073479466ef8f252e06344befadd2ee708c83327c.png{"width":"340px"} "SortingLayer")

#### Destination Setting when Returning to Town
You can use objects to specify where the character arrives when returning to town from the mine. 

1. Find and place the appropriate preset in **Preset List - Object**.
2.  Rename it to **object_returnPoint**.

> <span style="color: #7cafc2">**Learn More**
> Due to the _TeleportService:TeleportToEntityPath(_UserService.LocalPlayer, string.format("/maps/Town%d/object_returnPoint",townIdx)) code in the UI_GameResult's **HandleKeyDownEvent**, it will find the location to return to town from the mine if you simply rename it to **object_returnPoint**.</span>

![object_returnPoint](https://mod-file.dn.nexoncdn.co.kr/bbs/1694078193760f07ab3174fa044399b114e8d2426ae1a.png "object_returnPoint")
# Placing an NPC
Let's place an NPC model to create a Mineral Store and a Weapon Store in the town. NPC models do not require any modifications. Place it wherever you want and rename it.
1. In **Workspace - MyDesk - NPC**, place the **npc_sell, npc_weaponShop** in the Scene.
![NpcModel](https://mod-file.dn.nexoncdn.co.kr/bbs/16940782193189b10e64e079d4283beb0c5575f6f7c8d.png{"width":"340px"} "NpcModel")
2. Rename it in the **Name** of NameTagComponent. 
3. Change the SortingLayer value of SpriteRendererComponent to **Npc**.

# Customize Mine
#### Set up a Ladder
1. In **Preset List - Ladder**, select a ladder and place it in the Scene.
2. Adjust the **SpeedFactor value of ClimbableComponent** to control the speed of movement on the ladder.
![SpeedFactor](https://mod-file.dn.nexoncdn.co.kr/bbs/1694069458364c5f11bdc69db4bc2a111fe6021becf54.png{"width":"450px"} "SpeedFactor")

#### Place a Returning Robot
Let's place a returning robot model in the mine, so the player can return to town. Place it wherever you want.
1. Find and place the **WorkSpace - MyDesk - Models - Portal - npc_returnRobot**.
![returnRobotModel](https://mod-file.dn.nexoncdn.co.kr/bbs/169407140336819164d4d5e52455dac917aaf7c2163b8.png{"width":"340px"} "returnRobotModel")
2. Change the SortingLayer value of SpriteRendererComponent to **Npc**.
#### Add a Mineral Vein Level
1. Find and place the **Workspace - MyDesk - Models - MiningTarget**.
![MiningTargetModel](https://mod-file.dn.nexoncdn.co.kr/bbs/1694069983687de4b328c30cb459ab644cfb00b0fbdc4.png{"width":"340px"} "MiningTargetModel")
2.  Change the **Level** value in the **MiningTargetObject** component to change the amount of HP required for mining. The mineral vein level should be the same difficulty as the mineral vein order. The Mineral vein level for Mine1-3 should be **3**, Mine2-1 should be **4**, etc. When you add a mineral vein level, you need to add the level of gems that you acquire in the bonus game.
![MiningTarget](https://mod-file.dn.nexoncdn.co.kr/bbs/1694069434204dda5f137834b447da7553a3b0c8f91f4.png{"width":"340px"} "MiningTarget")
3. Open the **WorkSpace - MyDesk - Tables - VeinInfo** and add data. Add data from all columns according to the level. For information on each row, refer to [Tables](docs?postId=1083{"target":"_self"}).

# Add New Equipment
1. Select the **Workspace - MyDesk - Tables - EquipmentInfo** to open the dataset. If you added new equipment, you must add a row and enter the equipment information. For information on each row, please refer to [Tables](docs?postId=1083{"target":"_self"}).
![EquipmentInfo](https://mod-file.dn.nexoncdn.co.kr/bbs/1694069696514b1df5a9962da49089547f6f611dba76f.png{"width":"740px"} "EquipmentInfo")
2. Add the pickaxe information to **Type1_RUID**. The equipment you use must use resources from the **Resource Storage - avataritem - 2H Weapon** category. 
![2HWeapon](https://mod-file.dn.nexoncdn.co.kr/bbs/17272549083571350626520174607ba939682aee65148.png{"width":"740px"} "2HWeapon")
3. Add the working clothes information to **Type2_RUID**. The equipment you use must use resources from the **Resource Storage - avataritem - Coat** category.
![coat](https://mod-file.dn.nexoncdn.co.kr/bbs/172725494853645184dcc654443abadeb392e52c3f62e.png{"width":"740px"} "coat")
3. Add the bag to **Type3_RUID**. The equipment you use must use resources from the **Resource Storage - avataritem - Cape** category.
![cape](https://mod-file.dn.nexoncdn.co.kr/bbs/1727255012584f3bd7f10e3504000a320a31c3535e568.png{"width":"740px"} "cape")
3. Add the shoes to **Type4_RUID**. The equipment you use must use resources from the **Resource Storage - avataritem - Shoes** category.

> <span style="color: #7cafc2">**Tip**
> When using an RUID as a thumbnail, attach the RUID with **thumbnail://**. </span>