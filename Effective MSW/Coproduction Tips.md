# Course Introduction
The coproduction feature is very convenient because multiple people can edit the World at the same time. However, when working together, errors sometimes occur. The MapleStory Worlds team is constantly working to minimize these errors and increase the reliability of the Maker. In this article, we would like to introduce some tips to keep in mind during the coproduction process.

##### Reference Guide
[Coproduction](/docs/?postId=670{"target":"_self"})

# Separation of development World and release World
After you've done the necessary operations in the development World, it is better to move it to the release World and release it.
Game production doesn't always go as successfully as the creators thought. It is only through continuous testing and iteration of bug fixes that you can make a game that works reliably. It can be even more inconvenient if the error occurs in a World coproduced by multiple people, as everyone else may have to stop building until the problem is resolved. As such, moving to the release World after making enough modifications in the development World can increase the stability of coproduction.

There are two ways to move a development World to a release World.

## mod File Import
The first way is to directly import the mod file.

1. In the development World, select **File - Export to File** and generate the mod file.
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16878479936495e1624fa89b04bc7bab4bedf7ce1549d.png "1")
2. In the release World, select **File - Import to File** and load the mod file created in step 1.
    ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16878482360306da1f21543e6403fb3242ca45575a9de.png "2")

## Transferring Individual World - Group World 
Currently, working on UI or maps in coproduction can cause unexpected errors to occur. For stable operations, it is recommended to move it to the coproduction World after processing it separately in the development World.
Here's how to do it.

1. Group members participate in coproduction work in their own personal World and save it.
2. On the [MapleStory Worlds Official Website](https://mod.nexon.com/{"target":"_self"}), select **Create Worlds - Group World**. In this step, press **Load Private World** and load the individual World saved in step 1.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1682303848272057910b381864310b96943a2b3ee5c06.png "3")
It is more reliable to "migrate the individual World to the group World" rather than "import the mod file directly". With this in mind, let's use it for coproduction.

However, since this operation method overwrites the development World file on the release World, there is an inconvenience of having to temporarily stop the work of other group members during this process. The MapleStory Worlds team is working hard to improve this inconvenience, so we ask for your understanding.

> <span style="color: #585858">**Learn more**
> You can move your individual works to the coproduction World using the Package function. Workspace's model, script component, and data set are included in the Package. Let's refer to the [Utilization of MSW Packages](/docs/?postId=647{"target":"_self"}) guide. </span>

# UI group separation
When working with UI, it is recommended to separate groups according to their intended use in advance. 
If someone Checks Out even the lowest Entity in the UI, the entire group is Locked so other group members cannot access it, and as such work efficiency may decrease. Also, it is useful to subdivide the group to ensure stability.
![ui](https://mod-file.dn.nexoncdn.co.kr/bbs/1660025984828228f10a19c6d4f54b8bee1272d70c009.png{"width":"200px"} "ui")
> <span style="color: #585858">**Learn more**
>For how to manage UI groups, refer to the [Utilizing and Controlling UI Editor](/docs/?postId=120{"target":"_self"}) guide. </span>


# Check In
If you leave the World without Checking In the entity you were working on in the coproduction World, the modifications are not reflected to the entire World. Therefore, it is recommended <span style="color: #dc9656">**to Check In from time to time during coproduction**</span>. In particular, when placing or modifying map entities, there is no separate save guide, so you must exit after Check In. Also, it is recommended to Check In from time to time when working on the UI.

If it is too much hassle to Check In individually, you can use the Bulk Check In function. 
* ![scene](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene.png "scene")In  Scene, press Ctrl + S.
* Or right-click ![workspace](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_workspace.png "workspace") Workspace - ![MyDesk](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_user_folder_no.png "MyDesk") My Desk and select <span style="color: #dc9656">**Bulk Check In**</span>.

# Other
## Update Check Out Information
If you go to the Instance Map while testing play in the Maker, it may happen that you don't see anything in the **Hierarchy** tab. In this case, close and reopen the **Hierarchy** tab for normal operation.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16619352961684eeaecab45624c77916199244e9ba2dd.png "4")
A similar phenomenon can occur in Workspace or UI. At this time, if the folder is folded and unfolded, the Check Out information is updated.

## Moving to a new World
If any of the following occurs during coproduction, there is a high probability that the World will have serious problems. 
* Game UI is visible in the script editor
* Even after UI modification and Check In, the work content is not reflected.

If the above occurs, it is recommended to suspend working immediately and resume work by Importing the World into a new World.

## Disconnection of Coproduction
If "The connection with the coproduction service has been lost. Please enter the Maker again." is displayed, the client must be completely shut down and reconnected for normal operation. If you only leave and enter the World, the same problem may repeat itself.

## Component Test
When testing a component that may have a bug, setting Component Enable using the creator's own Name or ppsn minimizes the impact on the work of other group members.
```lua
if _UserService.LocalPlayer.Name == "CreatorName" then
     TestComponent.Enable = true
end
```

## Initialize DataStorage Data
The <span style="color: #dc9656">**File > Setting > Create > Resetting data storage**</span> function initializes the current creator's own data just like the personal World. It does not initialize all data of other group members.
![data](https://mod-file.dn.nexoncdn.co.kr/bbs/1665140002110d148498678434a68ae7d8067a142b567.png "data")

## Ignoring errors and testing
If <span style="color: #dc9656">**File - Setting - Create - Script - Unplayable in case of syntax error**</span> is unchecked, other group members' script errors are ignored and the play test may be conducted.
![script](https://mod-file.dn.nexoncdn.co.kr/bbs/1665140132063959e3809b04b4b14981c77ea75f79b73.png "script")

> <span style="color: #7cafc2">**Cautions**
>Using this function constantly during coproduction can accumulate the potential risk. Ignoring errors hinders you from fixing the problems you must fix, resulting a serious bug after releasing the World.
>Moreover, other operations that relate to the errors ignored during the production process may seem like an error of another operation. It can delay the work.
>MapleStory Worlds recommends you find the cause of the errors and fix them rather than ignoring them. It is ideal to use it only temporarily for absolutely necessary reasons during coproduction.</span>

## Setting Map Name
The map name must be set in English.