# Course Introduction
Let's learn how to limit the number of a user's server function that is used.

##### Reference Guide
[Preparing for Packet Modulation](https://maplestoryworlds-creators.nexon.com/ko/docs?postId=1102{"target":"_blank"}) 
[World Instances](https://maplestoryworlds-creators.nexon.com/ko/docs?postId=984{"target":"_blank"})
[How to Make Instance Maps](https://maplestoryworlds-creators.nexon.com/ko/docs/?postId=540{"target":"_blank"})

# Learning about RateLimitService
Execution functions for servers are created to directly execute specific actions in the server, so when packets are modulated maliciously or when the same action is repeated, it can cause problems for the World. The `RateLimitService` limits the number of server execution functions that are used by a number set by the creator and can be used to enhance security for a World by setting a cooldown when a user uses a function that has been restricted. The use of Native API's server functions and server functions created by the creator can be restricted by `RateLimitService`. 
It is recommended to use RateLimitService with consideration to the room's charateristics in a way that fits the creator's World. Keep in mind that use restrictions for specific rooms don't affect other rooms, since that RateLimitService exists for each room.

> <span style="color: #585858">**Learn More**
> If you wish to use RateLimitService flexibly, you can do so from Logic.</span>

#### Spending and Recharging Tokens
To use `RateLimitService`, you must understand what tokens are. Tokens are a temporary call limit for creators to limit the number of tokens used. 
Players that reach the use limit won't be able to call functions until the tokens are recharged. The creator needs to set a limit to the number of tokens, and the gap between token recharges as parameter variables of a function to limit usage. Depending on World design, you may want to set the recharge interval shorter than other functions for functions that players call frequently.

Let's understand tokens by looking over the parameter variables for `SetServerFunctionRateLimitForLogic()` from `RateLimitService`. 

```lua
void SetServerFunctionRateLimitForLogic(string logicName, string functionName, int maxToken, float tokenRefillPerSecond)
```

Determine the number of **maxToken** and set the number of tokens that are recharged every second with **tokenRefillPerSecond**. The following is detailed meanings of parameter variables.

* **logicName**: The name of the logic that has the function that limits usage.
* **functionName**: The name of the function that limits usage.
* **maxToken**: The max number of tokens set by the creator which is deducted each time the user calls a function.
* **tokenRefillPerSecond**: The number of tokens that are recharged every second. For example, if it's set to 0.2, tokens will increase by 1 after 5 seconds. 

# Use Examples
Let's make an example that prevents players from calling the `MoveToMapPosition()` function multiple times in a short window of time. This will limit the `MoveToMapPosition()` function under `PlayerComponent` to be only called once every 3 seconds.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17320008539686a2dfc3bc0ec4a84921966a3cb61b1c4.gif "sample_ko")
#### Map Creation
Create an **InteractionEntity** on the map for teleporting, and place the arrival location, **Destination**, on a tile. Let's assume that it takes 3 seconds from moving with InteractionEntity until reaching another moveable point on this map. If someone attempts to move on the map faster than 3 seconds, it can be assumed that the function has been modified. 
Let's make an example World that limits function use with `RateLimitService` to prevent such modification.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1731998004301d26b2a6ee3fa43b39b779b5e178e7a80.png "MAP")
#### Creating InteractionEntity
1. Create a new **InteractionEntity** by selecting **Map01- Create Entity - Create Empty**.
2. Add **TransformComponent, SpriteRendererComponent, InteractionComponent**.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1731995623163f098c06a614f4f65b32ee64b0075c1ad.png{"width":"400px"} "1")
3. Create **TeleportComponent** by selecting **Workspace - MyDesk - Create Scripts - Create Component**.
4. Add **TeleportComponent** to the created InteractionEntity.
5.  Add a **Destination** property to **TeleportComponent**.

    ```lua
    Property:
    [None]
    Entity Destination = nil
    ```

6. Set the Destination entity that was created on the map by the creator as the property path in the **InteractionEntity** property window.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1731995613556801c7b7d6d2641c2861813f93712fc1b.png "2")

7. Write the following code so that the player is sent to the **Destination entity** when a player interacts with the created portal entity by pressing `E`.

    ```lua
    Event Handler:
    [client only][self]
    HandleInteractionEvent(InteractionEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: InteractionComponent
        -- Space: Server, Client
        ---------------------------------------------------------

        -- Parameters
        local InteractionEntity = event.InteractionEntity
        ---------------------------------------------------------

        -- Send my player entity to the `Destination` entity if the interaction key is pressed.
        if InteractionEntity == _UserService.LocalPlayer then
            local localPlayer = _UserService.LocalPlayer
            local currMap = self.Entity.CurrentMap
            local destPos = self.Destination.TransformComponent.WorldPosition:ToVector2()
            localPlayer.PlayerComponent:MoveToMapPosition(currMap.Name, destPos)
        end
    }
    ```

#### Creating Teleport UI
Let's make a UI that allows moving to the Destination entity at any time. Using this UI will be subject to usage limits.

1. Create a new **UIButton** by selecting **Hierarchy - ui - DefaultGroup - Create Entity - Create Button**.
2. Select the size and color of your choice.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/173199759809163519c556d1f4457b5407341c986eeb7.png "3")
3. Create a new **CheatingComponent** by selecting **Workspace - MyDesk - Create Scripts - Create Component**.
4. Add **CheatingComponent** to **DefaultPlayer**.
5. Set the path of the Entity that will be the **Destination** of the property.

    ```lua
    Property:
    [None]
    Entity Destination = /maps/map01/Destination
    ```

