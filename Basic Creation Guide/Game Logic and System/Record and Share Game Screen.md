# Course Introduction
In MapleStory Worlds, you can use `ScreenRecordService` to record the game screen and save it as a file. Additionally, you can easily share videos recorded on mobile devices using  `MobileShareService`.
Let's take a look at some examples of how to use `ScreenRecordService` and `MobileShareService`.

##### API Reference
[ScreenRecordService](/apiReference/Services/ScreenRecordService{"target":"_self"})
[MobileShareService](/apiReference/Services/MobileShareService{"target":"_self"})

# Recording Mode
`ScreenRecordService` contains four recording modes (ScreenRecordMode). 
Each platform has its own restrictions, and there are instances of some recording modes not being functional on all platforms. Please refer to the table below for more details.

| Recording mode | Description | Windows/macOS | iOS | Android |
| :---: | :---: | :---: | :---: | :---: |
| ScreenOnly | Record screen | O | O | O |
| ScreenAndGameAudio | Record screen + game sound | O | O | O |
| ScreenAndMic | Record screen + mic | O | O | X <br>(replaced by ScreenAndGameAudioAndMic) |
| ScreenAndGameAudioAndMic | Record screen + game sound + mic | X<br>(replace by ScreenAndMic) | O | O |

> <span style="color: #585858">**Learn More**
> - You cannot record on devices that do not properly support Image and Audio codecs.  
> - In this case, the `StartRecordToFileAndWait()` and `StartRecordToPhotoLibraryAndWait()` functions will return "ScreenRecordStartResult.DeviceNotSupported".</span>

# Recording a Gameplay Video
Let's look at an example of recording a video using `ScreenRecordService`.
Press the **[Record]** button to start recording, and the **[Stop]** button to end the recording and save the file.

1. Enter the UI Editor and add a new UIGroup with the name <span style="color: #dc9656">**RecordGroup**</span>.
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1678416050163b3c472a5574e427ab4c6c79e5be50684.png "1")
    <br>
    Then set **DefaultShow** of **RecordGroup** to **true**.
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1678423634296cf0cab991d24471889baefac1f5ee4b5.png "7")
    <br>
2. Add a button and name it <span style="color: #dc9656">**Btn_Record**</span>.
    <br>
3. Add **TextComponent** to **Btn_Record** and set the properties as follows:
    | Component | Property | Value |
    | :---: | :---: | :---: |
    | @cols=1:@rows=2:TextComponent | Text | Record |
    | FontColor | #FFFFFF |  
    <br>
4. Click **Duplicate** to copy the button in the context menu of the **Btn_Record** button.
    <br>
5. Change the name of the copied button to <span style="color: #dc9656">**Btn_Stop**</span> and enter <span style="color: #dc9656">**Stop**</span> in the **Text** field.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16784163562145cecc803405a4a1aa358ed0dc4280deb.png "3")
    <br>
6. In the context menu of **Workspace - MyDesk**, click **Create Scripts - Create Logic** to create a new logic. Change its name to <span style="color: #dc9656">**ScreenRecordServiceTest**</span>.
    ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1678416161538b4fd072b7b974d418fb80474e83e816a.png "4")
    <br>
7. Open **ScreenRecordServiceTest** in the Script Editor. 
    <br>
8. Add the following to **Property**.
    ```lua
    Property:
    [None]
    string LastRecordedPath = ""
    ```
    <br>
