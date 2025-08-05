# Course Introduction
By setting the execution space of the function, the client and server can communicate easily without a separate process. It's convenient because it's processed automatically, but there are some limitations and it may not work as the creator intends. There are some logs, but you may not be able to determine why some don't work.
In this guide, we'll look into cases where you can't use execution space control.

# Types Not Supported by Parameters
Functions have several kinds of parameters. This parameter is sent along when execution control is used in other spaces. At this time, the parameter should be changed to a <span style="color: #dc9656">**"Transportable"**</span> format and then sent.

Some parameters, like **number** or **string**, are changed to a format that's been <span style="color: #dc9656">**promised**</span> to be transferable. However, if there is no promised format or if it is a format that is difficult to promise, it cannot be transmitted. The most common format is <span style="color: #dc9656">**"any"**</span>. Any has the advantage of being compatible with any type, but because of this, it cannot be changed to a transportable format. Therefore, if you use an execution control function in another space, an error will occur at that point.

<span style="color: #dc9656">If an error such as **"Failed to convert argument"**</span> occurs, it receives an unavailable type of (argument) among the type of corresponding parameters. In that case, just change the parameter type to something else.

# Using Execution Space Control Function from Server's OnBeginPlay()
Basically, in MapleStory Worlds, the server opens first and then creates the World. When each client connects to the server, it receives information from the server and configures the client.

One of the most common mistakes of creators is <span style="color: #dc9656">**"to send information to the client when the server starts"**</span>. Usually, in this case, an execution space control function is put in `OnBeginPlay()`, but the problem occurs here.
At the time the server is created, `OnBeginPlay()` is called, but at that time, the client is not connected and cannot receive the information transferred from `OnBeginPlay()` to the client. So, if you write the code like this, it won't work properly.

If the client needs to receive some information from the server when connecting, there is a way to notify the server of its BeginPlay timing in `OnBeginPlay()` of the client and wait for processing.

```lua
Method:
[Client Only]
void OnBeginPlay()
{
    self:NotifyClientOnBegin()
}

[server] 
void NotifyClientOnBegin()
{
    -- Send Data To Client
}
```

# Use Execution Control Function with Server from Localized Entity
Execution space control functions have a default location where they operate (Server or Client). They are set in pairs, so **A Entity** sends to the **A Entity** in the other space. For instance, if you use Client execution space control function in **A Entity**'s **B Component**, it will be transferred to **A Entity**'s **B Component**.

If the pair does not exist, the function transfer does not take place. That's because there are <span style="color: #dc9656">**Entities without pairs**</span>.

Entities that are called <span style="color: #dc9656">**Localized Entity**</span> are each created separately in their own clients, so they don't exist in the server. An example would be the **UI Entity**. The **UI Entity** is an entity that only exists in the client, so it doesn't exist in the server.

Therefore, it won't work if you try to use the execution space control function in **UI Entity**, since **UI Entity** does not exist on the server. **UI Entity** does not send a separate message saying that it does not exist on the server, so be careful when using it.
Therefore, if you'd like to exchange information with the server from **UI Entity**, you need to <span style="color: #dc9656">**transfer via World Entity**</span>.

# Understanding Local Processing
In MapleStory Worlds, you can easily implement multiplayer by default. We recommend you process most functions based on multiplayer. 

However, some cases require local concepts. The main ones are as follows.
* Localized Entity 
* Input Processing
* Processing my character

**UIEntity**, **enter**, and **my character** are all <span style="color: #dc9656">**concepts that only exist in my client**</span>, so they require additional processing. In particular, to process the concept of **my character** within the client, MOD supports `_UserService.LocalPlayer.

Let's take a look at the local processing method through the following example.
Make **SkillComponent** and put it in **Player**. Let's say we've written logic that uses the ultimate skill when we **enter** the S key on the keyboard.

```lua
Event Handler:
[service : InputService]
HandleKeyDownEvent (KeyDownEvent event)
{
    -- Parameters
    local key = event.key
    --------------------------------------------------------
    if key == KeyboardKey.S then
        self:UseUltimateSkill()
    end
}
```

If you write the code as above, a problem will occur when playing multiplayer. **Player** exists in both your client and other people's clients. So even if someone else presses the S key, all clients' player characters will use their ultimate skills.
Your code should only process the person who entered the S key. Therefore, after checking whether the Entity is your Entity, if it is correct, let's use the ultimate skill, otherwise, let's not use the ultimate skill.

Modify the above logic as follows.

```lua
Event Handler:
[service : InputService]
HandleKeyDownEvent (KeyDownEvent event)
{
    -- Parameters
    local key = event.key
    --------------------------------------------------------
     
    if self.Entity ~= _UserService.LocalPlayer then -- Check LocalPlayer(My)
        return
    end
     
    if key == KeyboardKey.S then
        self:UseUltimateSkill()
    end
}
```

Let's look at another example.
Creators are making RPGs. We created a script component to manage money and added a **money** property to it. Whenever **money** changes, it also tends to update the UI.

Let's write the logic as follows.

```lua
Method:
void SetMoney(number money)
{
    self.money = money
    local moneyUI = _EntityService:GetEntity("1f6dcd10-6306-4a47-bd41-f0a897685b73")
    moneyUI.TextComponent.Text = tostring(money)
}
```

However, if you write the code like this, there is a problem that your UI changes when other people acquire money. Therefore, only the UI of the person who earned the money needs to be processed and updated.

Like this, problems related to the concept of Local usually occur when multi-testing, although there is no problem when testing alone. If you understand the local concept in advance, you'll be able to write better code.