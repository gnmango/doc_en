# Course Introduction
We'll learn how to debug a script using the debugging function.
# Breakpoints
Script debugging begins by setting breakpoints. Let's see how to set or clear breakpoints for a code line that may malfunction.
#### Setting Breakpoints
To debug from the Script Editor, you first have to set breakpoints.
You can set breakpoints by pressing <span style="color: #dc9656">**F9**</span> from a code line.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/163705967832018b0bcb305064e1ab0588a93b33ded86.png "1")
<Select a code line, then press F9 to open a breakpoint ![break_point](https://mod-file.dn.nexoncdn.co.kr/bbs/163452037264550d6cc9a394d421686b324e622531625.png "break_point") on the left side.>
#### Clearing Breakpoints
Select the code line with a breakpoint and then press <span style="color: #dc9656"> **F9 again**</span> to clear the breakpoint.
If you clear a breakpoint, the game will not stop at that point when run in debug mode.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1637059714014a97f7af47e9e448687ce6993a387d639.png "2")
<br>
#### Managing Breakpoints
It may be difficult to check current breakpoints if you have set them in several lines and several scripts. Then you can use the **Breakpoints** panel to check all the breakpoints.
Click and open **Panels - Debug - Breakpoints** in the navigation menu.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16875077319895fd3c24546fb466abb6f59901a6fd11d.png "3")
<br>
The **Breakpoints** panel provides the script name, function name, and line information.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16370597948003bc1552ec1b4445ebb17f3c2e15f2989.png "4")

| List | Explanation |
| --- | --- |
| Entry | Script name (e.g. script component or script logic with breakpoint) |
| Method | Name of Method with breakpoint within Entry |
| Line | Position of line with breakpoint within Method |

In the **Breakpoints** panel, you can search for breakpoints, but also delete them or move to that location as well.

#### Moving to Breakpoints
Double-click the breakpoints list, or select **Move** from the context menu to move to the position with a breakpoint.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1637059803373a398dd6a85c944c8bb9223a41618be5d.png "5")
#### Deleting Breakpoints
You can delete a breakpoint by clicking **Remove** from the right-click menu of the breakpoint you'd like to delete.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/163705981310639ae31b71811480d9462f92fd1c649dc.png "6")
# Debug
When you're done setting breakpoints in your script, execute the world in debug mode to check the code.
#### Starting Debug
You can start debugging with the **[Debug Start/Stop]** button ![Tool\_Debug](https://mod-file.dn.nexoncdn.co.kr/bbs/163453034389022f3433c55154836a7b16c2f001ec1b2.png).
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/16353141412294342625aa5754de983403c8517115938.png "7")
<br>
When you start debugging, the game plays the same as when you start the game, but when the code with a breakpoint is executed, play stops and you can see the state of the code.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401357743c56f6df990bf4b47b085b20b7c6838b1.png "8")
<br>
#### Assessing Code
During debugging, you can check each variable or value assigned to a property.
You can learn more about assigned values by using the ![TabWatcher](https://mod-file.dn.nexoncdn.co.kr/bbs/1634524682293d425b6b49f234132a341a80cdc17a080.png "TabWatcher") **WatchExpress** tool. You can also simply **mouse over** a variable or property of the executed code line to check the **allocated value, execution space, and type** with **tooltip**.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/165640134890606dadbdca6f74d999f16111fbdc8599e.png "9")
(Ex: You can see the delta value passed as a parameter.)
#### Stopping Debug
You can stop debugging by clicking the **[Debug Start/Stop]** button or **[Start]** button.
![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1637059841169be829952b87b4af59e11572af9bd3e3f.png "10")
#### Running Code during Debugging
What should we do if there is no problem with the code in the breakpoint, or if we want to check the code below as well?
We can't set breakpoints to all code lines we'd like to check, and it would be inefficient to start/stop debugging to set and clear breakpoints each time.
When play has stopped at the breakpoint, you can execute the code that comes after the breakpoint. You can check the state or flow of the code with executing the code by unit through **procedure unit execution**, **code execution step by step**, etc. To execute codes by unit, you can use the following buttons next to the Start/Stop button after debugging has begun.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1637059903339201b8d7bc1c84a1c8aff5223d3546515.png "11")

