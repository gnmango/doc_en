# Course Introduction
When localizing, the text to be translated may contain mutable strings. Let's find out what functions you can use in this case and see how to use them through examples.
Before proceeding with this course, it is helpful to refer to the [Understanding Localization] guide.

# Convert to Format
When localizing, the text to be translated may contain mutable strings depending on the World's environment. 
For example, rather than simply translating <span style="color: #dc9656">**"리나"**</span> to <span style="color: #dc9656">**"Rina"**</span> in English, let's think about translating the sentence (format) of <span style="color: #dc9656">**"Hello, {0}!"**</span>. Let's assume {0} blank can contain multiple NPC names, such as "리나 (Rina)", "스탄 (Stan)", "알렉스 (Alex)". While <span style="color: #dc9656">**"Hello, {0}!"**</span> has a specific format, filling its blanks with appropriate words depending on the situation to output a string could be called <span style="color: #dc9656">**"Convert to Format"**</span>.
<br>
A format has an (Argument hole). In the example sentence above, **{0}** is an **Argument hole**.
![arghole](https://mod-file.dn.nexoncdn.co.kr/bbs/167480440826756abe6fbe7164b5588aa34f246d61f4f.png "arghole")
One string of the format may contain multiple **Argument holes**.
<br>
Use the `GetTextFormat()` or `SmartFormat()` function when converting to format. Let's take a closer look at how to use each function.

## GetTextFormat
The `GetTextFormat()` function receives **Key** from **LocalizationTable** as an argument. It finds the text corresponding to this **Key**, and if there is an additionally delivered format argument (format argument), fills it in according to the format, and returns the result.

For example, as shown below, 2 NPCs are standing, and when the player character meets (collides) with each NPC, let's output the sentence <span style="color: #dc9656">**"Hi, NPCName!"**</span> in the orange text box.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1674106811875c0dbb4e97c5a4c7595813713f1ed11ce.png "5")
<br>
1. Place 2 NPCs on the map and modify the name of each NPC entity as below.
    | NPC | Entity Name | 
    | :---: | :---: |
    | ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1674097464492fb3054921620425a80664d1380f3f695.png "6") | Stan | 
    | ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/16740974755475f8d0c294b5a491f907eefb8acc7bad1.png "7") | Rina | 
    <br>
2. Add **TriggerComponent** to **NPC Stan** and **Rina**.
    <br>
3. Add the text entity to **ui - DefaultGroup** in the UI Editor and rename it <span style="color: #dc9656">**TextBox**</span>. Then set each property as below.
    | Property | Value |
    | --- | --- |
    | ImageRUID | b9ab8455d5737bf4cb21433f00fdc19d |
    | Color | ![17](https://mod-file.dn.nexoncdn.co.kr/bbs/16752124711410bee46bc546d4bc68fd23e8476ca817e.png "17") |
    <br>
4.  Create **LocalizationTable** and enter as follows.
    | ID | Key | Source | ko | en |
    | :---: | :---: | :---: | :---: | :---: | 
    | 1 | UI_TEXT_NPC_Stan | Stan | 스탄 | Stan |
    | 2 | UI_TEXT_NPC_Rina | Rina | 리나 | Rina |
    | 3 | UI_TEXT_HELLO | Hello, {0}! | Hi, {0}! | Hello, {0}! |
    <br>
5. Generate a new script component <span style="color: #dc9656">**LocalizationTest**</span>. Then, add the **LocalizationTest** component to **Workspace - DefaultPlayer**.
    <br>
6. Add properties to **LocalizationTest** as follows.
    ```lua
    Property:
    [None]
    Entity Stan = /maps/map01/Stan
    [None]
    Entity Rina = /maps/map01/Rina
    ```
    <br>
7. Add the `OnBeginPlay()` function and compose its content as follows. Set the execution space to **client only**.
    ```lua
    [client only]
    void OnBeginPlay()
    {
        self.Stan.NameTagComponent.Name = _LocalizationService:GetText("UI_TEXT_NPC_Stan")
        self.Rina.NameTagComponent.Name = _LocalizationService:GetText("UI_TEXT_NPC_Rina")
    }
    ```
    <br>
8. Add **TriggerEnterEvent** to the event handler. Set the execution space to **client only**.
    Then write the content as below.
    ```lua
    [client only][self]
    HandleTriggerEnterEvent(TriggerEnterEvent event)
    {
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        --------------------------------------------------------
        local npcName = TriggerBodyEntity.NameTagComponent.Name
        local translateText = _LocalizationService:GetTextFormat("UI_TEXT_HELLO", npcName)
        _EntityService:GetEntityByPath("/ui/DefaultGroup/TextBox").TextComponent.Text = translateText
    }
    ```
    <br>
9. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. 
    Retrieves appropriate text according to the language setting of the client.
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16741095688573170d1d86f0543609e51f38f2bce3942.gif "1")
    ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1674109590083201ff51e11794e63b0343f945ff0e693.gif "11")

