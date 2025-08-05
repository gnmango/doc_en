# CurrencyInfo
Dataset used to add a goods type. To add goods other than gold, add a row.
Enter the Key value in the **Dest** column, and enter the translation result in the LocalizationTable.

| Name | Description |
| --- | --- |
| Name  | The name of the goods. You must enter the **key** value. |
| RUID | The RUID of the image representing the goods. |
| Desc | Enter a description for the goods. Hover the mouse over the goods in your inventory to show this description. You must enter the **key** value. |
# EquipmentInfo
Dataset where you enter the equipment information. To add equipment level 5 or higher, add a row.
Enter the **Key** value in the **Name, FlavorText** columns, and enter the translation result in the LocalizationTable.
| Name | Description |
| --- | --- |
| Cost | The price of the equipment. The same level of equipment costs the same price. You must enter only **number**. |
|Type1_Name  | The name of the pickaxe. You must enter the **key** value. |
|Type1_RUID  | Icon RUID of the pickaxe. Icon to display in the inventory or store. You must enter in the form of **Thumbnail://RUID**. |
| Type_Ability | The mining power of the pickaxe. You must enter only **number**. |
|Type1_FlavorText | The flavor text for the pickaxe. You must enter the **key** value. |
|Type2_Name  |The name of the working clothes. You must enter the **key** value.  |
|Type2_RUID  |Icon RUID of the working clothes. Icon to display in the inventory or store. You must enter in the form of **Thumbnail://RUID**. |
|Type2_Ability  |HP of the working clothes. You must enter only **number**. |
|Type2_FlavorText | The flavor text for the working clothes. You must enter the **key** value.|
| Type3_Name  | The name of the bag. You must enter the **key** value.  |
| Type3_RUID  | Icon RUID of the bag. Icon to display in the inventory or store. You must enter in the form of **Thumbnail://RUID**.  |
| Type3_Ability  | The number of bags that can be stored. You must enter only **number**. |
| Type3_FlavorText |The flavor text for the bag. You must enter the **key** value.|
| Type4_Name  | The name of the shoes. You must enter the **key** value.  |
| Type4_RUID  | Icon RUID of the shoes. Icon to display in the inventory or store. You must enter in the form of **Thumbnail://RUID**.  |
| Type4_Ability  | The MP value of the shoes. You must enter only **number**. |
| Type4_FlavorText |The flavor text of the shoes. You need to enter **key** value. |
# LocalizationTable
Table used in LocalizationTable. If you have a sentence that needs to be translated, add a row.

| Name | Description |
| --- | --- |
| Key | The key value. |
| Type | The categorization of the sentence. |
| Source | The place where the sentence is used. |
| ko | The result of Korean translation. |
| en | The result of English translation. |
# MapInfo
Table for map information. Add a row to add a new town, mine, etc. 
Enter the **Key** value in the **TownName, MineName** columns, and enter the translation result in the LocalizationTable.
| Name | Description |
| --- | --- |
|TownName| The name of the town. You must enter the **key** value.|  
|MineName| The name of the mine. You must enter the **key** value. |  
|RecPower_min1| The minimum recommended mining power for Floor 1 of the mine. It is best to set the mining power required for a mine at 1/4 HP of the mineral vein. You must enter only **number**.  |  
| RecPower_max1| The maximum recommended mining power for Floor 1 of the mine. It is best to set the maximum mining power for a mine at 1/4 HP of the next floor's mineral vein. You must enter only **number**. |  
| RecPower_min2 |The minimum recommended mining power for Floor 2 of the mine. It is best to set the mining power required for a mine at 1/4 HP of the mineral vein. You must enter only **number**. | 
| RecPower_max2 | The maximum recommended mining power for Floor 2 of the mine. It is best to set the maximum mining power for a mine at 1/4 HP of the next floor's mineral vein. You must enter only **number**.  | 
| RecPower_min3| The minimum recommended mining power for Floor 3 of the mine. It is best to set the mining power required for a mine at 1/4 HP of the mineral vein. You must enter only **number**. |  
| RecPower_max3 | The maximum recommended mining power for Floor 3 of the mine. It is best to set the maximum mining power for a mine at 1/4 HP of the next floor's mineral vein. You must enter only **number**. | 
| HPUseRateInMine| The HP consumed in the mine per second. Regardless of the number of floors, a mine costs the same amount of HPs per second.|  
|DefaultDepth1| The depth of Floor 1 of the mine. The depth of a mine is measured by adding up all the floors of the mine. Therefore, the depth of the Floor 1 must always be **0**.  |  
| DefaultDepth2 | The depth of Floor 2 of the mine. Enter the last depth of Floor 1 and the next number. You must enter only **number**. |  
| DefaultDepth3 | The depth of Floor 3 of the mine. Enter the last depth of Floor 2 and the next number. You must enter only **number**. | 
# MineralInfo
A dataset of mineral information. Add a row to add a new mineral.
Each row contains information about one mineral. The minerals you get as a base reward for mining are indices 1-3. 
Enter the Key value in the **Name, Type, Desc** columns, and enter the translation result in the LocalizationTable.
| Name | Description |
| --- | --- |
|Name  | The name of the mineral. You must enter the **key** value. |
| RUID  |Icon RUID of the mineral.  |
| Cost | The price of the mineral when sold to a shop. You must enter only **number**. |
| Type | The type of the mineral. You must enter the **key** value. |
| Desc | The flavor text for the mineral. You must enter the **key** value. |
# ToolInfo
Dataset for the exploration tool information. You can edit the tool information by changing its content. Each row contains information about one exploration tool.
Enter the Key value in the **Name, Desc** columns, and enter the translation result in the LocalizationTable.
| Name | Description |
| --- | --- |
| Name |The name of the exploration tool.  You must enter the **key** value.  |
| RUID | Icon RUID of the exploration tool. |
| Desc | The description of the exploration tool.  You must enter the **key** value. |
# VeinInfo
Dataset containing the mineral vein information. The Miner Simulator Remake World has the mineral vein data up to Level 3. If you need a higher level of mineral vein, add a row and enter the data.

| Name | Description |
| --- | --- |
|Level  | The level of the mineral vein.  |
| HP | The max HP of the mineral vein. You must enter only **number**. |
| BonusGameChance | The chance of a bonus game appearing when mining the mineral vein. You must enter only **real number**. e.g. 0.1 |
| Bonus_JewerlyChance | The chance to receive a reward for winning the bonus game. You must enter only **real number**. |
| Bonus_FossilChance | The chance to receive a fossil reward for winning the bonus game. You must enter only **real number**. |
|Bonus_GoldChance  |The chance to receive a gold reward for winning the bonus game. You must enter only **real number**.  |
|  Bonus_ChairChance | The chance to receive a chair reward for winning the bonus game. You must enter only **real number**. |
|Bonus_GoldAmount  | The standard value of the amount of the gold reward for winning the bonus game. It is provided randomly within +- 20%, so you must enter only **number**. |
| KeyBoxHP |The max HP of the mineral vein, which is a key chest. You must enter only **number**.  |