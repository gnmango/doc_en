# Course Introduction
Let's learn about LocaleDataSet, which is necessary for translating the text of the World.

##### Reference Guide
[Understanding Localization](docs/?postId=951{"target":"_self"})
[Localizing to Format](docs/?postId=968{"target":"_self"})
[Automatic Translation]((docs/?postId=1072{"target":"_self"}))
# Introduce LocaleDataSet

LocaleDataSet is the dedicated DataSet for translation. LocaleDataSet has two types: **Default and WebSync**. You can create LocaleDataSet of **Default or WebSync** from **Workspace - MyDesk - Create LocaleDataSet**.
![LocaleDataSet](https://mod-file.dn.nexoncdn.co.kr/bbs/16983152847384b0c9b653fc74b3ea842f39a739ac243.png%7B%22width%22:%22540px%22%7D)
#### Default
This is the default LocaleDataSet. Key, Source, Note is comprised of the Key, Source, and Note columns as a default, and the creator can add more columns to add translations and manage them. The Default type LocaleDataSet isn't linked with the official website's managed translations. You can add language codes by pressing the add column button and selecting from the list of language. Duplicate language codes can't be added.

![default](https://mod-file.dn.nexoncdn.co.kr/bbs/16988269998795f9915d9c6c5478d8c93413d8b9bf3e7.png{"width":"540px"} "default")
#### WebSync
WebSync LocaleDataSet is the LocaleDataSet that processes the contents translated by the translation management on the official website. You can check it by linking it to the LocaleDataSet Editor in Maker. It saves the last editing time and links with the translation management on the official website.
WebSync LocaleDataSet can only edit the Key, Source, and Note columns, and you cannot add new columns in the Maker. If you synchronize the translation, an unmodifiable column is automatically added.

![WebSync](https://mod-file.dn.nexoncdn.co.kr/bbs/169837602461617090b6ba590446daa40528e6ab4150c.png{"width":"540px"} "WebSync")

# LocaleDataSet Editor
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1753842642413a2078451cc064854bd72cc29d58dec02.png "LocaleDataSet")
| Numbers | Description |
| --- | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1")  | **Column Cell**<br>Defines the data name of each row, i.e. the column name. The default fixed column cells cannot be deleted.<br><ul><li><span style="color: #dc9656">**Key**</span>: The identifier key. You can't use duplicated keys.</li><li><span style="color: #dc9656">**Source**</span>: Source language, source content</li><li><span style="color: #dc9656">**Note**</span>: Enter the additional information you want to provide to the translator. You can find this information from the translation management on the official website.</li></ul> |
| ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") | **Cell**<br>Each set is an input text field. Insert the data value suitable for the definition of each row. The inserted data values are all saved as strings. |
| ![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg "3") | **Add Row Button**<br>Add a new row at the end of the row by pressing the button.|
| ![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg "4")  | **Add Column Button**<br>Add a new column at the end of the column by pressing the button. If the LocaleDataSet is set to Default, you can add language codes to add translation. It is unavailable if it's WebSync.|
| ![5](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg "5")| **Sync Button**<br> Synchronize the translations from the official website's managed translations. Press the Sync button to add a column to the editor. Added columns can't be done from the maker.<br> **Load Data from the Web**<br>You can load data from a Google SpreadSheet.<br>After writing data in a Google SpreadSheet, input the data's URL into the pop-up window to overwrite the data with that in the Google SpreadSheet. However, only Google SpreadSheet URLs that have been posted on the web can be used. <br>When using the Import function, the header rows need to be in the order of Key, Source, and Note to match the LocaleDataSet format. All other data will be deleted before importing.<br>![NO_05_1](https://mod-file.dn.nexoncdn.co.kr/bbs/16577012857253f3f35d4a11f4cfbb9faaa60264c18b9.png "NO_05_1")<br>**CSV Import**<br>You can load CSV files saved on the computer to overwrite the current data. You can only load CSV files that have been saved as utf-8.<br>When using the Import function, the header rows need to be in the order of Key, Source, and Note to match the LocaleDataSet format. All other data will be deleted before importing. |

# Manage Translation
The WebSync-type LocaleDataSet created by the Maker can be translated on the official website. You can also link the contents translated by the translation management to the LocaleDataSet editor in the Maker.
Translation management is available at **Official website - Create Worlds - Creator's Created Worlds - Translation Management - World Contents**. MapleStory Worlds currently supports Korean and English translations only.

![WebLocalizing](https://mod-file.dn.nexoncdn.co.kr/bbs/172724402417118aff0d5161145cd98d8e0a6feaeeae7.png{"width":"960px"} "WebLocalizing")

| Numbers | Description |
| --- | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1") | **Worlds Content** <br>List of LocaleDataSet created by Maker. |
|  ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") | **Sync UP**<br>You can update the information of LocaleDataSet created by the Maker by pressing the button.  |
| ![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg "3") | **Search Bar**<br> You can search for translations by Key, Source, and Note. |
| ![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg "4") | **Translation**<br> You can view and translate the Key, Source, and Note information together. You can save a translation by pressing the **[Save]** button. To link the translation to the Maker, you need to press the **[Sync]** button in the Maker.  |