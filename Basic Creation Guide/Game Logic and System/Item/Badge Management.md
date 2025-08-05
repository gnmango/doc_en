# Course Introduction
Let's learn about the Badge Viewer, how to use it, and the functional limitations by badge state.

# Badge Viewer
This is the window where you can register and manage badges in the Maker. Registered badges are automatically classified in each folder according to the badge grade. The main context menu is shown as below.

![badgeviewer](https://mod-file.dn.nexoncdn.co.kr/bbs/166444482138718392dcdaa9c440ca19d6f1c6ca9acbc.png)

| Menu Name | Explanation |
| ----- | --- |
| Copy Badge ID | Copies the ID of the registered badge. |
| Copy Badge Thumbnail RUID | Copy the thumbnail RUID of the registered badge. |
| Create New Badge | Creates a new badge. |
| Edit Badge | Modifies the registered badge. |
| Open Badge List(Web) | Opens an external browser window to check the state of the badges you have created. |
| Refresh Viewer | Refresh the list of badges. |

You can check badge information by hovering your mouse over the registered badge.
![Information](https://mod-file.dn.nexoncdn.co.kr/bbs/16751414919496d184ad530e2496fa89fe5471583d0bc.png)

# Feature Limitations by Badge State
Features are limited depending on the state of the badge. Please refer to the table below.
Badges set to Waiting can be changed to Unreleased so they won't be inquired when searching all badges.

| State | Information Inquiry in the World | Exposing Badges in the World | Actual Acquisition | Test Acquisition | Modifying Information for Maker/Web | Exposing Badges on Official Website | 
| --- | ---------- | ---------- | ----- | ------ | ----------- | ------------- |
| **Unreleased** | Unavailable | Not Visible | Unavailable | Available | Available | Not Visible |
| **Waiting** | Available | Visible | Unavailable | Available | Available | Visible | 
| **Obtainable** | Available | Visible | Available | Available | Available | Not Visible | 
| **Unobtainable** | Available | Visible | Unavailable | Available |Available | Visible |  
| **Restricted** | Available | Visible | Available: Restriction is marked when earned | Available: Restriction is marked when earned | Available | Not Visible |

#### Badge Restriction Guide
Restrictions can be put on badges created by creators according to the [Operating Policy](https://maplestoryworlds.nexon.com/legal/policy{"target":"_self"}) of MapleStory Worlds.
The name of the restricted badge will be altered to Violation of Operating Policy and become visible to users. In addition, the badge thumbnail will be altered.
You can check the contents of the badge restriction in the tooltip of Badge Viewer. 

For inquiries regarding restricted products, please visit [Customer Center](https://cs.nexon.com/HelpBoard/Nexon?gamecode=363{"target":"_self"}).

# Differences Between the Maker and World
Depending on the environment where badges are earned, the following differences exist.

| Item | Maker | Released World |
| --- | --- | ------ |
| Badge Acquisition Information | Badges are always checked as absent at the start of test play. When the test play ends, remove the acquired badge information. | Once acquired, badge information cannot be changed. |
| Badge Inquiry | Can be queried in all states: **<span style="color: #dc9656">Unreleased, Waiting for Acquisition, Acquirable, and Acquisition Unavailable</span>**. | Can be queried in all states: <span style="color: #dc9656">**Waiting for Acquisition, Acquirable, and Acquired**</span>. |
| Obtainable State | Can be queried in all statuses, which are **<span style="color: #dc9656">Unreleased, Waiting, Obtainable, and Unobtainable</span>**. |Acquirable only in <span style="color: #dc9656">**Obtainable state**</span>. |

##### Reference Guide

* [Badge Registration]
* [Using Badge Service]