9.  Add the `Record()` function and write its content.
    ```lua
    void Record()
    {
        _UIPopup:Open("Do you want to start recording?", function()
    	    -- set recordMode to ScreenOnly, and excludeSystemUI's option to false)
    	    local result = _ScreenRecordService:StartRecordToFileAndWait(ScreenRecordMode.ScreenOnly, false)
    	    if result == ScreenRecordStartResult.Success then
    		    _UIToast:ShowMessage("Start recording")
    	    else
    		    _UIToast:ShowMessage("Failed : "..tostring(result))	
    	    end
    	end, function()
    		_UIToast:ShowMessage("Canceled")
    	end)
    }
    ```
    You can use the `StartRecordToFileAndWait()` or `StartRecordToPhotoLibraryAndWait()` functions when recording a video.
    The file path for where videos are saved is based on the function used here.

    | Platform | Save path using StartRecordToFileAndWait() | Save path using StartRecordToPhotoLibraryAndWait() |
    | :---: | :---: | :---: |
    | Windows | @cols=2:@rows=1:My PC > Videos > MapleStory Worlds > World ID > MapleStory Worlds_*.mp4 | 
    | macOS  | @cols=2:@rows=1: Home > Videos > MapleStory Worlds > World ID > MapleStory Worlds_*.mp4 | 
    | iOS | My Files > My iPhone > MaplestoryWorlds > MapleStory Worlds > World ID > MapleStory Worlds_*.mp4 | @cols=1:@rows=2:Photos / Gallery |
    | Android | Internal Storage > Android > data > com.nexon.mod > files > MapleStory Worlds > World ID > MapleStoryWorlds_*.mp4 | 
    <br>
10. Add `ButtonClickEvent` to the event handler to start recording by clicking the **Btn_Record** button. 
    Set the event sender to **Entity** as shown below.
    ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1678417754286bb465689e3e44568a5cc351a0e7688c6.png "5")
    <br>
    After that, copy the code shown below.
    ```lua
    entity Btn_Record(/ui/RecordGroup/Btn_Record)
    HandleButtonClickEvent(ButtonClickEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        local Entity = event.Entity
        ---------------------------------------------------------
        self:Record()
    }
    ```
    <br>
11. Add the `Finish()` function and its associated content.
    Use the `FinishRecordAndWait()` function to stop recording. It returns the save path upon creating the recorded file.
    ```lua
    void Finish()
    {
        _UIToast:ShowMessage("End recording")
        self.LastRecordedPath = _ScreenRecordService:FinishRecordAndWait()
        log("Output : ", self.LastRecordedPath)
    }
    ```
    > <span style="color: #585858">**Learn More**
    > However, there is no sharing function in the Windows environment, so if you use the `StartRecordToFileAndWait()` function, replace it with the `StartRecordToPhotoLibraryAndWait()` function and call it. If you start and stop recording with `StartRecordToPhotoLibraryAndWait()`, an empty value will be returned as the save path.</span>
    <br>
12. Add `ButtonClickEvent` to the event handler to stop recording by clicking the **Btn_Stop** button as follows. 
    Set the event sender to Entity as shown below.
    ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1678417899020da7233c0e9a64655bbfe9fa5cecbfc0e.png "6")
    <br>
    After that, write the content as seen below.
    ```lua
    entity Btn_Stop(/ui/RecordGroup/Btn_Stop)
    HandleButtonClickEvent(ButtonClickEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        local Entity = event.Entity
        ---------------------------------------------------------
        self:Finish()
    }
    ```
       <br>
13. Click the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. 
    Click the **[Record]** button to start recording and the **[Stop]** button to stop recording and save the file.
    ![recording](https://mod-file.dn.nexoncdn.co.kr/bbs/1678424192506294a4b8f784946a8aa06241618708fc2.gif "recording")
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1678424210461bbb4c320b08348fab23a6029336dd50a.png "8")

# Hide the UI and Record Video
Let's see how to hide the UI and record a video.

1. Go to the UI Editor, add one **UIText** to **DefaultGroup**, and set its property as follows.
    ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1678430164183080b4d5df681412da8450337321bb50d.png "9")
    | Property | Value |
    | --- | --- |
    | RectSize | X = 700, Y = 100 |
    | Text | ScreenRecordService TEST |
    | FontSize | 50 |
    | ImageRUID | b9ab8455d5737bf4cb21433f00fdc19d |
    | Color | A = 100%, #FFFFFF |
      <br>
2. Open **ScreenRecordServiceTest** in the Script Editor. 
      <br>
3. Write the following content in **Property**.
    ```lua
    Property:
    [None]
    EntityRef DefaultGroup = /ui/DefaultGroup
    ```
      <br>
