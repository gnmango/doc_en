# Course Introduction
MapleStory Worlds provides various World configuration features in WorldConfig. The features of WorldConfig have an overall impact on the World you create.
##### Reference Guide
* [Execution Space Control](/docs/?postId=210{"target":"_self"})
* [Effective MSW 2](/docs/?postId=560{"target":"_self"})
* [Preparation for Packet Modulation](/docs?postId=1102{"target":"_blank"})
* [Server Verification Example](/docs?postId=1084{"target":"_self"})
# Introduce WorldConfig
WorldConfig can change various configurations for the World created by the creator. Press **Workspace - WorldConfig**, and the detailed settings will appear in the **Property Editor window**.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17114447139367ac91d0b1e984ea3bba1d2ee1e30882e.png "WorldConfig")

# LegacyAnimation
On March 23, 2023, the avatar movements and animations in MapleStory Worlds were changed to behave similarly to the original MapleStory. If the creator's World was created before the update date, enable LegacyAnimation to apply the avatar movements and animations from the previous MapleStory World. However, we do not recommend applying LegacyAnimation to the Worlds created after the update date, as the current avatar movements are similar to the original MapleStory.
The previous and current movements are as follows.
| true | false|
| --- | --- |
| ![legacy](https://mod-file.dn.nexoncdn.co.kr/bbs/16957010690636f2b8b04e7494501a3765817083d211d.gif{"width":"675px"} "legacy") | ![animation](https://mod-file.dn.nexoncdn.co.kr/bbs/1695701089240b13b2ad54232422582e9fd9f8b9c8e04.gif{"width":"640px"} "animation") |

# PlayerEntityAuthorityCheck
Calls the server functions of components belonging to the player entity only by the <span style=\"color: #dc9656\">**local client**</span>. Enabling this setting can increase the security of the World. For example, the `MoveToMapPosition()` function of a component belonging to DefaultPlayer whose execution space is server cannot be called by any client other than the local client. When you execute the function, an error message appears in the console window.

```lua
Method:
[client only]
void NewFunction ()
{
    local targetEntity = nil
    for key, value in pairs(_UserService.UserEntities) do
    	if key ~= _UserService.LocalPlayer.Name then
    		targetEntity = value
    		break
    	end
    end
    
    if targetEntity ~= nil then
    	targetEntity.PlayerComponent:MoveToMapPosition("map01", Vector2(0, 3))
    end
}
```

![PlayerEntity](https://mod-file.dn.nexoncdn.co.kr/bbs/169571933009917b0b6c1d9994fe9a9e71fa7ff2dacbd.png{"width":"860px"} "PlayerEntity")

# ServiceAuthorityCheck
Changes all Server functions to behave as <span style=\"color: #dc9656\">**ServerOnly functions**</span>. Enabling this setting can increase the security of the World. For example, if you run a function whose etion space is server in the client only function as shown below, an error message appears in the console window when it is executed.
```lua
Method:
[client only]
void OnMapEnter (Entity enteredMap)
{
    _TeleportService:TeleportToMapPosition(me, currentPosition + Vector3(0, 2, 0), currentMapName)
}
```

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/169570858853819399808907c4a219ad5a93aa6dcab19.png{"width":"640px"} "1")

# RestrictedPlayerEntitySync
Sets the **Sync** property of the native component belonging to the player entity to prevent the values changed on the client from being synchronized to the server. Enabling this setting can increase the security of the World. For example, if you type a message in `ChatBalloonComponent.Message` on the client, the chat balloon will not appear on other players' screens, since it is not synchronized to the server. 

```lua
[client only]
void ShowChatBalloon ()
{
    local chatBalloon = self.Entity.ChatBalloonComponent
    chatBalloon.ChatModeEnabled = false
    chatBalloon.AutoShowEnabled = true
    chatBalloon.Message = "MapleStory Worlds"
}
```

However, the following components are not affected by the RestrictedPlayerEntitySync. Note that if you belong to the player entity, synchronization from client to server is always performed.

 * MovementComponent
 * PlayerControllerComponent
 * StateComponent
 * TransformComponent

# SourceLanguage
If the player uses the [Automatic Translation](/docs/?postId=1072{\"target\":\"_self\"}) feature in the World, it uses the language of SourceLanguage as the translation target. Currently, you can use Korean and English as the original language. More languages may be available after future updates.


# LocalWorkspace
This allows you to download a creator's World data to the creator's desktop (Local). You can save various entries and data from the World as files on your desktop. For more information, please refer to the [LocalWorkspace](/docs/?postId=1165{\"target\":\"_blank\"}) guide.