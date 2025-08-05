# Course Introduction
Let's learn how to use World Shop Viewer and about its functional limitations by product state.

# World Shop Viewer
This is the window where you can register and manage products in the Maker. Registered products are automatically classified in each folder according to product type. The main context menu is shown below.

![WorldShopViewer](https://mod-file.dn.nexoncdn.co.kr/bbs/16696848050101636660134be4dbebf8ba54f4c009767.png "WorldShopViewer")

| Name | Description |
| --- | --- |
| Copy Item ID | Copy the ID of the registered product. |
| Copy Item Thumbnail RUID | Copy the thumbnail RUID of the registered product. |
| Create New Item | Create a new item or pass. Click the menu to open the product registration page. |
| Edit Item | Modify the registered product. |
| Open Item List (Web) | Open an external browser window where you can check the states of the products you have created. |
| Refresh Viewer | Refresh the list of products. |

You can check product information by hovering your mouse over the registered products.
![ItemInformation](https://mod-file.dn.nexoncdn.co.kr/bbs/1669686269480e70f6ff61d3b488da37ca9c8d301cbdd.png "ItemInformation")

# Functional Limitations by Product State
Functions are limited depending on the state of the product. Please refer to the table below.
You can change a Waiting for Sale product to an Unreleased state so it is not inquired when searching all products.

| State | Information Inquiry in the World | Revealing Product in the World | Actual Purchase Availability | Test Purchase Availability | Modifying Product Information | Revealing a Product on the Official Homepage |
| --- | ---------- | ---------- | -------- | --------- | ----------- | ------------- |
| **Unreleased** | Unavailable | Unavailable  | Unavailable | Available | Available | Unavailable |
| **Waiting** | Available | Visible | Unavailable | Available | Available | Not Visible |
| **On Sale** | Available | Visible | Available | Available | Available | Pass Visible Only|
| **Sale Ended** | Available | Visible | Unavailable | Available | Available | Not Visible |
| **Restricted** | Available | Visible | Unavailable | Available | Available | Not Visible |

#### Product Restriction Guide
Restrictions can be placed on products created by creators (https://maplestoryworlds.nexon.com/legal/policy{"target":"_self"}) according to the [Operating Policy] of MapleStory Worlds.
The name of a restricted product will be altered to Violation of Operating Policy and become visible to users. Its product image will also be changed to the default image.
You can check the contents of the product restriction in the tooltip of **World Shop Viewer**. 

For inquiries regarding restricted products, please visit [Customer Center](https://cs.nexon.com/HelpBoard/Nexon?gamecode=363{"target":"_self"}).

# Difference between Maker and World
Depending on the environment where products are purchased, the following differences exist.

| State | Maker | Released World |
| --- | --- | ------ |
| Coin Deduction | You can purchase items without spending coins. | Coins are deducted. |
| Purchase Pop-up | At the bottom of the product purchase window, the phrase 'No coins are deducted for test purchases' is displayed. | The balance is displayed at the bottom of the product purchase window. |
| Product Inquiry | Products can be inquired in all states, which are <span style="color: #dc9656">**Unreleased, On Sale, Waiting, and Sale Ended**</span>. | Products can be inquired only in <span style="color: #dc9656">**Waiting, On Sale, Sale Ended**</span> states. |
| Purchase | Products are purchasable in all states, which are <span style="color: #dc9656">**Unreleased, On Sale, Waiting, and Sale Ended**</span>. | Products are purchasable only in <span style="color: #dc9656">**On Sale**</span> state. |

##### Reference Guide
Users need to be notified before using paid probability-based content as per the Probability Disclosure guideline. Please refer to [Probability Information Disclosure](/docs/?postId=667%7B%22target%22:%22_self%22%7D) when creating products.

* [Registering Products](/docs/?postId=581{"target":"_self"})
* [Utilization World Shop Services](/docs/?postId=659{"target":"_self"})