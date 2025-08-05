# Course Introduction

**PixelRendererComponent** allows creators to assign any color to an empty sprite, pixel by pixel. You can change the color of pixels in a specific location only, specify the color of the entire sprite, or change it to a random color. First, let's look at the properties of **PixelRendererComponent** and see how to use it through an example.
##### API Reference
[PixelRendererComponent](https://mod-developers.nexon.com/apiReference/Components/PixelRendererComponent{"target":"_self"})

# PixelRendererComponent

Let's take a look at **PixelRendererComponent**'s properties of the following.
![00](https://mod-file.dn.nexoncdn.co.kr/bbs/169033280248125b25ac7adff41e0a1e7b6d3a06778a6.png)

| Name | Type | Main Function |
| --- | --- | ----- |
| Width | int | The horizontal length of the sprite. |
| Height | int | The vertical length of the sprite. |
| OrderInLayer | int | Determines the priority within the same Layer. A greater number indicates higher priority. |
| SortingLayer | string | When two or more Entities overlap, the priority is determined according to the Sorting Layer. |
| IgnoreMapLayerCheck | boolean | Does not perform automatic replace when designating the Map Layer name to SortingLayer. |

# Change Color by Pixel
Let's look at an example of how to use **PixelRendererComponent**.

## Change pixel colors
You can use the `SetPixel` function to change the color of a pixel at any location in the sprite.
Let's make a random color pixel appear at a random location on the sprite when the button is pressed.
![001](https://mod-file.dn.nexoncdn.co.kr/bbs/165336981276805b48f5770644fda8af385755872a17d.gif "001")

1.  **Press Hierarchy - map01 - Create Entity - Create Empty** to generate the new <span style="color: #dc9656">**SetPixelTest**</span>.
    <br>
2.  Add **TransformComponent** to **SetPixelTest** entity and enter <span style="color: #dc9656">**X = 10 and Y = 10**</span> in the **Scale property**. Then add **PixelRendererComponent** as well.
    ![02](https://mod-file.dn.nexoncdn.co.kr/bbs/16903328503829c4718ba1607411297b9379c8f304622.png)
        <br>
3.  Generate a new script component <span style="color: #dc9656">**PixelRendererTest**</span>.
    <br>
4.  Add **PixelRendererTest** component to the **Property** of the **SetPixelTest** entity that you created before.
    <br>
5.  Open **Workspace - MyDesk - PixelRendererTest** and add **Property** as shown below.
    ```lua
    Property:
    [None]
    Color color = Color(0,0,0,0)
    [None]
    Color lastColor = Color(0,0,0,0)
    [None]
    number lastX = 0
    [None]
    number lastY = 0
    ```
    <br>
6.  Create the new <span style="color: #dc9656">**SetPixel**</span> button by pressing ![button](https://mod-file.dn.nexoncdn.co.kr/storage/icons/UI/icon_button.png) in the UI Editor.
    <br>
7.  Add a **TextComponent** to the **Property** of the **SetPixel** button and modify its properties.
    | Component | Property | Value |
    | :---: | :---: | :---: |
    | TextComponent | Text | SetPixel |
    <br>
8.  Generate the new script component <span style="color: #dc9656">**ButtonClick_SetPixel**</span>.
    <br>
9.  Add **Property** to the **ButtonClick_SetPixel** script as follows.
    ```lua
    Property:
    [None]
    Entity PR = /maps/map01/SetPixelTest 
    ```
    <br>
10. Open the **PixelRendererTest** script and write the `SetRandomPixel` function.
    ```lua
    [client only]
    void SetRandomPixel()
    {
        local pixelRendererComponent = self.Entity.PixelRendererComponent
    
        local x = _UtilLogic:RandomIntegerRange(1, pixelRendererComponent.Width)
        local y = _UtilLogic:RandomIntegerRange(1, pixelRendererComponent.Height)
    
        local r = _UtilLogic:RandomDouble()
        local g = _UtilLogic:RandomDouble()
        local b = _UtilLogic:RandomDouble()
        local a = _UtilLogic:RandomDouble()
    
        -- Using SetPixel, change the pixel value.
        pixelRendererComponent:SetPixel(x, y, Color(r,g,b,a))
    
        self.lastColor = Color(r,g,b,a)
        self.lastX = x
        self.lastY = y
    
        log("X : "..tostring(x)..", Y : "..tostring(y)..",Color = "..tostring(Color(r,g,b,a)))
    }
    ```
    <br>
11. Add **ButtonClickEvent** to the **ButtonClick_SetPixel** script.
    ```lua
    Event Handler:
    HandleButtonClickEvent (ButtonClickEvent event)
    {
        -- Parameters
        local Entity = event.Entity
        --------------------------------------------------------
        -- By clicking the button, return the SetRandomPixel function.
        self.PR.PixelRendererTest:SetRandomPixel()
    ```
    <br>
12. Add the **ButtonClick_SetPixel** component to the **Property** of the **UI - DefaultGroup - SetPixel** button.
    <br>
13. Press **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") for a test. When pressing the **SetPixel** button, check if the pixels appear randomly on the sprite and it shows a log.

## Change Multiple Pixel Colors
You can use the `SetPixels` function to change the color of a pixel in the sprite.
Let us make a random color pixel appear on the sprite when the button is pressed.
![002](https://mod-file.dn.nexoncdn.co.kr/bbs/16534560711537b9397a8eb804578b2045aa1d5517c9a.gif "002")
1. Create the new <span style="color: #dc9656">**SetPixels**</span> button by pressing ![button](https://mod-file.dn.nexoncdn.co.kr/storage/icons/UI/icon_button.png) in the UI Editor.
    <br>
2. Add a **TextComponent** to the **Property** of the **SetPixels** button and enter <span style="color: #dc9656">**SetPixels**</span> in the **Text** section.
    <br>
3. Generate a new script component <span style="color: #dc9656">**ButtonClick_SetPixels**</span>.
    <br>
4. Add **Property** to the **ButtonClick_SetPixels** script as follows.
    ```lua
    Property:
    [None]
    Entity PR = /maps/map01/SetPixelTest 
    ```
    <br>
5. Open the **PixelRendererTest** script and write the `SetPixelsTest` function.
Before that, first add `[None] SyncTable<Color> colors` to the existing **Property** of **PixelRendererTest**.
    ```lua
    Property:
    [None]
    Color color = Color(0,0,0,0)
    [None]
    Color lastColor = Color(0,0,0,0)
    [None]
    number lastX = 0
    [None]
    number lastY = 0
    [None]
    SyncTable<Color> colors -- Add
    ```
    Write the `SetPixelsTest` function after setting the foregoing.
    ```lua
    [client only]
    void SetPixelsTest()
    {
        local width = self.Entity.PixelRendererComponent.Width
        local height = self.Entity.PixelRendererComponent.Height
    
        local colors = {}
    
        for i=1, width * height do
            local r = _UtilLogic:RandomDouble()
            local g = _UtilLogic:RandomDouble()
            local b = _UtilLogic:RandomDouble()
    
            colors[i] = Color(r,g,b,1) 
        end
    
        -- You can use SetPixels to change the color of a pixel in the sprite.
        self.Entity.PixelRendererComponent:SetPixels(colors)
    }
    ```
    <br>
6. Add **ButtonClickEvent** to the **ButtonClick_SetPixels** script.
    ```lua
    Event Handler:
    HandleButtonClickEvent (ButtonClickEvent event)
    {
        -- Parameters
        local Entity = event.Entity
        --------------------------------------------------------
        -- By clicking the button, call the SetPixelsTest function.
        self.PR.PixelRendererTest:SetPixelsTest()
     }
    ```
    <br>
7. Add the **ButtonClick_SetPixels** component to the **Property** of the **UI - DefaultGroup - SetPixels** button.
    <br>
8. Press **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") for a test. By pressing the **SetPixels** button, you will see the sprite displaying random colors pixel by pixel.
## Change sprite size and multiple pixel colors
You can use the `ResetWithColors` function to change the size of the sprite and the color of its pixels.
![007](https://mod-file.dn.nexoncdn.co.kr/bbs/1653456362928aa034bd72e7b4298ae6919d32fde6401.gif "007")
The example code will be as follows.
Creating a button, adding a function, and attaching **ButtonClickEvent** to the button are the same as in the example above.
```lua
[client only]
void ResetColorsTest()
{
    local newWidth = _UtilLogic:RandomIntegerRange(8, 32)
    local newHeight = _UtilLogic:RandomIntegerRange(8, 32)
     
    local colors = {}
     
    for i=1, newWidth * newHeight do
        local r = _UtilLogic:RandomDouble()
        local g = _UtilLogic:RandomDouble()
        local b = _UtilLogic:RandomDouble()
     
        colors[i] = Color(r,g,b,1) 
    end
 
    -- You can use ResetWithColors to change the size of the sprite and the color of the pixels.
    self.Entity.PixelRendererComponent:ResetWithColors(newWidth, newHeight, colors)
}
```

# Changing the Entire Sprite
You can use the **PixelRendererComponent** function to change the overall color of the sprite.

## Changing Sprite to Solid Color
You can use the `FillColor` function to change the entire sprite to a solid color. It can be changed to a specified color or to a random color as shown below.
![003](https://mod-file.dn.nexoncdn.co.kr/bbs/1653376631359d2f630081b4b4e74835f35ec5df965d1.gif "003")
The example code will be as follows.
Creating a button, adding a function, and attaching **ButtonClickEvent** to the button are the same as in the example above.
```lua
[client only]
void FillColorTest()
{
    local r = _UtilLogic:RandomDouble()
    local g = _UtilLogic:RandomDouble()
    local b = _UtilLogic:RandomDouble()
     
    -- You can use FillColor to change the overall color of the sprite.
    self.Entity.PixelRendererComponent:FillColor(Color(r,g,b,1))
}
```

## Change Sprite Size and Overall Color
You can use the `ResetWithColor` function to change the size and overall color of the sprite.
![004](https://mod-file.dn.nexoncdn.co.kr/bbs/1653378536538b82441c0538b4e2da706381061607fe5.gif "004")
The example code will be as follows.
Creating a button, adding a function, and attaching **ButtonClickEvent** to the button are the same as in the example above.
```lua
[client only]
void ResetColorTest()
{
    local newWidth = _UtilLogic:RandomIntegerRange(8, 32)
    local newHeight = _UtilLogic:RandomIntegerRange(8, 32)
 
    local r = _UtilLogic:RandomDouble()
    local g = _UtilLogic:RandomDouble()
    local b = _UtilLogic:RandomDouble()
         
    -- You can use ResetWithColor to change the sprite size and overall color.
    self.Entity.PixelRendererComponent:ResetWithColor(newWidth, newHeight, Color(r,g,b,1))
}
```

## Change Alpha Value
You can use the `SetAlpha` function to change the alpha value of the entire sprite.
![005](https://mod-file.dn.nexoncdn.co.kr/bbs/165337928365796df7621cff04bc68247b9ffe1280269.gif "005")
The example code will be as follows.
Creating a button, adding a function, and attaching **ButtonClickEvent** to the button are the same as in the example above.
```lua
[client only]
void SetAlphaTest()
{
    local a = _UtilLogic:RandomDouble()
    -- You can use SetAlpha to change the alpha value of the entire sprite.
    self.Entity.PixelRendererComponent:SetAlpha(a)
}
```

# Getting Sprite Color
Let us see how to use the `GetPixel` and `GetPixels` functions to get the position and color of a pixel and display it in the World.

## Getting Pixel Colors
You can use the `GetPixel` function to get the position and color of the most recently changed pixel.
![006](https://mod-file.dn.nexoncdn.co.kr/bbs/165344235792573c3640770c64858902a84cf1af10904.gif "006")
1. Press **Hierarchy - map01 - Create Entity - Create Empty** to generate the new <span style="color: #dc9656">**CheckGetEntity**</span>.
    <br>
2. Add **TransformComponent** to **CheckGetEntity** and enter <span style="color: #dc9656">**X = 10, Y = 10**</span> for the **Scale** value. Then add **PixelRendererComponent** as well.
    <br>
3. Create the new <span style="color: #dc9656">**GetPixel**</span> button by pressing ![button](https://mod-file.dn.nexoncdn.co.kr/storage/icons/UI/icon_button.png) in the UI Editor.
    <br>
4. Add **TextComponent** to the **Property** of the **GetPixel** button and enter <span style="color: #dc9656">**GetPixel**</span> in the **Text** section.
    <br>
5. Generate the new script component <span style="color: #dc9656">**ButtonClick_GetPixel**</span>.
    <br>
6. Add **Property** to the **ButtonClick_GetPixel** script as follows.
    ```lua
    Property:
        [None]
        Entity PR = /maps/map01/SetPixelTest 
    ```
    <br>
7. Open the **PixelRendererTest** script and write the `GetPixelTest` function.
Before that, first add the following content to the existing **Property** of **PixelRendererTest**.
    ```lua
    Property:
    [None]
    Entity PRCheck = /maps/map01/CheckGetEntity 
    ```
    Write the `GetPixelTest` function after setting the foregoing.
    ```lua
    [client only]
    void GetPixelTest()
    {
        -- Use GetPixel to get the coordinates and color of one of the most recently displayed pixels in SetPixelTest.
        local color = self.Entity.PixelRendererComponent:GetPixel(self.lastX, self.lastY)
        -- Display the value received above in CheckGetEntity using SetPixel.
        self.PRCheck.PixelRendererComponent:SetPixel(self.lastX, self.lastY, color)
    }
    ```
    <br>
8. Add **ButtonClickEvent** to the **ButtonClick_GetPixel** script.
    ```lua
    Event Handler:
    HandleButtonClickEvent (ButtonClickEvent event)
    {
        -- Parameters
        local Entity = event.Entity
        --------------------------------------------------------
        -- By clicking the button, call the GetPixelTest function.
        self.PR.PixelRendererTest:GetPixelTest()
     }
    ```
    <br>
9. Add **ButtonClick_GetPixel** to the **Property** of the **UI - DefaultGroup - GetPixel** button.
    <br>
10. Press **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") for a test. When pressing the **SetPixel** button and then the **GetPixel** button, check whether the most recently displayed pixel in **SetPixelTest** is also displayed well in **CheckGetEntity**.

## Getting All Pixels of Sprite
You can use the `GetPixels` function to get all the pixel values of the entire sprite.
![008](https://mod-file.dn.nexoncdn.co.kr/bbs/16534576466290e6655858bc24915b190bc343b8daf73.gif "008")
The example code will be as follows.
Creating a button, adding a function, and attaching **ButtonClickEvent** to the button are the same as in the example above.
```lua
[client only]
void GetPixelsTest()
{
    -- Get the pixel values of SetPixelTest using GetPixels.
    local getPixels = self.Entity.PixelRendererComponent:GetPixels()
    -- Display the value received above in CheckGetEntity using SetPixels.
    self.PRCheck.PixelRendererComponent:SetPixels(getPixels)
}
```