# Course Introduction
A localization process is a must if a creator wishes to ensure their world is enjoyed by users of multiple regions. 
`LocalizationService` helps to provide text or resources that match the user's app language settings. In this lesson, we will explore the concept of translation and how to use `LocalizationService`.
##### Reference Guide
[Localizing to Format](docs/?postId=968{"target":"_self"})
[Automatic Translation]((docs/?postId=1072{"target":"_self"}))
[LocalDataSet](docs?postId=1120{"target":"_self"})
##### API Reference
[LocalizationService](/apiReference/Services/LocalizationService{"target":"_self"})

# What is localization?
In the simplest sense, <span style="color: #dc9656">**localization**</span> is basically <span style="color: #dc9656">**translating**</span> a word or phrase that the user sees in the worlds. For example, a World may have been created in Korean, but when users who have set their client language to English enter that World, a word like **사과**</span> will be automatically translated as <span style="color: #dc9656">**apple**</span>. However, the difference between translating and localizing is supporting not only the basic meaning of the language, but also the cultural context and tone of the world's many elements. 

Localization is the process of translating the language and various elements of a world appropriate to the desired language and culture. An example would be how different regions list their dates. In Korea, the order is <span style="color: #dc9656">**Year/Month/Day**</span>, (01/31/2023). In America and Canada, the order is normally <span style="color: #dc9656">**Month/Day/Year**</span> (01/31/2023). In others, the order can be <span style="color: #dc9656">**Day/Month/Year**</span> (31/01/2023). 
The way numbers are written also differs in different regions. For example, people write the number <span style="color: #dc9656">**1234567.89**</span> as <span style="color: #dc9656">**1,234,567.89**</span> in South Korea, the United States, and Canada. They use a comma (,) as the thousands separator and a period (.) as the decimal point. However, in France, Germany, and Austria, they write it as <span style="color: #dc9656">**1.234.567,89**</span>. They use a period (.) as the thousands separator and a comma (,) as the decimal point. Some regions even use spaces instead of thousands separators, such as <span style="color: #dc9656">**1 234 567.89**</span>. 

Therefore, considering the expectations of users from different cultures and regions, it is necessary to express dates or numbers in formats that correspond to those regions. As such, localization is not just a simple translation, but the entire process of adjusting content so that users connecting from each cultural area can use content without confusion or inconvenience.

## Localization of MapleStory Worlds
MapleStory Worlds is aiming for a long-term global one build. As such, it is possible for clients from different regions to be connected together on a single server. So MapleStory Worlds supports simultaneous translation into multiple languages from a single text.
![101](https://mod-file.dn.nexoncdn.co.kr/bbs/167479872265151ac9e8a10ee49638f070ced7e26e7b1.png "101")

However, the server should send the Localizing Key instead of directly sending the localized message itself. It communicates with the localizing key and outputs different data for each client language setting. Let's take a look at how to do actual localization in MapleStory Worlds with an example.

# Translate Text
You need to create a translation table to translate text according to the client language setting.

#### Create LocaleDataSet
Let's use LocaleDataSet to translate text in a simple way.

1. Add a text entity in the UI editor.
    ![islocal](https://mod-file.dn.nexoncdn.co.kr/bbs/1686728912583c7cafe2f6fa244b49e51195f776c6ed2.png "islocal")
    Modify the property in **TextComponent** for the new text entity as follows.
    | Component | Property | Value |
    | :---: | :---: | :---: |
    | @cols=1:@rows=3:TextComponent | FontSize | 50 |
    | Text | UI_Hello |  
    | IsLocalizationKey | True |  
    
2. Create **LocaleDataSet** and enter the following.
    | ID | Key | Source | ko | en |
    | :---: | :---: | :---: | :---: | :---: | 
    | 1 | UI_Hello | Hello! | 안녕하세요! | Hello! |
 
3. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test with the client's language set to Korean. <span style="color: #dc9656">
    You can see that **안녕하세요!**</span> is displayed. 
    ![hello1](https://mod-file.dn.nexoncdn.co.kr/bbs/1674808092988390b7fa2c4e648b28503ad01d9a744a5.png "hello1")
  
4. Change **File - Setting - Others - General - Language** to (English) in the navigation menu.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1727238891918e6796e5e12344992b411122ba49c8ebf.png "3")
    
 5.  Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test.  <span style="color: #dc9656">**Hello!**</span> is displayed.

    ![hello2](https://mod-file.dn.nexoncdn.co.kr/bbs/167480819908819eb8fb051a2483d878600f5f3fa548f.png "hello2")

## GetText Function
You can easily set the **TextComponent** text to be translated with **IsLocalizationKey**, but use the `GetText()` function when you need to translate other texts or control translation in a script.
The `GetText()` function finds a row in the **Key** column of the translation table with the same value as the key received as argument and returns the text value from the same column as the client language code in that row.

Let's look at how to display text for each language setting of the client with a simple example. Let's place 2 NPCs on the map and display appropriate names according to the language settings.

1. Place 2 NPCs on the map. In this example, we have placed the two NPCs below.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/167410631018585e61a9abb2544af96dbb18c261313f3.png "8")
    * npc-931
    * npc-845
  
2. Modify the name of each NPC entity as shown below.
    | NPC | Entity Name | 
    | :---: | :---: |
    | ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1674097464492fb3054921620425a80664d1380f3f695.png "6") | Stan | 
    | ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/16740974755475f8d0c294b5a491f907eefb8acc7bad1.png "7") | Rina | 
   
