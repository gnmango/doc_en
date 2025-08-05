# Course Introduction
Local Workspace allows you to download a creator's World data to your desktop.
Let's find out how to use the local Workspace, synchronize it with the Maker, and create a World.
##### Reference Guide 
* [Coproduction]
* [Coproduction Group Member's Level and Permission]
* [WorldConfig]
* [Using LocalWorkspace]

# Introducing Local Workspace
**Local Workspace** allows you to download a creator's World data to your desktop (Local). You can save various entries and data of the World as files in your desktop.
Files that have been saved on the computer can be used in various ways based on creator creations using the local workspace function. You can save them as back-up files in case of an emergency, manage shared files as a group, or even manage files in conjuction with shape management tools. 

> <span style="color: #dc9656">**Caution!**
> **LocalWorkspace is a preview feature, and it may not be stable. There may be unintended bugs, and the feature may be removed via future updates. Therefore, it is advised that you make backup files and a backup World, so you can create your World without LocalWorkspace.**</span>

# Activating a Personal Local Workspace

1. Activates **Workspace - WorldConfig - LocalWorkspace**.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1718943333689397acd2763cb44bba91abfa9a163d1e4.png "2")
2. Select the path the file will be saved to from the folder selection window.
3. Check if the world's file has been downloaded by opening the selected folder. 
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/172914247719117cb875a22f64dbcbb83f754e5fc4b57.png "3")
4. Change the world from Maker or add a new script entry to save to the selected folder. 

# Activate the Group's Local Workspace
Only the **group leader and manager** can enable the Local Workspace feature for the first time. Group members are only allowed to designate the path of the Local Workspace.

#### Group Leader and Manager
Local Workspace is available on Coproduction Worlds and can only be enabled by the group leader and manager. 
You will download the world's data to the path the creator has seleceted upon activating LocalWorkspace. You can activate the LocalWorkspace in the following way.

1. Turn on the recovery mode via **Panels - Cooperation - Members**. In recovery mode, group members are not allowed to modify the World.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1718943320876d4fdf86c67f2483e974e5e0cef47768a.png "1")

    > <span style="color: #7cafc2">**Tip**
    > All group members must check in their work before turning on the recovery mode. It is advised that you save your work as a separate file as backup.</span>

2. Select **Workspace - WorldConfig - LocalWorkspace** to enable.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1718943333689397acd2763cb44bba91abfa9a163d1e4.png "2")
3. In the folder selection window, designate the path where the files will be saved. Once downloaded, coproduction will be disabled.
4. Open the designated path to confirm the files were successfully downloaded. 
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/172914247719117cb875a22f64dbcbb83f754e5fc4b57.png "3")
5. Share the World files with your group members.
6. If you modify the World in the Maker or add a new script entry, changes will be saved in the designated folder. 

#### Group Member
Once the group leader or manager shares the World file, group members can download it to their local desktop. Then, if group members enter the World with Local Workspace enabled, the folder selection window will appear.
They can designate the path on their desktop for the Local Workspace and modify the World in the Maker.

1. Download the World data shared among the group.
2. Designate the folder where the World data will be downloaded.

# Using the Local Workspace
#### Local Workspace Folder
Once LocalWorkspace is enabled, the World data will be downloaded in four different folders.

 | Name | Description  |
 | --- | --- |
 | Global | The folder containing the various entries in the world. |
 | map | This is the Hierarchy's map entry folder. |
 | RootDesk| A folder containing Workspace - MyDesk's entries. |
 | ui | This is the Hierarchy's ui entity folder.|

#### Local Workspace and Remote Workspace
Once Local Workspace is enabled, there are two separate storage locations for Local and Remote Workspace. Creators can synchronize the Local Workspace and Maker as needed to create their World. When they need to record revisions or release their World, they will synchronize their Remote Workspace and Local Workspace.

#### Synchronizing Maker to Local 
After saving the World, any changes made in the Maker will be synchronized as local files at the designated path.

