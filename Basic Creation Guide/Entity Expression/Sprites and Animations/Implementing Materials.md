# Course Introduction
You can use materials in many other renderer components besides **SpriteRendererComponent**. This allows creators to make their own visually colorful and unique Worlds.
In this lesson, we will look at how to use materials in different components. We'll also look at an example of how to control a material with a script.

# Use Materials in Various Components
You can use materials in many renderer components.
The method is the same as applying a material to **SpriteRendererComponent**.

> <span style="color: #585858">**Learn more**
>You can learn how to create and apply materials in the [Utilizing Materials](/docs/?postId=828{"target":"_self"}) guide.</span>

Materials can be applied to various renderer components. However, some renderer components have limited shader options available for use. To check the available shaders for each component, select the component on the **Shader Picker** window.
![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1727180781072360fe8c368824af7a5df25df3b8429d7.png "13")

# Replacing a Material with a Script
Let's take a look at the example of an entity's material being replaced when a player makes contact.

1. Create a new material and enter <span style="color: #dc9656">**Material01**</span> as the name.

2. Select the entity you want and place it in **Scene**.
    This example used **Preset List - npc-964**.
    ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/166849476370816e1074387b747d684879d5a30fcb8a8.png "10")

3. Press the ![Common_Select](https://mod-file.dn.nexoncdn.co.kr/bbs/1668408907363c261c9faa43d4ee3b131624dd723e3a2.png "Common_Select") button in **Property Editor - SpriteRendererComponent - MaterialID** of **npc-964** and select **Material01** in the **Reference** window.
    ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1668494966129a62b76ffce2740339a667c03d48974c8.png "11")

4. Select **Shader - Outline - InnerOutline** in Property Editor of **Material01**.
    ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/16684932469197db2f5663ffe4856adf503ff2bb3e8e3.png "9")

5. Generate the new script component <span style="color: #dc9656">**MaterialTest**</span> and add it to **npc-964**.

6. Add **TriggerComponent** to **npc-964** to confirm the collision with the player character.

7. Create a new material and enter <span style="color: #dc9656">**Material02**</span> as the name.

8. Select **Shader - ColorEffect - Rainbow** in Property Editor of **Material02**.

9. Click **Copy Entry ID** in the context menu of **Workspace - MyDesk - Material02**.
    ![12](https://mod-file.dn.nexoncdn.co.kr/bbs/166856445652712d2f198c1c94887a49a7b3a9075715f.png "12")
 
10. Open the **MaterialTest** script and add **TriggerEnterEvent** as follows.
    Paste the **Entry ID** of the copied **Material02** from above to **ChangeMaterial**.
    ```lua
    HandleTriggerEnterEvent (TriggerEnterEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: TriggerComponent
        -- Space: Server, Client
        ---------------------------------------------------------
        
        -- Parameters
        -- local TriggerBodyEntity = event.TriggerBodyEntity
        ---------------------------------------------------------
        
        self.Entity.SpriteRendererComponent:ChangeMaterial("material://Entry ID")
        -- Paste Entry ID of Material02 you copied from above to "material://Entry ID" part
    }
    ```

11. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. You can see that the shader changes from **InnerOutline** to **Rainbow** when the player character makes contact.
    ![test](https://mod-file.dn.nexoncdn.co.kr/bbs/1668496643847defbbbec4521479f96f8f9c2fc2768f2.gif "test")

# Material Property Control
You can control material properties by using scripts in the world.
Let's see how to control material properties through an example.

1. Create a world with the "default" template.
    ![0](https://mod-file.dn.nexoncdn.co.kr/bbs/1656419849546d61c1ab96b5d454a971009e8053fe3c1.png "0")

2. Place the objects you want to interact with on the map.
    In this example, we'll use **Preset List - Object - object-273**. 
    Then, change **object-273**'s name to <span style="color: #dc9656">**StarTree**</span>.
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16728125334251c9368198cf54d0a93fc0d2f46d44ce6.png "1")

3. In the context menu of **Workspace - MyDesk**, click **Create Material** to create a new material, and set its name as <span style="color: #dc9656">**NewMaterial**</span>.

4. Set the **Shader** property as **Blurry - Pixel** in the Property Editor of **NewMaterial**.
    ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16733186086991f9a5b0bd0204fccaf1aaa7cbfe781ed.png "2")

5. In the Property Editor of the **StarTree** object, apply **NewMaterial** to **MaterialID** of **SpriteRendererComponent**.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1672813499766954bfde0c3e44620a6138288f25d19d9.png "3")

6. Generate the new script component <span style="color: #dc9656">**MaterialTest**</span> and add the **MaterialTest** component to the **StarTree** entity. 

7. Make the **PixelateSize** property change based on the distance between the player and the object. 
Add the `OnUpdate()` function to the **MaterialTest** component and set the execution space to **client only**. After that, fill in the `OnUpdate()` function as below.
    ```lua
    [client only]
    void OnUpdate(number delta)
    {
        local userPosition = _UserService.LocalPlayer.TransformComponent.Position
        local selfPosition = self.Entity.TransformComponent.Position
         
        local v = Vector2(userPosition.x - selfPosition.x, userPosition.y - selfPosition.y)
        local dist = Vector2.Distance(Vector2.zero, v)
         
        local changeValue = {["PixelateSize"] = dist * 16 + 16;}
        -- Copy and paste the Entry ID of NewMaterial in NewMaterial Entry ID below.
        _MaterialService:ChangeMaterialProperty("NewMaterial Entry ID", changeValue)
    }
    ```

8. You'll need to copy and paste your own created **Entry ID** of **NewMaterial** in **NewMaterial Entry ID** from the code written above.
    In the context menu of **NewMaterial**, click **Copy Entry ID**.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/167331896565459a96e99c98942dab9fb9c85f4013d86.png "8")

    Then, open the **MaterialTest** script and paste it to "NewMaterial Entry ID" of the `OnUpdate()` function.

9. Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test. 
As the player gets closer to the **StarTree** object, notice that the tree becomes increasingly pixelated.
![pixel](https://mod-file.dn.nexoncdn.co.kr/bbs/16733192431971878d51398c04d65bcb4083bbba1cab6.gif "pixel")