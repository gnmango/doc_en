# Course Introduction
MapleStory Worlds provides an automatic translation feature for languages not supported by creators. This feature lessens the translation burden for creators and makes it easier for people of different languages to enjoy the Worlds.
In this course, let's explore how the automatic translation feature works and how to configure it.

##### Reference Guide
[WorldConfig]
[Understanding Localization]
[Localizing to Fit Format]

##### API Reference
[TextComponent](/apiReference/Components/TextComponent{"target":"_self"})
[TextInputComponent](/apiReference/Components/TextInputComponent{"target":"_self"})
[TextRendererComponent](/apiReference/Components/TextRendererComponent{"target":"_blank"})
[ChatBalloonComponent](/apiReference/Components/ChatBalloonComponent{"target":"_self"})


# About Automatic Translation Concept
Automatic translation is a feature that uses machine translation to provide translations for languages that are not officially supported by the creator.

For example, imagine that a creator whose native language is English, but who is not a bilingual person, has created and released a World.
This creator creates the World alone, so it would take too long to provide multilingual translations of every word and sentence used in the World. In this case, the creator can use the automatic translation. The creator releases a World after writing only the original text in their native language, the basis for automatic translation, and setting the supported language to English only.

If a player in this World who only uses Korean changes the World Language setting to Korean, that player can play the World in Korean. Among the languages that appear in the <span style="color: #dc9656">**Language (Auto Translation)**</span> under the World Settings, select Korean, the source language to translate, to translate the World's language.

> <span style="color: #585858">**Learn More**
> * The settings information for the automatic translation option is stored on a per-device basis.
> * Please note that if the content to be translated exceeds 5,000 characters, it will not be translated.</span>

# How to Set Automatic Translation
The creator must activate <span style="color: #dc9656">**AllowAutomaticTranslation**</span> for the player to use the auto translation function from language settings. This property is located in `TextComponent`, `TextInputComponent`, `TextRendererComponent`, and `ChatBalloonComponent`. Let's look at how to set up the auto translation function using an example.

1. Activate <span style="color: #dc9656">**AllowAutomaticTranslation**</span> from the `TextComponent`, `TextInputComponent`, `TextRendererComponent`, and `ChatBalloonComponent` for text entities that will be auto translated.
![AllowAutomaticTranslation](https://mod-file.dn.nexoncdn.co.kr/bbs/16983007897332bb3b13efdb14c27baa6ce77d2fd6f42.png{"width":"640px"} "AllowAutomaticTranslation")
2. When you release a World, set the original language set by the creator and the language that you provide translation to <span style="color: #dc9656">**Supported Languages**</span>. Languages not set as supported languages at release will be automatically translated. 
In the released World, all entities with AllowAutomaticTranslation enabled will be translated automatically if the player sets the target language for automatic translation from the World Language. 

![Realase](https://mod-file.dn.nexoncdn.co.kr/bbs/1727252833488d67e8db9410f4f89a1051628519188a2.png{"width":"640px"} "Realase")

><span style="color: #7cafc2">**Tip**
> The original language chosen by the creator as the basis for the translation can be set in **Workspace - WorldConfig - SourceLanguage**.</span>
> ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1698826078961925a793678514dc0821fb980cf8072ee.png "1")

><span style="color: #585858">**Learn More**
> When a creator creates a tempoary input in the world using a language not set as the SourceLanguage, the auto translation may not function. This is because the auto translation is processed assuming the language set as the SourceLanguage regardless of the written language. </span>

# How Automatic Translation Works
Players who have accessed the released World can choose which language they want to play in from the World Language Settings window. Unsupported languages for that World will appear as targets for automatic translation in the World Language list. All entities with the <span style="color: #dc9656">**AllowAutomaticTranslation property**</span> enabled will be translated automatically if the player sets the target language for automatic translation from the World Language.

![language](https://mod-file.dn.nexoncdn.co.kr/bbs/173977520738997b512c5efbc4fb1a2b32d77182fd8ce.png{"width":"740px"} "language")