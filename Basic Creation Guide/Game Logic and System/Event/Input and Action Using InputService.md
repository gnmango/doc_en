# Course Introduction
All games proceed with the user's input and feedback for that input. MapleStory Worlds can contol actions that respond to user input through InputService.
Here, we will learn how to add actions following a user's input.
# Reference Guide
[Event System](https://mod-developers.nexon.com/docs?postId=73{"target":"_self"})
[Entity Event System](https://mod-developers.nexon.com/docs?postId=176{"target":"_self"})

# InputService
When users make a specific input in MapleStory Worlds, **InputService** triggers events based on the player's input. Specific actions can be triggered through the event handler.
The user's input event only takes place in the client, so each event handler is executed in the client as well.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1656484816187fe9902929f28472d9b0cfb1472e42b89.png "1")

#### Event Handler Added
Event handlers for inputs can be added from the script component.

1. Add a temporary entity after creating a script component. 
2. Press the [+] button from **Event Handler** to open the search event window and select an event to be added to the handler.
![3-1](https://mod-file.dn.nexoncdn.co.kr/bbs/16564848334143ca6dd5a61364269b6e867fec890d184.png "3-1")

3. When the handler is added, check if the event sender is selected as **InputService**.
![4-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1635337530912f087a6d685594d2face6f4fd8080cf3a.png "4-1")

# Traits for Each Event Type
Each event related to user inputs provided from `InputService` has a different trigger point and count based on the input made.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/163533908473345305a0d43684fbe93435fb968f297f8.png "5")

#### KeyDownEvent
`KeyDownEvent` is an event that's triggered when a key is pressed. If a `KeyDownEvent` is triggered, a **HandleKeyDownEvent** is executed. `KeyDownEvent` will be sent as the handler's parameter, and the user's input key willl be sent as enum in the event.
Next is an example of displaying the log when pressing the `T` key.

```lua
Event Handler:
[service: InputService]
HandleKeyDownEvent(KeyDownEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: InputService
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    local key = event.key
    ---------------------------------------------------------
    
    if key == KeyboardKey.T then
        log("Press T!")
    end
}
```

![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1687831927516aca6074b6ce04c4cad71ca952bf27f52.png "8")

You can control the Key input using the `IsKeyPressed()` and `IsAnyKeyPressed()` functions from InputService without the use of an event handler.

* `IsKeyPressed()` returns true when the key is pressed and false when it isn't.
* `IsAnyKeyPressed()` returns true if any key is input at any time, and false when no keys are pressed.
    
Next is checking the key inputs using two functions.

```lua
Method:
[client only]
void OnUpdate(number delta)
{
    local enterKeyPressed = _InputService:IsKeyPressed(KeyboardKey.Return)
    local anyKeyPressed = _InputService:IsAnyKeyPressed()

    if enterKeyPressed == true then
        log ("Enter key pressed.")
    end

    if anyKeyPressed == true then
        log ("Any key pressed.")
    end
}
```
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17435591317669f3c092550504c2bb292862d22f84ade.png "KeyPressedLog")

#### KeyUpEvent
`KeyUpEvent` is an event that's triggered 1 time when a key is released after being pressed. **HandleKeyUpEvent** is executed when a `KeyUpEvent` is triggered, and it's executed once when the event is triggered. KeyUpEvent is sent as a handler's parameter, and the user's key values are included as the enum value for the `KeyboardKey` type for events.
Next is an example of outputing a log when pressing and then releasing the `T` key.

```lua
[service: InputService]
HandleKeyUpEvent(KeyUpEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: InputService
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    local key = event.key
    ---------------------------------------------------------
    
    if key == KeyboardKey.T then
        log("Release T!")
    end
}
```

![10](https://mod-file.dn.nexoncdn.co.kr/bbs/168783201564551e95fb513c84de492d85265720ecda0.png "10")

#### KeyHoldEvent
KeyHoldEvent is triggered each frame while a key on the keyboard is held, and the HandleKeyHoldEvent is also executed each time an event is triggered. KeyHoldEvent is sent as a handler's parameter and includes the user's key input information like other events.
Next is an example of outputting a log while a key is being held.

```lua
[service: InputService]
HandleKeyHoldEvent(KeyHoldEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: InputService
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    local key = event.key
    ---------------------------------------------------------
    
    if key == KeyboardKey.T then
        log("Hold T!")
    end
}
```

![12](https://mod-file.dn.nexoncdn.co.kr/bbs/1687832098369dbf2798f3dbb4658b96fe2923bc09b03.png "12")
#### KeyReleaseEvent
`KeyReleaseEvent` is an event that triggers once when a key is released after making an input. **HandleKeyReleaseEvent** is also executed when the event is triggered.
It's similar to `KeyUpEvent`, but if you wish to add an action upon entering Release status from the Hold status for even a single frame, `HandleKeyReleaseEvent` is used. `KeyReleaseEvent` is sent as the handler's parameter and enters including the user's key input information.
Next is an example of outputting a log when releasing a key after holding it down.

```lua
[service: InputService]
HandleKeyReleaseEvent(KeyReleaseEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: InputService
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    local key = event.key
    ---------------------------------------------------------
    
    if key == KeyboardKey.T then
        log("Release T!")
    end
}
```

![14](https://mod-file.dn.nexoncdn.co.kr/bbs/1687832181689b1e8caaae89744ebb1237a697880ffb9.png "14")
#### ScreenTouchEvent
`ScreenTouchEvent` is an event triggered once when the screen is clicked or tapped. **HandleScreenTouchEvent** is also executed when the event is triggered. `ScreenTouchEvent` is sent as the handler's parameter and includes the coordinates (Vector2) of the tap on the screen.
Next is an example of outputting the tap location coordinates onto the console window.

```lua
[service: InputService]
HandleScreenTouchEvent(ScreenTouchEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: InputService
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    -- local TouchId = event.TouchId
    local TouchPoint = event.TouchPoint
    ---------------------------------------------------------
    
    log("Touch Point : "..tostring(TouchPoint))
}
```

![16](https://mod-file.dn.nexoncdn.co.kr/bbs/1687832268666958cebf85bba4b81954c6b7335523127.png "16")
#### ScreenTouchHoldEvent
`ScreenTouchHoldEvent` is triggered for every frame the screen is tapped.
**HandleScreenTouchHoldEvent** is executed each time the event is triggered. `ScreenTouchHoldEvent` is sent as the handler's parameter and includes the coordinate information of the tap.
Here is an example of outputting the tap's coordinates on the console window when it is being held.

```lua
[service: InputService]
HandleScreenTouchHoldEvent(ScreenTouchHoldEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: InputService
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    -- local TouchId = event.TouchId
    local TouchPoint = event.TouchPoint
    ---------------------------------------------------------
    
    log("Touch Point : "..tostring(TouchPoint))
}
```

![18](https://mod-file.dn.nexoncdn.co.kr/bbs/1687832343471d67ee1e1564d4caf9cd6c15c13c8cf10.png "18")

#### ScreenTouchReleaseEvent
`ScreenTouchReleaseEvent` triggers once when releasing the screen after holding it. **HandleScreenTouchReleaseEvent** is also executed when the event is triggered. `ScreenTouchReleaseEvent` is sent as the handler's parameter and includes the coordinates of where the release was.
Next is an example outputting the last tap coordinates.

```lua
[service: InputService]
HandleScreenTouchReleaseEvent(ScreenTouchReleaseEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: InputService
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    -- local TouchId = event.TouchId
    local TouchPoint = event.TouchPoint
    ---------------------------------------------------------
    
    log("Touch Point : "..tostring(TouchPoint))
}
```

![20](https://mod-file.dn.nexoncdn.co.kr/bbs/168783240469618ce325044bd41799cc760387afe94b9.png "20")