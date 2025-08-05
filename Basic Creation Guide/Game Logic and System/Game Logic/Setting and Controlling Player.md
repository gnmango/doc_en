# Course Introduction
Player models are useful when making the initial properties of characters, but they are also useful when moving characters or controlling their status. Let's look into the concept of PlayerComponent and PlayerControllerComponent, and the relationship between an action and state animation so that the creator can make the player move as intended.
# PlayerComponent Concept
The <span style="color: #dc9656">**PlayerComponent**</span> component is a collection of traits of players that differ from entities. The most common example is in PvP mode, where players can damage each other. The function of PlayerComponent is used to control players. You can let the player die, or move the player to another map or location. Player models without PlayerComponent will not be recognized as players. As it is only used in Players, you can also use PlayerComponent to identify Players. For instance, you can move the entity with PlayerComponent to a certain location, or put it in dead status under an additional condition.

#### Introducing Major Properties

| Property | Explanation  |
| --- | --- |
| MaxHp | Sets the Player's maximum HP. Default value is 1000.<br> The value is linked with HP in the UI status window model, so the change will be reflected in the UI model.  |
| PVPMode | If activated, players can damage each other. |
| RespawnDuration | Sets the time taken until respawn after player's death. Default value is 5 seconds.|
| Enable | Sets whether to activate or deactivate the component. |
| Nickname | Refers to nickname Player set in web. If Player model has NameTagComponent, the player's nickname will be shown.<br> It is not shown in the Property Editor. You can use it when composing scripts.  |
| RespawnPosition | Sets the location where player will respawn. If not specified, it will follow SpawnLocation or the map entrance location.<br> It is not shown in the Property Editor. You can use it when composing scripts. |
| RespawnTime |  Refers to time taken until player re-enters map. It is not shown in the Property Editor. You can use it when composing scripts.  |
| UserId | A unique identifier for the player. It can be used in the targetUserId parameter of the Client execution space control function. |
# Concept of PlayerControllerComponent
<span style="color: #dc9656">**PlayerControllerComponent**</span> executes actions and processes certain conditions according to input. For example, if you connect an action where a particular animation is played upon pressing the attack key, it will operate at every input of the attack key. However, when the player is climbing a ladder, the climbing animation will play. In this case, the attack animation should not be played. Therefore, when the player is on a ladder, you can add a condition: Pressing the attack key will not play the attack animation. 

The basic Player model has both PlayerControllerComponent and StateComponent. However, if you delete either of them, the model will not work properly. That's because PlayerControllerComponent is referring to StateComponent. In order for PlayerControllerComponent to work, Player model also needs StateComponent. 

><span style="color: #7cafc2">**Tip**
> PlayerControllerComponent processes input, action, and condition, while StateStringToAvatarActionComponent takes care of playing the animation.
> If the user presses a key to trigger Input, PlayerControllerComponent detects it and processes the Action after changing to a proper State.
> Then StateComponent processes the change of State and the action, triggering StateChangeEvent.
> Animation-related component like AvatarRendererComponent will receive this event to play the animation. </span>

#### Introducing Properties 

| Property | Explanation  |
| --- | --- |
| AlwaysMovingState | If activated, the moving animation will keep playing, even if character stays still. |
| FixedLookAt| If you change the value, the direction of the moving animation will not change, even if the character changes direction.<br> You can use it to make the character face one direction only. |
| LookDirectionX | If you change the value to a positive number, the default state is the character looking to the right. |
| Enable | Sets whether to activate/deactivate the component.|

#### Basic Controls - key, value 
These are default moving and action keys in MapleStory Worlds. You can use the keys as they are, or add more keys if necessary.

| key | value |
| --- | --- |
| W, UpArrow | MoveUp |
| A, DownArrow | MoveLeft  |
| S, LeftArrow| MoveDown |
| D, RightArrow | MoveRight |
| Space  | Jump | 
| LeftAlt | Jump2 | 
| LeftControl | Attack |
| C | Sit |

><span style="color: #7cafc2">**Tip**
> LeftAlt will be integrated to Jump. Please keep it in mind when creating.</span>

# Example
#### Moving to a Different Map after a Limited Time
You can use `MoveToMapPosition()` to move players to a particular position. You can use this function to move users to several maps so that they can play a new game in a new map.

