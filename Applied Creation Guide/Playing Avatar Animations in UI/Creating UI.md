Let's use the AvatarGUIRendererComponent to play an animation for the avatar's status in the UI.
The following guides will be useful.
##### Reference Guide
* [Controlling Avatar Animations](/docs/?postId=820{"target":"_self"})
* [Representing Avatars in UI](/docs/?postId=953{"target":"_self"})
* [Basic UI Components](/docs/?postId=744{"target":"_self"})
* [Utilizing and Controlling UI Editor](/docs/?postId=120{"target":"_self"})
* [StateComponent](/apiReference?postId=363{"target":"_self"})
* [AvatarStateAnimationComponent](/apiReference?postId=596{"target":"_self"})
* [AvatarGUIAnimationComponent](/apiReference?postId=954{"target":"_self"})
# Creating UI
Let's create a UI to display the avatar, weapon list, and the skill to be played.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/17079824109951bed04c2ed4b4012b69dc4c57f18ad9d.png "1")
#### UIAvatarPreview
1. Create a new **UIGroup**.
![UIGroup](https://mod-file.dn.nexoncdn.co.kr/bbs/17090169521825e3d68cd26e944979c8472c3b8c3c40d.png{"width":"230px"} "UIGroup")
2. Create the new **UISprite** and rename it to **UIAvatarPreview**.
3. Change the color of the sprite and place it so that the UI is centered on the screen.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1708654175198c74a720c9e74407a9b78d6b3f39f69ea.png "8")
#### ScrollLayout
Let's create a ScrollLayout UI to display the weapon list. Use the **ScrollLayoutGroupComponent**. 
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1708508082154c0e8843003934594ad12faf4623d700f.png "2")

1. Add the new **UISprite** as a child of UIAvatarPreview. Rename it to **Scroll_Layout**.
2. Add **ScrollLayoutGroupComponent** in **Scroll_Layout**. Set the property as follows and feel free to adjust the color and size. 

    * Type: Vertical
    * Spacing: 10
    * ChildAlignment: UpperCenter 
    * UseScroll: True

#### UISkillEffectRenderer
Let's create a place to play the skill effect animation.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/170851084310770637954036d4ad6bd736a5e834d3d63.png "4")
1. Add the **UISprite** as a child of UIAvatarPreview and rename it to **UISkillEffectRenderer**.
2. Place the UI in the bottom left space. 
![22](https://mod-file.dn.nexoncdn.co.kr/bbs/1709014754846f234aeaa1c3e4dad913cb38c9fe00af4.png "22")
#### Creating UIAvatarGUIRenderer UI
Let's refer to [Represent Avatar in UI](/docs?postId=953{"target":"_self"}) to display avatars in the UI.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1708510862722881045e34f2442ac81d50707d6eb3695.png "5")

1. Add the **UIEmpty** as a child of **UIAvatarPreview**.
2. Rename it to **UIAvatarGUIRenderer** and adjust its size and location.
3. Add the **CostumeManagerComponent, AvatarGUIRendererComponent, AvatarStateAnimationComponent, and StateComponent**. 
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/17079861690667fdb22225fe94ff192a0a224a350feac.png "2")

# Turning the UI Off and On
Let's display the Animation Playback UI when an entity in the Scene collides.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1708649639628370aac60d1284eccb392ddc8a1af381e.gif "2")
1. Select and place entities that may collide with the player. Rename it to **display**.
    * RUID: 5338cb745c644b2d83bf16127b52b292

    ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/170851200810043de2087b6c44720bd4b97e50a7a09e6.png{"width":"340px"} "11")

2. Add the **TriggerComponent** and adjust the Collider to match the size of the entity.
    ![12](https://mod-file.dn.nexoncdn.co.kr/bbs/170851202226265ebaf9bf7ab4e06b7c0a1fd79f7e15c.png "12")

3. Create the new **DisplayComponent** and add it to the **display** entity. 
    ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1708514183195eb0dc30f09b54896b57c45e8eb4a2d3b.png "5")

4. Add the new **ShowingUI** property.

    ```lua
    Property:
    [None]
    EntityRef ShowingUI = /ui/UIGroup/UIAvatarPreview
    ```

5. Write the following so that you can add the OnBeginPlay() function and make the showingUI false.

    ```lua
    Function:
    [client only]
    void OnBeginPlay()
    {
        self.ShowingUI.Enable = false
    }
    
6. Add the new ToggleUI() function and add the boolean-type **show** parameter.
    
    ```lua
    [client only]
    void ToggleUI (boolean show)
    {
        self.ShowingUI.Enable = show
    }
    ```

7. Add the **TriggerEnterEvent and TriggerLeaveEvent** and write the following to turn the UI on and off when each event occurs.

    ```lua
    Entity Event Handler:
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
          
        if self:IsClient() then
            self:ToggleUI(true)
        end
    }
    
    [self]
    HandleTriggerLeaveEnter (TriggerLeaveEnter event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: TriggerComponent
        -- Space: Server, Client
        ---------------------------------------------------------
    
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        ---------------------------------------------------------
        
        if self:IsClient() then
            self:ToggleUI(false)
        end
    }
    ```
    
8. Press the **[Start]** button to confirm that the UI appears when the player collides with the entity.