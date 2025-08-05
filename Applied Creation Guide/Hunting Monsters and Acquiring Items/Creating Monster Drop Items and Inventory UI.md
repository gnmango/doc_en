# Course Introduction
> <span style="color: #585858">This guide follows the order below.
> Part 1. [Let's Spawn Some Monsters]
> **▶ Part 2.** [**Creating Monster Drop Items and Inventory UI**]
> Part 3. [Writing an Inventory Script]
> Part 4. [Writing an Inventory UI Script]
> Part 5. [Writing an Item Script]</span>

Continuing from last time, in this lesson let's define the items that will drop when monsters are killed in the item table.
We'll also add an inventory UI.

# Creating an Item Table
First, let's create an item. An item table in **DataSet** can create individual items.

1. In the context menu of **Workspace - MyDesk**, click **Create DataSet** and enter the name as <span style="color: #dc9656">**UserItemDataSet**</span>. 
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1659661729006e5721ddcf57b483b988411014b023136.png "1")

    > <span style="color: #585858">**Learn More**
    > If you use `_ItemService` in the script, you should create the name of the data set as <span style="color: #dc9656">**UserItemDataSet**</span>. If you do not use `_ItemService`, you can name it whatever you would like. 
    > In this guide, we will use `_DataStorageService` instead of `_ItemService`, but we will use **UserItemDataSet** for convenience.</span>
2. Open **UserItemDataSet** and set the following in the Data Editor.
    | ID | Name | IconRUID |
    | :---: | --- | --- |
    | 1 | RedPotion | 323920d2805d4ec1aa85eca9e3e1cab2 |  
    | 2 | BluePotion | 7e9b39b73f4945b1a8d77db6c99a6946 |
    | 3 | IronSword | 682ef6cc7aa64076820f7e1da4dd54f8 |
   ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16938261287833ee862abd59f4b74930a7f94ff6ad946.png{"width":"640px"} "1")

# Adding Inventory UI
Create an inventory UI for your items.
1. Click the ![ui](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_ui.png{"width":"16px"} "ui") button to open the UI Editor.
    <br>
2. From **Preset List**, click **Button collection**, **Inventory** to add UI. Move the position by dragging, so it doesn't overlap with the existing UI.
    ![22](https://mod-file.dn.nexoncdn.co.kr/bbs/1659959486996e63cae97be33487ab542d2e6aa257e1c.png "22")
    <br>
    If **InventoryBtnPath** in **Property Editor - UIInventory component** of the **Inventory** entity is empty, the inventory doesn't open when pressing the button. Click the right hamburger button (![Common_menu](https://mod-file.dn.nexoncdn.co.kr/bbs/163453706197553d527cb3ea34392bc2829f15976f3d8.png "Common_menu")) and click **Reset All Properties** to link the inventory window to open when pressing the button.
    ![22-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16373057873214e1d4994f8834292a9904f34fb279d3c.png "22-1")