| Button | Function | Shortcut | Explanation |
| --- | --- | --- | --- |
| ![12](https://mod-file.dn.nexoncdn.co.kr/bbs/163531428695222092623c5ee4bbdb6c765b0257765d4.png "12") | Move to next breakpoint | F5 | Run code from current line to next code line with a breakpoint. |
| ![13](https://mod-file.dn.nexoncdn.co.kr/bbs/16353142973766005f7a18f6241e0853fcdb4b117d794.png "13") | Execute by procedure | F10 | Execute the code line by line.<br>If there's a function in the focused line, move on to next line, assuming the function has been executed. |
| ![14](https://mod-file.dn.nexoncdn.co.kr/bbs/1635314349045496e747d90b649a7aa14b0fb1388f0af.png "14") | Execute code by stage | F11 | Executes code line by line, but if there's a function in focused line, enter the code to execute codes within the function line by line. |
| ![15](https://mod-file.dn.nexoncdn.co.kr/bbs/163531436027902cc8e8685d04d26a2a7d6659e0ed05d.png "15") | Exit procedure | Shift + F11 | Finish calling the function currently running, and move to the location where it was called. |

# Watch Expression
You can hover over a variable or property to check individual values, but it gets inconvenient to watch the constant change in certain variables on each line.
To avoid this, we can register a property or variable declared within the function to **Watch Expression**. By doing this, we can convienetly keep track of values each time the code is executed.
<br>
Click and open **Panels - Debug - Watch Expression** in the navigation menu.
![16](https://mod-file.dn.nexoncdn.co.kr/bbs/16875080353841809a26bb9494dbc9cc9bf775f4ff075.png "16")
<br>
During debugging, write the variable or property name to **Add Expression** to figure out its value. 

>*<span style="color: #7cafc2">*Tip**
> In the case of **variables declared in the currently focused function, the variable name** is registered, and in the case of **property, it is registered in the form of self.property name**.</span>

![17](https://mod-file.dn.nexoncdn.co.kr/bbs/163531441012044c1f499c222457c8428da2cff7ffeb4.png "17")
<br>
Once a variable or property is registered to **Watch Expression**, you can run codes line by line to track the change in values.

| Before executing code in function | After executing code in function |
| --- | --- |
| - Starts debugging, focuses on the first line of NewMethod3 function which has breakpoint.<br>- Receives "Debug12" as NewMethod3 function's parameter.<br>- Variable text has not been declared yet.<br>- Default value 0 is assigned to Property. | - Uses F11 to run code up to the 4th line.<br>- Declares variable text and assigns "Debug123".<br>- Adds 1 to Property value. |
| ![18](https://mod-file.dn.nexoncdn.co.kr/bbs/16370600155057ca13181b27649018ee5ba886944a710.png "18") | ![19](https://mod-file.dn.nexoncdn.co.kr/bbs/16370600271603294fb67b3024471b88820dda2b0b976.png "19") |
| ![20](https://mod-file.dn.nexoncdn.co.kr/bbs/163531445300862c1a06ecc354207b15e5206b731a731.png "20") | ![21](https://mod-file.dn.nexoncdn.co.kr/bbs/1635314465563af8446e05b504689aa99b9ee139f7e54.png "21") |

# Call Stack
Call Stack is used to check where a function currently being debugged was called from. Click and open **Panels - Debug - Call Stack** in the navigation menu.
![22](https://mod-file.dn.nexoncdn.co.kr/bbs/168750821078566a306d100c6405e8845e38ba39689a8.png "22")

Like the example below, if you're debugging the inside of `Method3` from a script that calls the `OnBeginPlay` function, `Method1`, `Method2`, and then `Method3`, you can use **Call Stack** to check the execution space and the sequence called up to `Method3`.
![23](https://mod-file.dn.nexoncdn.co.kr/bbs/16353144857503a4fa032527a41bd85f26876272b25a1.png "23")
<br>