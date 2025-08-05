# Course Introduction
While MapleStory Worlds' Lua Profiler is a script-focused analysis tool, the Performance Profiler (hereafter Profiler) shows an overall analysis of factors that may be affecting the performance of the current World. With profiling, you can identify the factors that are slowing down your World's performance, and remove them to improve its performance.
In this course, we'll learn how to run and use the Profiler and how to interpret the results of the data analysis.

##### Reference Guide
[Utilize Lua Profiler]

# Execute Profiler
You can run the Profiler by entering **Panels - Profiler** in the Maker.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1689323157908f695d8d7daab4e6ca5b9a94148205fc1.png "1")

Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button to enter the play state and press the **[Analysis Record]** button on the Profiler to start recording. Press it again to stop recording.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1689323138212ef0142ece16f49dc939fcd1bb58e7b51.png "2")

> <span style="color: #585858">**Learn More**
> If you enter an instance map from a static map or exit an instance map to a static map while the profiler is recording, the recording is reset. </span>

# Look at Profiler
Let us look at the UI of Profiler specifically.
![ui](https://mod-file.dn.nexoncdn.co.kr/bbs/1689748450026758f03f4c54c42338295dfa5bfd4a04a.png "ui")


|	Major Classification	|	Numbers	|	Item	|	Description	|
| --- | --- | --- | --- |
|	@cols=1:@rows=9:①<br>Analysis Control Area	|	1-1	|	Clear	|	Resets previously recorded analysis results.	|
|	1-2	|	Analysis Record	|	Starts recording an analysis. Press again to stop recording.	|
|	1-3	|	The Oldest Timeline	|	Goes to the first timeline.	|
|	1-4	|	The Previous Timeline	|	Goes directly ahead of the selected timeline. You can also use the <br>`Left Arrow` key. 	|
|	1-5	|	The Next Timeline	|	Goes directly behind the selected timeline. You can also use the <br>`Right Arrow` key.	|
|	1-6	|	The Current Timeline	|	Goes to the final timeline.	|
|	1-7	|	Timeline Indexes	|	Shows the timeline index.	|
|	1-8	|	Profiler Config	|	![config](https://mod-file.dn.nexoncdn.co.kr/bbs/168956161350735b564735a634ee1b5fc873165db74c7.png "config")<br>The analysis chart area settings window. <ul> <li>X Axis Size: Sets the number of indexes visible on the X-axis. </li> <li>Fill Chart: Fills the graph.</li></ul>	|
|	1-9	|	Category	|	<ul> <li>Client: Shows the analysis chart for the client area. </li> <li>Server: Shows the analysis chart for the server area.</li></ul>	|
|	@cols=1:@rows=5:②<br>Analysis Chart Area	|	2-1	|	Analysis Item Information	|	Shows analysis items.	|
|	2-2	|	Chart	|	This is the area where the chart of each analysis item is recorded.	|
|	2-3	|	Select Timeline	|	<ul> <li>Left click to select the timeline you want. </li> <li>Right-click and move the mouse left and right to pan the graph.</li> <li>Click and hold the mouse wheel and move the mouse to the left to collapse the graph and to the right to expand it.</li></ul>	|
|	2-4	|	Information Tooltip	|	If you hover the mouse over a chart, a tooltip shows details about that timeline.	|
| 2-5 | Guideline | <ul> <li>The recommended values for stable performance are shown as a guideline (light blue line).</li> <li>Hover the mouse over a guideline to see details in a tooltip. </li></ul> |
|	@cols=1:@rows=3:③<br>Analysis Result Area	|	3-1	|	Analysis Details	| <ul> <li>Shows a detailed analysis result of the items selected in 2-1.</li> <li>Items to track in addition to fixed items can be recorded by checking the **Chase** checkbox.</li></ul> 	|
|	3-2	|	Search	|<ul> <li>Shows items that match your search query.</li> <li>English is not case-sensitive.</li> <li>Press the **[X]** button to clear all search terms entered.</li></ul>	|
|	3-3	|	Expand / Collapse Results	| You can expand or collapse all analysis results.		|

# Data Analysis
As users use PCs and mobile devices of varying specifications, the quality of the play experience can vary considerably even within the same World. However, you can understand the World's performance if the analysis values are roughly as follows.
The values below can be used as a rough guide, and you may experience an unnoticeable drop in performance before this.

### FPS (Client)
FPS is an element measured by the client, and is related to responsiveness and play screen. 
* It affects the play screen while playing. Display issues may occur if the FPS drops.
* It is recommended to maintain a minimum of 30 FPS.

| Item | Description |
| --- | --- |
| World FPS | <ul> <li>Shows the FPS of the World core that processes the creator's logic.</li> <li>FPS can drop for a variety of reasons, including inefficient logic, excessive sync properties, and calling functions in other execution spaces through execution space control.</li></ul>  |
| Engine FPS | <ul> <li>Shows the FPS of the engine, which is primarily responsible for computing input, networking, and graphics.</li> <li>Engine FPS can drop significantly if you have a lot of entities, or use excessive effects and particles.</li></ul> |

### Tick (Server)
Tick shows how many times the server is performing operations per second. It has a significant impact on the responsiveness of communication with the server.
* A lower server tick may feel similar to a higher ping (network latency).
* Server tick can be dropped due to inefficient logic in the World, excessive sync properties, use of different execution space and packet flooding caused by bad logic.
* For genres where responsiveness is important, such as in PvP or battle, it is recommended to maintain a minimum of 30 server ticks.
* For Worlds where real-time responsiveness is not critical, it is recommended to maintain a minimum of 10 server ticks.
* The higher the server ticks (up to 60 ticks), the more responsive the server.

<br>
> <span style="color: #585858">**Learn More**
>Packet Flodding occurs when too many packets (around 100,000 or more per second) are sent or received. During the test play on the client or the Maker, even smaller amounts of packets can impact performance.
> Packet flooding is often caused by calling a function in a different execution space per frame or tick.
> ![packet](https://mod-file.dn.nexoncdn.co.kr/bbs/1689732427563cd559f693ea145369ebb2c7a75cd1e0d.png "packet")
> The figure above outlines the packet forwarding when information from User1 is forwarded to User2 and User3, and User2 and User3's information is also forwarded to other users. The above situation is where a single user generates a single packet per frame or tick, but multiple calls in a single frame will increase the packet volume much more.
> Packet flooding is more likely to occur in multiplayer games than in single-player games.
> The following are common causes of packet flooding and how to resolve them.
> * When the <span style="color: #dc9656">**TargetUserId is not used**</span>.
>   * Reduce unnecessary function calls by calling functions only to specific users via TargetUserId.
>   * For more information, refer to "Sending responses to specific clients only" in the [Effective MSW 1] guide.
> * <span style="color: #dc9656">**When it is necessary to synchronize client values**</span>
>   * Sometimes you need to synchronize client values with other users through the server or by calling other space functions. 
>   * In this case, instead of synchronizing from the server, you can replace them with sync properties or collect values from different clients together in one tick and pass them with a single function call. Reducing the number of synchronizations helps to resolve packet flooding.</span>

### Memory
Shows the amount of memory used by the client.
It is recommended to avoid excessive memory usage. MapleStory Worlds may be forced to end if memory usage exceeds 1 GB, especially on mobile environments.

### Entity
Shows the total number of entities per map, per UI group, and the number of times entities have been created or destroyed. Frequent creation/destruction of entities may degrade the overall performance. This may vary depending on the specification of the device, but in mobile environments you may experience performance degradation if there are more than 5,000 entities. Therefore, it is recommended to keep the number of entities 5,000 or less.

| Item | Description |
| --- | --- |
| Total Entity | The number of all entities created in the current World.<ul> <li> Client <ul> <li>Only maps where the user entity exists are loaded.</ul><li>Server</li><ul> <li>When doing the test play in the Maker, maps on which the user entity moved at least once will be loaded and not destroyed again. </li><li>The physical server loads entities from all maps on the first run.</li></ul>|
| Construct / Destroy Entity | <ul> <li>Shows the number of entities created and destroyed.</li><li>If all entities in a map are moving, it is also counted even if no entities are actually created / destroyed. This will be improved later.</li><li>The number of creation/destruction of effects, particles, and damage skins can also be recorded, but they do not have a significant impact on performance because there is (Pooling).</li></ul> |

> **<span style="color: #7cafc2">Tip</span>**
> <span style="color: #7cafc2">Pooling means Object Pooling. It is a way to improve performance by creating objects in advance and reusing them.</span>

##### Improve Entity-Related Performance Degradation
If entities are degrading the World's performance, you can improve it in the following ways.

* **If There Are Too Many Entities**
If there are too many entities on the client side, this can affect not only the World FPS, but also the Engine FPS.
If too many entities are placed in the UI, consider disabling the UI group rather than individual UI entities. This may improve the performance of Engine FPS.
    | UI Entity Disable | UI Group Disable |
    | :---: | :---: |
    | ![ui01](https://mod-file.dn.nexoncdn.co.kr/bbs/16897413642185c6ae74a889e4a9e9a77f12573388e67.png "ui01") | ![ui02](https://mod-file.dn.nexoncdn.co.kr/bbs/168974137819127d563f222d94d33815194e818333c02.png "ui02") |
    If there are too many entities on the server side, this can affect Tick. Also, if a new server should be created in the released game, the initialization process can take a long time. The best way is to reduce the number of entities.
    <br>

* **If the Entities Are Created/Destroyed Too Many Times**
Excessive creation and destruction of entities may adversely affect the World performance. You can reduce the impact on performance with object pooling.

### Execute Space Count
Shows the number of times you have called a function in another execution space, e.g. when calling a server function from the client or calling a client function from the server.

| Item | Description |
| --- | --- |
| Send Count | The number of function calls sent in other execution spaces. |
| Receive Count | The number of function calls received in other execution spaces. |

##### Improve Performance Degradation Related to the Execute Space Count
Higher Execute Space Counts can have a negative impact on FPS or Tick. It should be modified to avoid excessively calling the functions that take too long. In particular, high Execute Space Counts can cause packet flooding, resulting in packet congestion at the server end.

### Execute Space Byte
Shows the amount of data when calling functions in different execution spaces. Normally, it increases proportionally to the Execute Space Count.
However, in some cases, the Count is low but Byte is high on the client. If the internet connection is unstable, the movement in the World may appear stuttering or choppy, even at high FPS. You can improve this by reducing the size of data delivered at once and the number of times it is delivered.

| Item | Description |
| --- | --- |
| Send Byte | The amount of data for the sending of function calls in other execution spaces. |
| Receive Byte | The amount of data for the receiving of function calls in other execution spaces. |

### Sync Property Count
The number of synchronizations of the Sync Property.

| Item | Description |
| --- | --- |
| Send Count | The number of sending of synchronizations per component and property. |
| Receive Count | The number of receiving of synchronizations per component and property. |

##### Improve Performance Degradation Related to the Sync Property Count
Higher Sync Property Counts can have a negative impact on FPS or Tick. Unlike the None-Sync Property, the Sync Property passes the synchronization logic internally. Using too many Sync Properties may degrade the World performance. Therefore, properties that do not require synchronization should be changed to None-Sync. Also, if it is not necessary to synchronize to every user, you can use TargetUserId of functions in another execution space to reduce the amount of synchronization.

### Sync Property Byte
The data amount for the synchronization of the Sync Property.

| Item | Description |
| --- | --- |
| Send Byte | The Byte of synchronization sending per component and property. |
| Receive Byte | The Byte of synchronization receiving per component and property. |

### TimerService
Information about the creation / destruction of timers using the Timer Service.

| Item | Description |
| --- | --- |
| Total Timer | The number of timers currently running or waiting. |
| Add Timer | <ul> <li>Shows how many timers have been created and where they were created.</li> <li>Effects that use the Effect Service are also counted because they use timers internally. However, the location of the call is not visible.</li></ul> |
| Remove Timer | The number of deleted timers. |

##### Improve TimerService-Related Performance Degradation
If the Total Timer increases and continues to accumulate, it can have a negative effect on FPS or Tick. Total Timer increases primarily when you pause a timer and don't remove it, or when you create a timer that repeats indefinitely and don't remove it. Therefore, you must either use the `TimerService:SetTimer()` function with the isRepeat set to true, or explicitly remove the timers created by the `TimerService:SetTimerRepeat()` function.
300 or more Total Timers can degrade World performance, so you need to avoid creating too many timers.

### Tween Logic
Information about Tweener creation/destruction related to Tween Logic.

| Item | Description |
| --- | --- |
| Total Tween | The number of the current Tweeners. |
| Add Tween | Shows the number of Tweeners created and where they were created. |
| Remove Tween | The number of deleted Tweeners. |

##### Improve Tween Logic-Related Performance Degradation
If the Total Tweener increases and continues to accumulate, it can have a negative effect on FPS or Tick. This happens when you set the value of the Tweener's **AutoDestroy** property to **false** and do not explicitly destroy it. Therefore, once the Tweener with **AutoDestroy** set to **false** is no longer in use, it must be reused or destroyed separately.
300 or more Total Tweens can cause World performance degradation, such as screen tearing or FPS drop, so you need to avoid creating too many Tweeners.