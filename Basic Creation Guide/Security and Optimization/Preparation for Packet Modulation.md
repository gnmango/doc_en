# Course Introduction
When the server and client exchange packets, data modulation can occur and cause problems in the creator's World. Let's learn about packets and how to prevent packet modulation to make the World better.

##### Reference Guide
* [WorldConfig]
* [Server verification example]

# Learn about Packets
A **Packet** is a unit of data exchanged between a server and client. In MapleStory Worlds, the client and server exchange data depending on the function's execution space. In this case, the client and server don't just exchange data; they communicate via **packets** of data. For example, let's say you call a server execution space control function from a client. The function will send a packet with information about which entity, component, function, and argument to execute to the server. The server will then execute based on that information.

# Learn about Packet Modulation
**Packet modulation** is the malicious alteration of packets during network communication. This is usually done by analyzing the packets sent over the network to determine the structure, then modulating the actual data portion. 

In MapleStory Worlds, someone can attempt to modulate entities, components, functions, arguments, target client information, etc. in packets. When you write a script,  ensure the server validates the data received before changing the actual data in order to prevent modulation of the client or packet.

A common method of packet modulation is to find the value of specific data in a packet and change the value when the packet is sent from the client to the server. This method can have a malicious impact on the game ecosystem by changing the value of in-game goods and items.
Imagine that you award users mesos, experience points, and a custom item set for defeating a monster. Users actually get 1,000 mesos, 1,000 experience points, and a Normal item, but packet modulation gives them a much better reward.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1706080639140c27ad828ae0a49aca8cec1d0a1e9da34.png "1")
# Packet Data Modulation 1
To prevent packet modulation, don't send packets containing actual gold, experience points, or item name data. Instead, send requests to the server and let it determine the actual data. It's important for the server to validate the received packets once more before applying them. For more information, see the [Server Verification Example] guide.

Let's look at examples of modulated entities and components and how to handle it. Say you want to send four pieces of data in one packet from the client to the server.

* **Entity Name**: PlayerEntity
* **Component Name**: MyPlayerDataComponent
* **Function Name**: RequestGetCoin
* **Function Argument**: 500

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17116124998769b237ee7005a4bd7a9d464cf615f5e4c.png "2")

These are two common examples of malicious data modulation.
1. Send **RequestGetCoin packets repeatedly to get gold without completing the quest.**
    * When the server gets a provision request, it confirms the conditions to get gold have been met. It doesn't reflect the request without verification.
2. **Change the argument value of the gold to get a lot of gold at once.**
    * The client does not send the actual amount of gold to be acquired. The server will provide gold with a value matching the client's request.

#### Setting Critical Function to Server Only
Sending unchanged content in packets from client to server increases the risk of modulation. Therefore, you should set the execution space control of critical functions to **server only** to prevent repeat packet sending, attempts to change argument values, etc. In addition, the server function must verify that the information the client sends has not been modulated.
For example, if you need to write a function that provides a reward, write it to call in the order of **client → server → server only** functions. Then the function that actually provides the reward must be a **server only** function.

If you send **1,000 mesos acquired** from the client to the server, someone may modulate to get more mesos or give it to another client. To prevent that, you should direct the reward request packet to the server, and make the actual payment server only. 

```lua
Method:
[client only]
void ClickGetRewardButton()
{
    self:RewardProcess()
}

[server]
void RewardProcess()
{
    -- Before calling the SendCoin() function, you need to verify that this is a valid request.
    self:SendCoin()
}


[server only]
void SendCoin()
{
    -- Actual provision logic 
}
```

> <span style="color: #7cafc2">**Tip**
> If you set every logic's execution space control to **server**, it can overload the server. Instead, let the client handle rendering processing, like animations, and let the server process critical elements, like goods and items.</span>

# Packet Data Modulation 2
Another method of packet modulation is player entity name modulation. This allows users to complete actions and get rewards without actually performing those actions. For example, PlayerEntityA actually played, but PlayerEntityB may get the reward. Because the player entity name was modulated, the gold was awarded to the wrong player. To prevent this, you must prevent any client other than the local client from calling the player entity's server functions. 

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17116125320499f54fab802ea4f0cbdbb81827d3955f7.png "3")

