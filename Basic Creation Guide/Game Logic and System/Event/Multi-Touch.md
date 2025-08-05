# Course Introduction
Multi-touch refers to the action of two or more pointers (such as fingers) touching the device screen at the same time. In this course, we'll walk through the examples and learn how to utilize the multi-touch actions.

# Multi-Touch
In MapleStory Worlds, you can recognize and utilize multi-touch actions in your worlds by using `ScreenTouch`-type events or `Touch`-type events.
<br>
The `ScreenTouch`-type events below occur at [InputService](/apiReference/Services/InputService{"target":"_self"}) when you touch the screen.
* [ScreenTouchEvent](/apiReference/Events/ScreenTouchEvent{"target":"_self"})
* [ScreenTouchHoldEvent](/apiReference/Events/ScreenTouchHoldEvent{"target":"_self"})
* [ScreenTouchReleaseEvent](/apiReference/Events/ScreenTouchReleaseEvent{"target":"_self"})
<br>

The `Touch`-type events below occur only when the entity includes [TouchReceiveComponent](/apiReference/Components/TouchReceiveComponent{"target":"_self"}).
* [TouchEvent](/apiReference/Events/TouchEvent{"target":"_self"})
* [TouchHoldEvent](/apiReference/Events/TouchHoldEvent{"target":"_self"})
* [TouchReleaseEvent](/apiReference/Events/TouchReleaseEvent{"target":"_self"})
<br>

When you create a world that uses multi-touch actions, you can refer to the descriptions above as a guide to utilize the events for your intended use.

## Example
Let's look at an example where we touch and move one or more entities with **TouchReceiveComponent** so that the entities can move accordingly.

1. From **Preset List - Object**, place two entities of your choice in the **Scene**. This example used <span style="color: #dc9656">**object-44**</span> and <span style="color: #dc9656">**object-49**</span>.
    ![01](https://mod-file.dn.nexoncdn.co.kr/bbs/1684290735891fcf50ce912fe48cf8999f06b6f04c0b8.png "01")
    <br>
2. In this example, we will use `TouchEvent`, `TouchHoldEvent`, and `TouchReleaseEvent`. These events occur only when the entity includes **TouchReceiveComponent**. 
    Therefore, we need to add **TouchReceiveComponent** to the two entities (object-44, object-49).
    ![02](https://mod-file.dn.nexoncdn.co.kr/bbs/1684300809932fceceea870bf4bdf8b179f481124f4f8.png "02")
    <br>
3. Create a new script component <span style="color: #dc9656">**MultitouchTest**</span>.
    Add this component to the two entities (object-44, object-49).
    ![03](https://mod-file.dn.nexoncdn.co.kr/bbs/16843009914475f72fc6fb811406eac45f2d96ac8d237.png "03")
    <br>
4. Open the **MultitouchTest** script and add the following property:
    ```lua
    Property:
    [None]
    SyncTable<integer> index
    [None]
    any handler = nil
    ```
    <br>
5. Add `TouchEvent` to the event handler.
    When a touch event occurs, call the `AddHandler()` function and pass on **TouchId**.
    ```lua
    Event Handler:
    [self]
    HandleTouchEvent(TouchEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: TouchReceiveComponent
        -- Space: Client
        ---------------------------------------------------------
         
        -- Parameters
        local TouchId = event.TouchId
        local TouchPoint = event.TouchPoint
        ---------------------------------------------------------
        self:AddHandler(TouchId)
    }
    ```
    <br>
6. Add `TouchHoldEvent` as well.
    Even when a touch action remains, call the `AddHandler()` function and pass on **TouchId**.
    ```lua
    Event Handler:
    [self]
    HandleTouchHoldEvent(TouchHoldEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: TouchReceiveComponent
        -- Space: Client
        ---------------------------------------------------------
         
        -- Parameters
        local TouchId = event.TouchId
        local TouchPoint = event.TouchPoint
        ---------------------------------------------------------
        self:AddHandler(TouchId)
    }
    ```
    <br>
7.  Add the `AddHandler()` function.
    Add the **integer** type of **touchId** as a parameter.
    Connect the `ScreenTouchHoldEvent` and `Move()` functions with each other.
    To unsubscribe later, you should have the handler of the connected event as a property.
    ```lua
    Method:
    [client]
    void AddHandler(integer touchId)
    {
        if self.handler ~= nil then
            return
        end
         
        table.insert(self.index, touchId)
        self.handler = _InputService:ConnectEvent(ScreenTouchHoldEvent, self.Move)
    }
    ```
    <br>
8. Add the `Move()` function.
    `Move()` receives a parameter of `ScreenTouchHoldEvent`, so it adds **any** type of **screenholdEvent**.
    When a user touches and moves an entity, this function should move the entity with that action.
    ```lua
    [client]
    void Move(any screenholdEvent)
    {
        local touchId = screenholdEvent.TouchId
        local touchPoint = screenholdEvent.TouchPoint
         
        local string
        for i =1, #self.index do
            if self.index[i] == touchId then
                self.Entity.TransformComponent.WorldPosition = _UILogic:ScreenToWorldPosition(touchPoint):ToVector3()
                break
            end
        end
    }
    ```
    <br>
9. Add `TouchReleaseEvent` as well.
    Unsubscribe from the event handler you had on `ScreenTouchHoldEvent` when no more entities are touched.
    ```lua
    Event Handler:
    [self]
    HandleTouchReleaseEvent(TouchReleaseEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: TouchReceiveComponent
        -- Space: Client
        ---------------------------------------------------------
         
        -- Parameters
        local TouchId = event.TouchId
        local TouchPoint = event.TouchPoint
        ---------------------------------------------------------
        for i =1, #self.index do
            if self.index[i] == TouchId then
            table.remove(self.index, i)
                if #self.index <= 0 then
                    _InputService:DisconnectEvent(ScreenTouchHoldEvent, self.handler)
                    self.handler = nil
                end
                break
            end
        end
    }
    ```
    <br>
10. Let's press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and test.
On PC, you can move an entity by clicking and dragging it with the mouse.
On mobile devices, you can also touch and move two entities at once using the touch screen.
![multitouch](https://mod-file.dn.nexoncdn.co.kr/bbs/16843006146740a5849ed9f2b4aae9ef3d48af36b18d5.gif "multitouch")