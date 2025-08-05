# Course Introduction
Use the vibration function of mobile devices to give extra feedback to your players.

# MobileVibratorService
You can enable vibration on mobile devices by utilizing `MobileVibratorService`. This feature is still in testing and is only available on mobile devices, so it will not work normally when tested in the Maker.
# Example
Let's make it so that each time a button is pressed, a notification will pop up accompanied by a vibration.

![Example](https://mod-file.dn.nexoncdn.co.kr/bbs/16691656921273488fc31718d491dafd206dc89dd0996.gif "Example")

1. Select **UI Editor - ui - DefaultGroup - Create Entity - Create Button** and place <span style="color: #dc9656">**UIButton_Vib**</span>.
    * ImageRUID: e00ce931b027545449e68e06b0f0b411
3. Create the new <span style="color: #dc9656">**Vibrate**</span> component and add it to **UIButton_Vib**.
4. Add ButtonClickEvent to **Vibrate** component and set it to **UIButton_vib** after changing to entity.
    ```lua
    Event Handler:
    [entity] (/ui/DefaultGroup/UIButton_Vib)
    HandleButtonClickEvent (ButtonClickEvent event)
    {
        local Entity = event.Entity
        --------------------------------------------------------
        if _MobileVibratorService:IsHWSupported() then
        	_MobileVibratorService:Vibrate()
        	_UIToast:ShowMessage("Vibration On")
        end
    }
    ```
6. Create a private World and test the vibration on your mobile device.

><span style="color: #b8b8b8">**Learn more**
> Toast UI is editable in the UI Editor - ToastGroup.</span>