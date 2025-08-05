# Course Introduction
Let's learn about property synchronization, one way to send data between server and client.
# Synchronization
Synchronization in MapleStory Worlds is a type of data transmission method between the server and the client, and means that properties of the same name existing in each space are set to the same value. Generally, a property is created both in server and client upon declaration. However, if the property is not synchronized, it can have the same name but different value.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1656921154697fd4f97fb346241e199e2bdcd95eb0ccd.png "1")
<br>
The creator can choose to assign different values for the same property depending on the space. However, it's more common to synchronize the values for more efficient management.
Then how can we synchronize properties?
<br>
MapleStory Worlds offers auto synchronization of properties. You don't need to do anything special. You just need to declare properties, and then set if the properties will be synchronized.

> <span style="color: #585858">**Learn more**
> Let's suppose we are creating character stat without synchronization.
> If we are making stat, first we create a component called Stat, followed by HP Property, MP Property, etc.
> And if we create something that reduces HP when the character is attacked by a monster, we create a logic that reduces the character's HP in the server.
> Regarding the properties of the server and the client, as they exist in different spaces, only the HP Property of the server renews its value while the HP property of the client doesn´t.
> Although the HP value of the character has been reduced, HP in the client (e.g. UI) isn't reduced.
> In this case, if the property of the server is changed, you have to either manually change the value of the client, or run the same logic in the server and client. It would be very inefficient and difficult to manage.
> ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16353351388052ce5c7972f5f47abb1d5ff853ed1c98f.png "2")
> <The character is dead, but its HP isn't reduced because property is not synchronized between the server and client.></span>

# Synchronization Direction
In general, property synchronization goes from server to client, except in some native components. That is, if you assign a value to property in server, the changed value is sent to the client to be assigned to the same property. It also means that if you assign value to a property in the client, the value in the server does not change. As a server is unique, value changed in server can be sent to each client. In contrast, there are several clients and they can all have different values. In this case, it is difficult for the server to decide which value to accept.

In MapleStory Worlds, synchronization from server to client is mostly limited. Also, it is not recommended to change a value in client while property is set to be synchronized, except in some exceptional cases.

* **In case of synchronizing with value changed in server**
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/165692168427992469f2764814b5dae5778479830821c.png "3")
<br>
* **In case of synchronizing with value changed in client**
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1656921707340d78a7ac5ff71456b8493dfbb3fc7736a.png "4")
<br>
Synchronization sends changed value from server to client to synchronize the values, which may result in some delays. This means that the client will not immediately accept the changes made in the server.
If you'd like to perform a certain action at the moment of synchronization, you can use OnSyncProperty, one of the Default Event Functions.
OnSyncProperty is called when a property value within the same component has been synchronized. It is useful when you output the synchronized value from the client.
For more information on OnSyncProperty, refer to the guide below.
[MSW Default Event Functions](https://mod-developers.nexon.com/docs?postId=163{"target":"_self"})
# Synchronization Setting
Now that we've learned about synchronization, let's see how to set the actual synchronization and how to synchronize different property types.
#### Synchronization Setting Method
Synchronization setting is very simple.
If you've added a property from Script Editor, you might have seen Sync on top of the property. This Sync means that the property is being synchronized.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16353351805975c7eee1f898d423a80562096893bcc95.png "5")
<br>
If you don't want to synchronize properties, click Sync to change it to None. None means that the property is not being synchronized, and you can manage property values differently depending on the space.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/16353351874159d1ae0e2b0e148ac8fe0075ef65b2f96.png "6")
<br>
#### Synchronization Possibility per Type
Some property types cannot be synchronized.

| Type | Synchronization Possibility |
| --- | --- |
| any | X |
| string | O |
| integer | O |
| number | O |
| boolean | O |
| table | X |
| SyncTable< v >  |  O  |
| SyncTable< k,v >  |  O  |
| Vector2 | O |
| Vector3 | O |
| Vector4 | O |
| Color | O |
| Entity | O |
| Component | O |
| EntityRef | O |
| ComponentRef | O |
