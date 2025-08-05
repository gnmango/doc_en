# Course Introduction
 Once you have created the World and started the test play, event functions will be called according to their life cycle. In contrast, [EditorService](/apiReference/Services/EditorService{"target":"_self"}) supports the use of scripts in the Maker. Creators can use **EditorService** to create the crafting tools they need.

# Try the editor service
Let's leave the log using EditorService.
1. Select **MyDesk- Create Scripts - Create Component** to add a script component, and name it <span style="color: #dc9656">**HelloEditor**</span>.
2. Add **EnterEditorEvent,  EnterPlayEvent** to Event Handler and write as below. 

    ```lua
    Event Handler:
    [service: EditorService]
    HandleEnterEditorEvent(EnterEditorEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: EditorService
        -- Space: Editor
        ---------------------------------------------------------

        -- Parameters
        ---------------------------------------------------------
        log("Enter Editor Event")
    }

    [service: EditorService]
    HandleEnterPlayEvent(EnterPlayEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: EditorService
        -- Space: Editor
        ---------------------------------------------------------
        
        -- Parameters
        ---------------------------------------------------------
        log("Enter Play Event")
    }
    ```

3. Add the HelloEditor component to **Hierarchy - World - Common**.
![03](https://mod-file.dn.nexoncdn.co.kr/bbs/16456799439142a316dfd3f4d423faf840a94b8681bc1.png{"width":"450px"} "03")

4. Click Start![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play") to call **EnterPlayEvent** and see if the log is printed.
![04](https://mod-file.dn.nexoncdn.co.kr/bbs/1687841635285b77ef2ece1e04d24808a25fdba02c8c4.png "04")

5. Quit playing and return to the UI Editor. Upon returning to the editor, **EnterEditorEvent** is called. **EnterEditorEvent** is also called during the first editor entry in out-of-game.
![05](https://mod-file.dn.nexoncdn.co.kr/bbs/168784164997278c571c6f54c49a282600c1412e2790d.png "05")


# Using editor-type UI
You can compose an editor-type UI by changing **GroupType** of UIGroupComponent.
The editor-type UI can be only viewed and operated while playing or editing the World. Let's check it with a simple example.
![06](https://mod-file.dn.nexoncdn.co.kr/bbs/16560458866694a2f964d7cbb44a1b4e966596d010277.png{"width":"700px"} "06")


1. Open the UI editor.
    ![07](https://mod-file.dn.nexoncdn.co.kr/bbs/16611491146220ecdd23bd75c4e0b893476b109727246.png "07")
    <br>
2. Add a new UIGroup.
    ![08](https://mod-file.dn.nexoncdn.co.kr/bbs/16560459550233fba6f0be20f40b8813dd49b8b984959.png "08")
    <br>
3. Select the added UIGroup.
    ![09](https://mod-file.dn.nexoncdn.co.kr/bbs/16560460059060c0a533e60744a3a94c3be9280876c60.png "09")
    <br>
4. Go to Property Editor and change GroupType of UIGroupComponent to <span style="color: #dc9656">**EditorType**</span>.
    ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1645680750596d7f27d2366374420b79ae4dfd6ef6d7e.png "10")
    <br>
5. Let's add a new button. Click the button in the upper left menu to add it and change the button name to <span style="color: #dc9656">**"Btn_EditorTest"**</span>.
    ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1656046223542857a7fa8661b424a8fb5bbd89e60eb72.png "11")

6. Click the **[Editor UI]** button on the right top of the scene screen. The added button is visible even when not in play state.
    ![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1656046080229263374149c6541d9b49665418b2943ec.png "13")
    The button can be clicked, so when it receives and processes the click event, you can make the button work as intended.

7. Open the **HelloEditor** component that was added earlier.
8. Add **ButtonClickEditorEvent** from Event Handler.
9. Set the target of that event to the **Btn_EditorTest** entity added earlier.
10. After adding the log in the form below, close the script editor.
    ```lua
    [entity] Btn_EditorTest(/ui/UIGroup/Btn_EditorTest)
    HandleButtonClickEditorEvent(ButtonClickEditorEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Editor
        ---------------------------------------------------------
        
        -- Parameters
        local Entity = event.Entity
        ---------------------------------------------------------
        log("Button Click")
    }
    ```

11. Let's click the placed button. You can see that the added log is normally printed.
![15](https://mod-file.dn.nexoncdn.co.kr/bbs/168784179772119cec289725b4252b43efb1499f22b93.png "15")
# Placement of Properties and Entities Using the Editor UI
You can use the script to access other components in editor state and modify their properties. In this case, property values are stored in a different way than in the World play.
During the World play, properties will revert to their original values set when the World was created when the user exits play and returns to the Maker. However, for the Editor UI, the changed values are saved. Most of the APIs provided by MapleStory Worlds can also be used in the editor environment.

#### Change the World Background in the Editor
The following is an example of changing the background of the World when clicking a button in editor state. If you save the changed World background, the World will keep the changed background.
```lua
Event Handler:
[self]
ButtonClickEditorEvent(ButtonClickEvent event)
{
    self.IsButtonClicked = not self.IsButtonClicked
    
    local backGroundRuid = "000000"
    local backGroundComp = _EntityService:GetEntityByPath("/maps/map01/Background").BackgroundComponent
    
    if self.IsButtonClicked then
        backGroundRuid = "000001"
    end

    backGroundComp:ChangeBackgroundByTemplateRUID(backGroundRuid)
}
``` 


#### Spawn an Entity in the Editor
You can also spawn and place entities on the editor when clicking a button. The following example spawns an object when clicking a button in editor state.

```lua
Event Handler:
[self]
HandleButtonClickEvent(ButtonClickEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: ButtonComponent
    -- Space: Client
    ---------------------------------------------------------

    -- Parameters
    -- local Entity = event.Entity
    ---------------------------------------------------------
    
    local spawnModelId = "000000"
    local spawnPosition = Vector2(0,0)
     
    _EditorService:SetSelectedModel(spawnModelId)
    _EditorService:CreateSelectedModel(spawnPosition)
}
```

# Event of Editor Services
The editor service does not use life cycles such as `OnBeginPlay()` or `OnUpdate()` because it does not play the World. Create scripts based on editor events. Detailed information about the available functions and events of the editor service can be found on the API reference site.
The most representative events of the editor service are **WorldLoadEditorEvent** and **EnterEditorEvent**.
 
 * **WorldLoadEditorEvent**: Executed once during the process of loading the World after running the editor. So it is a good event for initialization.
* **EnterEditorEvent**: Called when the World is loaded for the first time after launching the editor and at the end of a test play. Note that it is called repeatedly.

Some common types of events are as follows.

| Type | Event |
| --- | --- |
| Events called at specific points in editor usage	 |EnterEditorEvent<br>WorldLoadEditorEvent<br>EnterPlayEvent  |
| Events for selecting an Entity | EntitySelectEditorEvent<br>EntityDeselectEditorEvent |
| Events for Creating and Deleting an Entity	 | EntityCreateEditorEvent<br>EntityDeleteEditorEvent |
| Events to check the location information clicked with the mouse | ScreenTouchEditorEvent<br>ScreenTouchHoldEditorEvent<br>ScreenTouchReleaseEditorEvent |
| Events to detect the button state | ButtonClickEditorEvent<br>ButtonStateChangeEditorEvent |
| An event to detect the state of a text input field. |TextInputValueChangeEditorEvent<br>TextInputEndEditEditorEvent  |

# Saving the Edited Information
You can save the changed value by placing an entity using **SpawnService** or editing the property's value. Changing and editing a property's value gives you the ability to alter a lot of information, but it may not be enough to store complex information.

**EditorService** provides the ability to access data and read and write values. Let's say you have organized your data as follows:

![16](https://mod-file.dn.nexoncdn.co.kr/bbs/16456812592398638debc633f4b4383d1001170926f89.png{"width":"550px"} "16")

To get the value of the col3 column of dash, you can write as follows.
```lua
local data = _DataService:GetTable("CustomEditorData")
local rowCount = data:GetRowCount()
 
for i=1, rowCount do
    local name = data:GetCell(i, "name")
    if name == "dash"then
        local value = data:GetCell(i, "col3")
        log("find value : "..value)
        break
    end
end
```

It can be used in the same way as the data service, but the way you set the data is different. To set the data, you must use the API of the EditorService.
The editor service supports `DataSetInsertRow()`, `DataSetRemoveRow()`, and `DataSetSetCell()` functions for this purpose.

Below is an example of adding data to the 4th line and setting related data.

```lua
local data = _DataService:GetTable("CustomEditorData")
local rowCount = data:GetRowCount()
 
_EditorService:DataSetInsertRow("CustomEditorData")
 
_EditorService:DataSetSetCell("CustomEditorData", rowCount + 1, "name", "skill1")
_EditorService:DataSetSetCell("CustomEditorData", rowCount + 1, "col1", "10")
_EditorService:DataSetSetCell("CustomEditorData", rowCount + 1, "col2", "20")
_EditorService:DataSetSetCell("CustomEditorData", rowCount + 1, "col3", "30")
```

If you want to change the col2 column value of dash to 40, write the script as follows.
```lua
local data = _DataService:GetTable("CustomEditorData")
local rowCount = data:GetRowCount()
 
for i=1, rowCount do
    local name = data:GetCell(i, "name")
    if name == "dash"then
        local value = data:GetCell(i, "col2")
        _EditorService:DataSetSetCell("CustomEditorData", i, "col2", "40")
        break
    end
end
```