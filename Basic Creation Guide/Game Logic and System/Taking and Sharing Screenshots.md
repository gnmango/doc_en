# Course Introduction
Let's look at how to capture a screen and save an image file using `ScreenshotService`.
In addition, we'll learn how to save and share screenshots on mobile.

##### API Reference
[ScreenshotService](/apiReference?postId=807{"target":"_self"})
[MobileShareService](/apiReference/Services/MobileShareService{"target":"_self"})

# ScreenshotService
You can use `ScreenshotService` to take and save screenshots of MapleStory Worlds on PC or mobile. You can also share screenshots by integrating with `MobileShareService`.
The functions provided by `ScreenshotService` are classified as follows according to their intended use.

 | Function | Intended use | Save path |
 | --- | --- | --- |
 | CaptureFullScreenToPhotoLibraryAndWait()<br>CaptureScreenRegionToPhotoLibraryAndWait() | Save screenshots to your photo library | <ul> <li>Windows: My PC > Pictures > MapleStory Worlds > World ID > fileName.png</li> <li>macOS: Home > Pictures > MapleStory Worlds > World ID > fileName.png</li> <li>Android: Gallery - Maplestory Worlds Album</li><li>iOS: Pictures</li></ul> |
 | CaptureFullScreenAsFileAndWait()<br>CaptureScreenRegionAsFileAndWait() | Saving screenshots for mobile sharing<br>(used in integration with MobileShareService) | <ul> <li>Windows: My PC > Pictures > MapleStory Worlds > World ID > fileName.png</li> <li>macOS:  Home > Pictures > MapleStory Worlds > World ID > fileName.png</li> <li>Android: Built-in memory > Android > data > com.nexon.mod > files > MapleStory Worlds > Screenshots > World ID > fileName_*.png </li><li>iOS: My file > My iPhone > MapleStory Worlds > Screenshots > World ID > fileName_ *.png</li></ul> |

Let's use the function by referring to the intended use and save path of each function.

