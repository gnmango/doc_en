# Course Introduction
MapleStory Worlds uses the Entity Event System to control events.
Let's implement one of the game elements of "Vampire vs Hunter" with the Entity Event System.
Looking at the following guides beforehand will be useful in understanding this course.
[Event System](https://mod-developers.nexon.com/docs?postId=73{"target":"_self"})

# Entity Event System
You need to understand the Entity Event System to know how to send and receive events.
A component can use an entity as a broadcaster.
Each component can register handlers and raise events through entities.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16560446599253c3fab51c71c43a681173496cc4ee5a4.png "3")
<br>
Senders also fire events through entities. Entities send events that have occurred to handlers.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16560447014255f87e1b80f4a4a85b4b4857f01923163.png "4")
<br>
By default, you attach events to your own entity, but you can also attach events to other entities depending on the situation, as shown in the figure below. In particular, the map entity and world entity often exchange events with each other, so the following form is often used.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16560447361361ac2b80437934439be6a9a35b95b41e4.png "5")

# Creating Sunrise Event
The event system has built-in native type events. Also, creators can declare or import event types themselves. Here's how to do it.
<br>
1. In the context menu of **Workspace - MyDesk**, click **Create EventType**.
![eventtype](https://mod-file.dn.nexoncdn.co.kr/bbs/168783084547591cb0c6685e148e799ab7a2caef6d579.png "eventtype")
<br>
2. Enter <span style="color: #dc9656">**SunriseEvent**</span> as the name of the new event type.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/163546931602941f745c54cb0488591fd3c1365f6e2e6.png "2")
<br>
3. Open the **SunriseEvent** script and add Property as follows. We'll use **isSunrise** to check the sunrise in **True / False**.
```lua
Property:
boolean isSunrise = false
```

# Creating Components and Entities to Process Event
Let's make "Vampire vs Hunter" logic with **SunriseEvent**.
Vampires and hunters will receive the **SunriseEvent** during normal combat and movement.
During the **Sunrise** state, hunters will recover HP through the warm sunlight, while vampires will lose HP.
<br>
1. Let's implement the logic of the **Sunrise** state. 
First, right-click **Workspace - MyDesk** and select **Create Scripts - Create Component** to create two new script components. Enter <span style="color: #dc9656">**VampireComponent**</span> and <span style="color: #dc9656">**HunterComponent**</span> as the names.
<br>
2. Place the vampire NPC entities and hunter NPC entities.
In our example, we placed the two NPCs below in **Preset List - NPC**.
    * **Vampire: npc-3842**
    * **Hunter: npc-4064**
