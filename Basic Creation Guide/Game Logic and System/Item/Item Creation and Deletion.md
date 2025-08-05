# Course Introduction
In this course, we will learn how to create and delete items.
We'll be going through the following examples.

* Obtaining an item from the map creates a random item in the inventory.
* After obtaining an item, a new item will be created on the same spot after 5 seconds.
* Obtaining another item from the map deletes all items in the inventory.

# Creating an Item Table
Let's start by creating individual items.
Individual items can be defined in the item table. **DataSet** can create the item table.
Open the context menu from **Workspace - MyDesk** and click **Create DataSet**.
Set its name to <span style="color: #dc9656">**UserItemDataSet**</span>.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1659661729006e5721ddcf57b483b988411014b023136.png "1")
<br>
> <span style="color: #585858">**Learn more**
>Creating an item passes the defined data to the item table.
>If you use a different name for the item table, the data will not be passed over.
>As such, you must name the item table **UserItemDataSet**.

<br>
Double-click the created **UserItemDataSet** to open Data Editor.
First, add 2 columns and name them as follows.
![item_3copy](https://mod-file.dn.nexoncdn.co.kr/bbs/163470329201303a32b2e6abd46be91bb61406acce0cc.png "item_3copy")
<br>
> <span style="color: #585858">**Learn more**
In the item, **Name, IconRUID, and Description** are required data, so create them with the same names as above.</span>

<br>
Let's add some data to the items that are needed in-game.
In the example, we'll add the **MaxStackCount and EffectAmount** columns.
![item_4](https://mod-file.dn.nexoncdn.co.kr/bbs/163470336846867242a6390754c5694eaac3cd773da56.png "item_4")
<br>
Create an item under each column and write the data.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1656659510837b79b1579311e41a29c7cfdd31709bde7.png "5")
<br>
Add an image to be used as an item icon to **Workspace**.
![14](https://mod-file.dn.nexoncdn.co.kr/bbs/163470407132182780528c94e4a88b622e8d63bf3c3f9.png{"width":"60px"} "14")  ![15](https://mod-file.dn.nexoncdn.co.kr/bbs/16347040853985fca895297de463887ac192f38bcdca2.png{"width":"60px"} "15")  ![16](https://mod-file.dn.nexoncdn.co.kr/bbs/1634704096454abc93bc20fb14144bdc2ab9d9311482e.png{"width":"60px"} "16")  ![17](https://mod-file.dn.nexoncdn.co.kr/bbs/1634704110489ee2e5ad6aa8544abb51ffa5bff0398a2.png{"width":"60px"} "17")  ![18](https://mod-file.dn.nexoncdn.co.kr/bbs/1634704122717081685317e9a425d9355cdc45ce05ce6.png{"width":"60px"} "18")
<br>
Open the context menu from **Workspace - MyDesk** and select **Import From - Import Image** to add images.
![ImportImage](https://mod-file.dn.nexoncdn.co.kr/bbs/16878275447201b5a1f02e9c140018a1ed696ef1c8e69.png "ImportImage")
![19-1](https://mod-file.dn.nexoncdn.co.kr/bbs/165665937154925a357cbc58a4577b508c57c30a2c163.png "19-1")
<br>
Enter the image RUID into **IconRUID** in the item table.
The RUID of an image can be copied by clicking **Copy RUID** in the context menu of each image.
![copyruid](https://mod-file.dn.nexoncdn.co.kr/bbs/16878279122598c71d67dc23e4ea6981bf22394a22507.png "copyruid")
<br>
Paste the copied RUID into **IconRUID** in the item table.
![21](https://mod-file.dn.nexoncdn.co.kr/bbs/165665958375411fb5a6f0fd74ab09659dc62686d344d.png "21")

# Create ItemType
Open the context menu of **Workspace - MyDesk** and click **Create Scripts - Create ItemType** to create a new **ItemType**.
Set the name of the new **ItemType** to <span style="color: #dc9656">**Potion**</span>.
<br>
> <span style="color: #7cafc2">**Tip**
> You can choose to create an ItemType for each item, like the example on the left.
> However, generally, we recommended creating an ItemType for each type, like the example on the right.</span>
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/16357553281761f7ea579470447b79b837aeb3f0bc3fa.png "7")  ![7-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1635755340293013f41e698e14213b3b7f609fc657fa6.png "7-1")

Open **Potion**, the ItemType you created, and add the necessary properties for the potion type item.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/163470344820046b9ef55b9db485495f82429417850eb.png "8")
<br>
After adding the `OnCreate()` function, write the contents.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/16357553544880d1191f6ddc446598724d2fce9e3ef88.png "9")
<br>
```lua
void OnCreate()
{
    self.Name = self.ItemTableData:GetItem("Name")
    self.MaxStackCount = tonumber(self.ItemTableData:GetItem("MaxStackCount"))
    self.EffectAmount = tonumber(self.ItemTableData:GetItem("EffectAmount"))
    log("Item is Created!")
}
```
<br>
><span style="color: #585858">**Learn More**
> Creating an item calls OnCreate, and data from the item table will be passed to the item entity.
> The data passed over to the item entity can be accessed with self.ItemTableData:GetItem("column name").</span>

Duplicate the above ItemType to add three more ItemType as follows.
![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1687829481362e88637cb403f453e851344c7d782fd0c.png "13")
# Adding Inventory UI
This time, add an inventory UI to display the created items.
In the top menu, click the ![ui](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_ui.png{"width":"16px"} "ui") button to open the UI Editor.
In **Preset List**, add **[Inventory]**, **[Button collection]** UI.
![22](https://mod-file.dn.nexoncdn.co.kr/bbs/1659959486996e63cae97be33487ab542d2e6aa257e1c.png "22")
<br>
If **InventoryBtnPath** is empty in the **UIInventory** component of the **Inventory** entity, open the ![Common_menu](https://mod-file.dn.nexoncdn.co.kr/bbs/163453706197553d527cb3ea34392bc2829f15976f3d8.png "Common_menu") hamburger menu and select **Reset All Properties**. Then the normal path is set as below.
![22-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16373057873214e1d4994f8834292a9904f34fb279d3c.png "22-1")
# Placing Item Entity and Adding Function
Press the ![ui](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_ui.png{"width":"16px"} "ui") button to exit UI edit mode. Place tiles and item entities on the map as follows.
![23](https://mod-file.dn.nexoncdn.co.kr/bbs/1637756481320c2e0c4ab0011419b8c001eaa398df0b6.png "23")
<br>
Create a script component to add a new function to the placed item entity.
Name the new script component <span style="color: #dc9656">**GiftBox**</span>.
![24](https://mod-file.dn.nexoncdn.co.kr/bbs/1656659256401f67aae760e8b43a5b62fb01dcbc612e2.png "24")
<br>
Add **TriggerEnterEvent** to the **GiftBox** script.
![25](https://mod-file.dn.nexoncdn.co.kr/bbs/16564196659068e46274b583442cfa37526fc257c05e4.png "25")
<br>
Then write the content as below.
```lua
[self]
HandleTriggerEnterEvent(TriggerEnterEvent event)
{
    -- Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    --------------------------------------------------------
    --Takes inventory of the entity that collided with the item entity. Returns if there's no inventory.
    local inventory = TriggerBodyEntity.InventoryComponent
    if inventory == nil then return end
     
    --Changes names and type names of items in UserItemDataSet into Table.
    local itemNames = {{"RedPotion",Potion},
        {"BluePotion",Potion},
        {"IronSword",Weapon},
        {"IronShield",Armor},
        {"GreenShirts",Clothes}}
     
    --Selects a random item to obtain.
    local index = _UtilLogic:RandomIntegerRange(1,5)
    local getItemName = itemNames[index]
     
    --Creates an item and puts it in the inventory of the entity that collided.
    _ItemService:CreateItem(getItemName[2],getItemName[1],inventory)
      
    --When an item is created in the inventory of the collided entity, deletes the item and reserves it to re-appear in 5 seconds.
    local myEntityPath = self.Entity.Path
    _TimerService:SetTimerOnce(function()
            --Path enters the Path for an entity that has this component attached to it.
            local myEntity = _EntityService:GetEntityByPath(myEntityPath)
            myEntity:SetEnable(true)  
        end, 5)
      
    self.Entity:SetEnable(false)
}
```
<br>
Add the **GiftBox** component and **TriggerComponent** to the item entity you placed on the map.
![26](https://mod-file.dn.nexoncdn.co.kr/bbs/1659662714941efaa23b3166645ae97fec8b1f42f581c.png "26")
<br>
Let's press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test.
Check if the item is created in the inventory UI whenever the character touches the item entity.
![27](https://mod-file.dn.nexoncdn.co.kr/bbs/165665955361463219cb3037e43e6893464ccb78a94d4.png "27")

# Deleting Items
Place any entity from **Preset List**.
![28](https://mod-file.dn.nexoncdn.co.kr/bbs/1637756501388a259dddae19f4870bed5f7494303f2dc.png "28")
<br>
Then add a new script component named <span style="color: #dc9656">**ItemDelete**</span>.
![29](https://mod-file.dn.nexoncdn.co.kr/bbs/1637756514221ac0df63469bf4810bf3a6c4f7f31222a.png "29")
<br>
Open the **ItemDelete** script and add **TriggerEnterEvent**.
![30](https://mod-file.dn.nexoncdn.co.kr/bbs/1656419693462d04cb7497fde45bb81a3e3552331eaf5.png "30")
<br>
Then write the content as below.
```lua
[self]
HandleTriggerEnterEvent(TriggerEnterEvent event)
{
    --Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    --------------------------------------------------------
    local inventory = TriggerBodyEntity.InventoryComponent
    local items = inventory:GetItemList()
    
    for index, item in pairs(items) do
    	_ItemService:RemoveItem(item)
    end
}
```

<br>
Add the **TriggerComponent** and **ItemDelete** component to the placed entities above.
![32](https://mod-file.dn.nexoncdn.co.kr/bbs/16377565496814502cb717a044fa5be2239c99a623f8b.png "32")
<br>
Let's press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test.
Check if all the items are deleted from the inventory whenever the character touches the entity on the left.