6. Link **UIButton** to **ButtonClickEvent** and write the following code so that the player can move to the Destination when they press the button. 

    ```lua
    Event Handler:
    [entity: UIButton(/ui/DefaultGroup/UIButton)]
    HandleButtonClickEvent(ButtonClickEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------

        -- Parameters
        -- local Entity = event.Entity
        ---------------------------------------------------------

        local currMap = self.Entity.CurrentMap
        local destPos = self.Destination.TransformComponent.WorldPosition:ToVector2()
        self.Entity.PlayerComponent:MoveToMapPosition(currMap.Name, destPos)
    }
    ```

#### Creating Use Limit Logic
Write the following code that limits the `MoveToMapPosition()` function under PlayerComponent when a user enters.
1. Create **RateLimitLogic** by selecting **Workspace - MyDesk - Create Scripts - Create Logic**.
2. Use the `SetServerFunctionRateLimitForComponent()` function to limit the use of the `MoveToMapPosition()` function under PlayerComponent when a player enters.
The number of tokens is 2, and the token recharge rate per second is 0.3.
    ```lua
    Event Handler:
    [service: UserService]
    HandleUserEnterEvent(UserEnterEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: UserService
        -- Space: Server
        ---------------------------------------------------------

        -- Parameters
        local UserId = event.UserId
        ---------------------------------------------------------

        local userEntity = _UserService:GetUserEntityByUserId(UserId)
        _RateLimitService:SetServerFunctionRateLimitForComponent(userEntity.Id, "PlayerComponent", "MoveToMapPosition", 2, 0.3)
    }
    ```

3. Write code that displays a warning log if there is a user that reaches the usage limit. You can know user informatiom by using `UserService`.

    ```lua
    [service: RateLimitService]
    HandleServerFunctionRateLimitEvent(ServerFunctionRateLimitEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: RateLimitService
        -- Space: Server
        ---------------------------------------------------------

        -- Parameters
        local FunctionName = event.FunctionName
        local ProfileCode = event.ProfileCode
        ---------------------------------------------------------

        local user = _UserService:GetUserByProfileCode(ProfileCode)
        log_warning("User '" .. user.Nickname .. "' has reached the function use limit of '" .. FunctionName .. "'.")
    }
    ```

4. Call the `MoveToMapPosition()` function by pressing the Teleport button multiple times in a short window of time after pressing [Start].
You can see that an error log has triggered in the Console window.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1737612072515e1810f9a2d444317ab7fe76daefac35c.png "6")