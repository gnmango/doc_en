# Course Introduction
Let's learn about the UI Editor and the UI Group, which are used to edit the UI.
##### Reference Guide
* [Controlling UI Entities]
* [Basic UI Components]
* [Creating UI]
# Introducing the UI Editor
With the UI Editor, you can create the Inventory UI or the Shop UI by configuring UI entities and combining them with the UI entities provided by MapleStory Worlds.
UI Editor is composed of the following.

![uieditor01](https://mod-file.dn.nexoncdn.co.kr/bbs/1671069935996bed5f94453e9487784be1efc342a3b10.png "uieditor01")

| Number | Name | Explanation |
| --- | --- | --- |
| ![NO01](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541272181b5c1a55fcf3d49b19734d25913c38583.jpg) | Preset List | You can use various UI Presets provided by the Maker. |
| ![NO\_02](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541300837cb541c2f44e046a79bb1901a885aa8ac.jpg) | UI Path Information | You can see the path information for the selected UI entity.<br>Choose the parent path to select the entity. |
| ![NO\_03](https://mod-file.dn.nexoncdn.co.kr/bbs/163454131465069e090278448490f965207e9a4a10348.jpg) | Canvas | This is where you place and edit UI entities.<br>What appears on canvas will be output to the game screen when the game is running. |
| ![NO\_04](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541326353d8628c1473944497bf376acb7a65ca45.jpg) | Basic Tools | You can place UI entities such as images and buttons. |
| ![NO\_05](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541338689678f574f21e54a6ca533737924124d7e.jpg) | UI Groups Panel | You can select and add/delete the UIGroup. |

#### Shifting to UI Editor
In the top menu, press the **[UI]** button to convert to UI Editor. The **Scene** screen changes to the screen for the UI Editor.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17191966018196e67fe462b44418f8ffd50225816127c.png{"width":"440px"}   "2")
# UIGroup
UIGroup is composed of **UITransformComponent, UIGroupComponent, and CanvasGroupComponent**. Since the UIGroup acts like a folder, it is not affected by the orders in the Hierarchy and UI Groups. If you enable the UI Editor, you can manage the UIs you have created in the UI Groups panel window. If you add a new UI group, it will be reflected in both the Hierarchy and the UIGroup.

UIGroup provides three groups by default.
* **ToastGroup**: UI Group provided for the convenience of creators. It is connected to the UIToast logic. 
* **PopupGroup**: UI Group provided for the convenience of creators. It is connected to the UIPopup logic. 
* **DefaultGroup:** UI group that is always enabled. Chat window and essential buttons such as attack and jump are placed in DefaultGroup. Different groups can be set as DefaultGroup depending on the creator's intent.

#### UI Groups Panel
In the UI Groups panel, you can see a thumbnail of the UIGroup you are creating. You can also rename the UIGroup. Names changed in the UI Groups are also reflected in the Hierarchy. If you select a group, the UI Editor screen changes to the corresponding group for editing.

![112](https://mod-file.dn.nexoncdn.co.kr/bbs/171316272172268387b11bb884f6790635819a8af7021.png{"width":"240px"} "112")
#### Hierarchy - ui
In the Hierarchy, you can see the UI entities that belong to the UIGroup as parent-child relationships. You can edit components of UI entities or change the parent-child relationship of UI entities. 

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1713249479009dae50f8dd5ba4aa78e16fa684e742644.png{"width":"240px"}  "14")

#### Always Enabling UIGroup
You can set whether or not to display using the **DefaultShow** property of **UIGroupComponent**. If you set the DefaultShow property as **true**![Editbox_Check](https://mod-file.dn.nexoncdn.co.kr/bbs/16346176407708cb3de01eaaf48a68ab2dd6fe1b1183f.png \"Editbox_Check\"), the UIs belonging to that UIGroup will always be visible.

![uieditor17](https://mod-file.dn.nexoncdn.co.kr/bbs/1635153219911450a6374261549e985b5defd8c1ca4fc.png "uieditor17")

# UI Entity
There are two ways to create UI entities. You can use the default UI entities or use UI presets.

#### Basic UI Entities
Basic UI entities provide the most commonly used features when creating UIs. It is composed of **image, button, scroll view, text, and input text**. Creators can also combine multiple UI entities to create a new UI.  Please refer to the [Basic UI Components].

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17132470861160af59286c47f4cdfb9012301dfc3d0c1.png "4")

| Icon | UI Model Name | Function Explanation | Included Component |
| --- | -------- | ----- | -------- |
|![workspace_image](https://mod-file.dn.nexoncdn.co.kr/bbs/1634599004241e68d5e91df374710866c7f10599d1513.png "workspace_image") | Image | Outputs an image you want to UI. It is primarily used for displaying icons. | <ul><li>UITransformComponent</li><li>SpriteGUIRendererComponent</li></ul> |
| 	![UiButton](https://mod-file.dn.nexoncdn.co.kr/bbs/1634524924735d7c5b055209548e4b04c59abb701251d.png "UiButton") | Button | Performs a certain action or function upon clicking/touching. | <ul><li>UITransformComponent</li><li>SpriteGUIRendererComponent</li><li>ButtonComponent</li></ul> |
| ![Ui_ScrollRect](https://mod-file.dn.nexoncdn.co.kr/bbs/1634525192881d1b9b9f851554f81b74425ff23f0ef57.png "Ui_ScrollRect") | Scroll View | Sorts a lot of information into a list or a grid. E.g.) Inventory, shop | <ul><li>UITransformComponent</li><li>SpriteGUIRendererComponent</li><li>ScrollLayoutGroupComponent</li></ul> |
| ![Ui_Text](https://mod-file.dn.nexoncdn.co.kr/bbs/16345252157689279f2e1212f460381577fe950d87703.png "Ui_Text") | Text | Used to display text in the UI. | <ul><li>UITransformComponent</li><li>SpriteGUIRendererComponent</li><li>TextComponent</li></ul> |
| ![Ui_InputField](https://mod-file.dn.nexoncdn.co.kr/bbs/1634525171304460516d8fc894f9f96220ee226cbd934.png "Ui_InputField") | Input Text | Users can enter the text. e.g.) Search window, ID/PW window, etc. | <ul><li>UITransformComponent</li><li>SpriteGUIRendererComponent</li><li>TextComponent</li><li>TextInputComponent</li></ul> |

#### UI Preset
MapleStory Worlds provides UI presets for each feature. Presets contain UI and scripts together, so when you deploy a preset, the included components are added to the MyDesk. After placing a preset, you can modify UIs and scripts to match the World you are creating. 
![editor04](https://mod-file.dn.nexoncdn.co.kr/bbs/1659671324185d35bc031f0f44baa86ade48918ac3311.png{"width":"240px"}  "editor04")
