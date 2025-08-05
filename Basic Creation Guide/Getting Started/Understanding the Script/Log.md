# Course Introduction
Let's learn about logs and see how to use them effectively in the World production process.

# About Logs
(Log) refers to a record of various kinds of information or problematic situations during the World creation process. Logs allow you to verify that the World you are currently working on is working as intended. If something does not work as intended, you can use the logs to figure out what the cause is and troubleshoot the problem.
Log types are categorized by the agent that leaves the log. Let's categorize them as System Logs and Creator Logs.

## System Log
This refers to a log that identifies and informs the system of informational messages or errors that occur in the World being created.
System Log errors are cateogized like the following based on significance.

|Importance|Icon| Description |
|---| :--: | --- |
Low |![info](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_info_s.png "info")|  Various information that doesn't impact how the world functions. (Info)  |
Medium | ![caution](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_caution_s.png "caution")|  Warnings for methods not recommended by the maker or when there is a high chance of an error. (Warning) |
High |![warning](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_warning_s.png "warning")|  Errors that will cause problems if not resolved. (Error) |

## Creator Log
This is the log that the creator makes using the log function during the World production process. This can be used to verify that the World is working as the creator intended.
The log() function outputs the value that was passed as a parameter to the console window. Variables, property values, or return values of functions can be output on the console window for verification.
For example, let's leave a log, "Hello Maple World", when the script component is executed.
```lua
[server only]
void OnBeginPlay() 
{
    log("Hello Maple World")
}
```

