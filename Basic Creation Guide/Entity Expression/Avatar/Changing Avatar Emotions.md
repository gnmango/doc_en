# Course Introduction
You can change the player avatar's expression using `PlayEmotion` of AvatarRendererComponent. You can connect a certain key with an emotion to express emotion, or play a particular emotion when hit by a monster. Displaying various emotions allows players to communicate actively and conveniently.

# Emotion Types
There are a total of 22 emotions. Emotions appear differently depending on the avatar's face. Thus, they might differ from what's seen in the guide.

| Emotion | key | Example |
| --- | --- | --- |
| Hit | Hit | ![hit](https://mod-file.dn.nexoncdn.co.kr/bbs/16396401188336e54b6adf42b4088ad1c61a60f9c8731.png "hit") |
| Smile | Smile | ![smile](https://mod-file.dn.nexoncdn.co.kr/bbs/16395549609346e876f6386bc4eda97306a53e89f0895.png "smile") |
| Troubled | Troubled  | ![troubled](https://mod-file.dn.nexoncdn.co.kr/bbs/16395541972918701350d6bd1425384b71f7eef150433.png "troubled") |
| Cry | Cry |  ![cry](https://mod-file.dn.nexoncdn.co.kr/bbs/1639554951149c507945a898d4ec58fd2629df70cbeee.png "cry")|
| Angry  | Angry | ![angry](https://mod-file.dn.nexoncdn.co.kr/bbs/16395560553809d8ab80b78c14f0faec4be3781b0a5fb.png "angry") |
| Bewildered | Bewildered  |  ![bewildered](https://mod-file.dn.nexoncdn.co.kr/bbs/16395560679398458bd2b686545c4bb2a8d20fe31f94b.png "bewildered")  |
| Stunned | Stunned | ![stunned](https://mod-file.dn.nexoncdn.co.kr/bbs/1639556098745cf8aff308154417c80014aa9b6e9d68e.png "stunned") |
| Vomit  | Vomit | ![vomit](https://mod-file.dn.nexoncdn.co.kr/bbs/163955611166849281f37d4bb4107a3dce003f53d1fbc.png "vomit") |
| Oops | Oops | ![oops](https://mod-file.dn.nexoncdn.co.kr/bbs/1639556132963546e5fdf3cde4a50b027994f3d086744.png "oops") |
| Cheers | Cheers |![cheers](https://mod-file.dn.nexoncdn.co.kr/bbs/16395561463349a5bd1cd4ff8472bbea5a534cb19b75e.png "cheers")  |
| Kiss | Chu | ![chu](https://mod-file.dn.nexoncdn.co.kr/bbs/163955615577069cd9078b46c4f16aabf999e635012bf.png "chu") |
| Wink |Wink | ![wink](https://mod-file.dn.nexoncdn.co.kr/bbs/1639556257543f249e7a3d02a4707bdee12ea1acf54e2.png "wink")| 
| Pain | Pain  | ![pain](https://mod-file.dn.nexoncdn.co.kr/bbs/16395562727335d82e3d6a88242839d6f176747b03346.png "pain") |
| Shining eyes | Glitter  | ![Glitter](https://mod-file.dn.nexoncdn.co.kr/bbs/166684095544355abeeb3a60e4e85a4118483915dfd1d.png "Glitter") |
| Despair | Despair |  ![despair](https://mod-file.dn.nexoncdn.co.kr/bbs/16395562836016f39a4e3e8a54ef8ae0566e3b8862318.png "despair")|
| Love | Love | ![love](https://mod-file.dn.nexoncdn.co.kr/bbs/1639556295930004061735ea14375bd2d90ede8cdc232.png "love") |
| Shine | Shine |  ![shine](https://mod-file.dn.nexoncdn.co.kr/bbs/163955630509906604406ad084f3284149221faaa08ce.png "shine")|
| Demonic | Blaze |![blaze](https://mod-file.dn.nexoncdn.co.kr/bbs/163955631518406f4825034d74f1cb1952c139f46face.png "blaze")  |
| Hum | Hum | ![hum](https://mod-file.dn.nexoncdn.co.kr/bbs/1639556324667dee4cdfa35eb41148d1becc71419f1fd.png "hum") |
| Bowing | Bowing | ![bowing](https://mod-file.dn.nexoncdn.co.kr/bbs/1639556334026de94b9e28d764207a8bd6e58d4429199.png "bowing") |
| Hot | Hot | ![hot](https://mod-file.dn.nexoncdn.co.kr/bbs/163955640187085d52926ecf749ee8d6884447137ecd6.png "hot") |
| Teasing | Dam | ![dam](https://mod-file.dn.nexoncdn.co.kr/bbs/1639556413232d9ef215a4d144933ab1ae2fc01aa618b.png "dam") |
| Sweat | qBlue |![qblue](https://mod-file.dn.nexoncdn.co.kr/bbs/1639556423490af2cf4bd2f4445898d4edc084cc4d99e.png "qblue")  |
# Changing Avatar's Emotion
Pressing number keys 1 through 5 will play each corresponding emotion for 5 seconds.

1. Create a new component in MyDesk, and open the Script Editor.
2. Press ![AddPropertyFunction](https://mod-file.dn.nexoncdn.co.kr/bbs/1635134683611e1fb1dfaa0c94f7386a012836b986188.png "AddPropertyFunction") Add next to Function: and select **New**. <br> ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/164006572761813e6a63db24c41c8a87930b114f0ae3d.png "1")
3. Change the function name to Emote, select More ![script_more](https://mod-file.dn.nexoncdn.co.kr/bbs/16345206995612a35d54577d8466da03a9fe452a5218c.png "script_more"), and click **Add Parameter** twice and enter parameter.<br>![parameter](https://mod-file.dn.nexoncdn.co.kr/bbs/164006784006957fd839bd3d546f8aa9c9e1639bfb3a0.png "parameter")
5. Use PlayEmotion to write emotions to play.
6. Add KeyDownEvent to Entity Event Handler, and write down input keys needed to play the emotions.

```lua
Method:
[server]
void Emote (number key, string userId)
{
    local avatar = _UserService:GetUserEntityByUserId(userId).AvatarRendererComponent

    if key == 1 then
        avatar:PlayEmotion(EmotionalType.Angry, 5) -- Plays Angry emotion for 5 seconds
    elseif key == 2 then
        avatar:PlayEmotion(EmotionalType.Cry, 5)
    elseif key == 3 then
        avatar:PlayEmotion(EmotionalType.Chu, 5)
    elseif key == 4 then
        avatar:PlayEmotion(EmotionalType.Smile, 5)
    elseif key == 5 then
        avatar:PlayEmotion(EmotionalType.Wink, 5)
    end
}
     
Event Handler:
[Client Only] [service: InputService]
HandleKeyDownEvent (KeyDownEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: InputService
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    -- local key = event.key
    ---------------------------------------------------------
    
    local myUserId = _UserService.LocalPlayer.OwnerId
    
    if key == KeyboardKey.Alpha1 then -- Press number 1, emotion from Emote function's key 1 will be played.
        self:Emote(1, myUserId) 
    elseif key == KeyboardKey.Alpha2 then
        self:Emote(2, myUserId)
    elseif key == KeyboardKey.Alpha3 then
        self:Emote(3, myUserId)
    elseif key == KeyboardKey.Alpha4 then
        self:Emote(4, myUserId)
    elseif key == KeyboardKey.Alpha5 then
        self:Emote(5, myUserId)
    end
}
```    

![emotion](https://mod-file.dn.nexoncdn.co.kr/bbs/1656036982832fc9cbd58c5f241c0a6b505178529fffb.gif "emotion")