## SmartFormat
`GetText()` or `GetTextFormat()` needs the translation table. If you want to use only the format function of `GetTextFormat()` without using the translation table, use the `SmartFormat()` function.
In `SmartFormat()`, instead of **Key** of **LocalizationTable**, the string to be output is directly received as an argument.
<br>
![format](https://mod-file.dn.nexoncdn.co.kr/bbs/16750583548154a12eeaac0d048d39e1820ca865996a3.png "format")
<br>
If you implement the above example using `SmartFormat()`, it will look like the following.
```lua
[client only][self]
HandleTriggerEnterEvent(TriggerEnterEvent event)
{
    -- Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    --------------------------------------------------------
    local npcName = TriggerBodyEntity.NameTagComponent.Name
    local translateText = _LocalizationService:SmartFormat("안녕, {0}!", npcName)
    _EntityService:GetEntityByPath("/ui/DefaultGroup/TextBox").TextComponent.Text = translateText
}
```

However, with this setting, "안녕" will be output in Korean even when the client language setting is English (en). Modify the event handler as below to output according to each language setting.
```lua
[client only][self]
HandleTriggerEnterEvent(TriggerEnterEvent event)
{
    -- Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    --------------------------------------------------------
    local npcName = TriggerBodyEntity.NameTagComponent.Name
    local localeId = _LocalizationService.CurrentLocaleId
    
    local translateTextKo = _LocalizationService:SmartFormat("Hello, {0}!", npcName)
    local translateTextEn = _LocalizationService:SmartFormat("Hello, {0}!", npcName)
    
    if localeId == "ko" then 
	_EntityService:GetEntityByPath("/ui/DefaultGroup/TextBox").TextComponent.Text = translateTextKo
    elseif localeId == "en" then
    	_EntityService:GetEntityByPath("/ui/DefaultGroup/TextBox").TextComponent.Text = translateTextEn
    end
}
```

# Utilizing Format
As seen above, **LocalizationService** provides two APIs to set the conversion formatting of text.

* **GetTextFormat(string key, any... args)**
* **SmartFormat(string format, any... args)**

The **Argument hole** included in the format can contain simple words, but can also contain characters converted according to certain rules. For example, Korean postposition or singular plural processing.
The format of **Argument hole** is:
![10](https://mod-file.dn.nexoncdn.co.kr/bbs/167411584046890fd957c327e4425ae0066bd4307301b.png "10")
Here, the contents in square brackets ([]) are optional elements. That is, you can simply put an index like {0}, but you can also set **formatterName** and **formatterArgument**. Using **formatter**, you can convert into natural sentences that fit the grammar of each linguistic cultural area.
<br>
You can use the two below as a **formatter**.
| formatterName | Function | Description |
| :---: | --- | --- |
| hpp | Korean postposition processing | It outputs one of the two formatterArguments depending on whether the last character of the string received as an argument contains a suffix or not. |
| plural or p | Singular, plural processing | <ul> <li>It processes the plural of a noun or unit expression based on a number received as an argument (including a string consisting only of numbers).</li> <li>The number of formatterArguments varies by language.</li> <li>For more detailed rules, refer to [Link](https://unicode-org.github.io/cldr-staging/charts/latest/supplemental/language_plural_rules.html#jv{"target":"_self"}). |

Let's take a closer look at how to use **formatter**.

## hpp: Korean Postposition Processing
In Korean, postpositions are parts of speech that add grammatical meaning by being attached to the end of a noun (noun, pronoun, numeral). Depending on whether there is a consonant as the noun's coda, the postposition attached to the back will be different.
For example, in the sentence <span style="color: #dc9656">**"나는 [   ]을/를 만납니다."**</span>, depending on the presence or absence of the last consonant of the word to enter the blank [ ], <span style="color: #dc9656">**'을'**</span> or <span style="color: #dc9656">**'를'**</span> will be attached.

| Words to enter the argument hole | Postposition | Completed sentence |
| :---: | :---: | :---: |
| 스탄 | 을 | 나는 스탄을 만납니다. |
| 리나 | 를 | 나는 리나를 만납니다. |
<br>
**hpp** is what allows you to process this. The basic format is shown below.
![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1674181700829ead15381640f44de9a63d997522d22d0.png "13")
<br>
| Standard | Postposition |
| --- | --- |
| When there is a final word in the preceding noun | it will be 은, 이, 을, 과, 아, 이여, 이랑, 으로 |
| When there is no final word in the preceding noun | it will be 는, 가, 를, 와, 야, 여, 랑, 로 |
<br>
Let's see how to use hpp through an example.

1. Open **LocalizationTable** and modify row 3 as shown below.
    | ID | Key | Source | ko | en |
    | :---: | --- | --- | --- | --- | 
    | 3 | UI_TEXT_HELLO | I meet {0}. | 나는 {0}{0:hpp:을\|를} 만납니다. | I meet {0}. |
    <br>
2. Modify the event handler as below.
    ```lua
    [client only][self]
    HandleTriggerEnterEvent(TriggerEnterEvent event)
    {
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        --------------------------------------------------------
        local npcName = TriggerBodyEntity.NameTagComponent.Name
        local translateText = _LocalizationService:GetTextFormat("UI_TEXT_HELLO",npcName)
        _EntityService:GetEntityByPath("/ui/DefaultGroup/TextBox").TextComponent.Text = translateText
    }
    ```
    <br>
3. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. 
    Retrieves appropriate text according to the language setting of the client.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16741799873045e413c513a6a4640a91b930f3b1da366.gif "3")
    ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16741799991931b62b30881134d439b8416dd3296fb7e.gif "4")
    <br>

## plural: Singular, Plural Processing
Korean does not require singular or plural processing, but there are languages that require singular or plural processing. 
The basic format of **plural** to process this is as follows.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/167418048551082631b1ae8ec4e51b5e53e487bac5857.png "11")

