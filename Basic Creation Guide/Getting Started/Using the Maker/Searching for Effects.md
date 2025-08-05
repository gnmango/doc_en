# Searching Resources 
At the moment, you can search for resources on both the web and in the Maker.

* [MSW Resources](https://maplestoryworlds.nexon.com/resource{"target":"_self"})<br>![resource02](https://mod-file.dn.nexoncdn.co.kr/bbs/16751334115260cc58291b0c846748ebae5c0e994c7d3.png{"width":"600px"} "resource02")<br>
* Maker's **Resource Storage** panel <br>![resource01](https://mod-file.dn.nexoncdn.co.kr/bbs/165957603963230101d3cac97480285f5496cd808cd84.png "resource01")

# Finding Resources by Skill Name
The Search Creation Resources page offers various tags, but the function needs improvements in accuracy. The keyword with the highest search accuracy is the <span style="color: #dc9656">**skill's unique name**</span>.

For instance, if you search for <span style="color: #dc9656">**Warrior Leap**</span>, a unique skill name, you'll find exactly what you are looking for.
![skill](https://mod-file.dn.nexoncdn.co.kr/bbs/167636060338538409298c06b4eb98587e1aa33d56821.png "skill")

> <span style="color: #7cafc2">If you do not know the exact skill name, you can get help from [Link](https://maplestory.nexon.com/Guide/GameInformation/Skill/4thPromotionAndHyperSkill{"target":"_self"}).</span>
> <span style="color: #7cafc2">Please use as reference.</span>
> <span style="color: #7cafc2">You can check for names from creator communities or by playing MapleStory.</span>

When you search for resources from the Sprite category, there will be more hits than in the Animation category, making it difficult to find the resource you want.
That's why we recommend using tags to find animation and narrow down the result first, before searching by unique names to find sprites.
# Finding Resources Using Tags
You can find most resources with tags.
For example, let's see how to find resources using tags, while looking at resources related to **Piercing Arrow**, a skill of Bowmen in MapleStory.

1. First, search <span style="color: #dc9656">**Piercing Arrow**</span> to find the resource.
![tag01](https://mod-file.dn.nexoncdn.co.kr/bbs/1675135197113573e4d6451c0470f932a9515d4543df8.png "tag01")
<br>
2. If you found the resource you were looking for, click on it to see the details, and then check the <span style="color: #dc9656">**#resource type#unique ID(#skillname#3221017)**</span> among the related tags.
    ![tag02](https://mod-file.dn.nexoncdn.co.kr/bbs/16753183379266aec5741cb224e13947d6f2080216440.png "tag02")
    Searching by clicking on this tag only returns results associated with the unique ID, making it easy to find resources.
    <br>
3. Perform a secondary search by skill-specific ID tag and find the desired resource.
![tag03](https://mod-file.dn.nexoncdn.co.kr/bbs/167531860314011c6b60710bc432b9d4753738df23526.png "tag03")
# Utilizing Skill Resources
All resources offered by MapleStory Worlds have their own unique <span style="color: #dc9656">**32-digit-RUID**</span>.
Creators can use this RUID to utilize resources from components or scripts.
![ruid](https://mod-file.dn.nexoncdn.co.kr/bbs/167531879574349dcb3bda0374ab093c179dbed6ce541.png "ruid")

Representatively, RUID is used for **SpriteRendererComponent, SpriteGUIRendererComponent, and EffectService**.
Pressing the ![copy](https://mod-file.dn.nexoncdn.co.kr/bbs/16747829119867539c98fe77f4fabba798c44bc837130.png "copy") **[Copy]** button next to RUID allows you to copy RUID easily.

#### SpriteRendererComponent
Most entities placed in the world and with images have **SpriteRendererComponent**.
If you assign the found resource's RUID to the value of the **SpriteRUID** property from **SpriteRendererComponent**, the image will change. If the input **RUID** is an animation, it will loop on after playing to **EndFrameIndex** at the speed of the **PlayRate** property.
You can see the detailed information in [SpriteRendererComponent](/apiReference?postId=385{"target":"_self"}).

![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1637746312852d02d7530fb0e4a4496989389875caa14.png "7")
#### SpriteGUIRendererComponent
Most entities placed in the UI and with images have **SpriteGUIRendererComponent**.
If you assign the found resource's RUID to **SpriteGUIRendererComponent**'s **ImageRUID** property, the image will change.
You can use it in a similar way as **SpriteRendererComponent**, but you need to change the **PreserveSprite** property's value to **NativeSize** to use it in its original size and ratio.
If you set the **PreserveSprite** property value as **NativeSize**, you can adjust the effect's size and offset with the **LocalScale** and **LocalPosition** property.
You can see the detailed information in [SpriteGUIRendererComponent](/apiReference?postId=387{"target":"_self"}).

![8](https://mod-file.dn.nexoncdn.co.kr/bbs/163774632551686f93d4782234199ae25cd8b6b55a4a8.png "8")
#### EffectService
You can use **EffectService** to call a resource with a script.
For the more detailed example, see [EffectService's Examples](/apiReference?postId=304{"target":"_self"}).

The example of use is as follows.
```lua
_EffectService:PlayEffect("f396262ddb6e4d5581360496bb4e9f86",self.Entity,Vector3(x,y,0), 0, Vector3(1,1,1))
```

##### Checking the Animation frame You'd Like to Use
To implement an attack determination at a specific timing when using an animation effect, you can use `SpriteAnimPlayerChangeFrameEvent`.

The example of use is as follows.
```lua
Event Handler:
[self]
HandleSpriteAnimPlayerChangeFrameEvent(SpriteAnimPlayerChangeFrameEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: ClimbableSpriteRendererComponent
    -- Space: Client
    ---------------------------------------------------------
    -- Sender: SpriteRendererComponent
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    -- local FrameIndex = event.FrameIndex
    -- local ReversePlaying = event.ReversePlaying
    -- local TotalFrameCount = event.TotalFrameCount
    -- local AnimPlayer = event.AnimPlayer
    ---------------------------------------------------------
    
    if FrameIndex == 7 then
        log("Triggers attack detection at frame 7.")
    end
}
```

##### Reference Guide
[Let's Sparkle with Skill Effects!]
