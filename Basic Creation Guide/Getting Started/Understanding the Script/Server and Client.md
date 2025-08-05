# Course Introduction
Multiplayer games use various communication models depending on the needs of the game. Among various communication models, MapleStory Worlds uses a server-client model.
In this course, we'll look into the concept and structure of server and client, the communication model of MapleStory Worlds, and how data is sent between spaces.
# Server and Client
Client refers to the individual program that runs from each user's device. The client mainly processes user input or information from the server to provide visible output to each user.
Server refers to the program that runs from a computer to multiple connected clients, or the computer itself. The server generally corresponds to each client's request, and builds major game functions and systems within the server so that all clients can have the same status.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1637228699029a52751daf6c943c0a46ca3ca575c1f9e.png "1")
# Server and Client Relationship
In a server-client model, a client doesn't communicate with another client. It only communicates with the server. The client receives user input, and according to the input, requests information to the server or informs the server about the current status. Upon receiving information from a client, the server sends information back to the client, or updates the game with the information, and then sends the updated content to all clients so that all users can play the game in the same environment.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16564010009004884f83abef040418e46508302f1d989.png "2")
<br>
Creators can build their own server-client structure as needed. However, if all logic and clients are built within clients while the server is only used for synchronization and communication, the structure would be vulnerable to hacking. For instance, as shown below, suppose clients process the character's level up, and then notify the information to other clients via the server.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401018765767a747699a14a67bfc9fd13ecf1fe18.png "3")
▲Example of handling major logic in client
<br>
In this case, the server will not be overloaded, as major logic is processed in individual clients, and the server only needs to send processed results to other clients.
However, if a user hacked his or her client, and that result is sent to the server, the game will be broken for other users as well.
From the example above, if User A tampers with the client to gain more EXP from monsters, the server will only receive tampered information. As a result, the server will notify all clients that UserA's player leveled up so fast.
Of course, you can have a verifying logic within the server. However, in that case, server and clients will have overlapping logic, which would be inefficient.
Therefore, in MapleStory Worlds, you are advised to implement major logic and system within the server, and have the client pass user input to the server or download game status and then pass it to the user. That is, the overall flow of the game will be placed in the charge of the server, while the client is like a bridge between users and the game.
In line with this, we can modify the level-up processing procedure like the following.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16564010313405c510273dc4b4835af50a54f45b1967b.png "4")
<br>
# Property and Function in Server and Client
Now that we know how the server and client communicate, let's see how the program created from the Maker is run in both the server and client.
If a creator declares property and function in a script, they will be created independently in both the server and client, unless set otherwise.
They have the same name, but since they exist in different spaces, it'd be better to consider them as separate properties or functions.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401057975779ce7742c954859a422193c5a0eb220.png "5")
<br>
Basically, calling functions and referring and assigning property values is done within the same space.
For example, let's say you're assigning value to property A from function B. Calling function B from the server will assign value to property A in server, but not to property A in client.
It is the same for the other way around. Calling function B from the client will assign value to property A in client, but not to property A in server.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401069226707b9a8609b349a28dc8f38ec44fb805.png "6")
<br>
Calling between functions works in a similar way. For example, you've declared functions A and B, and if function B is called from client's function A, it will only call function B from client, not that from server.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/165640107972755dfdec07cd545baae6c1149b76567bb.png "7")
<br>
To sum up, what's been declared in server runs in server only, and what's been declared in client runs in client only.
So, how can we make the server-client interact for the sake of multiplayer?
# Synchronization and Execution Space Control
Server and client communicate in many different ways.
MapleStory Worlds offers synchronization to send data or status, and execution space control to send behaviors or actions that should happen in certain situations.
#### Synchronization
While synchronization can have several meanings, in MapleStory Worlds, it refers to the synchronization of property values in the server and client. For instance, if you change the property value from one space, the value in the other space changes automatically. It's more convenient for the creator, as there is no need to do anything special to synchronize property values in server and client.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401093501c90d2e065ea248dfaa21ba67b640e1cd.png "8")
<br>
However, not all properties all synchronized.
Properties may or may not be synchronized depending on their function and characteristics. For example, if there's a component that mainly operates in clients, the component won't have to send data to a server. In this case, the property will not be synchronized.
Script component allows you to use property synchronization. The creator can choose to synchronize depending on their component or style, which will reduce unnecessary procedure and boost efficiency.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/165640110289849c2637a9a3c4b6ea395b2e202164e89.png "9")
<Properties may be or may not be synchronized depending on their setting.>
<br>
In general, synchronization goes one-way, from server to client. For instance, if property A is assigned a value from server, property A in client will be synchronized to have the same value. However, if a value is assigned to property A from client, property A in server will not go through any change.
This is because the relationship between server and client is one-to-many, not one-to-one. Let's say there are clients A and B, and property C is assigned the value of 1 in client A and 2 in client B. Then the server cannot decide with which it should synchronize.

| Synchronize from server to client | Synchronize from client to server |
| --- | --- |
| ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401113982d9490c00f9664a3db5c45b69ca8dbc3c.png "10") | ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1656921707340d78a7ac5ff71456b8493dfbb3fc7736a.png "11") |

Sometimes you synchronize value from client to server, but except for a few exceptional components, the synchronization goes from server to client, due to the relationship between server and client.
#### Execution Space Control
Apart from synchronization, server and client communication requires a process to request and execute an "action" defined in a space.
Let's take the level-up process from above as an example. Upon receiving user input, client requests an "action" of "monster attack" to server. Then the server executes the "action", "monster attack" as requested. Execution space control refers to this process of requesting and executing actions defined each space.
Execution space control can be set differently depending on the creator's intent.
The "action" from above is a "function" in a script. Depending on the setting of function in script, the request can be sent from client to server, but also from server to client as well.
Let's take the below as an example. If you set function B, declared in server, to be executed in client, it can be called from other functions within the server, but as it is run from the client, it can also be used like a client function that can be called from the server.
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/165640112429409df3933835246e2834bb887ab92c354.png "12")
<Request and execution when execution space is set as client for MethodB, and server for MethodC>

