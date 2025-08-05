# Course Introduction 
MapleStory Worlds provides many effect resources that were used in MapleStory.
By calling such effects at appropriate times and locations, we can create more exciting games.
In this course, we will learn about how to call effects via script by executing an example where skill effects are exposed whenever a particular key is pressed.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1657073589665cedf2671ce1f46b1966a5a7b8ce7817f.gif "7")
<br>
Looking at the following guides beforehand will be useful in understanding this course.
[Script Editor Orientation](https://mod-developers.nexon.com/docs?postId=56{"target":"_self"})
[Referring to Entities and Components](https://mod-developers.nexon.com/docs?postId=164{"target":"_self"})
# EffectService
Let's examine a service in MapleStory Worlds to call effects, **_EffectService**.
**_EffectService** is a service with an effect control function. Functions to call effects include the `PlayEffect` and `PlayEffectAttached` functions.
Each function's function is as follows:
<br>
##### PlayEffect (string animationClipRUID, Entity instigator, Vector3 position, number zRotation, Vector3 scale, boolean isLoop = False)
* You can call effects in the desired size in a particular fixed location.
* Parameter: 
    * animationClipRUID: RUID of the calling effect resource.
    * instigator: Entity for importing map information. Enter same entity with the map where the effect will be called.
    * position: Enter the location vector where effects will be called.
    * zRotation: Enter rotation value. Enter 0 if you don't need rotation.
    * scale: Enter the vector value of the size of the effect you will call. Basically, you can enter Vector3(1, 1, 1).
    * isLoop: Enter true to loop the effect, or false to play the effect once. 

##### PlayEffectAttached(string animationClipRUID, Entity parentEntity, Vector3 localPosition, number localZRotation, Vector3 localScale, boolean isLoop = False)
* Although it calls effects as `PlayEffect`, it selects the parent entity of the effect that will be called and sets the location of calling based on the location of the parent.
* Parameter: 
    * animationClipRUID: RUID of the calling effect resource.
    * parentEntity: Enter the entity that will be the parent of the effect that will be called. The called effects are created as the child of the entities.
    * localPosition: Enter the location vector value regarding how far it will be called based on the position of the parent entity.
        Basically, if you enter Vector3(0,0,0), effects are created in the same location as the parent entity.
    * localZRotation: Enter how much more it will rotate based on the rotation value of the parent entity.
    * localScale: Enter vector value of the size of the effect you will call. Basically, you can enter Vector3(1, 1, 1).
    * isLoop: Enter true to loop the effect, or false to play the effect once.

We briefly looked into the function that calls effects. Let's dive into the example.
# Making a Key Entering Function
Prior to the function of calling skills, we will first create a function for receiving the event when a key is entered.
1. In the **Workspace**, add a new component, and enter <span style="color: #dc9656">**NewSkill**</span> as its name.
    ![newcomponent](https://mod-file.dn.nexoncdn.co.kr/bbs/16878457449028bebb7ed9b9141288f93dd176baca7c5.png "newcomponent")
    <br>
2. Add a handler to the **NewSkill** component to handle keystroke events.
    Press the **[+]** button and add **KeyDownEvent**.
    <br>
3. Write the code for **HandleKeyDownEvent** as follows.
    ```lua
    [service: InputService]
    HandleKeyDownEvent(KeyDownEvent event) 
    {
        -- Parameters
        local key = event.key
        --------------------------------------------------------
        if key == KeyboardKey.Q then
            log("Q")   
        elseif key == KeyboardKey.W then
            log("W")
        elseif key == KeyboardKey.E then
            log("E")
        elseif key == KeyboardKey.R then
            log("R")
        end
    }
    ```
    <br>
4. In the property of **Workspace - DefaultPlayer**, add the **NewSkill** component.
    ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/163548611402098c3ded642444abfadea659e0d4c11e4.png "4")
    <br>
5. Let's press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and test.
    Try pressing the Q, W, E, R buttons in order and check if log appears in the console window.
    ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/168784592446662b0ed4106354178bf8d999b4e63ce09.png "5")
# Calling Effects When Inputting a Key
This time, we will call the following skill effects whenever each key is entered.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/16570736554871ba7e07a399d41e9ae395e244a86397f.png "6")
<br>
First, we will compose a script with the `PlayEffect` function from the **_EffectService** functions.
1. Modify **HandleKeyDownEvent** of **NewSkill** component as follows.
    ```lua
    [service: InputService]
    HandleKeyDownEvent(KeyDownEvent event) 
    {
        -- Parameters
        local key = event.key
        --------------------------------------------------------
        local transform = self.Entity.TransformComponent
        local x = transform.Position.x
        local y = transform.Position.y
        local z = transform.Position.z
        
        if key == KeyboardKey.Q then
            _EffectService:PlayEffect("f396262ddb6e4d5581360496bb4e9f86",self.Entity,Vector3(x,y,z), 0, Vector3(1,1,1), false)
        elseif key == KeyboardKey.W then
            _EffectService:PlayEffect("aae828e6daa342a996320e32dc5c2ab6",self.Entity,Vector3(x,y,z), 0, Vector3(1,1,1), false)
        elseif key == KeyboardKey.E then
            _EffectService:PlayEffect("0da34c3dd7ba4c2890107d743b8c7a97",self.Entity,Vector3(x,y,z), 0, Vector3(1,1,1), false)
        elseif key == KeyboardKey.R then
            _EffectService:PlayEffect("5f67e1efe0b84d9c9111a21ecfedc937",self.Entity,Vector3(x,y,z), 0, Vector3(1,1,1), false)
        end
    }
    ```
    <br>
