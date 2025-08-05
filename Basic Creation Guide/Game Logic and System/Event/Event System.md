# Course Introduction

Creators can write logic in several ways. One way is to utilize **events**.
This time, we will look at the MapleStory Worlds event system and how to handle different events.

# Configuration of Event-Based Logic

When writing logic, **things to be done at certain times** may occur. In this case, you should decide what the **agent where the action arises** will do.

For example, let's say the following is implemented when a character is attacked in the world:

1. <span style="color: #dc9656">**Reduce character's HP**</span>
2. <span style="color: #dc9656">**Show the damage**</span>
3. <span style="color: #dc9656">**Make the character invincible for a short time.**</span>

To implement this, we need to call a function that performs each function when the character is attacked. Here, if the creator wants to add a special effect to the hit effect, just add <span style="color: #dc9656">**4. Display the effects of being attacked.**</span>.
<br>
With this standard logic, the **agent where the action arises** manages actions. However, event-based logic manages the actions in the **action executor**. That is, the **agent where the action arises** only informs necessary information and timing upon action occurrence, and **action executor** decides the individual action to take.

Let's go back to the above example.
When a character is attacked, instead of calling each function, it sends the only **attacked situation** as an event. The **action executor** who received the event performs **internally determined actions** (reduces HP, shows damage, makes the character invincible for a while). For this to work, all the **action executor** needs to do is **subscribe** to when the event occurs. If you want to add a hit effect, you can add an additional **subscribe** to the **attacked situation** event in the effect management area. 

Imagine receiving a notification from a famous streamer. The streamer will send notifications to subscribers when some notifications are necessary. It's up to each subscriber to decide what action to take when receiving that notification.
<br>
In summary:
* In <span style="color: #dc9656">**Standard Logic**</span>, the <span style="color: #dc9656">**agent where the action arises**</span> gets things done.
* In the <span style="color: #dc9656">**Event System**</span>, the <span style="color: #dc9656">**action executor**</span> does that.

# Configuration of Event System

MapleStory Worlds' event system is composed of three major parts.

| Element | Explanation |
| :---: | --- |
| Event (Event) | : Refers to the occurrence of an incident within a logic. <br>It is data carrying information identifying the Event type along with other additional information. |
| Handler | : The acting agent that processes an event upon receiving it. <br>There are similar terms such as Listener and Subscriber. |
| Sender | The entity that dispatches the event. <br>There are similar terms such as Emitter and Dispatcher. |

Let's say you mail a notice to the entire class. Then the Event, Handler, and Sender would look like this:
* Event: Notice
* Handler: Students in the class
* Sender: Postal carrier

# Pros and Cons of the Event System

Event systems aren't necessarily better than standard logic. Event-based logic is one of many development approaches, which all have their pros and cons. Therefore, it is recommended to utilize the appropriate logic according to the needs of the creator.

## Pros
* Not dependent on any other components or functions. It is advantageous for creating a decentralized structure.
* You can add an action to the operation without having to modify the agent where the action arise.
* There is no need to know another component's information.

## Cons
* It is difficult to understand the overall flow of an event when it occurs. This is because you don't know the time of execution because it processes each one individually.
* Debugging can be a bit difficult.
* Sequential processing can be difficult.

# Summary
Today we looked at the concept of the event system. MapleStory Worlds uses the Entity Event System. The Entity Event System uses entities as a relay station. 
Let's see more details in [Entity Event System](/docs/?postId=176).