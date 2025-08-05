# Course Introduction
Let's learn about TargetUserSync, a property synchronization method.

##### Reference Guide
* [Properties](/docs/?postId=205{"target":"_self"})
* [Property Synchronization](/docs/?postId=208{"target":"_self"})
* [Server and Client](/docs/?postId=207{"target":"_self"})

# Learn TargetUserSync
TargetUserSync is a more direct way to optimize the Sync property synchronization. TargetUserSync only synchronizes properties of components contained in PlayerEntity that are manipulated by the player. If a property value is changed on the server, it synchronizes the property value only to users who can manipulate the player entity whose property value has changed.
TargetUserSync is recommended for values that do not need to reference other users' information. This includes personal data such as personal goods, achievements, and supplies.
If you use the TargetUserSync property in a component of an entity other than PlayerEntity, TargetUserSync will not work properly. In this case, it behaves the same as the None property.

><span style="color: #7cafc2">**Tip**
>TargetUserSync works based on the connected client and the PlayerEntity that it manipulates. This means that it only works with component scripts that apply to the entity, and the TargetUserSync is not available in the properties of the script below.
> * EventType, ItemType, BTNodeType, StateType, StructType, Logic </span>

#### TargetUserSync Settings
TargetUserSync can be set in the synchronization settings of the property. The types that can be synchronized are the same as Sync.
* Synchronizable types: string, integer, number, boolean, SyncTable< v >, SyncTable< k,v >, Vector2, Vector3, Vector4, Color, Entity, Component, EntityRef, ComponentRef

![TargetUserSync1](https://mod-file.dn.nexoncdn.co.kr/bbs/16988033334991153af9548234020a1c66e3872cadf4a.png "TargetUserSync1")

It is also possible to use the Sync and TargetUserSync together in one component. Like the Sync property, you can use the OnSyncProperty to see when the TargetUserSync changes.
![TargetUserSync2](https://mod-file.dn.nexoncdn.co.kr/bbs/16987294568417ecb037ea67e4e35a64f34c4a401f740.png "TargetUserSync2")
# Difference between Sync and TargetUserSync
For properties set to Sync, when you change the property value on the server, the value is synchronized from the server to all users with that entity on the map in the client direction. If you are creating a World that checks and processes information of other users in real time, you can use the properties set to Sync. However, changes to property values set to Sync are synchronized to all users of the map, so you should use it with optimization in mind.

#### Sync Method
Imagine that you added the following components to all player entities. The meso property is used to manage the user's goods. Therefore, the user shouldn't know the meso values of other users.

```lua
Property:
[Sync]
integer meso = 0

Method:
[server only]
void IncreaseMeso()
{
	self.meso += 1
}
```
If clients A and B are connected to the same map, the property values of the entities seen by the server and client, respectively, are as follows.

![TargetUserSync1](https://mod-file.dn.nexoncdn.co.kr/bbs/1698802548910ae0fdd13b6e14f4ab5bd34c52b2f5cd4.png "TargetUserSync1")

In this case, PlayerEntity A's `InscreaseMeso()` is called to change the PlayerEntityA value from all clients when the changed meso property value of Entity A is synchronized. This is because the Sync property synchronizes the changed Meso value to the PlayerEntityA of all clients on the same map without restriction.
If you synchronize to all clients, ClientB will waste computation and packets due to unnecessary property synchronization processing. The server also wastes packets, since it has to send unnecessary packets to all the clients.
<br>
![TargetSyncProperty2](https://mod-file.dn.nexoncdn.co.kr/bbs/169925298455090466d29434b4a46b24070db138b88d5.png "TargetSyncProperty2")
#### TargetUserSync Method
If you change the meso property to TargetUserSync and then call `IncreaseMeso()` for the PlayerEntity on the server, the meso value is synchronized only to Client A, which has a manipulation control right of PlayerEntity. PlayerEntity A on Client B is treated the same as None, where nothing changes.

```lua
Property:
[TargetUserSync]
integer meso = 0

Method:	
[server only]
void IncreaseMeso()
{
	self.meso += 1
}
```

<br>
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/169872111296512b78d9fec8f463f8fd68d25ea813523.png "3")