2. Let's press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and test.
    Try pressing the Q, W, E, R buttons in order and check if the effect is called.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/16570737747217dd51981074941c29e9f0fcb75018662.gif "8")
# Calling Effects Using PlayEffectAttached
In fact, the effect call method discussed above is not suitable for skill effects. If implemented like the example above, when you move the character after pressing the Q, W, E, or R keys, the character will move out of the effect.  Since `PlayEffect` is a function suitable for calling an effect in a fixed position as mentioned briefly above, it is not suitable for an effect that should always be floating on the character, such as a buff skill. In this case, we use a function called `PlayEffectAttached`.
In case of `PlayEffectAttached`, you can designate the target entity on which the effect will float, the parent entity here, so you can eliminate the above separation of the character and the effect.
Let's modify the script to use `PlayEffectAttached` instead of `PlayEffect` this time.

1. Modify **HandleKeyDownEvent** of **NewSkill** component as follows.
    ```lua
    [service: InputService]
    HandleKeyDownEvent(KeyDownEvent event) 
    {
        -- Parameters
        local key = event.key
        --------------------------------------------------------
        if key == KeyboardKey.Q then
            _EffectService:PlayEffectAttached("f396262ddb6e4d5581360496bb4e9f86",self.Entity,Vector3(0,0,0), 0, Vector3(1,1,1), false)
        elseif key == KeyboardKey.W then
            _EffectService:PlayEffectAttached("aae828e6daa342a996320e32dc5c2ab6",self.Entity,Vector3(0,0,0), 0, Vector3(1,1,1), false)
        elseif key == KeyboardKey.E then
            _EffectService:PlayEffectAttached("0da34c3dd7ba4c2890107d743b8c7a97",self.Entity,Vector3(0,0,0), 0, Vector3(1,1,1), false)
        elseif key == KeyboardKey.R then
            _EffectService:PlayEffectAttached("5f67e1efe0b84d9c9111a21ecfedc937",self.Entity,Vector3(0,0,0), 0, Vector3(1,1,1), false)
        end
    }
    ```
    <br>
2. Let's press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and test.
    Try pressing the Q, W, E, R buttons in order and check if the effect is called.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1657074462609d91dd217137d4049a2aa8ff3aa5df599.gif "9")