#### Utilizing PlayerEntityAuthorityCheck
The **PlayerEntityAuthorityCheck** of WorldCofig ensures that the server functions of the player entity's components are called only by the local client. Since it's impossible to call server functions from other clients, it is impossible to modulate the packet. If someone attempts to call a server function from another player entity, they will get an error like the one below. 

![PlayerEntity](https://mod-file.dn.nexoncdn.co.kr/bbs/169571933009917b0b6c1d9994fe9a9e71fa7ff2dacbd.png{"width":"860px"} "PlayerEntity")

><span style="color: #7cafc2">**Tip**
> A local player entity's presence is verified by both the client and server.</span>

Disabling PlayerEntityAuthorityCheck may cause rewards to go to the wrong player. 
If PlayerEntityA hunted a monster, it is normal to award mesos to PlayerEntityA. However, if someone modulates with the entity information of the packet to PlayerEntityB, the server function of PlayerEntityB may be called, and the mesos may be awarded to PlayerEntityB.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17116125839800b14b16908dc40c3bdb3eb44b3777e0d.png "4")

#### Server Function Caller senderUserId
The **senderUserId** has the **client's userId** information that called the server function as an argument of the server function. The senderUserId is assigned by the server and is not modulated by the client.
For example, if User A in the World wants to destroy a specific item in their inventory, write a script as follows. In this script, if another client calls the `RequestDropItem()` function, User A's item may be deleted.

```lua
[server]
void RequestDropItem(Entity targetItem)
{
    if self:HasItemInInventory(targetItem) then
        return
    end
}
```

Use the senderUserId to keep other users' items safe when a user destroys an item. You can confirm that the user who called the `RequestDropItem()` function is the one whose item will be deleted from inventory. 
If the `RequestDropItem()` function is called from a client other than the local client, write the following to prevent the item from being deleted from inventory.

```lua
[server]
void RequestDropItem(Entity targetItem)
{
    if senderUserId ~= self.Entity.PlayerComponent.UserId then
        return
    end

    if self:HasItemInInventory(targetItem) then
        self:RemoveItemInInventory(targetItem)
    end
}
```

# Using ServiceAuthorityCheck
MapleStory Worlds allows creators to change the execution space control of service functions for convenient production. The **ServiceAuthorityCheck** in WorldConfig can make all server functions of the service behave as **server only** functions.
For example, functions of TeleportService are server functions that allow a player entity to teleport a specific player entity. However, if you enable ServiceAuthorityCheck, the Teleportservice function changes to server only and cannot be called from the client. This helps prevent unintended player entities from being teleported.


# RestrictedPlayerEntitySync Use
You can enable **RestrictedPlayerEntitySync** of WorldConfig to prevent the **Sync** property of the native component belonging to the player entity from synchronizing values changed on the client to the server.
By default, the Sync property of a native component synchronizes to all clients whenever the value changes on the server. On the contrary, values changed on the client are not synchronized to the server. However, if the native component belongs to a player entity, it will synchronize the values changed on the client to the server.
This is to allow the client to change state immediately upon user input. Because the user input is processed on the server first, and when the results are sent to the client, the user may notice a delay in the response to the input. However, someone may modulate with the player entity's data using this synchronization method. For example, even in a World without the Chat UI, a client can modulate with the value of the ChatBalloonComponent's property to display a chat balloon on the avatar.

> <span style="color: #7cafc2">**Tip**
> The following components are not affected by the RestrictedPlayerEntitySync for input responsiveness, even though they are included in the player entity. Synchronization always occurs from the client to the server when it is included in a player entity, regardless of whether it is enabled or disabled.</span>
> * <span style="color: #7cafc2">MovementComponent</span>
> * <span style="color: #7cafc2">PlayerControllerComponent</span>
> * <span style="color: #7cafc2">StateComponent</span>
> * <span style="color: #7cafc2">TransformComponent</span>