> <span style="color: #dc9656"> **Warning!**
> When the workspace's entry or folder name is set as rules that aren't allowed by OS, there may be issues when saving to local.
> The issue must be fixed by checking if the names that fall under the conditions below exist before activating the local workspace.</span>
> * <span style="color: #dc9656">**`<` (Less-than Sign), `>` (Greater-than Sign), `:` (Colon), `"` (Double Quotation Mark), `/` (Slash), `\` (Back Slash), `|` (Vertical Bar), `?` (Question Mark), `*` (Asterisk)**</span>
> * <span style="color: #dc9656">Use of reserved words **CON, PRN, AUX, NUL, COM1~COM9, LPT1~LPT9** </span>
> * <span style="color: #dc9656">There is a **period or space** at the start or end of a file or folder name</span>
> * <span style="color: #dc9656">**Exceeded 255 characters** including total path</span>

#### Synchronizing Local to Maker
You can use **Refresh, Reimport, ReimportAll** in the right situation through the context menu from Hierarchy and Workspace.
Exit MapleStory World, download the World file, and then load the World in the Maker to import local files and synchronize with them.
If you've downloaded the World's metadata file using the configuration management tools while using the Maker, you may need to synchronize the local data with your Maker manually.

 |Name | Description | 
 |---------|----------|
 | Refresh | Refresh and load files in the Maker that were only modified in Local Workspace.|
 | Reimport | Reflect local changes in individual scripts or Dataset entries. |
 | Reimport All |Applies all of the files in Hierarchy, Workspace from the local workspace. If you run into issues when loading individual files, you can try Reimport All to solve the issue. Re-loads the world after loading all files. | 

> <span style="color: #dc9656">**Warning!**
> **Files downloaded to a local workspace have unique information, so we don't recommed directly copying or editing files from local. Doing so may cause synchronization errors due to randomly edited files.**
> **However, mLua files can be edited safely by using the mlua VS Code Extension, and CSV files can be freely edited.**</span>

#### Synchronizing to a Remote Workspace
Creators can save world data to MapleStory World's DB for LocalDataset synchronization, recording Revisions, etc. 
Select **File - Sync to Remote Workspace** to synchronize local files to the Remote Workspace. Revisions will be recorded based on the data of the Maker before the synchronization.
Only the **group leader and manager** can synchronize group worlds to the remote workspace.

#### Release of World
When a world is released, the latest local workspace content from the point of release is synchronized to the remote workspace.
If changes made on maker were not saved on local, you must first synchronize maker to local, and then local to Remote Workspace before releasing the World.
For group worlds that use a local workspace, only the **group leader and manager** can release the world.

# Deactivating Personal Local Workspace
If you're no longer using a local workspace or if you enabled it by accident, you can disable the feature. Before disabling the feature, you need to save the data in a local workspace to remote workspace and then deactivate the local workspace.

* Deactivate **Workspace - WorldConfig - LocalWorkspace**.

# Deactivating a Group's Local Workspace
When a group's local workspace is deactivated, the maker will be changed back to coproduction mode.

1. Disable the feature via **Workspace - WorldConfig - LocalWorkspace**.
2.  Check if the maker has been changed to coproduction mode.

# Local Workspace and File Sharing Tools
You can more efficiently create worlds by using separate file sharing tools alongside the local workspace. 
Since group worlds will no longer support coproduction features when the local workspace is activated, group members won't be able to synchronize changes in maker. So you will need a shared storage space where files can be shared to and downloaded from.
Using cloud storage or configuration management tools (svn, git) will allow all members to keep track of changes made in the World data and conduct individual tests by applying only necessary files on the Maker.

# Linking Local Workspace and VS Code
The mLua file of the selected item from the maker will automatically open in VS Code when the VS Code link function is activated. The **LocalWorkspace** item is added to **File - Setting - Create** when **LocalWorkspace** and **UseExtendedScriptFormat** are activated. The VS Code can be linked by activating this item. The VS Code link option is only supported on **Windows**.
You can create the `.vscode` folder, `.code-workspace` file, and `launch.json` file under the local workspace by linking the VS Code. 

> <span style="color: #7cafc2">**Tip**
> The line numbers of **log messages** from the **Console, Build Console** triggered while linked will display as the VS Code's line numbers. </span>

> <span style="color: #7cafc2">**Tip**
> The `launch.json` file is needed when using the VS Code Debugger. You can view details in [Debugger for mLua] (/docs?postId=1323{"target":"_blank"}).</span>

#### Activating VS Code Link
1. You can activate the VS Code link from **File - Setting - Create - LocalWorkspace**.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1753841640003ea80d3ce3b164791a4229e6dadcd42bb.png{"width":"640px"} "16")
2. When the VS Code is linked, the VS Code's `Code.exe` path will be set automatically. If the path isn't set, then it can be set directly by using the **[Path Settings]** button.
![17](https://mod-file.dn.nexoncdn.co.kr/bbs/17538416870989b42bb936b2e4076a5a993e78a4af7ee.png{"width":"640px"} "17")

#### View from the Script Maker
Worlds that are linked with the VS Code will have all items changed to **read-only**, making it impossible to directly edit the script from the maker.
If you want to view scripts from the maker, you can do so by pressing **Context menu - Open In Script Editor (Read Only)**.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17496310661647421ad7e3f2b40c799cbfbb98ca75a0d.png{"width":"450px"} "12")