# Course Introduction
To create a more realistic world, you will learn how to play and control audio clips appropriately using [**SoundComponent**](/apiReference/Components/SoundComponent{"target":"_self"}) and [**SoundService**](/apiReference/Services/SoundService{"target":"_self"}).

# SoundComponent
To create a more realistic world, you can play sounds here and there and adjust their volume depending on the distance between the player and a given entity. Using **SoundComponent**, you can insert various sounds and play them appropriately for an immersive feeling when interacting with a specific object.
<br>
You can add **SoundComponent** to your entity and play or stop the sound. Audio clips use the **Audio RUID** of **Resource Storage** or the **RUID** of the audio clip added by the creator.
For detailed information about controllable properties, refer to the [**SoundComponent**](/apiReference/Components/SoundComponent{"target":"_self"}) section.

## Example
You can make your world more immersive with new sounds that play when approaching an object or a monster.

### SetListenerEntity function
For one-of-a-kind staging, you can adjust the volume depending on the distance from a given entity with **SoundComponent** and `SetListenerEntity()`.
When a component using this function is attached to a specific entity, the volume increases as it gets closer, and decreases to 0 as it moves away. When the audio clip enters the audible point, the sound is heard again. However, it's not replaying after pausing, but simply changing the volume, and does not immediately follow the last heard part. If the player moves using skills like teleporting, the volume will decrease immediately.
<br>
1. Add **SoundComponent** to a specific object.
2. Create a new script component <span style="color: #dc9656">**ObjectSound**</span> in **MyDesk**.
3. Write the following code after adding the `OnBeginPlay()` function and add it to the entity to which **SoundComponent** has been added.
4. Let's press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test. Check if the volume changes with sideways movements.
```lua
[client only]
void OnBeginPlay()
{
    self.Entity.SoundComponent.AudioClipRUID = "12f37a0d7970482e82ceb45718f92674"
    self.Entity.SoundComponent.Loop = true
    self.Entity.SoundComponent:Play()
    wait(2)
    self.Entity.SoundComponent:SetListenerEntity(_UserService.LocalPlayer)
}
```

$$youtube
/CVZ4WeZVdUI
$$

### SetCameraAsListener property
You can use the **SetCameraAsListener** property. As described in the **SoundComponent** property, enabling **SetCameraAsListener** adjusts the volume of sound according to the distance between the center of the screen and the audio clip.
Let's take a quick look at how to use the **SetCameraAsListener** property.
<br>
1. After placing any object in **Preset List** to **Scene**, enter the following **RUID** to **SpriteRendererComponent - SpriteRUID**.
    * SpriteRUID : <span style="color: #dc9656">**c5cf1b7c8d6447f7979b544e166965fc**</span>
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/167333830964936922246ca3c4abb9779d0b72e0eb37d.png "1")
    <br>
2. Add **SoundComponent** to the object and set as follows.
    | Property | Value |
    |---|---|
    | AudioClipRUID | 74222ba4b41d48dd97175fd691124a67 |
    | HearingDistance | 10 |
    | PlayOnEnable | true |
    | SetCameraAsListener | true |
    | Loop | true |
    <br>
3. Let's press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test. When you get closer to the object you are hammering, you hear a sound effect, and when you get farther away, the sound is muted.
$$youtube
/sdNCAsm5Nj0
$$
<br>
However, remember that even if the **SetCameraAsListener** property is activated, if there is a listener entity specified through the **SetListenerEntity** function, the specified entity becomes the listener.


# SoundService
SoundService is used to call the required function where the creator wishes to control sounds. You can use various functions to play or stop the audio clip or to play it only in a designated area. For detailed functions, please refer to [**SoundService**](/apiReference?postId=316{"target":"_self"}).

## Example
### Adding UI Button Sound Effects
You can make designated sound effects play whenever UI buttons are pressed.

1. Place a button from UI Editor.
2. Create a new script component with a name of <span style="color: #dc9656">**EffectComponent**</span> and add it to the button.
3. Add **ButtonClickEvent** to the event handler to detect the click event. 
4. Enter **AudioClipRUID** and the volume of the audio clip to be played when the button is pressed.

```lua
Event Handler:
[client only]
HandleButtonClickEvent(ButtonClickEvent event) 
{
    -- Parameters
    _SoundService:PlaySound("21600ec9d3a04cfeb69c6fe0c50e197c",1)
}
```

$$youtube
/KA9t1pzrt7Q
$$
### Skill effects and sound effects
You can make the action more dynamic by adding effect skills and sound effects to the default attack skill.

1. Create a new <span style="color: #dc9656">**SkillSound**</span> script component in **MyDesk** and add it to **DefaultPlayer**.
2. Add **KeyDownEvent** to the event handler and write as follows.
```lua
Event Handler:
[clinet only] [service: InputService]
HandleKeyDownEvent(KeyDownEvent event)
{
    -- Parameters
    local key = event.key
    --------------------------------------------------------
    if key == KeyboardKey.LeftControl then
        _SoundService:PlaySound("02fe29e7670c447cb23c4ee4e5bf5674", 0.3)
        _EffectService:PlayEffect("01f39ee5971246ed8306bc471d44a1fd", self.Entity, self.Entity.TransformComponent.Position, 0, Vector3(1,1,1), false)
    end
}
```

$$youtube
/TAfkne24qvw
$$