3. Create **LocaleDataSet** and enter as follows.
    | ID | Key | Source | ko | en |
    | :---: | :---: | :---: | :---: | :---: | 
    | 1 | UI_TEXT_NPC_Stan | Stan | 스탄 | Stan |
    | 2 | UI_TEXT_NPC_Rina | Rina | 리나 | Rina |
    
4. In the context menu of **Workspace - MyDesk**, click **Create Scripts - Create Logic** to add a new logic with the name <span style="color: #dc9656">**LocalizationTest**</span>.
   
5. Add properties to **LocalizationTest** as follows.
    ```lua
    Property:
    [None]
    Entity Stan = /maps/map01/Stan
    [None]
    Entity Rina = /maps/map01/Rina
    ```
    
6. Add the `OnBeginPlay()` function and enter the following. Set the execution space to **client only**.
    ```lua
    [client only]
    void OnBeginPlay()
    {
        self.Stan.NameTagComponent.Name = _LocalizationService:GetText("UI_TEXT_NPC_Stan")
        self.Rina.NameTagComponent.Name = _LocalizationService:GetText("UI_TEXT_NPC_Rina")
    }
    ```
   
7. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test with the client's language set to Korean. 
    You can see that the NPC names appear as below.
    ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/16741065097102ce4bfb23e3c453aaae7dc1b04f14365.png "9")
    <br>
    If you change the language settings to English, you can see that the NPC names appear in English as shown below.
    ![9en](https://mod-file.dn.nexoncdn.co.kr/bbs/1674106659362b89a441ef8b045ed811972bb1bdd84e7.png "9en")

### Translator
The **LocalTranslator** property is in **LocalizationService**.
* The type of the **LocalTranslator** property is [**Translator**](/apiReference/Misc/Translator{"target":"_self"}).
* **LocalTranslator** includes **LocaleId** (string type property).

The next two lines of code return the same value.
```lua
_LocalizationService:GetText("UI_TEXT_NPC_Stan")                   -- Stan
_LocalizationService.LocalTranslator:GetText("UI_TEXT_NPC_Stan")     -- Stan
```

**LocaleId** of **LocalTranslator** is the language code for the currently executing client.
For example, if the language for the currently executing client is Korean, the **LocaleId** of **LocalTranslator** is **ko**.
If you call a function such as `GetText()` directly without using **Translator** created separately in **LocalizationService**, the function is performed based on the **LocaleId** for **LocalTranslator**.
If the current client language setting is Korean and you want to import an English translation, you can write the code as follows.

```lua
local enTranslator = _LocalizationService:GetTranslatorForLocale("en")
enTranslator:GetText("UI_TEXT_NPC_Stan") -- Stan
```

# Resource Localization
The resources as well as the text can be localized as needed.

Let's go through an example.

1. Press the [ ![image](https://mod-file.dn.nexoncdn.co.kr/storage/icons/UI/icon_image.png "image") ] image button to create an empty image and change its name to **SpriteTest** in the UI editor.
    
2. Generate a new script component, <span style="color: #dc9656">**SpriteLocalization**</span>. Then, add the component to the **SpriteTest** image created above.
    
3. Open the **SpriteLocalization** script and add the property as follows.
    ```lua
    Property:
    [None]
    string ResourceKey = ""
    ```
  
4. Add the `OnInitialize()` function and write its content as follows. Set the execution space to **client only**.

    ```lua  
    [client only]
    void OnInitialize()
    {
        self.Entity.SpriteGUIRendererComponent.ImageRUID = _LocalizationService:GetText(self.ResourceKey)
    }
    ```
    
5. In the **SpriteTest** property editor, enter <span style="color: #dc9656">**RESOURCE_LOGO**</span> to **SpriteLocalization - ResourceKey**.

    ![14](https://mod-file.dn.nexoncdn.co.kr/bbs/1674184215431d6a44d479721419f9e694e289de686ce.png "14")
    
6. Open **LocaleDataSet** and add the following content.
    * **Key** : <span style="color: #dc9656">**RESOURCE_LOGO**</span>
    * **Source** : <span style="color: #dc9656">**3110502e753742d39794e042c2657ce5**</span>
    * **ko** : <span style="color: #dc9656">**7ae6a288ee884b75a44851c59b595789**</span>
    * **en** : <span style="color: #dc9656">**3110502e753742d39794e042c2657ce5**</span>
    <br>
7. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. 
   The Korean logo appears when the language setting is Korean, and the English logo appears when the language setting is English.

    ![15](https://mod-file.dn.nexoncdn.co.kr/bbs/16741847984983e92b71d5e1f48d3b3469a98d7feb597.png "15")