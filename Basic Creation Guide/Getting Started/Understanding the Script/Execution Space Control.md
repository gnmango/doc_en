# Course Introduction
Let's learn about the concept and utilization of execution space control. This will make it easier to control the flow between the client and server.
##### Before We Begin
To understand this course, it is recommended that you review the following guide.
[Server and Client](https://mod-developers.nexon.com/docs?postId=207%7B%22target%22:%22\_self%22%7D)
# Data Communication Between Server and Client
**Client - Server** requires data transfer to control the flow between them. For example, what would it take to request "Dance for 3 seconds!" from a server to a client?
<br>
Let's change the example into a conversation between two people.
* One person thinks of what to say. (Create message)
* Then, they use their hand or mouth to deliver the message. (Send message)
* Then, the other person receives that message. (Receive message)
* Lastly, the receiver understands the meaning of the words and responds. (Action, Result)
<br>
<span style="color: #dc9656">**"Create message → Send message → Receive message → Action/Result"**</span>
<br>
Traditionally, the server writes a request (message) and sends it to the client. Upon receiving the message, the client reads it to perform the action or make further judgment. The process is the same as the conversation example, <span style="color: #dc9656">**Create message → Send message → Receive message → Act according to message**</span>.

| Message sending stage | Reference image |
| --------- | ------ |
| Create message | ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401323520768509b4f80845fbbab4ed52115a3894.png "1") |
| Send message |![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16564013023916352d2183c50401b86d22ac463231a6b.png "2") |
| Receive message | ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/165640121527182b4e74d157e455189b2bb8292740da5.png "3") |
| Act according to message |![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1637223532474323d466a087a4bb6b9527d3de064b96a.png "4") |

However, the process is inefficient when dealing with a high volume of messages. Increasing the number of messages will increase the network cost. 
<br>
Transmitting the same meaning as a sign instead of sending the entire message is more efficient. Using codes requires encoding and decoding processes, but the communication size is smaller than sending entire messages. For example, if the message **Code-45** is supposed to mean **Dance for X seconds**, you only use **Code-45** and **3 seconds** for communication.

| Message sending stage | Reference image |
| --------- | ------ |
| Create message |  ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401323520768509b4f80845fbbab4ed52115a3894.png "1") |
| Encode message | ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401224520466e3724c21a4973bcd77332dbbe3d04.png "5") |
| Send message | ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1637223644960d8351081fc44495b9425a32213d9b15d.png "6") |
| Receive message |  ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1637223663644d87a77c2927c414a91198c3ebf796f08.png "7")|
| Decode message | ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/165640123296943009a72c7884f78b9960258a0c805dc.png "8") |
| Act according to message | ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1637223532474323d466a087a4bb6b9527d3de064b96a.png "4") |

There is less information, but more of a process to go through. MapleStory Worlds offers execution space control to control the above process more easily.
**Execution space control** helps the client-server communication and controls complex communication flow more easily. It takes time to become proficient, but you must understand **execution space control** to take advantage of client-server communication.
# Execution Space Control
To understand how execution space control works, let's look at the context of a function call.
Let's say there's a `PlaySoundEffect` function that makes a certain sound effect. The function would be used in clients only since sound effects should be heard from individual PCs. If a server has to make the sound effect at a certain moment, the server will create and send a message about the sound effect. Then the client will create the receiving part to call the `PlaySoundEffect` function.