1. In ![workspace_UserFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/1634600723422190820861d63456fa40e21031bd5ffa2.png "workspace_UserFolder") <span style="color: #dc9656">**MyDesk**</span>, create a new PlayerManagement component, and add it to ![workspace_MyAvatar](https://mod-file.dn.nexoncdn.co.kr/bbs/16346008379922a9b756c3912461db7195808cb554abd.png "workspace_MyAvatar") DefaultPlayer.
2. Let's use `TimerService` to count 10 seconds, and enter the name and location of the map to move to in 10 seconds.<br> The name of the map must be the same as the name in Hierarchy. 

><span style="color: #b8b8b8">**Learn more**
> `TeleportService:TeleportToEntityPath()` can move all Entities, but `PlayerComponent:MoveToMapPosition()` can only move players.</span>

```lua
Method:
[server only]
void OnBeginPlay()
{
    _TimerService:SetTimerOnce(function()
        for userId, userEntity in pairs(_UserService.UserEntities) do
            userEntity.PlayerComponent:MoveToMapPosition('map02',Vector2(2,2)) 
        end
    end, 10)
 }
```

![MoveToMapPosition](https://mod-file.dn.nexoncdn.co.kr/bbs/1656586279970675fa3b0db73418d92fccc5d03da1a6e.gif "MoveToMapPosition")
#### Respawning from Where the Player Died
Player's respawn position depends on SpawnLocation and map entrance location, but the creator can choose to respawn the player from where they died.

1. In ![workspace_UserFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/1634600723422190820861d63456fa40e21031bd5ffa2.png "workspace_UserFolder") <span style="color: #dc9656">**MyDesk**</span>, create a new PlayerSetting component, and add it to ![workspace_MyAvatar](https://mod-file.dn.nexoncdn.co.kr/bbs/16346008379922a9b756c3912461db7195808cb554abd.png "workspace_MyAvatar") DefaultPlayer.
2. We'll use `ConnectEvent` to get the location where the player died and set it as the respawn location.

```lua
Method:
[client only]
void OnBeginPlay()
{
    self.Entity:ConnectEvent(DeadEvent, function()
        self.Entity.PlayerComponent.RespawnPosition = self.Entity.TransformComponent.Position
    end)
}
```

![RespawnPosition](https://mod-file.dn.nexoncdn.co.kr/bbs/16565863334284c08832fec434f4789427b1b5a33d6ef.gif "RespawnPosition")
#### Moving the Player to Certain Location
Among numerous entities, you can identify an entity with PlayerComponent as a player and move the player to a certain location. Let's make sure the TriggerComponent included in certain entities can identify PlayerComponent. Only players can identify this entity, so entities in the same space, like monsters, cannot be moved. 

1. Place an entity that can collide with the player in the map, name it <span style="color: #dc9656">**MagicStone**</span>, and add TriggerComponent.
2. To designate a certain entity's path as the destination, add a portal object in the map.
3. In ![workspace_UserFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/1634600723422190820861d63456fa40e21031bd5ffa2.png "workspace_UserFolder") <span style="color: #dc9656">**MyDesk**</span>, add a new PlayerMangement component, and add it to ![workspace_MyAvatar](https://mod-file.dn.nexoncdn.co.kr/bbs/16346008379922a9b756c3912461db7195808cb554abd.png "workspace_MyAvatar") DefaultPlayer. 
4. If the entity that collided with a certain entity has PlayerComponent, the player will be moved to the <span style="color: #dc9656">**input path ('maps/map01/portal-3')**</span>.

```lua
Method:
[server only]
void OnBeginPlay()
{
     local MagicStone = _EntityService:GetEntityByPath('/maps/map01/MagicStone') -- Enter the path of the entity that will collide with player. 
     MagicStone:ConnectEvent(TriggerEnterEvent, function(event)
        if event.TriggerBodyEntity.PlayerComponent then
            event.TriggerBodyEntity.PlayerComponent:MoveToEntityByPath('/maps/map01/portal-3')
        end
     end)
}
```

![moveentity](https://mod-file.dn.nexoncdn.co.kr/bbs/1656586368440e1c78d968dba467b9b2d5cca978b4996.gif "moveentity")
#### Creating Conditional Actions
You can add a condition that can influence players' basic properties.
With `AddCondition()` function, you can add conditions that can limit player actions. You can put limitations on the basic functions, attack, jump, and so on, under certain conditions for more fun. 

1. In ![workspace_UserFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/1634600723422190820861d63456fa40e21031bd5ffa2.png "workspace_UserFolder") <span style="color: #dc9656">**MyDesk**</span>, create a PlayerSetting component, and add it to ![workspace_MyAvatar](https://mod-file.dn.nexoncdn.co.kr/bbs/16346008379922a9b756c3912461db7195808cb554abd.png "workspace_MyAvatar") DefaultPlayer.
2. Write a condition under which Jump action will be executed.<br> If you write the condition to execute Jump when the value is bigger than 10, the opposite will also be true. 
3. Add a log so that you can see whether pressing the space bar to jump executes the jumping action.

```lua
Method:
[client only]
void OnBeginPlay()
{
    local playerController = self.Entity.PlayerControllerComponent
    playerController:AddCondition("Jump", function()
        if self.Entity.PlayerComponent.Hp < 10 then
            log("Stop Jumping")
            return false
        end
        return true
    end
}
```

![stopjumping](https://mod-file.dn.nexoncdn.co.kr/bbs/1656586397424b751fb67c7974fe694521961c9a7ecab.gif "stopjumping")
#### Deleting Existing Key, Creating New Key
Creators can choose to delete existing keys and replace them with new keys. You can use this function to delete the moving keys (W, A, S, and D), and replace them with other keys.

1. In ![workspace_UserFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/1634600723422190820861d63456fa40e21031bd5ffa2.png "workspace_UserFolder") <span style="color: #dc9656">**MyDesk**</span>, create a PlayerSetting component, and add it to ![workspace_MyAvatar](https://mod-file.dn.nexoncdn.co.kr/bbs/16346008379922a9b756c3912461db7195808cb554abd.png "workspace_MyAvatar") DefaultPlayer.
2. Use `RemoveActionKey` to delete the action connected to the left Ctrl key. Then use SetActionKey to compose a script that links <span style="color: #dc9656">**Attack to F key**</span>.

```lua
Method:
[client only]
void OnBeginPlay()
{
    local playerController = self.Entity.PlayerControllerComponent
    playerController:RemoveActionKey(KeyboardKey.LeftControl)
    playerController:SetActionKey(KeyboardKey.F, "Attack")
}
```
##### Reference Guide
* [Editing Initial Character Properties](/docs?postId=48{"target":"_self"})
* [Attack and Hit](/docs?postId=206{"target":"_self"})