# Course Introduction
Let's try to use `WorldShopService` to make items and passes registered on the Web purchasable. Keep in mind that in an unreleased World, even if you release a product, you cannot actually sell it.

# WorldShopService
When using the registered product in the World, you must use `WorldShopService`. A user who has successfully made a purchase is entitled to receive the product provided by the creator. 

# Use Example
Register items and passes. If you purchase the item, your potions immediately increases by 100; and if you press Attack after purchasing the pass, your portions see an additional increase of 15.

![example](https://mod-file.dn.nexoncdn.co.kr/bbs/16757382237655b20a67b51774a548bdef6461ef14507.gif "example")
#### Preparation
Follow [Registering Products](/docs?postId=581{"target":"_self"}) to register the item and pass.

| Product Type| Item |  Pass |
| --- | --- | --- |
| Product Name | Potion Item |  Potion Add-on Pass |
| Price | Free | Free  |
| Sale Period | Indefinite |  Indefinite |
| Product Thumbnail | ![Item](https://mod-file.dn.nexoncdn.co.kr/bbs/1669032688633b964bbc6fc0b4f5784c7bf5e7461f58f.png "Item") | ![Pass](https://mod-file.dn.nexoncdn.co.kr/bbs/1669032702800d3d79eebd2dd463990f7b0fe404b06e7.png "Pass") |  
| Product Description | Provides 100 potions free of charge. | Provides additional potions.  |

#### Creating a Product Purchase Window UI
1. Select **UI Editor - DefaultGroup - Create Entity - Create UIEmpty** to place it, and change its name to <span style="color: #dc9656">**MiniShop**</span>.
2. Create a new **Item Entity** and place it under the **MiniShop** Entity. Then, use text to make a product purchase window as seen in the following image.
    ![ShopUI](https://mod-file.dn.nexoncdn.co.kr/bbs/16757375937446a32c302638c4bce9c6293df77961900.png "ShopUI")
3. In World Shop Viewer, select a product, open the context menu, and choose **Copy Item Thumbnail RUID**.
![Copy](https://mod-file.dn.nexoncdn.co.kr/bbs/1669687890427bd427f642355464eaa03d156a1a6c6f7.png "Copy")
4. Select the image entity to show the product image and paste the product RUID into ImageRUID of **SpriteGUIRendererComponent**.
5. Copy the created Item Entity and place it side by side under the **MiniShop** Entity.
![ShopUI](https://mod-file.dn.nexoncdn.co.kr/bbs/1669687708602a247b62ec387466cb86b7290c316f2e4.png{"width":"450px"} "ShopUI")
6. Change the name of the copied entity to <span style="color: #dc9656">**Pass**</span> and add the pass RUID.    
7. ![off](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/CheckboxOff.png "off") Disable the **Enable** property of MiniShop.

#### Creating a Shop Button UI
We're going to create a button for opening and closing the product purchase window.
![ButtonShop](https://mod-file.dn.nexoncdn.co.kr/bbs/1669181482837083a5716f94f4b9dac17c23994305949.png "ButtonShop")
1. Place a button of **UI Editor - DefaultGroup - ShopOpenBtn**.
2. Look for the shop-shaped sprite and apply it to **SpriteGUIRendererComponent - ImageRUID**.
    * ImageRUID: 92a05d93672f65b4f991279cf1f0b4e6
3. Create a new <span style="color: #dc9656">**OpenShopBtn** </span>script component and add it to ShopOpenBtn.
![ShopOpenBtn](https://mod-file.dn.nexoncdn.co.kr/bbs/1668771870564d9e4aa0cc7d84cf78a4d010cf2254125.png "ShopOpenBtn")
5. Write what you see below to open and close the Minishop by clicking ShopOpenBtn. 

```lua
Method:
[client only]
void OpenShop()
{
    local Shop = _EntityService:GetEntityByPath("/ui/DefaultGroup/MiniShop")
    local Item = Shop:GetChildByName("Item") 
    local Pass = Shop:GetChildByName("Pass") 
        
    if Shop.Enable == false then
    
        Shop:SetEnable(true)   
                      
    else
    
        Shop:SetEnable(false)
        
    end
}

Event Hander:
[self]
HandleButtonClickEvent(ButtonClickEvent event)
{
    -- Parameters
    local Entity = event.Entity
    --------------------------------------------------------
    self:OpenShop()
}
```

#### Creating a Product Acquisition Confirm Window UI
Let's create a UI that shows the number of potions and passes when a product is successfully purchased.
1. Go to **UI Editor - DefaultGroup - MyPotionInfo** to place an image entity.
2. Place the image and text entities down there and change the name as seen in the following image.
![info](https://mod-file.dn.nexoncdn.co.kr/bbs/166918924041307df0d08c4824221a462c16df2e2b840.png{"width":"550px"} "info")
#### Importing Product Information to Purchase Window
1. Create a new **ItemSlot** script component and add it to the **Item, Pass UI Entities**.
2. Open the **ItemSlot** component script editor window and add a property.

    ```lua
    Property:
    [None]
    string ItemID = ""
    [None]
    Entity PurchaseBtn = nil
    ```
3. In World Shop Viewer, open the context menu of the product and select Copy ID.
![CopyID](https://mod-file.dn.nexoncdn.co.kr/bbs/16691907394815f8bdb41e2224869a012f97b5d142317.png "CopyID")
4. Add the copied ID to the ID of the ItemSlot components of both the item and the pass. 
![PropertyItemID](https://mod-file.dn.nexoncdn.co.kr/bbs/1669191109035f825047ab3a2458ab3e37daa4f3c6401.png{"width":"326px"} "PropertyItemID")
5. Select **MyDesk - Create Script - Create EventType** to create a new **ShopOpenEvent**, which will allow you to update the information every time the purchase window is opened.
![ShopOpenEvent](https://mod-file.dn.nexoncdn.co.kr/bbs/166903451819499a0605245bf40c5ad0497ac93cacefb.png{"width":"120px"} "ShopOpenEvent")
6. Write what you see below to display product information on the product purchase window with a click of the purchase button using `WorldShopService:PromptPurchase()` in the **ItemSlot** component.

    ```lua
    Method:  
    [client only]
    void PurchaseItem()
    {
        _WorldShopService:PromptPurchase(self.ItemID)
    }
    
    [client only]
    void OnBeginPlay()
    {
        self.PurchaseBtn = self.Entity:GetChildByName("PurchaseBtn")
        self.PurchaseBtn:ConnectEvent("ButtonClickEvent", function(event) 
            self:PurchaseItem()
        end)
    }
    ```
 7. Add **ShopOpenEvent** to the Event Handler to enable receiving events with a click of the shop button. Click self, change it to entity, and choose the button entity.
    ![EventHandler](https://mod-file.dn.nexoncdn.co.kr/bbs/1668773199934df13c978c30e49b0989d36024f8a5ce4.png "EventHandler")

    ```lua
    Method:
    [client only]
    void RefreshShop()
    {
        self._T.ItemName = nil
        self._T.ItemName =  self.Entity:GetChildByName("ItemName").TextComponent
        local SearchResult = _WorldShopService:GetProductAndWait(self.ItemID)
        self._T.ItemName.Text = SearchResult.Name
    }
    
    Event Handler:
    [entity] ShopOpenBtn (/ui/DefaultGroup/ShopOpenBtn)
    HandleShopOpenEvent(ShopOpenEvent event)
    {
        -- Parameters
        --------------------------------------------------------
        self:RefreshShop()
    }
    ```

#### Offering Products 
Write what you see below in<span style="color: #dc9656"> **MyShopLogic**</span> to provide 100 potions when a successful purchase of a potion is made.
 Use `WorldShopService:SetProcessPurchaseCallback` to provide the product when a purchase is made for it. If the product is successfully offered, the return value should be true, and if not, false.

```lua
Method:
[server only]
void OnBeginPlay()
{
    _WorldShopService:SetProcessPurchaseCallback(self.ProcessPurchase)
}


[server only]
boolean ProcessPurchase(any purchaseInfo)
{
    local userEntity = _UserService:GetUserEntityByUserId(purchaseInfo.UserId)
     
    if _EntityService:IsValid(userEntity) == false then
        log_error("Invalid user! PurchaseId: " .. purchaseInfo.PurchaseId .. " / " ..
                    "UserId: " .. purchaseInfo.UserId .. " / " ..
                    "ProductId: " .. purchaseInfo.ProductId)
        return false   
    end
    
    if purchaseInfo.ProductId == "Enter ItemID" then     
        userEntity.PlayerReward:GetPotion(100)
    end
    if purchaseInfo.ProductId == "Enter PassID" then
    
        userEntity.PlayerReward:PassCheck()
    
    end
    
    return true
}
```
#### Reflecting Product Purchase to UI
Let's have a purchased product reflected to the owning user.
1. Create a new <span style="color: #dc9656">**PlayerReward**</span> component and add it to <span style="color: #dc9656">**DefaultPlayer**</span>.
2. Add a property as follows.

    ```lua
    Property:
    [None] 
    TextComponent PotionValue = /ui/DefaultGroup/MyPotionInfo/PotionValue     
    [Sync]    
    number Potion= 0    
    [None]
    TextComponent PotionValue = /ui/DefaultGroup/MyPotionInfo/PassValue        
    [Sync]
    boolean HasPass = false
    ```

3. Write what you see below to have 100 potions provided when a Potion Item is purchased.
    ```lua
    Method:
    void GetPoint(number value)
    {
        self.Potion = self.Potion + Value
    }
    
    [client only]
    void OnSyncProperty(string name, any value)
    {
        if self.Entity ~= _UserService.LocalPlayer then
            return
        end
                
        if name == "Potion" then
            self.PotionValue.Text = value
        end
    }
    ```

4. Write what you see below to change the pass ownership UI when a Potion Pass is purchased. Enter the registered pass ID in Pass ID.

    ```lua
    [client]
    void PassCheck()
    {
        if self.Entity ~= _UserService.LocalPlayer then 
            return 
        end
        
        if _WorldShopService:UserHasPassAndWait(self.Entity.Name, "Pass ID") then 
            self.PassValue.Text = "O"
            self:PassUpdate(true)
        else
            self.PassValue.Text = "x"
            self:PassUpdate(false)
        end
    }
    
    [server only]
    void OnBeginPlay()
    {
        self:PassCheck()
    }
    
    [server]
    void PassUpdate(boolean value)
    {
        self.HasPass = value
    }
    ```

5. Write what you see below to have 15 additional potions offered whenever the pass owner presses Attack.

    ```lua
    Event Handler:
    {
        -- Parameters
        local ActionName = event.ActionName
        local PlayerEntity = event.PlayerEntity
        --------------------------------------------------------
        if self:IsServer() == false then 
            return 
        end 
        
        if self.HasPass == false then 
            return 
        end 
        
        if ActionName ~= "Attack" then 
            return 
        end 
        
        self:GetPotion(15)
    }
    ```

> <span style="color: #7cafc2">**Tip**
> Passes can be purchased only once, so you need to check in advance whether they are available for repurchase.
> If you press the purchase button of the pass you have already bought, a window appears to inform you that purchase is not allowed.</span>
> ![AlreadyGotItem](https://mod-file.dn.nexoncdn.co.kr/bbs/16696875138032c157b741e5a484c82f5119bb21c8722.png "AlreadyGotItem")

> <span style="color: #7cafc2">**Tip**
> Be sure to check that a pass can only be owned by the purchasing user before you release it.
> We recommend adding a user to the Maker before releasing the pass in the World and testing if the pass can only be owned by the purchaser.</span>


##### Reference Guide
Users need to be notified before using paid probability-based content as per the Probability Disclosure guideline. Please refer to the [Probability Information Disclosure](/docs/?postId=667{"target":"_self"}) when creating products.
* [Registering Products](/docs?postId=581{"target":"_self"})
* [Product Management](/docs?postId=658{"target":"_self"})