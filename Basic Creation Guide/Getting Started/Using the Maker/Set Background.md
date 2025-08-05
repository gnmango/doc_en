# Course Introduction
Let's set up the map's background using **Preset List** and **BackgroundComponent**.

# Setting a Template Background
You can select various backgrounds of MapleStory from **Preset List - Background**.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1660872151969aea4b5e0f90c4c2ab1c556a2ea900127.png "1")

After selecting a preset background, you can adjust the speed at which it moves (i.e. **ScrollRate**).
![1a](https://mod-file.dn.nexoncdn.co.kr/bbs/1676524214491fd465cd70f484b7baa55a9c7cceb1a78.png "1a")

The higher the **ScrollRate** value, the faster the background will move. A negative ScrollRate will cause the background to move in the opposite direction.

| ScrollRate | Image |
| :---: | :---: |
| 1 | ![s1](https://mod-file.dn.nexoncdn.co.kr/bbs/1676524614371f682d76a305e4987aa04507430a3f891.gif "s1") |
| 10 | ![s2](https://mod-file.dn.nexoncdn.co.kr/bbs/1676524628557d5e56dce60f74ab59a962793a594b408.gif "s2") |
| -10 | ![s3](https://mod-file.dn.nexoncdn.co.kr/bbs/16765246400798c2f36ddf63a47bc92883f9d493b22d6.gif "s3") |

# Setting Solid Background
You can set a solid background from **Preset List - Solid Background**.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1660872533050bd027ce1934b47669a0f3d3f82994180.png "2")

If you want to set different colors from those provided as presets, press the **[+]** button to open the color selector and set the color you want.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1660872694830338c5a59114a46aa9d41eeecbfae1203.png "3")

# Setting a Gradient Background
Select **Hierarchy - Current Map - Background** and change **Type** to **Gradient** in **BackgroundComponent** of the Property Editor to set a gradient background. 
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1675686701848d25aebc9b2b645519a894cb17489b07a.png "7")
Select the top, middle top, middle bottom, and bottom colors for gradation from the color palette. In addition, use the **MiddleTopRatio, MiddleBottomRatio** properties to decide where to place the middle top and middle bottom colors. Likewise, you can easily create a gradient background in the desired style by using **Gradient** Type.

# Setting Image Background
## Setting the Background with a Saved Image
In **Preset List - Image Background**, you can set the background using an image you have saved.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/166242041253011d9fd857ab84531a074ed6982123704.png "4")

## Setting Web Image Background
You can set images from the web as the background.
1. Copy the image link that you want to set as the background.
    ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1662420478272932b474fef2c4e7589e49f76d4ce4b4f.png "5")
    <br>
2. After setting the Type of BackgroundComponent to Web, paste the copied address into WebURL.
    ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1662419129189d4b5d5fb2028458baa10f740ecfbac45.png{"width":"750px"} "6")
    <br>
3. Let's press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") Start and test. You can see that the image on the web has been set as the background as below.
    ![bg](https://mod-file.dn.nexoncdn.co.kr/bbs/166241921017719b68e8cb4674133892283180eb6a3e2.gif "bg")