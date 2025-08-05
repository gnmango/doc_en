# Course Introduction
The MapleStory Worlds Maker offers a **UI Editor** to edit UI. You can implement the character's name, stat window, chat window, etc. using the UI Editor. You can also create a scenario that brings the game to life.
Let's create the Welcome Message UI at the top of the screen using **image** and **text**, basic features of the UI Editor.
![22](https://mod-file.dn.nexoncdn.co.kr/bbs/16607859751670dad1471dc514cef891e7f484d61bf20.png "22")

##### Reference Guide
* [UI Editor](/docs?postId=120{"target":"_blank"})
* [Basic UI Components](/docs?postId=744{"target":"_blank"})
* [Controlling UI Entities](/docs?postId=1154{"target":"_blank"})

# Adding Text to a New Group
Let's add <span style="color: #dc9656">**Hello MapleStory Worlds**</span> in the top middle of the screen.

1. On the bottom right, click the ![Common_layer_add](https://mod-file.dn.nexoncdn.co.kr/bbs/16345369150225f875c8c218444c58189ffac05201631.png "Common_layer_add") button to add a new group.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16566580669786e89df499cff4c42aaa5046cd1e5126d.png "4")
<br>
When you first add a new group, **UIGroup** is created, and **UIGroup** will be selected in the group selection window.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1635466100477be5787d22e4745bca8948e07dc1fe6aa.png "5")
<br>
2. Add a text box to the added **UIGroup**. From the left toolbox, click the ![Ui_Text](https://mod-file.dn.nexoncdn.co.kr/bbs/16345252157689279f2e1212f460381577fe950d87703.png "Ui_Text") button. The text box is placed in the center.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1635466110759f18631e58b2845e08eefdadee9584e42.png "6")
    <br>
3. Let's set the location and size of the text box.  Enter the value in **Property Editor - UITransfromComponent** as follows.
    * PosX = 0
    * PosY = 400
    * Width = 500
    * Height = 100
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1635466119736567a06e7efb749f0bd593cf5583c7ca0.png "7")
    <br>
3. Let's change the background color of the box. Set the **Color** value of **SpriteGUIRendererComponent** as follows.
    * R: 255
    * G: 255
    * B: 255
    * A: 100%
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1719208795002a31a47af5b3940e6a51f594f60237bf7.png "7-1")
<br>
4. Click the right button on **ImageRUID** of **SpriteGUIRendererComponent** to open the **Resource Picker** window.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/168085693599993c114d51e92442c911b153b975dc647.png "9")
<br>
5. In the **Resource Picker** window, a list of images that can be used as the background of the text box is output.
    * RUID: b2efe9b7579bd494baecc7a2ef3
![TextAndImage_10](https://mod-file.dn.nexoncdn.co.kr/bbs/1680857186821081809892ed1431e9ad72ed3ee213001.png "TextAndImage_10")
<br>
6. Select the image and you'll see the UI applied as follows.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/16354667716403f561d9f75ef4c77ace222e41916872f.png "11")
<br>
7. Edit the property of **TextComponent** as follows.
    | Component | Property | Value |
    | --- | --- | --- |
    | @cols=1:@rows=2:TextComponent | Text | Hello MapleStory Worlds |
    | FontSize | 30 |
    <br>

    > <span style="color: #7cafc2">**Tip**
    > This text box is visible in the UI Editor, but not visible during actual play. This is because the added **UIGroup** is set to be invisible by default.</span>

8.  Select **Hierarchy - ui -  UIGroup**.
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/165950430779879b381eacaa846408419a337d605f4bc.png "12")
<br>
9. Make the **UIGroup** always visible by enabling the **DefaultShow** of **UIGroupComponent** in the Property Editor of **UIGroup**.
![TextAndImage_13](https://mod-file.dn.nexoncdn.co.kr/bbs/1634690751616957f4000f31a406da4fcb2651c1803ed.png "TextAndImage_13")
<br>
10. Click the ![Tool_UI](https://mod-file.dn.nexoncdn.co.kr/bbs/163453120840744616a62243642e889159a68a78a56c2.png "Tool_UI") button at the top to stop the UI Editor.
![ui](https://mod-file.dn.nexoncdn.co.kr/bbs/16607866498590ff06626406440d4a3a3d75e72800f6e.png "ui")
<br>
11. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. You'll see the newly added text is shown in the play screen.
![15](https://mod-file.dn.nexoncdn.co.kr/bbs/16607861764157e8a105a04e642c09ef7ec13e3622d6a.png "15")

# Adding Images
Let's add a meso image to make the text box stand out a little more.
![16](https://mod-file.dn.nexoncdn.co.kr/bbs/1660786201599b49010a72c294a5eb6bc3d8850eb706b.png "16")
<br>
1. After opening the UI Editor, click **UIGroup**.
![17](https://mod-file.dn.nexoncdn.co.kr/bbs/16354668571302757ee322edb4681a95ecc00359dbe19.png "17")
<br>
2. From the left toolbox, click the **[Image]** button to add an image UI.
![18](https://mod-file.dn.nexoncdn.co.kr/bbs/1660786345914ecc31fc8aa174ed39195af5c34ed77c2.png "18")
<br>
3. In the Property Editor of added images, click the **ImageRUID** right button of **SpriteGUIRendererComponent** to open the **Resource Picker** window.
    * RUID:fd255d50cb5177545a0e71c4a84   
![002](https://mod-file.dn.nexoncdn.co.kr/bbs/16808600045139383ec457a95496990e109b3e927509f.png "002")

4. Enter the value in Property Editor - **UITransfromComponent** as follows. 
    * Pos X = -257
    * Pos Y = 403
    * Width = 70
    * Height = 70
  ![21](https://mod-file.dn.nexoncdn.co.kr/bbs/1656658276777578d07057c8148a09fb8eced28b3a654.png "21")

5. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. You can see that the images and text boxes are well placed.
![22](https://mod-file.dn.nexoncdn.co.kr/bbs/16607862245222f3b07edaa1f4535b86ee27851a9646a.png "22")