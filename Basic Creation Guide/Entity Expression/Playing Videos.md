# Course Introduction
You can import and play your videos, or import videos that you'd like to share.
# YouTube-related Components
You can use a video as UI or place it in the world. YoutubeGUIPlayerComponent is used when a video is used as UI, and YoutubePlayerWorldComponent is used when placed in the world. Using it as UI will fix the video at a single location. It will stay at that position even if the user wanders around the map. Using it in the world will fix the video at a placed location, and will basically stay there even when the player moves around.
![youtube](https://mod-file.dn.nexoncdn.co.kr/bbs/1639617553145463b36f7117043408f8c0fea93f888fe.gif "youtube")
> <span style="color: #7cafc2">**Tip**
>Even if you set the video to be played immediately, it will be played when you test the game. </span>

#### YoutubePlayerWorldComponent Property
| Property |Explanation  |
| --- | --- |
| ControllerScale | Sets the size of the buttons and the seek bar to control the video.|
| IgnoreMapLayerCheck| Activating it will disable the function to designate SortingLayer with the name shown in the map layer. |
| LocalScale | Sets the video size. |
| OrderInLayer | Aligns layer. You can set the order of entities within a layer.|
| ShowController | Shows UI to control video. |
| SortingLayer| SortingLayer will decide the display priority when 2 or more YoutubePlayers overlap.|
| Loop | Plays the video repeatedly.|
| PlayOnAwake | Activating it will auto-play the video upon entering the world.| 
| Shuffle | Activating it will shuffle the playing sequence of videos. | 
| Urls| Enter the URL of the video to play. <br><li>Size: The input number will be how many video Urls you can enter.| 
#### YoutubePlayerGUIComponent Property
| Property | Explanation |
| --- | --- |
|Loop| Activating it will play the video repeatedly.|
|PlayOnAwake | Activating it will auto-play the video upon entering the world.|
|Shuffle| Activating it will shuffle the sequence of videos.|
|Urls| Enter the URL of the video to play. <br><li>Size: The input number will be how many video Urls you can enter.
# Playing Video in the World
1. Select <span style="color: #dc9656">**![Tab_Workspace](https://mod-file.dn.nexoncdn.co.kr/bbs/1634542479186400047df886b445e959cb1503f312508.png "Tab_Workspace") Workspace - ![workspace_NativeFolder](https://mod-file.dn.nexoncdn.co.kr/bbs/1634598960433b8c2accd227347b29b0efb94f1320156.png "workspace_NativeFolder") NativeModel - ![workspace_Model](https://mod-file.dn.nexoncdn.co.kr/bbs/16345998699992db91451454e4357a1a26f2fc8d94b61.png "workspace_Model") YoutubePlayerWorld**</span> to place it.<br>![youtube01](https://mod-file.dn.nexoncdn.co.kr/bbs/1638869874178184b15bdee814320afc34e98cac68bdf.png "youtube01")
2. Enter 2 for Size, and enter the video Url. 
    * https://www.youtube.com/watch?v=hvzp4kOtaSw
    * https://youtu.be/3Kom6vz6nic
3. Set the property value, then click ![Common_SoundPlay](https://mod-file.dn.nexoncdn.co.kr/bbs/1635317657654c59c47ffc44d414db579b8d2fc0715a8.png "Common_SoundPlay") Start to check.<br>![youtube02](https://mod-file.dn.nexoncdn.co.kr/bbs/163886989504016bb3fe6c3374db3ae0875f29add252f.png "youtube02")
# Playing Video with GUI
1. Switch to ![TabUI](https://mod-file.dn.nexoncdn.co.kr/bbs/16345246682320aa0a7ca88a64b33b1bdeddb56363f63.png "TabUI") UI Editor and select the <span style="color: #dc9656">**![Tab_Asset](https://mod-file.dn.nexoncdn.co.kr/bbs/1634542426491c95a16f3ea3e4ffcb78909e4c6bbbd3d.png "Tab_Asset") YouTube model in Preset List**</span> to place it on the screen.<br>![youtube15](https://mod-file.dn.nexoncdn.co.kr/bbs/1639102697425418eca0a352b42ab89c28ba2ce5e2a15.png "youtube15")
2. Enter value of 2 for Size, and enter the video Url.
    * https://youtu.be/3Kom6vz6nic
    * https://www.youtube.com/watch?v=hvzp4kOtaSw
3.  Set the property value, then click ![Common_SoundPlay](https://mod-file.dn.nexoncdn.co.kr/bbs/1635317657654c59c47ffc44d414db579b8d2fc0715a8.png "Common_SoundPlay") Start to check.<br>![youtube03](https://mod-file.dn.nexoncdn.co.kr/bbs/1638871880838e765a1ff0c754da9b2396f95522c81c0.png "youtube03")

# Function Use Example
Video will be played from 00:20 upon world entrance, and entering a specific key while playing will trigger a function.
You can use YoutubePlayerWorldComponent and YoutubePlayerGUIComponent functions in the same way. The example will use YoutubePlayerWorldComponent. 
Example Url: https://www.youtube.com/watch?v=9pL_b1fl9LQ&t=1s

1. Make a YoutubePlayerController script component, and add it to the placed YouTube model.<br>![youtube20](https://mod-file.dn.nexoncdn.co.kr/bbs/16391219258191430dbcee5304feb98a12e950231b349.png "youtube20")
2. Let's compose a script that will control the video when a certain key is entered.
    * Video plays automatically from 00:20 upon world entrance
    * Pressing the P key stops the video
    * Pressing the L key stops the pauses and resumes the video
    * Pressing the 0 key jumps back to the start of the video

$$youtube
/wKeMhEK2p8Q
$$

><span style="color: #b8b8b8">**Learn more**
>Because the execution control of all functions is configured using ClientOnly, not all users are seeing the same screen at a given time.
>If you'd like the users to see the same part of the video simultaneously, you need to synchronize.</span>
#### Playing Video from a Certain Point
Set video's starting point as 00:20 and play from there.
```lua
[client only]
void Jump(int timeInSeconds, string targetUserId)
{
    void OnBeginPlay()
    self.Entity.YoutubePlayerWorldComponent:Jump(20.0)
}
```

#### Press P to Stop Video
```lua
[client only]
void Pause(string targetUserId) 
{
    void Pause() 
    self.Entity.YoutubePlayerWorldComponent:Pause()
}
```

#### Press 0 to Play from the Beginning 
```lua 
[client only]
void PlayFromStart()
{
  self.Entity.YoutubePlayerWorldComponent:Play()
    self.Entity.YoutubePlayerWorldComponent:Jump(0)
}
```

#### Press L to Pause and Resume Video
```lua 
[client only]
void PlayPause(string targetUserId)
{
    self.Entity.YoutubePlayerCommonComponent:PlayPause()
}
```


#### Utilizing KeyDownEvent
For a key input event, you need to utilize KeyDownEvent from <span style="color: #dc9656">**EntityEventHandler**</span> as shown below.
Write a function that will be executed upon key input in Event Handler.
```lua
[Service: InputService] 
HandleKeyDownEvent(KeyDownEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: InputService
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    -- local key = event.key
    ---------------------------------------------------------

    local keyStr = tostring(key)
    if keyStr == "P" then
        self:Pause()
    elseif keyStr == "Alpha0" or keyStr == "Keypad0" then
        self:PlayFromStart()
    elseif keyStr == "L" then
        self:PlayPause()
    end
}
```

Composing script with each component will look like the followings. 
* Left: YoutubePlayerGUIComponent
* Right: YoutubePlayerWorldComponent
![youtube20](https://mod-file.dn.nexoncdn.co.kr/bbs/1639051697502ac3f6036d2984b4098b7f24116e3a2eb.png "youtube20")<span style="color: #7cafc2">