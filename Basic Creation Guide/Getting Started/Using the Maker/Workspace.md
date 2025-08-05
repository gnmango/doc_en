# Course Introduction
You can use **Workspace** to manage entities, scripts, and images more efficiently.
##### Reference Guide
[Entity, Component, Property](/docs?postId=54{"target":"_self"})
[Model](/docs?postId=55{"target":"_self"})
# Introducing Workspace
**Workspace** is where you can manage your resources in a folder type. In order to create a World more efficiently, you should be able to find what you need via **Workspace**. By default, the **Workspace** panel is located at the center right. Creators can choose to move its location.
**Workspace** consists of **BaseEnvironment**, which saves models and scripts provided by the maker, and **MyDesk**, which saves resources created by the creator. Also, for more convenience, it offers **DefaultPlayer**.

> <span style="color: #7cafc2">**Tip.**</span>
> <span style="color: #7cafc2">In the Maker, when you cannot find the **Workspace** panel, click **Panels - Workspace**.</span>

![Workspace](https://mod-file.dn.nexoncdn.co.kr/bbs/16970914230526448d81201d64b46ace8099e009114fc.png "Workspace")

**Workspace's** parent folder (entry) is as follows.

| Icon | Name | Description |
| --- | --- | --- |
| ![workspace_MyAvatar](https://mod-file.dn.nexoncdn.co.kr/bbs/16346008379922a9b756c3912461db7195808cb554abd.png "workspace_MyAvatar") | DefaultPlayer | It refers information of Player from NativeModel. The player avatar entity that is actually created belongs to the World.<br>Upon the start of the game, player avatar is created at the starting point. |
|![workspace_NativeFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/1634598960433b8c2accd227347b29b0efb94f1320156.png "workspace_NativeFolder") | BaseEnviroment | Basic Component, Service, Logic, and Event provided by Maker are saved.|
| ![workspace_UserFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/1634600723422190820861d63456fa40e21031bd5ffa2.png "workspace_UserFolder") | MyDesk | <span style="color: #dc9656">**Resources added by creator, Models and Components expanded or duplicated from BaseEnvironment will be added.**</span><br>You can change the World's setting in WorldConfig. <br> Newly added resources are saved under the MyDesk folder. You can add folders when necessary to manage resources efficiently.|

# Searching Workspace
![Common_search01](https://mod-file.dn.nexoncdn.co.kr/bbs/1634538429925640e864de1094ea8a4df3109e38dbb7d.png "Common_search01")Search from the search box to look for resources or models quickly.
* The search criterion is the name of the resource/model.
* The search scope is all of **Workspace**. 
* When you enter a search term, only results that match the criteria are displayed.
![workspace007](https://mod-file.dn.nexoncdn.co.kr/bbs/16607022459523d659807533b45bd80d145f3d879a9fe.png "workspace007")

# WorldConfig
Modifies the World's configuration. There may be more settings in WorldConfig after future updates. If you click WorldConfig, the property editor window will show settings that can be modified. For more information on WorldConfig, please refer to the [WorldConfig](/docs?postId=1101{"target":"_self"}) guide.
* **LegacyAnimation**: Applies movement and animations from previous MapleStory Worlds to your character. 
* **PlayerEntityAuthorityCheck**: Changes the Server functions of Components belonging to the Player entity to prevent them from being called from any Client other than the local Client. Enabling this setting provides a higher level of security.
* **ServiceAuthorityCheck**: Changes all Server functions of Service operating in Native to work as ServerOnly functions. Enabling this setting provides a higher level of security.
* **SourceLanguage**: If a player uses auto-translation feature in the World, use the language of SourceLanguage as the translation subject. Currently, you can use Korean and English as the original language. More languages may be available after future updates.

# BaseEnvironment
They are folders with Maker's basic features. It consists of two folders.
* **NativeScripts**
* **NativeModel**

You cannot change the order of **BaseEnvironment** or move sublists. 
All layers of **NativeScripts** cannot be duplicated/deleted. To modify the properties of **NativeScripts** and use them as desired, you need to extend the respective script. In addition, **NativeModel** can be duplicated or extended for utilization.

The main entries of **BaseEnvironment** are as follows.

| Icon | Name | Description |
| --- | --- | --- |
| ![workspace_LockedNativeFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/163459994812299d7858eb2d142a9a57caaec1f0a09a7.png "workspace_LockedNativeFolder") | NativeScripts | Component, Service, Logic, Event, Misc are saved as subfolders. <br>Scripts in this folder cannot be deleted. <br>Component can be expanded, and expanded components will be saved to the MyDesk folder. |
| ![workspace_LockedNativeFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/163459994812299d7858eb2d142a9a57caaec1f0a09a7.png "workspace_LockedNativeFolder") | Component | This is the folder that saves basic components that cannot be modified or deleted. <br> You can extend Component to use.|
| ![workspace_LockedNativeFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/163459994812299d7858eb2d142a9a57caaec1f0a09a7.png "workspace_LockedNativeFolder") | Service | This is the folder that saves basic services that cannot be modified or deleted. |
| ![workspace_LockedNativeFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/163459994812299d7858eb2d142a9a57caaec1f0a09a7.png "workspace_LockedNativeFolder") | Logic | This is the folder that saves basic logic that cannot be modified or deleted. |
| ![workspace_LockedNativeFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/163459994812299d7858eb2d142a9a57caaec1f0a09a7.png "workspace_LockedNativeFolder") | Event | This is the folder that saves basic events that cannot be modified or deleted. |
| ![workspace_LockedNativeFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/163459994812299d7858eb2d142a9a57caaec1f0a09a7.png "workspace_LockedNativeFolder") | Misc | This is the folder where other scripts are saved. |
| ![workspace_NativeFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/1634598960433b8c2accd227347b29b0efb94f1320156.png "workspace_NativeFolder") | NativeModel | This is the folder that saves basic models from Maker. You can place models in Scene to use them right away. <br>Although you cannot move to a different folder or create models, you can still utilize them by cloning or extending as needed.<br>The extended models are stored in the MyDesk folder. |

# MyDesk
**MyDesk** collects and manages music, script, image files, and extended components added by the creator.
**MyDesk** is like storage that adds new items in the same hierarchy. You need to create a new folder in **MyDesk** to save and manage them in the same location.

><span style="color: #7cafc2">**Tip**</span>
> <span style="color: #7cafc2">**Workspace** is like storage, while **Hierarchy** is the concept for the current operation. </span>
><span style="color: #7cafc2">Place a resource added to **MyDesk** into the World, and it will create the entity in **Hierarchy**.</span>
>  <span style="color: #7cafc2">Deleting the original file from **MyDesk** will delete the information from the entity referring that file, but the entity created will not be removed.</span>
> <span style="color: #7cafc2">If the entity was placed directly from **MyDesk**, **Missing** is added to the name of the entity (resource), and it turns red.</span>
> ![workspace002](https://mod-file.dn.nexoncdn.co.kr/bbs/16359122256435b2c3432bf4e40b69b2cfe2175942941.png "workspace002")

**UIPopup** and **UIToast** are logics that can be modified or deleted at the creator's choosing. Depending on the production, you can choose to delete them anytime. You can also choose to extend or duplicate them as is necessary.

| Icon | Name | Description |
| --- | --- | --- |
| ![workspace_logic](https://mod-file.dn.nexoncdn.co.kr/bbs/163459925288465e2645e5e334c0e96f20e740f8d8e5d.png "workspace_logic") | UIPopup | It's a pop-up message logic. You can modify the original logic or delete it.|
| ![workspace_logic](https://mod-file.dn.nexoncdn.co.kr/bbs/163459925288465e2645e5e334c0e96f20e740f8d8e5d.png "workspace_logic") | UIToast | It's a toast message logic. You can modify the original logic or delete it.|

#### Adding Resource to MyDesk
1. Click the **[+]** button on the right side of the search box to open the Context menu.
2. Choose an item.
3. The new item will be added under **Workspace's** **MyDesk**.

><span style="color: #7cafc2">**Tip**</span>
> <span style="color: #7cafc2">Since resources added by the creator can all be changed, they are created within **MyDesk** .</span>

![workspace05](https://mod-file.dn.nexoncdn.co.kr/bbs/16874792631302293c55974c04671a6aa795ca68ae9d2.png "workspace05")
#### Deleting MyDesk Folder
You can delete empty folders immediately.
Deleting the folder that contains resources will delete all the resources in the folder. To prevent mistakes, when deleting a folder containing resources, press **Delete** and then click the **[Yes]** button in the warning window to delete it.

![workspace04](https://mod-file.dn.nexoncdn.co.kr/bbs/1656904249966670999d9056642848204c30fdfa8d219.png "workspace04")

# Workspace Context Menu 
The context menu list is composed of the following.

#### DefaultPlayer Major Context Menu

| List | Description |
| --- | --- |
| Duplicate | Create a model by duplicating DefaultPlayer. |
| Open | It is used to check the status or changes of the player.|
| Add Component | Add components to DefaultPlayer. |
| Add New Component | Create a new component and add it to DefaultPlayer. |
| Extend | Create an extended model of DefaultPlayer. |
| Copy Entry ID | Copy the ModelID of DefaultPlayer. |

#### BaseEnvironment Major Context Menu
You cannot access the context menu from a locked folder.

| List | Description |
| --- | --- |
| Duplicate | Clones NativeModel. |
| Open | Open selected item. |
| Add Component | Add components to NativeModel.  |
| Add New Component | Add a new component to NativeModel. |
| Place to Hierarchy | Place selected model in Scene. |
| Extend | Create an extended model of the selected model. |
| Copy Entry ID | Copies ID of selected model. |

#### MyDesk Context Menu
| List | Sub-list | Description |
| --- | --- | --- |
| Create Folder | - | Creates a new folder. |
| @cols=1:@rows=8:Create Scripts | Create Component | Create a new script component. |
| Create EventType | Creates a new event type. |
| Create ItemType | Creates a new item type. |
| Create BTNodeType | Create a new BTNode type. |
| Create StateType | Create a new StateType type. |
| Create StructType | Create a new StructType type. |
| Create Logic | Creates a new logic. |
| Create Script | Creates a top-level script where you can start composing from scratch.   |
| Create DataSet | - | Create a new DataSet. |
| Create TileDataSet | - | Create a new TileDataSet. |
| Create AtlasBlueprint | - | Create a new AtlasBlueprint. |
| Create Material | - | Create a new material. |
| Create Model | - | Create a new empty model. Manually add a new component. |
| Copy Entry ID | - | Copy the ID of the selected entry. |
| @cols=1:@rows=4:Import From | Import Scripts  | Imports script from PC. |
| Import Image | Imports image file from PC.  |
| Import Sound | Imports sound file from PC. |
| Import Package | Loads package from PC. |