4. Modify the `Record()` function as shown below.
    ```lua
    void Record()
    {
        _UIPopup:Open("Do you want to start recording?", function()
    		-- Change the visibility of the UIGroup you want to hide to false.
    		self.DefaultGroup.Visible = false
    		-- Change the excludeSystemUI option to true to hide the right top basic UI.
    		local result = _ScreenRecordService:StartRecordToFileAndWait(ScreenRecordMode.ScreenOnly, true)
    		if result == ScreenRecordStartResult.Success then
    		    _UIToast:ShowMessage("Start recording")
    		else
    		    _UIToast:ShowMessage("Failed : "..tostring(result))	
    		end
    	 end, function()
    		_UIToast:ShowMessage("Canceled")
    	 end)
    }
    ```
      <br>
5. Modify the `Finish()` function as shown below.
    ```lua
    void Finish()
    {
        _UIToast:ShowMessage("End recording")
        self.LastRecordedPath = _ScreenRecordService:FinishRecordAndWait()
        -- Add the following to make the UI visible again after recording stops.
        self.DefaultGroup.Visible = true
        log("Output : ", self.LastRecordedPath)
    }
    ```
    <br>
6. Click the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. 
    Click the **[Record]** button to hide the UI and record video. When the recording stops, the UI will reappear.
    ![recording2](https://mod-file.dn.nexoncdn.co.kr/bbs/167843224107397026e661dfd4bdeb1e578fabfac9229.gif "recording2")

# Share Recorded Video on Mobile
You can easily share recorded videos on mobile by using `MobileShareService`. 
Videos to be shared must be recorded using the `StartRecordToFileAndWait()` function. However, the recording time is limited to a maximum of 2 minutes.
Let's look at an example of sharing a recorded video on mobile devices.

1. Click **Duplicate** to copy the button in the context menu of the **Btn_Record** button.
    <br>
2. Change the name of the copied button to <span style="color: #dc9656">**Btn_Share**</span> and enter <span style="color: #dc9656">**Share**</span> in the **Text** field.
    ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1678757498013b1075f65b0e44c508965220c93cbddda.png "10")
    <br>
3. Open **ScreenRecordServiceTest** in the Script Editor.
    <br>
4. Add the `ShareVideo()` function and write its associated content.
    ```lua
    void ShareVideo()
    {
        if self.LastRecordedPath ~= "" then
        	log("FilePath: ", self.LastRecordedPath)
        	local shared = _MobileShareService:ShareFileAndWait(self.LastRecordedPath)
        	if shared then
        		_UIToast:ShowMessage("Share success")
        	else
        		_UIToast:ShowMessage("Share Failed")
        	end
        end
    }
    ```
    <br>
5. Add `ButtonClickEvent` to the event handler to share recorded videos by clicking the **Btn_Share** button.
    Set the event sender to **Entity** as shown below.
    ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1678757658575e7b78c6fa2f843b2ac8e70e29a8e911d.png "11")
    <br>
    After that, write the content as seen below.
    ```lua
    entity Btn_Stop(/ui/RecordGroup/Btn_Share)
    HandleButtonClickEvent(ButtonClickEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        local Entity = event.Entity
        ---------------------------------------------------------
        self:ShareVideo()
    }
    ```
    <br>
6. After launching, make sure your code operates as intended on mobile. Click the **[Record]** button to start recording, click the **[Stop]** button to stop recording, and click the **[Share]** button to share the video.
    ![share](https://mod-file.dn.nexoncdn.co.kr/bbs/167877261209707c7dda7630d4754b846c68e6b21d002.gif "share")

# Caution
- If there are multiple microphones, you must provide the player a choice to select which one to use. In such cases, use the ‘GetMicrophoneDevicesAndWait()’ function to select which mic the player will use as an input.
- iOS: Recording is only possible if permissions are enabled for **Settings - MapleStoryWorlds - Photos - Only Add Photos**. 
- AOS: Permission must be allowed for **App Info - MapleStory Worlds - Storage** to allow for recording with devices that are below Android 10.