For example, in English, 1 is treated as singular, but the rest are treated as plural. So you receive two **formatterArguments** in total.
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/1674180498732cb6f503c59a940ab8340f688dd1470b6.png "12")

Let's use it as below.
```lua
_LocalizationService:SmartFormat("I have {0:plural:an apple|{} apples}.", 2) -- I have 2 apples.
```
As above, if you want to output the string received as an argument only in one **formatterArgument**, use empty braces {}.

> **<span style="color: #7cafc2">Tip</span>**
> <span style="color: #7cafc2">**plural** can be shortened to **p**.</span>

## Exception
If the format is not correct as in the following, the original text is output.
##### If formatterName is wrong
If spaces are included in **formatterName** or there are typos, the original text will be output as it is.
```lua
-- 1) Language setting English: In case formatter name includes spaces
log(_LocalizationService:SmartFormat("I have {0: plural:an apple|{} apples}.", 2))
```
![18](https://mod-file.dn.nexoncdn.co.kr/bbs/1675214317926babf748aa5a14e4db96a7f039a29cc3d.png "18")
<br>
```lua
-- 2) If the formatter name itself is incorrect (e.g. hpp is incorrectly written as hp)
log(_LocalizationService:SmartFormat("나는 {0}{0:hp:을|를} 만났다.", "스탄"))
```
![19](https://mod-file.dn.nexoncdn.co.kr/bbs/16752143336956dc58a0870ac444e9df8d70251208021.png "19")
<br>
If you modify it as below, the properly converted string will be output according to the format.
```lua
log(_LocalizationService:SmartFormat("I have {0:plural:an apple|{} apples}.", 2))
```
![18a](https://mod-file.dn.nexoncdn.co.kr/bbs/1675214386354a550210300ea457d82d969f5ecb641a8.png "18a")
<br>
```lua
log(_LocalizationService:SmartFormat("나는 {0}{0:hpp:을|를} 만났다.", "스탄"))
```
![19a](https://mod-file.dn.nexoncdn.co.kr/bbs/16752144025748615eeea77e545de9bb158996665c1ad.png "19a")

<br>
##### If the number of formatterArgument entered is different from the number requested
In this case, depending on the situation, it may be output normally, or it may be output as it is because it fails to convert to fit the format.
Please refer to the example below.
```lua
-- Language Setting Korean: Use plural
log(_LocalizationService:SmartFormat("아이템 {0:plural:한 |여러 }개를 획득했다.", 1))
-- Result: Original text output (because Korean plural receives only one formatter argument)
```
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1675213948789797bb2bfe84142ce88c090892532edf1.png "2")

```lua
-- Language Setting English: When the number of formatter arguments is different from the required number
log(_LocalizationService:SmartFormat("I have {0:plural:one}.", 1))
-- Result: Original text output (because English plural receives only two formatter arguments)

-- Language Setting English: When the formatter argument is left blank
log(_LocalizationService:SmartFormat("I have {0:plural:|}.", 1))
-- Result: normal output (using an empty string as a formatter argument)
```
![22](https://mod-file.dn.nexoncdn.co.kr/bbs/167521385644376cb49230ea64165b8e61d50f95b8c01.png "22")

<br>
##### When not a pair of brackets {}
If the curly brackets are overlapped or the format is incorrect, the conversion will not work properly and the text will be output as it is.

<br>
##### If the number of formatArgument and the index of the argument hole do not match
In this case, depending on the situation, it may be output normally, or it may be output as it is because it fails to convert to fit the format.
Please refer to the example below.
```lua
-- Language Setting English: The number of format arguments is greater than the maximum index of the argument hole +1
log(_LocalizationService:SmartFormat("I have {0:plural:an apple|{} apples} and {1:plural:a cherry|{} cherries}.", 2, 1, 1))
-- Result: normal output

-- Language Setting English: The number of format arguments is less than the maximum index of the argument hole
log(_LocalizationService:SmartFormat("I have {0:plural:an apple|{} apples} and {1:plural:a cherry|{} cherries}.", 2))
-- Result: Original Text Output

-- Language Setting English: Use by skipping the index of the format argument
log(_LocalizationService:SmartFormat("I have {1:plural:an apple|{} apples}.", 0, 1))
-- Result: Normal Output
```
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1675214079174ce82f7da440c49c1a1208d193461cad1.png "4")