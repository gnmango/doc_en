# GuideKeyComponent
Enables the Guide UI when the player enters the range of an entity with this component.

#### Property

| Execution Space Control | Type | Name | Description |
| ---- | --- | --- | --- |
| None | string | GuideText | The **Key** value of the text to appear in the Guide UI. You need to change the property attached to the entity. This property is automatically translated, so you should write it as a Key value. If necessary, add keys and data values to **LocalizationTable**. |
| None | Entity | GuideEntity | The Guide UI entity. It is specified automatically by the OnBeginPlay(). |

#### Function

| Execution Space Control | Type | Name | Description |
| ---- | --- | --- | --- |
| client only | void | OnBeginPlay | Specifies the self.GuideEntity. It puts the translation results into the Guide UI. |

#### Entity Event Handler

| Execution Space Control | Event Relayer | Name | Description |
| ---- | --- | --- | --- |
| client only | self |InteractionEnterEvent | Enables the Guide UI when the player entity approaches an entity that contains this component. |
| client only | self |InteractionLeaveEvent | Enables the Guide UI when the player entity leaves the vicinity of an entity that contains this component. |
| client only | self |InteractionEvent | Disables the Guide UI when an interaction is started. |

# MiningTargetObject

#### Property

| Execution Space Control | Type | Name | Description |
| ---- | --- | --- | --- |
| Sync | number | Level | The level of the mineral vein or key chest. You need to change the value in the Property Editor window.<br> <ul><li>Mineral Vein: Set the same as the number of the mine.</li> <li>Key Chest: Set the same as the town number of the mine.</li><ul>  |
| Sync | boolean | IskeyBox | Property that checks whether the entity is a key chest. The value is true if it is a key chest. It changes the property value directly to use. |

#### Function

| Execution Space Control | Type | Name | Description |
| ---- | --- | --- | --- |
| client only | void | OnBeginPlay | Refreshes the name of NameTagComponent. It appears as **Lv.? Mineral Vein or Key Chest**. |
| server | void | GiveReward | Function that gives a reward for successful mining.<br>Finishes the function if normalSuccess is false, regardless of whether the bonus game was successful or not. If the mineral vein was mined, it will give a reward that matches the self.Level. If a key chest is mined, changes Playerdata.TownAdmissions to self.Level + 1.<ul><li>normalSuccess: The default success. If true, it is a success.</li><li>bonusSuccess: The success of the bonus game. If true, it is a success.</li><li>player: The player entity that will receive the reward.</li></ul> |
| client | void | SetKeyBoxResultUI | Function called by the self.GiveReward(). If the player acquired a key chest, enables the **Nth Town Unlocked** pop-up. |

#### Entity Event Handler

| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- |  --- |
| client only | self| InteractionEvent2 | Executes the mining game when an entity with this component interacts with the player entity. |
| - | InputService  | KeyDownEvent | Press the Esc key to close the **Nth Town Unlocked** pop-up. |

# NPCInteractionComponent
Component that enables the UI by interacting with the NPC.
#### Property

| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| None | Entity | TargetUI | The UI entity to enable. You should change the property in the Property Editor window. |
| None | string | InteractionSoundRUID | The music source RUID to play when enabled. If there are no sounds to play, leave this blank. You should change the property in the Property Editor window.|
#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client only | void | OnBeginPlay | The overridden function.|
#### Entity Event Handler

| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| client only | self | InteractionEvent2 | Enables UI when an entity with this component interacts with the player entity.|

# Portal_MineToMine
Component added to the portal from mine to mine.

#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client only | void | OnBeginPlay | The overridden function. Displays the name of the mine the player moved to and the recommended mining power. Loads the information from the MapInfo dataset.|
#### Entity Event Handler
| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| client only |self | InteractionEvent2 | Executes the mine movement when an entity with this component interacts with the player entity. | 

# Portal_MineToTown
Component added to the returning robot that moves from mine to town.

#### Entity Event Handler
| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| client only |self | InteractionEvent2 | Executed when an entity with this component interacts with the player entity. Finishes the exploration and enables the End Exploration UI. | 

# Portal_TownToMine
Component added to the portal from town to mine.

#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client only | void | OnBeginPlay | The overridden function. Displays the mine name and recommended mining power in the portal from town to mine.  |

#### Entity Event Handler
| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| client only | self | InteractionEvent2 | Executed when an entity with this component interacts with the player entity. Enables the Mine Loading UI and lets the player enter the mine. | 

# Portal_TownToTown
Component added to the portal from town to town.
#### Property
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| Sync | number | DestinationTownIdx | Index of the destination town. You need to enter the value directly to the portal in the Property Editor window.|
| None | number | UseCooldown | The cooldown time before the portal can be used again. Returns if the UseCooldown7` value is 0 or more in the Interaction Event Handler of the Portal_TownToTown component.|

#### Function

| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| client only  | void | OnBeginPlay | Displays the name of the destination town in the portal from town to town. Enables/disables child entities based on whether the player can move to the portal.  |

#### Entity Event Handler
| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| client only | self |InteractionEvent2 | Executed when an entity with this component interacts with the player entity. Moves the player to town if the player has permission to enter. Displays a Locked message if the player doesn't have permission to enter.| 
| client only | localplayer | TownAdmissionUpdated | Executed when PlayerData.TownAdmission, the **LocalPlayer**'s town travel permission, is updated. If the player has permission to enter the town, the lock image in the portal will be disabled.| 

# ToolBoxObject
Component attached to the exploration tool box inside the mine. When the player interacts with a tool box, the player acquires an exploration tool.
#### Property

| Execution Space Control | Type  | Name | Description |
| --- | --- | --- | --- |
| None | number |reservatedTimerId | Property to prepare the situation where an exploration tool is acquired multiple times over a short period of time. Records the reserved **TimerId**.  |
#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client | void | SetEnableToolBoxToastUI | Function that refreshes and enables the Acquisition Notification Pop-up UI that will be displayed when acquiring an exploration tool. Uses TimerService to close the Pop-up UI after 4.5 seconds and records the **id** in the self.reservatedTimerId. If there is already a recorded self.reservatedTimerId, removes the reservation. |

#### Entity Event Handler
| Execution Space Control | Event Relayer | Name | Description |
| --- | --- | --- | --- |
| client only | self | InteractionEnterEvent2 | Disables the Guide UI when the player entity approaches an exploration tool box.  |
| client only | self | InteractionEnterLeaveEvent2 | Enables the Guide UI when the player entity moves away from an exploration tool box. |
| client only | self | InteractionEvent2 | Executed when the exploration tool box interacts with the player entity. Treats for the player to randomly acquire one of the exploration tools from 1 to 4. Disables the exploration tool box as well. |

# Tool_Trampoline
Component attached to the **Trampoline** among the exploration tools. When interacting, uses the trampoline to jump high in the Y-axis.
#### Property

| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| None | number | Cooldown | The cooldown time before the trampoline can be used again. |
#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| client only | void | OnUpdate | Function that reduces the remaining cooldown time by delta.|
| client | void | UseTool | Function that applies the effect of using a trampoline. Makes the player jump towards the Y-axis.  |
#### Entity Event Handler
| Execution Space Control | Event relayer | Name | Description |
| --- | --- | --- | --- |
| client only | self | InteractionEvent2 | Executed when the trampoline and the player entity interact. Calls the UseTool(). |
