# Course Introduction
You may experience the game running slow in various circumstances when you are playing the game you made. In such a situation, identifying the bottleneck for the game processing speed overload and subsequently modifying it is called optimization. Optimization improves the overall performance of the game even if you only solve some issues. The Lua Profiler helps you complete script optimization work much more easily.
Today let's learn about what the <span style="color: #dc9656">**Lua Profiler**</span> is and look at the use method.

<span style="color: #585858">**Learn More**
> To use the Lua Profiler, you need the latest updated Windows 7 or higher operating system. You also need to install `.NET Core 3.1 Desktop Runtime` or a higher version. If you cannot execute the Lua Profiler, please install [.NET Core 3.1 Desktop Runtime](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-desktop-3.1.27-windows-x64-installer{"target":"_self"}) and try again.</span>

##### Reference Guide
[Lua Profiling Summary Report](docs/?postId=1046{"target":"_self"})

# About Lua Profiler
## Execute Lua Profiler

### Execute in the World where you are Creating
If you press <span style="color: #dc9656">**Window - Lua Profiler**</span> on Create Game screen, Lua Profiler executes.
![profiler1](https://mod-file.dn.nexoncdn.co.kr/bbs/16588152380315f27ceb0abf148f5952f65813e870f7f.png "profiler1")
<br>
If you press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") Start followed by the **[Recording]** button of the Lua Profiler while playing, the recording starts.
If you execute Lua Profiler while testing the play, recording starts immediately after the execution.
![ui](https://mod-file.dn.nexoncdn.co.kr/bbs/16599472883762d82c3ab3435482ab40812db3ef2c76f.gif "ui")

### Execute in the Released Game
Even when you play the released game, you can execute Lua Profiler. 
1. First, press the setting button on the upper right.
![setting](https://mod-file.dn.nexoncdn.co.kr/bbs/1659057882093e645caa2c4204a45828e93b96a599f60.png "setting")
<br>
2. When you select the Other Tab, turn on <span style="color: #dc9656">**Use Lua Profiler**</span>, and then press the <span style="color: #dc9656">**Execute Lua Profiler**</span> button, recording starts right away while executing Lua Profiler.
![setting2](https://mod-file.dn.nexoncdn.co.kr/bbs/16590740514746335e90b06064f4caa8fde29ce4af89e.png{"width":"750px"} "setting2")

## Look at Lua Profiler
Let us look at the UI of Lua Profiler specifically.
![profilerUI](https://mod-file.dn.nexoncdn.co.kr/bbs/1659947090864a710fa6bf2d64deb81df2b9d083d5477.png{"width":"800px"} "profilerUI")

### ① File
![file](https://mod-file.dn.nexoncdn.co.kr/bbs/168413955956620e175ed60fa4ea180d5c1dc01a99bbb.png "file")
* <span style="color: #dc9656">**Load / Save**</span>: You can save the data you collected with Profiling into XML or load it.
* <span style="color: #dc9656">**Export Report**</span>: You can export the [Lua Profiling Summary Report] to XML.
* <span style="color: #dc9656">**Options**</span>
    ![option](https://mod-file.dn.nexoncdn.co.kr/bbs/1658887084903492a0fe98e0f4f3381e0f12434ac2320.png "option")
    | List | Sub-list | Description |
    | :---: | :---: | --- |
    | Record Max Frames | - | If the records pile up a lot, the Lua Profile slows down. For this reason, it discards the previous data when exceeding the designated number of frames recorded. The default value is 1,000 frames, but if we assume 60 frames per second, approximately 16 seconds will be recorded. If you want to record much longer, you can change the number of frames. |
    |Record Timeline Max Seconds|-|It is the maximum time to save the data in the Timeline chart.|
    |@cols=1:@rows=4:Active Module| Execution Time | It is an execution time of Function or Scope. <br>Execution time is the most important part of the indicator, so you must measure it. |
    | Native scope | It measures the time of the Native area where the script has not been used. |
    | Memory | It is an Lua Memory Allocation Size that occurred during the Function or Scope execution. |
    | Network | This is Network Write that occurred during the Function or Scope execution. <br>It is mostly an execution control call, and sometimes the Network Write occurring from a different Thread is recorded. |

### ② Connect / Disconnect
* Lua Profiler connects to the game being played and operates by collecting data.
* By pressing the Disconnect button, the Lua Profiler's game connection ends.

### ③ Context
* Context refers to an execution flow from the Core.
* You can select from Contexts, such as Server, Client, Instance, and Editor.
* When you select Context, you can see the data in the ⑤ Chart field and the ⑨ Profiling field.

### ④ Select Graph
* You select what graph from **Elapsed Time, Network Send, Network Receive, Lua Memory** you want to indicate on the ⑤ chart field.
* If you check and record **Network** on **File > Option**, you can see the information of **Network Send, Network Receive**.
* If you check and record **Memory** on **File > Option**, you can see the information of **Lua Memory**.

### ⑤ Chart
* You can select between **FrameChart** and **TimelineChart**, but FrameChart is used most often.
* You can click the chart field to select a certain frame. 
* In the chart field, you can see the graph you have checked in ④. If you right-click the chart, you can see the next menu.
  ![chart](https://mod-file.dn.nexoncdn.co.kr/bbs/1658893776888960b3923990b449bb99c0ee2defb2c0f.png{"width":"750px"} "chart")
    * **Save Image**: It saves the current chart as a png image.
    * **Copy Image**: It copies the current chart image to the clipboard.
    * **Zoom to Fit Data**: It shows the whole chart as fitting the screen size. It is useful when you see the chart by zooming in and then want to see the whole chart again.
    * **Help**: It is a guide on how to control charts.
    * **Open in New Window**: It opens the current chart in a separate window.

### ⑥ Recording
* When you press the button, Profiling starts. When you press it once more, Profiling ends.

### ⑦ Clear
* It deletes the profiling data that has been recorded so far.

### ⑧ Frame Data
* You can select All to add up all frames or select separate frames to see the data.
  ![framedata](https://mod-file.dn.nexoncdn.co.kr/bbs/16588974397120d5a96e886b647eca63856d08516834a.png "framedata")

### ⑨ Profiling Result
* You can see the Profiling result.

| Item | Description |
| :---: | --- |
| Name | It is the name of the Function or Scope. <br>In Property, Get and Set are also handled as a function. |
| Category | It is divided largely into Lua and Native. <br>There are Function, Getter, Setter, and Scope for the sub-category. |
| Call Count | It refers to the number of calls. |
| Total Time (ms) | It is the total time of execution. |
| Total Self Time (ms) | It is the time taken from the function excluding the subordinate sample. |
| Average Time (ms) | It is the average time of execution. <br>It is the Total Time/Call Count value. |
| Peak Time (ms) | It is the longest execution time from the one-time call. |
| Peak Self Time (ms) | It is the longest function execution time from the one-time call. |
| Network Usage | It is the Network Size that occurred during the function execution time. <br>It generally takes the execution control function the most. |
| Lua Memory | It is an Lua memory allocation size that occurred during the Function execution. |
| Lua Memory (Self) | It is an Lua memory allocation size that occurred in the function. |

### ⑩ Log
* It indicates the Lua Profiler log.
* By pressing the Clear button, the previous logs are deleted.

# Data Analysis
Let us examine how we should proceed with Lua Profiling by analyzing the sample data.
![sample](https://mod-file.dn.nexoncdn.co.kr/bbs/165994423724064220755849b4970b212baf200482c9b.png "sample")
![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1") Select Server for Context ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") and select All for Frame Data. 
When the profiling data is collected, let us first check the items below.
* Frame's Total Self Time (![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg "3"))
* Function with high share of time (![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg "4"))
* Function with high Peak Time (![5](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg "5"))

Let's look specifically at the above ![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg "3"), ![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg "4"), ![5](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg "5").

## Frame's Total Self Time
If you look at the sample data, you know that the Total Self Time takes over 62% of the whole. The Self Time in Frame is the time taken from Native, excluding the script execution, so a specific analysis of Native Scope is required.
Let us check the <span style="color: #dc9656">**Native Scope**</span> on <span style="color: #dc9656">**File - Option**</span> of the Lua Profiler and then do profiling again.
![nativescope](https://mod-file.dn.nexoncdn.co.kr/bbs/16590581743509659d9f6757e4497ac32594f3d715eb9.png "nativescope")
<br>
If you look at the category of the data you collected again, you know that there was a specific analysis for Native Scope.
![nativescope2](https://mod-file.dn.nexoncdn.co.kr/bbs/1659944748212c0a5c7969ae24263842732d10d984ba7.png "nativescope2")
The Frame's Total Self Time has been reduced drastically. Instead, the Native Function, TriggerComponent's OnUpdate, takes a considerable share, over 18%. You know that optimization should be considered because the number of calls is high, and the Peak Time is also high.

## Function with High Share Time
The function with the highest share time is also a prioritized analyzing target.
![sample2](https://mod-file.dn.nexoncdn.co.kr/bbs/1659945181899df0e60b1ce264dc3ab43d41b59e3cfe3.png "sample2")
If you look at the ServerElapsedSeconds of MonsterController - OnUpdate function in this sample, you can see that the average execution time is very short, but there are a lot of calls, so the total execution time becomes long. If the number of calls is high, even for simple property access, the cost increases as much as that. If such a part is found when you profiled the produced game, you should consider script optimization.

## Function with High Peak Time
If there is a function with a high execution time, although the number of calls is low, you should review it. If you perform <span style="color: #dc9656">**Shift + Click**</span> on the chart field, the frame with the highest total execution time will be selected from the 10% range of surroundings. With this, you can understand the function that is causative in Peak Time and judge whether to modify it. 
You cannot say the function has a problem because the Peak has been observed once. There is a possibility that profiling noise occurred, so you should optimize if you performed profiling several times and Peak keeps happening from the same part.

# Extract Script Function Sample
If you found a script function that you think necessitates profiling result modification but the contents of the function are too long, you might need a detailed analysis. Let's look at a sample extraction in such a case.
```lua
void Function1(string arg1)
{
    profiler.beginscope("FirstLoop")
    for i = 1, 10000 do
    	local en = self.Entity
    end
    profiler.endscope()
    
    profiler.beginscope("SecondLoop")
    for i = 1, 10000 do
    	local en = self.Entity
    end
    profiler.endscope()
}
```

<br>
If you set `profiler.beginscope("name")` ~ `profiler.endscope()` for the function to analyze as above, you can extract a sample from the profiling data as shown below.
![sample3](https://mod-file.dn.nexoncdn.co.kr/bbs/1659945606718a0afa01deb2f43c7ba0905c583f6d2d5.png "sample3")