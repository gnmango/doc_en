# Course Introduction
When you create a world and need a sprite, you can choose what MapleStory Worlds provides, or import and use what you've created on your own as a creator. You may also upload an image generated during your world play to the world and share it with other users. 
Images generated during play in MapleStory Worlds are called **dynamic sprites**. You can upload a **dynamic sprite** to your world by containing it in the data type **RawImage** and use it by issuing an **RUID**.
<br>
In this course, we're going to study an example of using the `CaptureFullScreenAsPixelDataAndWait()` function of [ScreenshotService](/apiReference/Services/ScreenshotService{"target":"_self"}) and returning the captured screenshot in **RawImage** format. This example will allow you to understand how to utilize dynamic sprites.

# Showing Screenshot to the World
Let's look at an example of pressing a specific key to capture a screenshot and placing it in a particular location.

1. Select **Hierarchy - Create Entity - Create Empty** to create **EmptyEntity**.
    Choose <span style="color: #dc9656">**Frame**</span> as the name of **EmptyEntity**.
    ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1668661121252c9d0293976864189952c8b9700eb8368.png "6")
    
2. Add **TransformComponent, SpriteRendererComponent** in the Property Editor of the **Frame** entity.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1679371574207593d86d4363d41f990a861fb084b13b7.png "8")
    
3. Set the desired sprite to **SpriteRendererComponent - SpriteRUID** in the Property Editor of the **Frame** entity.
    e.i.) SpriteRUID : <span style="color: #dc9656">**93c2673261874c76b55459a4add2c382**</span>
    ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16685976953935f7b707c48a74bd8bf90747904f83a34.png{"width":"400px"} "2")
    
4. In the context menu of **Hierarchy - Frame**, click **Create Entity as child - Create Empty** to generate a child entity.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16862000936226739727d0fd544f681d63208b2fcd4e7.png "3")
    Choose <span style="color: #dc9656">**Canvas**</span> as the name of **EmptyEntity**.
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/16686612052390bc3806fb1234ebfbd94ca85473612bc.png "7")
    
5. Add **TransformComponent, SpriteRendererComponent** in the Property Editor of the **Canvas** entity.
    
6. In the **Scene**, drag the **Canvas** entity to the center of the **Frame**.
    ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1668598281353fa2830c39674415282afcba0cf2f041f.png "4")
    
7. Create a new script component and name it <span style="color: #dc9656">**SpriteUploadTest**</span>.
    
8. Select **Hierarchy - Common** and then add the **SpriteUploadTest** component in the Property Editor. 
    
9. Open **SpriteUploadTest** and add **Property** in the Script Editor as below.
    ```lua
    Property:
    [None]
    Entity FrameEntity = /maps/map01/Frame
    [None]
    Entity CanvasEntity = /maps/map01/Frame/Canvas
    ```

10. Add the `OnBeginPlay()` function and compose its content as follows.
    The `SetSpriteUploadValidationCallback()` function decides whether to allow an upload request sent by a user. Write a code that returns **true** if you'd like to allow the request, and **false** if not. In this example, we need to allow the upload, so let's make it return **true**.
    ```lua
    Method:
    void OnBeginPlay()
    {
        if self:IsServer() then
            -- Set it as true to allow an upload request from a user.
            _ResourceService:SetSpriteUploadValidationCallback(function(userId)
                return true
            end)
            
            return
        end
    }
    ```

11. Add the `CaptureAndUpload()` function and compose its content as follows.
    ```lua
    [client only]
    void CaptureAndUpload()
    {
        local frameRUID = self.FrameEntity.SpriteRendererComponent.SpriteRUID%%
        
        local sprite = _ResourceService:LoadSpriteAndWait(frameRUID)
        -- Check the horizontal and vertical pixels of the frame entity sprite.
        local frameWidth = sprite.Width
        local frameHeight = sprite.Height
        
        -- Receive the captured screenshot image.
        local error, screenshot = _ScreenshotService:CaptureFullScreenAsPixelDataAndWait()
        
        -- Check the horizontal and vertical pixels of the screenshot.
        local screenshotWidth = screenshot.Width
        local screenshotHeight = screenshot.Height
        
        -- Calculate the ratio for resizing the screenshot.
        -- The screenshot should fit in the Frame, so make it a little smaller.
        local horizontalRatio = frameWidth / screenshotWidth * 0.96
        local verticalRatio = frameHeight / screenshotHeight * 0.67
        
        -- Upload the screenshot image in sprite format.
        _ResourceService:RequestSpriteUploadAsync(screenshot, function(error, ruid)
        	self.CanvasEntity.SpriteRendererComponent.SpriteRUID = ruid
        	-- Enter the ratio calculated above and adjust the scale of the Canvas.	
        	self.CanvasEntity.TransformComponent.Scale = Vector3(horizontalRatio, verticalRatio, 1)
        end) 
    }
    ```

12. Add **KeyDownEvent** to **Event Handler** as below.
    ```lua
    Event Handler:
    HandleKeyDownEvent(KeyDownEvent event)
    {
        -- Parameters
        local key = event.key
        --------------------------------------------------------
        -- You can take a screenshot by pressing 1 on the keyboard.
        if key == KeyboardKey.Alpha1 then 
    	    self:CaptureAndUpload()
        end
    }
    ```

13. Press the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test. Take a screenshot by pressing **1** and check whether the screenshot appears in the **Frame**.
    ![RawImage](https://mod-file.dn.nexoncdn.co.kr/bbs/16686484619418af80fe87df5450b86bea39286b9186c.gif "RawImage")

> <span style="color: #585858">**Learn more**
> * A resource to upload cannot exceed 3MB.
> * You can only make one upload request at a time. Uploads fail if the existing request hasn't finished yet and a new request is submitted.
> * Sprites created during Maker play are temporary resources and will disappear when play stops.
> * Uploads are limited to five times per minute in the world. If too many upload requests are submitted to the world, some of them could fail. Be sure not to send requests at the same time from multiple clients.
> * Uploaded screenshots may not appear if you have added a virtual player during testing in the Maker or if you have been to the instance room. They will be properly shown in the actual game when it launches.</span>