After writing the foregoing, if you test by pressing **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play"), you can check the log on the console window.
![01](https://mod-file.dn.nexoncdn.co.kr/bbs/1653635780944b306b67d58ac48f68585d42061a8ae00.png "01")

### Types of Creator Log Functions
Creators can define and use three types of log functions as they see fit.

| Log Function | Description|
|---| :--: | 
| log() | The log's icon will display as information (Info)![info](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_info_s.png "info"). | 
| log_warning() | The log's icon will display as a warning (Warning) ![caution](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_caution_s.png "caution").| 
| log_error() | The log's icon will display as an error (Error)![warning](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_warning_s.png "warning").| 

```lua
[server only]
void OnBeginPlay()
{
    log("test_info")
    log_warning("test_warning")
    log_error("test_error")
}
```
After writing the foregoing, if you test by pressing **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play"), you can check the log on the console window.
![02](https://mod-file.dn.nexoncdn.co.kr/bbs/16546769028283859434d3a424558adaf5c5164899732.png "02")
> **<span style="color: #7cafc2">Tip</span>**
> <span style="color: #7cafc2">When creating a world, logs are made frequently to confirm that everything is working as intended. However, if too many logs are made during play testing, it may be easy to overlook crucial logs. We recommend creators organize logs while working on their worlds.</span>

# Check the Log
You can check the log on the console window of the maker. You can open the console window by clicking the <span style="color: #dc9656">**Maker's top menu - Panels - Console**</span>.
The console is divided into console and build console as shown below.

## Console
The console outputs logs of information, warnings, and errors that occurred during the playtest.
![console](https://mod-file.dn.nexoncdn.co.kr/bbs/1654137093143eb77525627fd4de5b7fc28d2c98e7d97.png "console")

| Numbers | Description |
| :---: | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1") | Deletes the logs remaining in the console window. <br>Click on the expand button on the right side of Clear to activate Clear on Play, ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") and click the Start button to automatically delete any logs remaining in the console window and start. |
| ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") | Click the button to merge duplicate logs into one. <br>![9](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_09.jpg "9") The total number of areas combined is displayed. |
| ![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg "3") | You can collect logs by execution space (Client / Server). |
| ![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg "4") | Search logs. |
| ![5](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg "5") | ![info](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_info_s.png "info") View the total number of info logs. <br> When this button is activated, ![8](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_08.jpg "8") this ![info](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_info_s.png "info") is where the information is visible, and it won't be visible when deactivated. |
| ![6](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_06.jpg "6") | ![caution](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_caution_s.png "caution") View the total number of warning logs. <br> When this button is activated, ![8](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_08.jpg "8") this ![caution](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_caution_s.png "caution") is where warnings are visible, and it won't be visible when deactivated.|
| ![7](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_07.jpg "7") | ![warning](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_warning_s.png "warning") View the total number of error logs. <br> When this button is activated, ![8](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_08.jpg "8") this ![warning](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_warning_s.png "warning") is where warnings are visible, and it won't be visible when deactivated. |
| ![8](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_08.jpg "8") | You can check the log's severity, recorded time, execution space, and details. |
| ![9](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_09.jpg "9") | You can see how many duplicate logs are combined. |
| ![10](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_10.jpg "10") | ![8](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_08.jpg "8") If you select a specific log, you can view detailed information about that log. <br>In the case of a script error, clicking the link moves to the corresponding line in the script. If it is an entity, the entity is selected in ![Hierarchy](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene_maker.png "Hierarchy") Hierarchy. |


## Build Console
Information, warnings, and error logs generated from scripts, logic, and data of the World being created are the output.
![buildconsole](https://mod-file.dn.nexoncdn.co.kr/bbs/16764466594148031d846b5b54c8c8c3d458c9cb97fee.png "buildconsole")

| Numbers | Description |
| :---: | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1") |You can view only the logs created by the current script by activating the Current scrip Only button. |
| ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") | Search logs. |
| ![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg "3") | ![info](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_info_s.png "info") View the total number of info logs. <br> When this button is activated, ![6](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_06.jpg "6") this ![info](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_info_s.png "info") is where the info is visible, and it won't be visible when deactivated. |
| ![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg "4") | ![caution](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_caution_s.png "caution") View the total number of warning logs. <br> When this button is activated, ![6](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_06.jpg "6") this ![caution](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_caution_s.png "caution") is where the warnings are visible, and it won't be visible when deactivated. |
| ![5](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg "5") | ![warning](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_warning_s.png "warning") View the total number of error logs. <br> When this button is activated, ![6](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_06.jpg "6") this ![warning](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_console_warning_s.png "warning") is where the errors are visible, and it won't be visible when deactivated. |
| ![6](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_06.jpg "6") | You can check the log type and contents.|
| ![7](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_07.jpg "7") | ![6](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_06.jpg "6") If you select a specific log, you can view detailed information about that log. <br>In the case of a script error, clicking the link moves to the corresponding line in the script. If it is an entity, the entity is selected in ![Hierarchy](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene_maker.png "Hierarchy") Hierarchy. |

# Released World Console
Creators can view logs by opening the console in a released world. The information, warnings, and error logs from the world's script, logic, data, etc, are output here.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1735021134960de6269099c084e1ebd3ebe4c18796a69.png "world console")

| Number  | Description |
| :---: | ---- |
|![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1")|Delete the logs remaining in the console window.|
|![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2")| The console can be folded and opened.|
|![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg "3")| Change the console to the minimized size. |
|![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg "4")| Saves the log as a txt file.<br> Save Path: C:\Users\User Name\AppData\LocalLow\nexon\MapleStory Worlds\ConsoleLogs\WorldID |
|![5](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg "5")| Adjusts the console window's transparency. |
|![6](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_06.jpg "6") | Delete the remaining logs and start log output by activating Clear on play. |
|![7](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_07.jpg "7")|Merges duplicate logs when the button is pressed. |
|![8](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_08.jpg "8")|View the total number of info, warning, and error logs. |
|![9](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_09.jpg "9")| Searches logs. |

# Use of Logs
There aren't many cases of everything working out as the creator intended in one try when writing scripts and conducting play tests. Creators can use logs with scripts appropriately to quickly identify any problems with a world to fix them. Let's use logs to reduce trial and error and create a more efficient script.

> <span style="color: #585858">**Learn More**
> Script errors can be resolved faster and more accurately by properly utilizing the debug capabilities with logs.
> Also refer to [Script Debugging](https://mod-developers.nexon.com/docs/?postId=202{"target":"_self"}). </span>