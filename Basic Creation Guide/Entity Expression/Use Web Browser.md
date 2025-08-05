# Course Introduction
If you use **WebViewComponent**, you can place a web browser in a World and explore it. Let's learn how to use a web browser in a World through a simple example.

##### API Reference
[WebViewComponent](/apiReference/Components/WebViewComponent{"target":"_self"})

# WebViewComponent Introduction
If you add **WebViewComponent** to an entity, you can place a web browser in the World.
Let's look at the properties of **WebViewComponent** first.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16903331826638ab8d0e6040e4a46848cae80cc1028fe.png)

| @cols=2:@rows=1:Property |Description|
| --- | --- |--- |
| @cols=2:@rows=1:ClickingEnabled	| Sets whether to enable clicks in WebView.	|
| @cols=2:@rows=1:HoveringEnabled | Sets whether the web element can be interacted with when the mouse is placed over WebView.	|
| @cols=2:@rows=1:ScrollingEnabled	| Set whether WebView is scrollable or not.	|
| @cols=1:@rows=3:DragMode | DragToScroll	| It operates as a scroll when dragging. <br> Therefore, even if you drag the scroll bar, it does not operate in a way that controls the scroll bar directly. Instead, it works like starting drag on the page. <br> To make it operate in the same way as dragging the scroll bar from Desktop, you should use DragWithinPage with DragMode.|
| DragWithinPage | If you drag it within a page, it usually operates like dragging with a mouse on the web page.|
| Disabled|Drag does not work. |
| @cols=2:@rows=1:DragThreshold | This is a threshold to ensure it recognizes dragging instead of clicking. If the number is very high, dragging will be recognized as a click.	|
| @cols=2:@rows=1:PixelPerUnit | It is the number of pixels to be displayed per 1 Unit. <br>It uses values from 100 to 500. <br>If the number is big, much more content can be shown, but the web elements will be shown as small.|
| @cols=2:@rows=1:ScrollSensitivity | Determines how much will scroll when the page is scrolled with the mouse wheel.	|
| @cols=2:@rows=1:Url | This is the URL of the webpage. <br>If you change the Url, the web page will load.<br> If you type empty strings or invalid forms of address, it will not work.	|
| @cols=2:@rows=1:SortingLayer | When two or more Entities overlap, the priority is determined according to the Sorting Layer.	|
| @cols=2:@rows=1:OrderInLayer | Determines the priority within the same Layer. A greater number indicates higher priority.	|
| @cols=2:@rows=1:IgnoreMapLayerCheck | Does not perform automatic replace when designating the Map Layer name to SortingLayer.	|

> <span style="color: #585858">**Learn More**
>1 Unit is a distance unit within a World. The distance of WorldPosition from (0,0) to (1,0) is 1 Unit.</span>

# Place Web Browser to World
If you place a web browser in the World, you can show the browser in a certain position.
![webview](https://mod-file.dn.nexoncdn.co.kr/bbs/1659077818567422ad8f56f884d0286111efe8a38b2ed.gif "webview")
<br>
1. Select the desired map from the **Hierarchy** tab, then open the context menu. Select **Create Entity - Create Empty** to create **EmptyEntity**.
<br>
2. In the property editor, add **TransformComponent** to **EmptyEntity**.
<br>
3. In the property editor, add **WebViewComponent** to **EmptyEntity** and enter the desired URL.
<br>
4. Change the **SortingLayer** property of **WebViewComponent** into **Layer1**.
<br>
5. After resizing the entity appropriately, press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** to begin the test. You can see the web browser in a certain position in the World.

# Place Web Browser using UI
If you place a web browser using UI, you can have the web browser always shown in a certain position on the screen.
![webviewui](https://mod-file.dn.nexoncdn.co.kr/bbs/165931683546031588f00cec548c0b1707a90878786ab.gif "webviewui")
<br>
1. Open the context menu for the desired UI group below **Hierarchy - ui**. Select **Create Entity - Create UIEmpty** to create **UIEmpty**.
<br>
2. In the property editor, add **WebViewComponent** to **UIEmpty** and enter the desired URL.
<br>
3. After resizing the entity appropriately, press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** to begin the test. The web browser is shown in a certain position on the screen.

# Function Use Example
## Control Web Browser by Key Input
Let's look at the example of controlling the web browser by key input.
1. In the **Workspace - MyDesk**, add a new script component and enter <span style="color: #dc9656">**WebViewTest**</span> as its name.
<br>
2. Add the **WebViewTest** script component into the property editor of the desired entity. Add **WebViewComponent**, too.
<br>
3. As shown below, write a script to control the web browser when pressing a certain key on the keyboard.
Add **KeyDownEvent** to **WebViewTest** script and write as follows.
    ```lua
    [service: InputService]
    HandleKeyDownEvent(KeyDownEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: InputService
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        -- local key = event.key
        ---------------------------------------------------------
        
        if key == KeyboardKey.Backspace then
        	self.Entity.WebViewComponent:GoBack()
        	-- Backspace : Go back
        end
        
        if key == KeyboardKey.Alpha0 then
        	self.Entity.WebViewComponent:GoForward()
        	-- 0 : Go forward
        end
        
        if key == KeyboardKey.Alpha5 then
        	self.Entity.WebViewComponent:Reload()
        	-- 5 : Refresh page
        end
        
        if key == KeyboardKey.Equals then
        	self.Entity.WebViewComponent:ZoomIn()
        	-- = : Zoom in screen
        end
        
        if key == KeyboardKey.Minus then
        	self.Entity.WebViewComponent:ZoomOut()
        	-- - : Zoom out screen
        end
        
        if key == KeyboardKey.Home then
        	self.Entity.WebViewComponent:Scroll(Vector2(0, -100))	
        	-- Home : Move to the Start page
        end
        
        if key == KeyboardKey.End then
        	self.Entity.WebViewComponent:Scroll(Vector2(0, 100))	
        	-- End : Move to the End page
        end
    }
    ```
    <br>
4. Let's press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** and then enter the keys set by the script to use each feature.

## Clicked Coordinates Record
Leave the coordinates of the points that are clicked on the web browser in the log.
You can use **WebViewClickedEvent** to check the **normalizedPosition** of the clicked point. **normalizedPosition** refers to the position where the upper left corner is expressed as (0,0) and the lower right corner is expressed as (1,1) when the web view is a flat coordinate system. The central coordinate on the screen is (0.5,0.5).
![normalizedPosition](https://mod-file.dn.nexoncdn.co.kr/bbs/1659333200156ced814fff9734a89a53c313e6f386874.png "normalizedPosition")
<br>
1. Add **WebViewClickedEvent** to **WebViewTest** script and write as follows.
    ```lua
    Event Handler:
    [self]
    HandleWebViewClickedEvent(WebViewClickedEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: WebViewComponent
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        -- local NormalizedPosition = event.NormalizedPosition
        ---------------------------------------------------------
        
        log("Clicked: ("..NormalizedPosition.x..", "..NormalizedPosition.y..")")
    }
    ```
    <br>
2. Press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** and test it. If you click a certain point of the web view, you can see the **normalizedPosition** coordinates of the position in the log.
![log](https://mod-file.dn.nexoncdn.co.kr/bbs/16593302650606bb94d2db1ca44a5882d429e6338bc84.png "log")