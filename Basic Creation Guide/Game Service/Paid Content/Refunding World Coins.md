# Course Introduction
In MapleStory Worlds, creators can sell products they have created. But they may also receive refund claims for all sorts of reasons from users who purchased those products.
MapleStory Worlds is planning to provide a product purchase-related log for the convenience of creators, but we are currently establishing a log inquiry system. Creators will need to record purchase information directly and add inquiry scripts until the system is completed. 
<br>
This time, we will take a look at the <span style="color: #dc9656">**Methods for Recording / Querying Product Purchase Information in the World**</span> and <span style="color: #dc9656">**Methods for giving refund to users**</span> based on it.

# Refund Obligation for World Coins
In principle, World Coins should be refunded to a purchaser if the item being sold by the creator falls under any of the cases below.
<br>
<span style="color: #dc9656">**1. Less than 7 days have passed since the date of purchase, and the user has not used the paid product (contents, items, etc.) 
2. In case the creator caused grave impact on the owned product (contents, items, etc.) by arbitrarily deleting or modifying the paid product (contents, items, etc.) 
3. The actual product is different from the thumbnail or description of the paid product (contents, items, etc.) in the world**</span>
<br>
> <span style="color: #585858">**Learn More**
>For details regarding World coin refunds, see [Creator Operating Policy](https://maplestoryworlds.nexon.com/legal/policy/699{\"target\":\"_self\"})**8. Please refer to the refund (returning coin) policy**.</span>

<br>
For No. 3, this particularly applies to the case where only the user's World Coins are consumed and the actual item is not provided due to a creator's error. In such cases, you first need to confirm if the purchaser's refund claim is legitimate. This is because you need to decide whether to provide a refund after confirming that purchasers did not receive products even though they paid coins as intended.
Now, let's take a look at how to query and record product purchase information.

# Recording Product Purchase Information
An example code of recording product purchase information follows.
```lua
-- Registering Callback function for the purchaser's product request
_WorldShopService:SetProcessPurchaseCallback(function(purchaseInfo)
    -- Record related information when handling of the purchase is completed.
    _LogStorageService:LogPurchaseInfo(purchaseInfo, "Item purchase completed")
end)
```

Set it so that the related information is recorded when the product purchase request occurs in the world.

# Recording Product Purchase Information
Let's take a look at the example code for querying recorded purchase information.
```lua
-- Query the recorded product purchase from November 25th to November 30th according to the universal time coordinated.
local fromDate = DateTime(2022, 11, 25)
local toDate = DateTime(2022, 11, 30)
_LogStorageService:GetPurchaseLogPagesAsync(fromDate, toDate, function(purchaseLogPages)  
    while true do
        -- Bring the list of the current page by GetCurrentPageDatas().
        local purchaseLogs = purchaseLogPages:GetCurrentPageDatas()
 
        for index, purchaseLog in ipairs(purchaseLogs) do
            -- Make rounds of pulled records and check their contents.
            -- You can enter any additional logics you want to record.
            log(purchaseLog)
        end
 
        -- You can check whether the current page is the last one with the IsLastPage property of PurchaseLogPages.
        if purchaseLogPages.IsLastPage == true then
            break
        end
 
        -- You can move to the next page by MoveToNextPageAndWait().
        purchaseLogPages:MoveToNextPageAndWait()
    end
end)
```
<br>
You can customize the purchase information query however you want. For example, you can make it a separate UI that is visible only to creators who logged in to the world that they created and released.
Product purchase information inquiry cannot be tested in the Maker and can be checked after the world release. Switching to the public world is recommended after checking whether the inquiry works properly in the private world.

# Refund Method
Creators must provide a refund to purchasers when it is decided that the purchaser's refund claim is legitimate by confirming the purchase information following the procedure above.
<br>
1. [MapleStory Worlds Official Website](https://maplestoryworlds.nexon.com/{"target":"_self"}) Open the purchase history page in my account after logging in.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16753920669483b019ac8f73845e08f79e16398454f96.png "1")
<br>
2. Click the **[World Coins History]** button in the purchase history page.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16753922605447272e649a5c84c558d4f7bf9accf598e.png "2")
<br>
3. Confirm the order number for refund in the World Coins Revenue History, and then click the **[Details]** button.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16856926730318b86b64d268949f586240986babea973.png "3")
<br>
4. Click the **[Refund]** button next to the profit sum in the order details information.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16856926978826649de57154c4988a74e2979dfb48d3a.png "4")
<br>
5. The refund will be processed when you click the **[Confirm]** button next to the confirmation popup window.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16856927193795669e54139df4d62938aaf1418538ae4.png "5")
<br>
6. Go back to World Coins History and confirm that the refund has been processed.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1685692739637aedaabd2d40c4f9dbbab71e7f2b53177.png "6")
<br>

## Caution
* Creators can refund <span style=\"color: #dc9656\">**World items and avatar items**</span> for up to 30 days following the date of the purchase.
* Refund methods differ based on the user account that made the request. The types of accounts are classified as Korean/others based on which region the user signed up in.
    * <span style="color: #dc9656">**Refund requests from Korean accounts**</span> must have the reasoning be verified as elligible. Following a review, a request can be approved or denied. Refund reasoning could be product mismatches, mistaken purchases, quality issues, simple change of heart, etc.
    * <span style="color: #dc9656">**Refund requests from other accounts**</span> can have requests approved or denied for purchases made within 30 days of the request.