# Screenshot Capture
## Full Screen Capture
Press the button to capture the full screen.
<br>
1. Create a new <span style="color: #dc9656">**FullScreenBtn**</span> button by pressing the ![btn](https://mod-file.dn.nexoncdn.co.kr/storage/icons/UI/icon_button.png "btn") button in the UI Editor.
<br>
2. Add **TextComponent** to **FullScreenBtn** and set the properties as below.
    | Property | Value |
    | :---: | :---: |
    | Text | Full Screen Capture |
    | FontColor | #ffffff |
    <br>
3. In the context menu of **Workspace - MyDesk**, click **Create Scripts - Create Logic** to create new logic. Change the name to <span style="color: #dc9656">**ScreenshotServiceTest**</span>.
<br>
4. Open **ScreenshotServiceTest** in Script Editor and add `ButtonClickEvent` in Event Handler.
    Set the event sender to **Entity** as shown below.
    ![01](https://mod-file.dn.nexoncdn.co.kr/bbs/1681187758910df0cdbd4dc784785850f16fb584abbdf.png "01")
    <br>
    After that, enter the content as below.
    ```lua
    [entity: FullScreenBtn(/ui/DefaultGroup/FullScreenBtn)]
    HandleButtonClickEvent (ButtonClickEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
        -- Parameters
        local Entity = event.Entity
        ---------------------------------------------------------
        local error, path = _ScreenshotService:CaptureFullScreenToPhotoLibraryAndWait("FullScreen", true)
        log(error, path)
        
        if error == ScreenshotError.Success then
        	self._T.lastPath = path	
        else
        	_UIToast:ShowMessage("Failed to save! "..tostring(error)..", Path: "..path)
        end
    }
    ```
    <br>
5. If you press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[ Start]** followed by the <span style="color: #dc9656">**Full Screen Capture**</span> button, the full screen screenshot is captured, and you can see the image saved path in the log.
![full](https://mod-file.dn.nexoncdn.co.kr/bbs/1681191751073ddae20ad59954d928e4210f758bec775.png "full")
<br>
By checking the saved FullScreen.png, you can see that the full screen has been captured as below.
![full2](https://mod-file.dn.nexoncdn.co.kr/bbs/16811917796840a7e9d430cf34b7d86aafb09c99c3a8b.png{"width":"750px"} "full2")

## Partial Screen Capture
Press the button to capture the partial screen.
<br>
1. Click **Duplicate** in the context menu of the **FullScreenBtn** button to copy the button and move it to an appropriate location.
<br>
2. Rename the copied button <span style="color: #dc9656">**PartialScreenBtn**</span>. Enter <span style="color: #dc9656">**Partial Screen Capture**</span> to **Text**. 
    ![02](https://mod-file.dn.nexoncdn.co.kr/bbs/16811921606221a01221441844aae815f4af11770bd37.png "02")
    <br>
3. Open **ScreenshotServiceTest** in Script Editor and add `ButtonClickEvent` in Event Handler.
    Set the event sender to **Entity** as shown below.
    ![03](https://mod-file.dn.nexoncdn.co.kr/bbs/168119229553433590599d6bb42e1948da148bef1a832.png "03")
    <br>
    After that, write the content as below.
    ```lua
    [entity: PartialScreenBtn (/ui/DefaultGroup/PartialScreenBtn)]
    HandleButtonClickEvent (ButtonClickEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
        -- Parameters
        local Entity = event.Entity
        ---------------------------------------------------------
        local startPixel = Vector2(600,0)
        local endPixel = Vector2(1100,700)
        
        local error, path = _ScreenshotService:CaptureScreenRegionToPhotoLibraryAndWait("PartialScreen", startPixel, endPixel)
        log(error, path)
        
        if error == ScreenshotError.Success then
        	self._T.lastPath = path	
        else
        	_UIToast:ShowMessage("Failed to save! "..tostring(error)..", Path: "..path)
        end
    }
    ```
    <br>
4. If you press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** followed by the <span style="color: #dc9656">**Partial Screen Capture**</span> button, the partial screen is captured, and you can see the image saved path in the log.
![Partial](https://mod-file.dn.nexoncdn.co.kr/bbs/16811926961341471a8a6920248e39480c0ee8bc6852f.png "Partial")
<br>
By checking the saved PartialScreen.png, you can see that the partial screen has been captured as below.
![Partial2](https://mod-file.dn.nexoncdn.co.kr/bbs/1681192771299639cd1d5758642508dce913d1b6afa73.png "Partial2")

> <span style="color: #585858">**Note**
>For partial screen capture, the location of screen capturing can differ depending on the user's screen resolution. Users should be aware of this when using the partial screen capture feature.</span>

# Share Screenshots from Mobile
Let's see how to share a screenshot from mobile. Sharing is not possible from a PC.
<br>
1. Click **Duplicate** in the context menu of the **FullScreenBtn** button to copy the button and move it to an appropriate location.
<br>
2. Rename the copied button <span style="color: #dc9656">**ShareBtn**</span>. Enter <span style="color: #dc9656">**Share**</span> in **Text**. 
    ![05](https://mod-file.dn.nexoncdn.co.kr/bbs/168119541043350d44d07d02c4bb98d2de0b5da0f7266.png "05")
    <br>
3. Open **ScreenshotServiceTest** from the script editor and search for the event linked to **FullScreenBtn**. Change ‘CaptureFullScreenToPhotoLibraryAndWait()’ to ‘CaptureFullScreenAsFileAndWait()’.
    ```lua
    [entity: FullScreenBtn (/ui/DefaultGroup/FullScreenBtn)]
    HandleButtonClickEvent (ButtonClickEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
        -- Parameters
        local Entity = event.Entity
        ---------------------------------------------------------
        -- Modified the following function to CaptureFullScreenAsFileAndWait() to save screenshot for mobile sharing.
        local error, path = _ScreenshotService:CaptureFullScreenAsFileAndWait("FullScreen", true)
        log(error, path)
        
        if error == ScreenshotError.Success then
        	self._T.lastPath = path	
        else
        	_UIToast:ShowMessage("Failed to save! "..tostring(error)..", Path: "..path)
        end
    }
    ```
    <br>
4. Add `ButtonClickEvent` to the event handler.
    Set the event sender to **Entity** as shown below.
    ![04](https://mod-file.dn.nexoncdn.co.kr/bbs/168119303326810820bf91ef54332aa0488d261883a41.png "04")
    <br>
    After that, write the content as below.
    ```lua
    [entity: ShareBtn (/ui/DefaultGroup/ShareBtn)]
    HandleButtonClickEvent (ButtonClickEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
        -- Parameters
        local Entity = event.Entity
        ---------------------------------------------------------
        local path = self._T.lastPath
        if path ~= "" then
        	local success = _MobileShareService:ShareFileAndWait(path)
        	log("Share : "..tostring(success))
        end
    }
    ```
    <br>
5. After pressing ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]**, press the <span style="color: #dc9656">**Full Screen Capture**</span> button to save the screenshot. Then press the <span style="color: #dc9656">**Share**</span> button to share the screenshot.

# Caution
You can only save a screenshot file once per second.