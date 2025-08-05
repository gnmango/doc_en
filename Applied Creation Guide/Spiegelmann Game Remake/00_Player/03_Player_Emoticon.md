# EmoticonData
Table that manages data related to emotes used by players. 
| Name | Description |
| --- | --- |
| Key | 	Key value that calls emotes, used by converting the value to KeyboardKey. |
| RUID |  RUID value of the emote.|
| Scale | 	Value to adjust images that are too large or too small, since different RUIDs have different size values. |
| PivotY | Value to adjust the y-axis position, since different RUIDs also have different pivots. |
| Explain | Reference value that describes the image. This value is not used for the World behavior.|

# EmoticonLogic
Logic that has emote-related functions used by the layer.

#### Property
| Synchronization | Type | Name | Description |
| --- | --- | --- | --- |
| None | SyncTable<string, boolean> |PlayerEmotionOn | Table that checks if an emote is currently in use. The 'key' value has the player's ID, and the 'value' has the true/false value to check if it is currently being used. |
#### Function
| Execution Space Control | Type | Name | Description |
| --- | --- | --- | --- |
| server | void | SpawnEmoticon | Function that creates the emote entities to float overhead. Returned if the current emote is already being played or if the player does not exist. |
| client | void | EmoticonClient | Function that displays emotes above the avatar's head. Returned without displaying an emote only when the player's in-game HP reaches 0. |