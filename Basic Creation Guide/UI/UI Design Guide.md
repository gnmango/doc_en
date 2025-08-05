# Course Introduction
The User Interface (UI) is an essential element for your world. The UI needs to provide the users with important information clearly and concisely. If elements such as score, health, remaining time, and interactive buttons are well-designed, users will spend less time figuring out how your world works and more time enjoying it.
Users can play MapleStory Worlds in both PC and mobile environments. Therefore, creators must design UIs that will work in either. Of course, it is also important to create a UI with charm and appeal that fits the concept of your world.

This session will introduce some points to consider and useful tips when designing UI in MapleStory Worlds.

# Interactivity
##### Layout Across Devices
If you only consider the user experience, it would be best to create a UI optimized for both PC and mobile. However, when creating a world, it can be difficult to create a UI for every device because of the different elements needed. Most of the UI can be made common across devices, but we recommend making custom UI only for the parts that must be different.

Let's take a look at the <Miner Simulator> example.
Most of the <Miner Simulator> UI is the same on PC and mobile, but the location of a few control buttons is different. Common UI text and button sizes are designed to be comfortable for mobile devices with small screens. This is because if you fit it to PC, it will be too small to use on a mobile screen.

| Device | Description |
| --- | --- |
| Mobile | ![mobile](https://mod-file.dn.nexoncdn.co.kr/bbs/16813599460498acfdab2130f4fcab496bf76e83d1ec9.png "mobile")<br> <ol> <li>This shows slots in a position where items can be easily used on mobile</li>  <li>Dash, jump, and E key operations are essential, so they are always displayed on the screen.</li>  <li>This shows the joystick.</li> </ol> |
| PC | ![pc](https://mod-file.dn.nexoncdn.co.kr/bbs/168135995962657eba9bd408f459eacaf3055e4f2499c.png "pc")<br> <ol> <li>Item slots appear in the lower left corner. Shortcut keys are displayed together for easy use.</li>  <li>Dash is an important key in this game, so it is shown along with the shortcut shift.</li> </ol>In addition, jumping on PC is a common operation in all MapleStory Worlds, so the button is not shown.  |

##### Don't Cover the Main UI
The core MapleStory Worlds UI is displayed at the right top of the screen in every world. This position cannot be moved or hidden. Therefore, you should design your world UI so it doesn't cover the main UI.

| O | X | 
| :---: | :---: |
| ![02](https://mod-file.dn.nexoncdn.co.kr/bbs/1681365023170ab166d98e0a64396aa51c2475b992350.png "02")<br>![03](https://mod-file.dn.nexoncdn.co.kr/bbs/168136503481552de0a69652143828a376d12fb7f04f3.png "03") | ![01](https://mod-file.dn.nexoncdn.co.kr/bbs/1681365003797e997565b1a80441a9f63c23d1f5d36c2.png "01") |

##### Design with Touch in Mind
You should set your buttons to be a size that mobile users can comfortably touch. Areas that are too small are difficult for users to touch. Although not required, we generally recommend setting the button size to at least <span style="color: #dc9656">**80 X 80px**</span>. If you make a button that is smaller than the recommended size during the design process, check in advance whether it's comfortable in a mobile environment.
Button spacing is also important. Buttons should be spaced far enough apart so that they don't risk touching other buttons. Keep an eye on the touch areas and margins to account for finger spacing.

> <span style="color: #585858">**Learn More**
> ![default](https://mod-file.dn.nexoncdn.co.kr/bbs/168137478767305e25de85e584f939475dba219a8896c.png "default")
> The default UI button size at the right top of the World is 80 X 80px. Please refer to this when designing the UI.</span>

# Readability
##### Font Size
When making worlds to be played on mobile devices, we recommend setting the font size to <span style="color: #dc9656">**24 pt**</span> or more. Of course, sometimes a smaller font size will be needed to display a lot of information in worlds, such as RPGs. However, keep in mind small font sizes may reduce readability. Therefore, we recommend not setting the size to lower than <span style="color: #dc9656">**18 pt**</span>. When you make worlds, make sure your text can be easily read on both PC and mobile devices.

> <span style="color: #585858">**Learn More**
> ![fontsize](https://mod-file.dn.nexoncdn.co.kr/bbs/16813812593511eb9305748904597aaf6ec9b49e77da9.png "fontsize")
> The font size of the description in the <Miner Simulator>'s shop category is 24pt. Please note this when setting the font.</span>

##### Contrast
Make the text stand out clearly using contrast between the text and the background color.

| O | X |
| :---: | :---: |
| ![02](https://mod-file.dn.nexoncdn.co.kr/bbs/1681381891683a58a23ae4ea3479a981d0540c32e5ad1.png "02") | ![03](https://mod-file.dn.nexoncdn.co.kr/bbs/16813819095597b17034789224a1f8fef16b9e54cc365.png "03") |

# Consistency
A consistent UI helps users understand what they're seeing more quickly. For this design, let's refer to the following.

##### Color Palette
Decide on a color palette early in the UI design phase, and use it consistently. If you use too many colors, your UI will be messy and confusing. It's good to decide on just a few colors to use in the UI, and keep things consistent by using the same colors for elements that function similarly. For example, if you decide to use blue for the [Yes] or [Confirm] buttons, keep those buttons blue across that world.

##### Icon
Use consistent icons to make it easier for users to recognize and use UI elements such as buttons and menus.

##### Text Format
You should use consistent formatting for size, font, and thickness of text elements used in the UI. After deciding the priority level, set the text size for each level, and unify the size and format of the text with the same importance level.

##### Image Format
This is a similar concept to unifying text formatting. It's best to unify formatting such as image size, border, and shadow according to the type and importance of information.

##### Button
You should use the same size for buttons with the same weight. Also, place buttons with similar functions in the same locations. If you decide to put positive buttons like "Yes" and "Confirm" on the right and negative buttons like "No" and "Cancel" on the left, you need to keep that placement consistent.

##### Animation
Use animation effects consistently to create an interactive experience within your UI.

# More Design Tips
### Reference Resolution
We recommend working at 1920 X 1080 resolution.

### User Resources
Creators can use their UI resources by registering them in Resource Storage. Please refer to the following when registering UI resources.

##### Resource Optimization
If the width and height of the sprite to be registered in Resource Storage is set to a multiple of 4, it will be automatically optimized when uploaded to reduce the size. For example, a sprite that has a width of 2000px and a height of 2000px will automatically be optimized when uploaded because both the width and height are multiples of 4.

##### Setting Filter Mode
You can change the filter mode in the resource details registered by the creator.
UI resources are mainly used by selecting the appropriate one from the two filters below.

| Filter Mode | Description |
| --- | --- |
| Point | Keep colors vivid. When the sprite is enlarged, the color boundaries are clearly visible. <br>This is mainly used for resources with clean image edges, such as pixel images. |
| Bilinear | Blend a specific color with surrounding colors to create a softer expression. <br>When a sprite is enlarged, the color boundaries may appear blurrier than the original sprite. |

### Font
MapleStory Worlds allows you to use four fonts.

| Font | Examples |
| --- | --- |
| Default (Noto Sans) | ![font01](https://mod-file.dn.nexoncdn.co.kr/bbs/1681288827131ccd383fd33f94b87a4e9032b766bf75b.png "font01") |
| Maple | ![font02](https://mod-file.dn.nexoncdn.co.kr/bbs/1681288844324c4ebcad014dd4f8d8cabbf1e412bd461.png "font02") |
| Bazzi | ![font03](https://mod-file.dn.nexoncdn.co.kr/bbs/168128885797032b65d1d08604b8abf5b6f38546a4c4b.png "font03") |
| Football | ![font04](https://mod-file.dn.nexoncdn.co.kr/bbs/16812888772139dc36fc5340147da9621e41a8ef0ca39.png "font04") |

The Noto Sans font has good compatibility with all languages. If you're considering translation into multiple languages for a global release, using the Noto Sans font is recommended.
The Maple, Bazzi, and Football fonts are suitable for use in Korean and English, but they do not support other languages, so we recommended them only for numbers or parts to be displayed in English.

### Thumbnail
Makes an image with a 16:9 aspect ratio.
We recommend creating at 960px X 540px size.
![thumbnail](https://mod-file.dn.nexoncdn.co.kr/bbs/168138308206981ffaf18bdae47138a452f4ff430dd64.png{"width":"600px"} "thumbnail")

### Covering the Entire Screen with the UI
When covering the entire screen with the UI, you usually set **UITransformComponent** to stretch the UI to the entire screen and set the coordinates to (0, 0, 0, 0).
![s01](https://mod-file.dn.nexoncdn.co.kr/bbs/1681871890337b500055a443f40ada2667021869a20d5.png "s01")

However, with this setting, some mobile devices may be pushed to the safe area and create a blank space.
![s02](https://mod-file.dn.nexoncdn.co.kr/bbs/168187195095784a41c74787c4984b78b3fc353e20573.png "s02")

Extending the coordinates to (-300, -300, -300, -300) avoids unnecessary blank space.
![s03](https://mod-file.dn.nexoncdn.co.kr/bbs/16818725626457d38a53401074fdab37f5381cd26bdbb.png "s03")