![2-3](https://mod-file.dn.nexoncdn.co.kr/bbs/1637219347748447c4479910f480ba61d509d0dbc2779.png "2-3")
<br>
3. Enter <span style="color: #dc9656">**Vampire, Hunter**</span> in the name of the placed NPC.
![2-4](https://mod-file.dn.nexoncdn.co.kr/bbs/1659663899996526b6c428aba43aa8c9761a74a10840a.png "2-4")
<br>
4. Add **VampireComponent** to the vampire entity and **HunterComponent** to the hunter entity.
![2-5](https://mod-file.dn.nexoncdn.co.kr/bbs/16372193694020abcd1a02ddd4758985ac713f77e6700.png "2-5") ![2-6](https://mod-file.dn.nexoncdn.co.kr/bbs/1637224347613343676d468044fea94a656c843eb9b60.png "2-6")

# Handler Logic
Let's write a **HunterComponent** and **VampireComponent** script.
First, we have to make sure the two components can receive **SunriseEvent**.
1. Open the **HunterComponent** and **VampireComponent** script. 
Click the **[+]** button from **Event Handler** to add **SunriseEvent**.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1635299466453111917919dd24abc9a26ab0b1e72ebcd.png "6")
<br>
2. The relay entity for **SunriseEvent** must be set to **map01**.
From the top of the handler, set event broadcaster as **map01**.
![6-1](https://mod-file.dn.nexoncdn.co.kr/bbs/163721938434624dd2b604ea948219e9049d898cac1f6.png "6-1")
<br>
3. Now let's put the logic to handle the **SunriseEvent** received by each.
First, write **HunterComponent** to increase the Hunter's HP with the rising sun.
    ```lua
    Property: 
    [Sync]
    boolean isSunrise = false
    [Sync]
    number Hp = 0
    
    Method: 
    [server Only]
    void OnUpdate(number delta)
    {
        if self.isSunrise == true then --Checks if the sun is up.
            self.Hp = self.Hp + delta --Gains HP while the sun is up.
            log("Hunter Hp : "..self.Hp) --Shows current HP on the Console window.
            if self.Hp >= 200 then self.Hp = 200 end --If HP increased to 200, the increasing stops.
        end
    }
    
    Event Handler: 
    [entity: map01(/maps/map01)]
    HandlerSunriseEvent(SunriseEvent event)
    {
        -- Parameters
        local isSunrise = event.isSunrise
        self.isSunrise = isSunrise
    }
    ```
    <br>
4. Write **VampireComponent** to decrease the Vampire's HP with the rising sun.
    ```lua
    Property: 
    [Sync]
    boolean isSunrise = false
    [Sync]
    number Hp = 0
    
    Method: 
    [server Only]
    void OnUpdate(number delta)
    {
        if self.isSunrise == true then --Checks if the sun is up.
            self.Hp = self.Hp - delta --Loses HP during the sun is up.
            log("Vampire Hp : "..self.Hp) --Shows current HP on the Console window.
            if self.Hp < 0 then self.Hp = 0 end --If HP dropped to 0, the decreasing stops.
        end
    }
    
    Event Handler: 
    [entity: map01(/maps/map01)]
    HandlerSunriseEvent(SunriseEvent event)
    {
        -- Parameters
        local isSunrise = event.isSunrise
        self.isSunrise = isSunrise
    }
    ```
    <br>
5. Set the value of **Hp** to <span style="color: #dc9656">**100**</span> in the Property Editor of both the vampire entity and the hunter entity.
![10-2](https://mod-file.dn.nexoncdn.co.kr/bbs/163729758603854822aa456724b65952386322fb97f20.png "10-2")
![10-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1637297514978d0b38497f6764e5a94c193d8f607908a.png "10-1")

# Logic for Event Occurrence
Let's create logic where the sun rises and sets at a specific time. 
<br>
1. We need a script component to manage the sunrise and sunset times.
In the context menu of **Workspace - MyDesk**, click **Create Scripts - Create Component** to make a new component. Enter <span style="color: #dc9656">**TimeManager**</span> as a name.
<br>
2. Open the **TimeManager** script component. Add the `OnUpdate` function to determine when the sun rises and sets.
    ```lua
    Property: 
   [Sync]
    boolean isSunrise = false
    
    Method: 
    [server only]
    void OnUpdate(number delta)
    {
        if self._T.Time == nil then self._T.Time = 0 end
        self._T.Time = self._T.Time + delta
        
        if self._T.Time >= 5 then --Sun sets and rises at a 5-second interval.
            self._T.Time = 0
            if self.isSunrise == true then
                self.isSunrise = false
            else
                self.isSunrise = true --If the sun is not up, isSunrise = false.
            end
            log(self.isSunrise)
            self:SendEvent(self.isSunrise)
        end
    }
    
    [server]
    void SendEvent(boolean isSunrise)
    {
        local event = SunriseEvent()
        event.isSunrise = isSunrise
        self.Entity:SendEvent(event)
        
        self.isSunrise = isSunrise
        self._T.Time = 0
    }
    ```
    <br>
3. Click **Add Component** in the context menu from **Hierarchy - map01**. Add the **TimeManager** component.
![11-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16596640785471f0b99b7de3f40bb9a25f6f76911431d.png "11-1")
You have now created all the necessary script components. We can send and receive the event of sunrise and sunset at a set interval.
<br>
4. In addition, let's make the Hunter's ultimate skill, Sunrise.
We'll use the Z key to call the event.
Add **KeyDownEvent** in **HunterComponent** and compose its content as follows.
    ```lua
    [service: InputService]
    HandleKeyDownEvent(KeyDownEvent event) 
    {
        -- Parameters
        local key = event.key
        --------------------------------------------------------------------------------
        if key == KeyboardKey.Z then --Press Z, and the message `Sunrise` appears on the Console window.
            log("Sunrise")
            local timeManager = self.Entity.CurrentMap.TimeManager
            timeManager:SendEvent(true) --Make Timemanager's Event set to true.
        end
    }
    ```
<br>
Now press Z, and the sun will rise.