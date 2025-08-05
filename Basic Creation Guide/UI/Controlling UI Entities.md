# Course Introduction
Let's learn how to control UI entities efficiently. When controlling UI entities, you must understand and create the relationship between the UI, server, and client.
##### Reference Guide
* [UI Editor](/docs?postId=120{"target":"_blank"})
* [Creating UI](/docs?postId=64{"target":"_blank"})
* [Basic UI Components](/docs?postId=744{"target":"_blank"})
# Controlling UI Entities
You can access UI entities in the same way as entities. For more information on how to access entities, please refer to the [Referring to Entities and Components](/docs?postId=164{\"target\":\"_self\"}) guide.
Since UI entities only exist in clients, they cannot be accessed from servers. When receiving UI entities or their components, you have to refer from a client function. If you have to send a server's output to UI, or process input from UI in a server, you have to use Execution Space Control so that each space can perform separately.

![300](https://mod-file.dn.nexoncdn.co.kr/bbs/16564192479937183015f46df4ea1a7ed3481b19e9821.png{"width":"740px"} "300")
![UIeditor80](https://mod-file.dn.nexoncdn.co.kr/bbs/1660700550069a309016cdd944831981b9f99331b6ff7.png "UIeditor80")

#### Button Click
This is an example of requesting a process from the server upon clicking the button. 

```lua
Method:
[client only]
void OnBeginPlay()
{
    local button = _EntityService:GetEntityByPath("ButtonEntityPath") 
    -- Enter path of the importing button entity to "ButtonEntityPath".
    button:ConnectEvent(ButtonClickEvent, self.OnButtonClickClient)
}

[client only] 
void OnButtonClickClient()
{
    --processing in client..
    self:OnButtonClickServer()
}

[server] 
void OnButtonClickServer()
{
    log("Start processing on the server")
}
```
     
#### Outputting Server Processing Results to the UI
```lua
Property:
[None]
number time = 0

Method:
[server only]
void OnUpdate(number delta)
{
    self.time = self.time + delta
    if self.time >= 3 then
        self.time = 0
        self:ShowToastMessage("Time Reset")
    end
}
    
[client] 
void ShowToastMessage(string text)
{
    local toastUIEntity = _EntityService:GetEntityByPath("UIEntityPath") 
    -- Enter path of the importing UI entity to "UIEntityPath".
    
    local textComponent = toastUIEntity.TextComponent
    
    -- print toast message
    textComponent.Text = text
    toastUIEntity:SetEnable(true)
     
    --reservate hide toast message
    local callback = function()
            toastUIEntity:SetEnable(false)
        end
    _TimerService:SetTimerOnce(callback,3)
}
```

# Controlling UI Display
You can control the display of the UI using the `SetEnable()` function. Let's take a look at a case where you have created your own pop-up and created a UI with the same hierarchy as shown below. This pop-up should appear and disappear at the same time, so you must set the **Enable** for each entity separately.

![uieditor06](https://mod-file.dn.nexoncdn.co.kr/bbs/1659671947080901d185518b64d008152c181b8eb1735.png "uieditor06")

```lua
void ShowPopupUI()
{
    local PopupUIEntity_1 = _EntityService:GetEntityByPath("/ui/DefaultGroup/MODImage_1")
    local PopupUIEntity_2 = _EntityService:GetEntityByPath("/ui/DefaultGroup/MODButton_1")
    local PopupUIEntity_3 = _EntityService:GetEntityByPath("/ui/DefaultGroup/MODButton_1_1")
    PopupUIEntity_1:SetEnable(true)
    PopupUIEntity_2:SetEnable(true)
    PopupUIEntity_3:SetEnable(true)
}
 
void HidePopupUI()
{
    local PopupUIEntity_1 = _EntityService:GetEntityByPath("/ui/DefaultGroup/MODImage_1")
    local PopupUIEntity_2 = _EntityService:GetEntityByPath("/ui/DefaultGroup/MODButton_1")
    local PopupUIEntity_3 = _EntityService:GetEntityByPath("/ui/DefaultGroup/MODButton_1_1")
    PopupUIEntity_1:SetEnable(false)
    PopupUIEntity_2:SetEnable(false)
    PopupUIEntity_3:SetEnable(false)
}
```

When creating a UI, for UIs that belong to a UI feature, it is recommended to make them as a **parent-child** relationship using the hierarchy. Make a certain UI entity the top-level entity and include the rest as child entities. The parent-child nature allows you to control any UI entity by referencing only the top-level entities. 
Example code for show/hide processing using the hierarchy. 

```lua
void ShowPopupUI()
{
    local PopupUIEntity = _EntityService:GetEntityByPath("/ui/DefaultGroup/MODImage_1")
    PopupUIEntity:SetEnable(true)
}
 
void HidePopupUI()
{
    local PopupUIEntity = _EntityService:GetEntityByPath("/ui/DefaultGroup/MODImage_1")
    PopupUIEntity:SetEnable(false)
}
```

# Controlling UIGroup
You can control the **UIGroup** using the entity's `SetEnable` function.

```lua
void ShowUIGroup_1()
{
    local UIGroup_1 = _EntityService:GetEntityByPath("/ui/UIGroup_1")
    UIGroup_1:SetEnable(true)
}
 
void HideUIGroup_1()
{
    local UIGroup_1 = _EntityService:GetEntityByPath("/ui/UIGroup_1")
    UIGroup_1:SetEnable(false)
}
```
