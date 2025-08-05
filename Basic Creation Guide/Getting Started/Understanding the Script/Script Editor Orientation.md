# Course Introduction
MapleStory Worlds can create the component or logic that a creator wants to generate with a Script Editor. The script's grammar uses **Lua Script** and supports various kinds of **API**.
When creating a World, you may have to modify scripts provided by MapleStory Worlds or written by other creators. We recommend familiarizing yourself with the basic usage of the Script Editor to help modify the script.

In this course, we're going to simply learn how to output logs using scripts and look into various convenient functions of Script Editor. Below is an example of using scripts.
$$youtube
GxjOaYhSUgE
$$


# Outputting Log Using Scripts
First, let's use scripts to output a log: "Hello Maple World".

## Creating a Script Component
Create a script component.
1. In the Maker, go to the **Workspace** tab.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/165603680025521f2b8ff66cd4e838b0a5f5bf9899a0e.png "3")
2. Click **Workspace - MyDesk - Create Scripts - Create Component** to create a new component.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1687496291799b05a128af08748b8bc69f2956c9de929.png "4")
3. Enter <span style="color: #dc9656">**HelloMapleWorld**</span> as the name of the new component.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1635422307109859acba010394878982655abfaa83495.png "5")
4. Double-click the **HelloMapleWorld** script component. Now you can edit the **HelloMapleWorld** script in the Script Editor.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1669634831972d857555921b0402db47a71d9d780f633.png{"width":"330px"} "6")

## Composing Scripts
Clicking on the **[+]** button of **Function** outputs the list of functions. Select the `OnBeginPlay` function from the list. This function is called once in the beginning when the script component is executed.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/16696348590924e768b248a29402fa18c03f247ac65c7.png "7")
<br>
Press the **[+]** button next to `OnBeginPlay()` to expand the function.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1669634880878d85c73c4caf1455fa8eebf7ad14f55ed.png{"width":"330px"} "8")
<br>
Write the function as follows.
```lua
[server only]
void OnBeginPlay()
{
    log("Hello Maple World")
}
```

> <span style="color: #585858">**Learn more**
> The `log` function is the most basic and most frequently used function when writing scripts.
> The value passed as a parameter of the `log` function is output to the console window.
> You can check variables, property values, or return values of functions by outputting them on the console window.
> For more details, please refer to the [Log](/docs/?postId=719{"target":"_self"}) guide.</span>

## Adding a Script Component to an Entity
After you create a script component, you must add it to an entity for the script to run. Hence, you must add the **HelloMapleWorld** component to one of the entities placed in the map.
 Add **HelloMapleWorld** to **DefaultPlayer**.
1. Click **Workspace - DefaultPlayer**.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/16354223715596e876b1f28734af39f0f4d3bd20cc2b0.png "11")
<br>
2. Click the **[Add Component]** button in the property editor.
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/163693537008760bc035a3de447b6a17afb92232368f8.png "12")
<br>
3. Search <span style="color: #dc9656">**HelloMapleWorld**</span> in the component search bar.
![ScriptEditorOrientation_13](https://mod-file.dn.nexoncdn.co.kr/bbs/1663845838156754ed5aec49a4dce923c4b51d0ae0abd.png "ScriptEditorOrientation_13")
In the search results, click on the **HelloMapleWorld** component.
<br>
4. Click the **Scene** tab.
![14](https://mod-file.dn.nexoncdn.co.kr/bbs/1669634932558359b1be73ec8433db07ef18d74a24620.png "14")
<br>
5. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test.
![ScriptEditorOrientation_15](https://mod-file.dn.nexoncdn.co.kr/bbs/1634603292745976e230077b04ade8cffb42ffcd420e8.png "ScriptEditorOrientation_15")
<br>
6. The "Hello Maple World" log is output on the console window.
![17](https://mod-file.dn.nexoncdn.co.kr/bbs/16638461874055a74dc917cc34d6e8421c897dcb9dc2f.png "17")

# Script Assist
The Script Editor of MapleStory Worlds supports various functions to write code more accurately and quickly. For more details, please refer to the [Script Assist](/docs/?postId=952{"target":"_self"}) guide.

# Script outline
When writing a script, it often becomes long and complicated. You can use the outline function to understand the overall skeleton of the script or to organize the content. You can turn on the Script Outline feature to see the list of properties, functions, and events at a glance.
![outline](https://mod-file.dn.nexoncdn.co.kr/bbs/1669705795169a6a7fc44da794355bc50b44303020f31.png "outline")

You can easily change the order by dragging on the list.
![outline2](https://mod-file.dn.nexoncdn.co.kr/bbs/1669706219051b8e968a98a314f348605e83adc5055e2.gif "outline2")

# Script Editor Zoom In/Out
You can use the zoom in/out feature in the Script Editor. Open a script, press **Ctrl(L)** and scroll up with the mouse wheel to zoom in on the script screen, scroll down to zoom out.
![scale1](https://mod-file.dn.nexoncdn.co.kr/bbs/1689130659090b30a483d52904d449052ea8bd44696a2.gif)
<br>
You can select the screen magnification ratio on the bottom left of the Script Editor. You can also enter the number manually between the range of 20 to 400%.
![scale2](https://mod-file.dn.nexoncdn.co.kr/bbs/1689130753750642810ea2be447f387f7793ca44611ff.png)