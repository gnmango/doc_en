# Course Introduction

Sometimes you might want to change the target of the camera, zoom in, or zoom out.
In this course, we will explore how to control the target camera using [CameraService](/apiReference/Services/CameraService{"target":"_self"}) and learn about camera transition effects.

##### Reference Guide

[Referring to Entities and Components](docs?postId=164%7B%22target%22:%22\_self%22%7D)
[TriggerComponent to Sense Entity Collision](docs?postId=175%7B%22target%22:%22\_self%22%7D)
[Event System](/docs?postId=73%7B%22target%22:%22\_self%22%7D)
[Entity Event System](/docs?postId=176%7B%22target%22:%22\_self%22%7D)

# Preset

To explore the functionalities of **CameraService**, let's create the following scenario:

* When a character touches an object, the camera's target changes to an NPC or zooms in/out.
* Camera starts to move, and returns to its original position after 5 seconds.
* Upon target change or zoom in/out, Blend effect is adjusted.

For this to work, the following presets are required.
We need to set the map and component.

#### Map Preset

Make a long horizontal map as shown below. Place SpawnLocation on the far left, and NPC on the far right.
Place an object next to SpawnLocation, which will be the trigger for controlling the camera upon contact with the character.
SpawnLocation Entity can be placed from the Special category of the Preset List. Object and NPC can be placed from the Object and NPC categories, respectively.
![cameraservicemapsetting](https://mod-file.dn.nexoncdn.co.kr/bbs/1635747662929b64966a13f79452686e64614e2872887.png)

#### Component Preset

Add the following components to the object and NPC placed above.

| Entity | Additional Component |
| --- | ------- |
| Object | <ul><li>TriggerComponent </li><li>New Script Component <ul><li>Make a new script component and name it CameraControl.</li><li>Make a new script component and add it to Object.</li><li>Implement the staging of camera when a character touches the object.</li></ul></li></ul> |
| NPC | CameraComponent |

Confirm if **DefaultPlayer** also has **CameraComponent** added to it. If the character does not have a **CameraComponent** attached to it, add one.
![cameraservice01](https://mod-file.dn.nexoncdn.co.kr/bbs/165966452699771f41d7f09c44b2a9d600a871338c465.png "cameraservice01")

Next, add **TriggerEnterEvent** to the script component that was added to the object to implement camera control. Set the event sender as **Self**.
![CameraService02](https://mod-file.dn.nexoncdn.co.kr/bbs/16878426310085e90821333e043a9917fef09d35f5ba7.png "CameraService02")

# Changing Camera Target

Now that the preliminary setup is complete, let's explore the functionalities of **CameraService** in more detail. First, let's learn how to set the target of a camera.

##### void SwitchCameraTo(CameraComponent cameraToSwitch)

During gameplay, the camera typically follows the player as the subject (target) and keeps the player in view on the screen. However, to change the camera's subject (target) for genre-specific reasons or special game effects, the camera can make use of the `SwitchCameraTo()` function provided by CameraService.

The `SwitchCameraTo()` function allows you to change the camera's subject (target).
When you want to move the camera away from the default subject, the player, and focus it on a specific entity, you can use the `SwitchCameraTo()` function. When you call the `SwitchCameraTo()` function, it accepts the path of the target entity that has **CameraComponent** attached to it, and it moves the camera's subject to the target entity.

The code below demonstrates how to use the `SwitchCameraTo()` function to change the camera target from the character to an NPC. Add the following content to the **TriggerEnterEvent** of the script component that was added during the preliminary setup.

```lua
[self]
HandleTriggerEnterEvent (TriggerEnterEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: TriggerComponent
    -- Space: Server, Client
    ---------------------------------------------------------
    
    -- Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    ---------------------------------------------------------
    --Retrieve the NPC entity that will serve as the camera's target.
    --The NPC path may vary depending on which NPC was placed.
    local NPCEntity = _EntityService:GetEntityByPath("/maps/map01/npc-13")
         
    --Use the SwitchCameraTo function to change the camera target to the NPC.
    -- Pass the CameraComponent of the NPC entity as a parameter to the SwitchCameraTo function.
    _CameraService:SwitchCameraTo(NPCEntity.CameraComponent)   
}
```

Press Start to check if the camera successfully changes its target.

$$youtube
oh7MVJlYWko
$$

#### number TransitionBlendTime

**TransitionBlendTime** is a property provided by **CameraService** that lets you set the time it takes for the camera to transition to the target subject.
If you set the **TransitionBlendTime** time before calling the `SwitchCameraTo()` function, you will be able to observe the camera smoothly transitioning to the target subject over the specified time. Therefore, by setting the time for **TransitionBlendTime** before calling the `SwitchCameraTo()` function, you can observe the camera smoothly transitioning to the new target over the specified **TransitionBlendTime** duration.
The code below demonstrates assigning a time value to **TransitionBlendTime**, resulting in a faster camera transition.

```lua
[self]
HandleTriggerEnterEvent (TriggerEnterEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: TriggerComponent
    -- Space: Server, Client
    ---------------------------------------------------------
    
    -- Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    ---------------------------------------------------------
    --Retrieve the NPC entity that will serve as the camera's target. Enter the path of the NPC entity placed at NPCPath.
    local NPCEntity = _EntityService:GetEntityByPath("/maps/map01/npc-13") 
    
    -- Set the transition time for the camera to switch to a different target. 
    _CameraService.TransitionBlendTime = 1  
    
    --Use the SwitchCameraTo function to change the camera target to the NPC.
    -- Pass the CameraComponent of the NPC entity as a parameter to the SwitchCameraTo function.
    _CameraService:SwitchCameraTo(NPCEntity.CameraComponent)
}
```

Press Start for a test.

$$youtube
/AfmN8SevPKs
$$

#### CameraBlendType TranstionBlendType

**TranstionBlendType** is a property that allows you to control the camera's movement when transitioning to a new subject (target).
Before calling `SwitchCameraTo()`, you can assign an enum value from CameraBlendType to **TranstionBlendType**, allowing you to change the blend type during the target transition. Below is the added code that assigns CameraBlendType.Linear to **TranstionBlendType** in the previous example, to change the camera's movement to be slightly more rigid.

```lua
[self]
HandleTriggerEnterEvent (TriggerEnterEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: TriggerComponent
    -- Space: Server, Client
    ---------------------------------------------------------
    
    -- Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    ---------------------------------------------------------
    --Retrieve the NPC entity that will serve as the camera's target. "NPCPath" enters the path of each placed NPC entity.
    local NPCEntity = _EntityService:GetEntityByPath("/maps/map01/npc-13")
         
    -- Set the transition time for the camera to switch to a different target.
    _CameraService.TransitionBlendTime = 1
         
    --Set camera movement.
    --Linear moves the camera at a constant speed to transition the view smoothly. 
    -- Since it transitions linearly, it may feel rigid and mechanical. 
    _CameraService.TransitionBlendType = CameraBlendType.Linear
         
    --Use the SwitchCameraTo function to change the camera target to the NPC.
    -- Pass the CameraComponent of the NPC entity as a parameter to the SwitchCameraTo function.
    _CameraService:SwitchCameraTo(NPCEntity.CameraComponent)
}
```

Press Start to test.
You will notice a different movement compared to the example without **TransitionBlendType** configuration.

###### Before Configuring TransitionBlendType

$$youtube
/YJRjVEfeCUQ
$$

###### After Configuring TransitionBlendType

$$youtube
/qV01ccSyGK4
$$

There are six blend types.
The default value of **TranstionBlendType** is **EaseInOut**, and it is automatically assigned if no other type value is specified.

| Type | Explanation |
| --- | --- |
| Cut | Immediately switches the position of the camera to that of CameraComponent. The screen is switched as if it performs a teleportation. TranstitionBlendTime is meaningless when TransitionBlendType is Cut. |
| EaseInOut | Softens the beginning and end of transition like a S-curve. Default value for Blend type. |
| EaseIn | Transition starts linearly and ends smoothly. |
| EaseOut | Transition starts smoothly and ends linearly. |
| HardIn | Transition starts smoothly and ends quickly. |
| HardOut | Transition starts quickly and ends smoothly. |
| Linear | Transition is done at constant speed. The transition will become linear, making it look stiff and machine-like. |

# Zoom Control

Let's learn how to control camera zoom in/out during play.
We won't be needing the code from above, so delete it, and start from below.
```lua
[self]
HandleTriggerEnterEvent (TriggerEnterEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: TriggerComponent
    -- Space: Server, Client
    ---------------------------------------------------------
    
    -- Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    ---------------------------------------------------------
}
```

#### void ZoomTo(number percent, number duration)

The `ZoomTo()` function performs a zoom in or out over a specific duration of time.
It takes **Percent** and **Duration** as parameter.
**Percent** represents the zoom value to be changed, and you pass a percentage value when calling the function.
If the new value is bigger than the current value, it zooms in, and zooms out if the value is smaller.
**Duration** is the time it takes to reach the value passed as a parameter (in seconds) from the current zoom value.

Below is an example code that uses the `ZoomTo()` function to zoom in on the character over a period of 5 seconds.

```lua
[self]
HandleTriggerEnterEvent (TriggerEnterEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: TriggerComponent
    -- Space: Server, Client
    ---------------------------------------------------------
    
    -- Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    ---------------------------------------------------------
    -- Zooms the camera to 150% for 5 seconds.
    _CameraService:ZoomTo(150, 5)
}
```

Press Start to test. Once the character touches the door, the camera will zoom in on the character (target) for 5 seconds.

$$youtube
/AlHk3y0xYCI
$$

#### void ZoomReset()

The `ZoomReset()` function is used to reset the zoom value to its initial value. By using `ZoomReset()`, you can reset the zoom value back to its initial value (100%) even after using `ZoomTo()` to change the zoom.

Continuing from the `ZoomTo()` example, here is example code that uses `ZoomReset()` to change the zoom value back to 100% after 5 seconds.
For clarity, **TimerService** is used. Please refer to the [TimerService](/docs?postId=47%7B%22target%22:%22\_self%22%7D) guide.

```lua
[self]
HandleTriggerEnterEvent (TriggerEnterEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: TriggerComponent
    -- Space: Server, Client
    ---------------------------------------------------------
    
    -- Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    ---------------------------------------------------------
    -- Zoom in to 150% for 5 seconds.
    _CameraService:ZoomTo(150, 5)
     
    -- Create a CallBack function that can be scheduled to run 5 seconds after the zoom-in is complete.
    -- Inside the CallBack function, call ZoomReset.
    local CallBack = function()
        _CameraService:ZoomReset() 
    end
     
    -- By using TimerService and passing the CallBack function to be executed 5 seconds after the zoom-in is complete.
    -- Make sure that the ZoomReset function inside the CallBack function can be executed.
    _TimerService:SetTimerOnce(CallBack, 10)
}
```

Press Start to test. You'll see that the camera zooms to 150% for 5 seconds after touching the door, then comes back to default position after another 5 seconds.

$$youtube
/WlMPj65y5jY
$$

# Other Features

**CameraService** provides several useful features in addition to its core functionalities.

#### Property

**<span style="color: #dc9656">number CurrentZoomRatio (read only)</span>**
The property returns the zoom ratio of the camera into a percentage value. Default value is 100.
Since this property is read-only, you should use the `ZoomTo()` function when you want to set the zoom ratio.

```lua
log("zoom : ".._CameraService.currentZoomRatio)  -- Outputs 100
```

#### Functions

**<span style="color: #dc9656">CameraComponent GetCurrentCameraComponent()</span>**
Brings in CameraComponent of the Entity the camera is currently tracking.

```lua
local cameraComponent = _CameraService:GetCurrentCameraComponent()
log(cameraComponent)
```