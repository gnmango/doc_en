# Course Introduction
You can compose UI entities in various formats by combining components. In this course, let's learn about the basic components that are often used.
##### Reference Guide
* [UI Editor](/docs?postId=120{"target":"_blank"})
* [Creating UI](/docs?postId=64{"target":"_blank"})
* [Controlling UI Entities](/docs?postId=1154{"target":"_blank"})
# UITransformComponent
[UITransformComponent](/apiReference/Components/UITransformComponent{"target":"_self"}) is a basic component that UI entities must contain. It is quite similar to **TransformComponent**, but the difference is that this operates in screen coordinates.
The main properties are as follows:

![UITransform](https://mod-file.dn.nexoncdn.co.kr/bbs/1657174537010c416d38c6dbd4357a590984d44bcb9ac.png "UITransform")

| Numbers | Items | Description |
| :---: | :---: | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1") | Coordinates | You set the location of UI. Required Properties vary depending on the Anchor Presets setting on the left.<br>You can find more details in the Anchor Presets section of [World Coordinates and Screen Coordinates](/docs/?postId=688{"target":"_self"}).  |
| ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") | UIMode | If UI Entity![Hierarchy](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene_maker.png "Hierarchy") is below the UI in Hierarchy, it is fixed with Screen. If it is below maps, it is fixed with World. |

# Output
#### SpriteGUIRendererComponent
[SpriteGUIRendererComponent](/apiReference/Components/SpriteGUIRendererComponent{"target":"_self"}) is a component that shows image resources in the UI. This component allows you to set it to show 'what' in 'which form' in the UI.
The main properties are as follows:

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1744880492943da5f0130b3564b8fb38d8921f50fe423.png "SpriteGUIRenderer")

| Numbers | Items | Description |
| :---: | :---: | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1") | ImageRUID | You set ImageRUID to show on the screen.|
|  ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") | Type | A method of displaying images. Select from Simple, Sliced, Tiled, or Filled. |
|  ![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg "4")  | Select if you want to use SortingLayer and OrderInLayer. Only activated when the SpriteGUIRendererComponent is added to an entity under World - maps. |
You can also set the way to show the image you have set in **ImageRUID** through other properties.

#### TextComponent
[TextComponent](/apiReference/Components/TextComponent{"target":"_self"}) is a component that shows text in the UI. When you use this with **SpriteGUIRendererComponent**, you should draw **TextComponent** above **SpriteGUIRendererComponent**. If the UI Entity contains **TextComponent**, you can double-click the entity and edit text easily.
The main properties are as follows:

![text](https://mod-file.dn.nexoncdn.co.kr/bbs/168671067460393c17d7821884fa9a30c00141833b74b.png "text")

| Numbers | Items | Description |
| :---: | :---: | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1") | Font | You set the font to show.<br>You can select from <span style="color: #dc9656">**Default, Maple Bazzi, and Football**</span>. |
| ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") | FontColor | You set the font color and transparency. |
| ![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg "3") | FontSize | You decide the font size. |
| ![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg "4") | Text | You write text to use. |

# Input
#### ButtonComponent
[ButtonComponent](/apiReference/Components/ButtonComponent{"target":"_self"}) is a representative component to use when receiving a user's input. You can receive the input information 'a user clicked the button'.
The main properties are as follows:

![ButtonComponent](https://mod-file.dn.nexoncdn.co.kr/bbs/1657592785058cf83097552544842a3b8a24257de7ce5.png "Button")

| Numbers | Items | Description |
| :---: | :---: | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1") | KeyCode | When pressing the button, it operates as if the specified keyCode was pressed. |
| ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") | Transition | You set the conversion method for button status. The properties you should set below will differ depending on the conversion method. <br><ul><li><span style=\"color: #dc9656\">**ColorTint**</span>: Color Conversion </li><li><span style=\"color: #dc9656\">**SpriteSwap**</span>: Sprite Replacement</li></ul> |

**ButtonComponent** can change the status of the button using [ButtonState](/apiReference/Enums/ButtonState{\"target\":\"_blank\"}).
*  Normal, Hover, Pressed, Released, Clicked

**ButtonComponent** does not have an output function. It is used with **SpriteGUIComponent** or **TextComponent** to inform the user that the button has been pressed. **ButtonComponent** only sends the information 'it was input,' so you need to define that you will process 'what' and 'how' after the button is pressed in Scripts. The general way is to detect the button input through **Event** and then write a logic.

The Events usually used are as follows:
* [ButtonClickEvent](/apiReference/Events/ButtonClickEvent{"target":"_blank"}): This event occurs when the button is pressed.
* [ButtonPressedEvent](/apiReference/Events/ButtonPressedEvent{"target":"_blank"}): This event occurs while the button is pressed.
* [ButtonStateChangeEvent](/apiReference/Events/ButtonStateChangeEvent{"target":"_blank"}): This event occurs when the button's status is changed.

#### TextInputComponent
[TextInputComponent](/apiReference/Components/TextInputComponent{"target":"_self"}) is a component that can receive text input.
The main properties are as follows:

![textinput](https://mod-file.dn.nexoncdn.co.kr/bbs/168957179254024254818dac64c37a05886983c29e0dc.png "textinput")

| Numbers | Items | Description |
| :---: | :---: | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1") | AutoClear | If it is True, it automatically initializes the input field after text input. |
| ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") | ContentType | It specifies the text type to input. <br> <ul> <li><span style="color: #dc9656">**Standard**</span>: Allows all inputs. </li> <li> <span style="color: #dc9656">**Autocorrected**</span>: Allows all inputs, and automatic modification is made in the platform with autocorrect. </li> <li><span style="color: #dc9656">**IntegerNumber**</span>: You can input numbers only. </li> <li><span style="color: #dc9656">**DecimalNumber**</span>: You can input numbers and one decimal point only. </li> <li><span style="color: #dc9656">**Alphanumeric**</span>: You can input A-Z, a-z, and 0-9. </li> <li><span style="color: #dc9656">**Name**</span>: Capitalize the first letter. </li> <li><span style="color: #dc9656">**EmailAddress**</span>: You can input English and number strings, including one @ symbol. You cannot enter consecutive periods (.). </li> <li><span style="color: #dc9656">**Password**</span>: You can input all characters, and they are shown with an asterisk (asterisk). </li> <li><span style="color: #dc9656">**Pin**</span> : You can input numbers only, and they are shown with an asterisk (asterisk). </li> <li><span style="color: #dc9656">**Custom**</span>: Users can set up the types directly, such as Line Type, Input Type, Keyboard Type, and String Verification.</li></ul> |
| ![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg "3") | LineType | <ul> <li><span style=\"color: #dc9656\">**SingleLine**</span> : Enter in a single line.</li> <li><span style=\"color: #dc9656\">**MultiLineSubmit**</span> : You can enter multiple lines, and it stops when the Enter is input.</li> <li><span style=\"color: #dc9656\">**MultiLineNewline**</span> : You can enter multiple lines, and the line changes when the Enter is input.</li></ul> |

**TextInputComponent** only receives input, so to show what is input, **TextComponent** is required. **TextInputComponent** and **TextComponent** are generally used together.
You need to define how to process the input text through **TextInputComponent** in the script. **TextInputComponent** also detects text input through **Event** and then writes a logic.

The Events usually used are as follows:
* [TextInputEndEditEvent](/apiReference?postId=444{"target":"_blank"}): This event occurs once the input is complete. It is used when it receives and processes a complete sentence or a word.
* [TextInputValueChangeEvent](/apiReference?postId=406{"target":"_blank"}): This event occurs when the input value is changed. It detects and processes each moment whenever input is made.