| Message sending flow | Reference image |
| --------- | ------ |
| Create message | ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/165640124223449295b986dc6462d99a93c10631c073d.png "9")|
| Encode message | ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/16564012529432da2881cf2af4b7e961f82a2b37643f8.png "10") |
| Send message |![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1637225094693f9b508dae1af4fa6b39c3412894d527d.png "11") |
| Receive message |![12](https://mod-file.dn.nexoncdn.co.kr/bbs/163722510582956aa6c66e3f14d40b7d1a87b3c97c7fd.png "12")  |
| Decode message |  ![15](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401313348532ea68ad6fa4904847c50414ec666dc.png "15")|
| Call function to message |![50](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401264321cd1173fba0444f66b0a9dbce57b8b314.png "50") |
| Execute |![16](https://mod-file.dn.nexoncdn.co.kr/bbs/163722545859347bd24cd99514181ae555fba18421eae.png "16") |

This process is better than the first method, but if it has to be repeated, an unnecessary process is repeated and efficiency is reduced.
For simplicity, consider a situation where the server directly calls the client's **PlaySoundEffect**.
![51](https://mod-file.dn.nexoncdn.co.kr/bbs/1656401276667c1a175db897f4e21aa3bfb6cb2fcaf73.png "51")


However, even if the server directly calls the function from the client, there is another flaw.
The components of MapleStory Worlds are used for both server and client. Therefore, functions belonging to the component can be called from the server or the client. You need to be aware of where the function was called (server or client), and where the function will be executed. 
<br>
For instance, if you're writing the code, "Execute **PlaySoundEffect** if current space is Client, and Client's **PlaySoundEffect** if space is Server.", you need to compose a branching statement.
```lua
void CallPlaySoundEffect()
{
    if isClient == true then
    --Execute PlaySoundEffect!
     
    elseif isServer == true then
    --Execute PlaySoundEffect from client!
    end
}
```
<br>
You can call functions from another space, but this would be inefficient as well, as you have to make If statments every time you call a function.
In MapleStory Worlds, you can set the execution space for calling and running each function. For example, if you set the `PlaySoundEffect` function, which is executed in the client, to be called from server, you don't have to compose a branching statement for `PlaySoundEffect` to run in the client.
```lua
[ServerOnly]
void CallPlaySoundEffect()
{
    self:PlaySoundEffect()  -- You can use it to call a function from the same space, regardless of space.
}

[client] --You can set where to call and run each function.
void PlaySoundEffect()
{
    --You can write the code.
}
```
# Execution Space Control Types
For each function, there are 6 execution space control types, and you can change the setting from the options. If you don't use execution space control, the function would be a common function, running where it was called.

| Execution Space Control Setting | When executing in the server | When executing in the client |
| -------- | ------------- | ------------- |
| Not used | Executed in the server | Executed in the client |
| client | Sent to and executed in the client | Executed in the client |
| client only | Ignored | Executed in the client |
| server | Executed in the server | Sent to and executed in the server |
| sever only | Executed in the server | Ignored |
| multicast | Executed in the server then sent to and executed in the client | Ignored |

![202](https://mod-file.dn.nexoncdn.co.kr/bbs/165640128942696725f39866847439127557f4942f7c0.png "202")
# How to Set Execution Space Control
Add a function, click the ![script_more](https://mod-file.dn.nexoncdn.co.kr/bbs/16345206995612a35d54577d8466da03a9fe452a5218c.png  "script_more") **[Menu]** button, and click **Execution Space Setting** to set the execution space. If you're not using execution space control, select **Not used**.

![20](https://mod-file.dn.nexoncdn.co.kr/bbs/1681782904494d8a8401e8e6c45c1a9310c2f558ecff2.png "20")
# Showing Execution Space Control
Displays the execution space control settings of script functions as icons.
The icons are as follows.

| Icon | Execution Space Control Setting |
| ---- | -------- |
| None | Not used |
| ![script_sent_to_client](https://mod-file.dn.nexoncdn.co.kr/bbs/1634520790600fcf43ce1e20540d99206368bae5bfb29.png "script_sent_to_client") | Client |
| ![clietn_only_line](https://mod-file.dn.nexoncdn.co.kr/bbs/16345204384345f2159bf5f7a4aadae190a6e66646a77.png "clietn_only_line") | ClientOnly |
| ![script_sent_to_server](https://mod-file.dn.nexoncdn.co.kr/bbs/1634520851421b73268f466814035ba41893c8b45fd9c.png "script_sent_to_server") | Server |
| ![script_server_only](https://mod-file.dn.nexoncdn.co.kr/bbs/1634520890852542e95e200a647819d47b85d5c4d157e.png "script_server_only") | ServerOnly |
| ![script_send_to_all](https://mod-file.dn.nexoncdn.co.kr/bbs/1634520769851d04d006c9114421f8e9ce34d5c47b721.png "script_send_to_all") | Multicast |

Functions with execution space control will have an icon indicating the setting status. 
Functions that cannot be executed will be indicated with the ![CantCallFunction](https://mod-file.dn.nexoncdn.co.kr/bbs/1635387179103da42aef656b24301ba63221cd02c4bbf.png "CantCallFunction") icon.
![25](https://mod-file.dn.nexoncdn.co.kr/bbs/16817831861368eaf4d7f03b545759cb8f9a98c291601.png "25")
# Execution Space Control Function's Parameter
When executing functions from different spaces, you can send data to the other space using parameters. 
For example, if you want the server to output a system message displayed in the client, call a client function on the server that prints the system message, and then deliver the message as a parameter to the function.
```lua
Method: 
[server only]
void OnUpdate(number delta)
{
    local users = _UserService.UserEntities
    local message = "Out of bounds"
    -- Check all players, and if player steps outside certain coordinates, print message
    for id, user in pairs(users) do
    	local posX = user.TransformComponent.Position.x
    	if posX > 100 then 
    	   -- Deliver the message as a parameter to the client function
    		self:PrintSystemMessage(message)
    	end
    end
}

[client]
void PrintSystemMessage(string message)
{
    log(message)
}
```
<br>
When executing functions from different spaces, you can send data to the other space using the parameters of the execution space control function. However, not all data can be transmitted. Let's take a look at the parameter types available in execution space control functions.
| Type | Availability |
| --- | --- |
| any | Not available |
| string | Available |
| integer | Available |
| number | Available |
| boolean | Available |
| table | Available |
| SyncTable< v > | Limited |
| SyncTable< k, v > | Limited |
| Vector2 | Available |
| Vector3 | Available |
| Vector4 | Available |
| Color | Available |
| Entity | Available |
| Component | Available |
| EntityRef | Available |
| ComponentRef | Available |
<br>
All types except the **Any** type can be used as parameters. However, for SyncTable< v > or SyncTable< k , v >, the types listed as "Available" are the only ones that can be used.
![27](https://mod-file.dn.nexoncdn.co.kr/bbs/1685938861944b5fa20659b7d4b3c809a765906fa012c.png "27")

## Parameter Specificity of Client Space Execution Functions
Consider a situation where Client A executes a server function, and the server sends a response to it. Usually the server sends a response to all clients.
![client01](https://mod-file.dn.nexoncdn.co.kr/bbs/168075980289219c2a09110834a149783309cd2192038.png "client01")

However, in some cases, you may only need to respond to Client A, who sent the request in the first place.
![client02](https://mod-file.dn.nexoncdn.co.kr/bbs/168075982031597f4c778957346d1969d09fdd4ff3eb0.png "client02")

MapleStory Worlds provides a function to send a response to a specific client only. The way to use this function is "to write the desired <span style="color: #dc9656">**UserId**</span> to send to the last parameter of the function whose execution space is Client". If you want to take a closer look at an example of using this function, refer to "Sending responses to specific clients only" in the [Effective MSW 1](/docs/?postId=559{"target":"_self"}) guide.