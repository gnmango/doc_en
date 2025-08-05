# Course Introduction
You can edit existing MapleStory Worlds' animations using the animation clip editor or use your own animations in the World.
Let's learn how to configure and use the animation clip editor.
# Introduction to the Animation Clip eEitor
![01](https://mod-file.dn.nexoncdn.co.kr/bbs/16892985632978ba85c6b5a034b8b98f424cf6cc68de5.png "01")


| Number | Description |
| --- | --- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg) | The sprite's loaded via **[Open New]**, and an **[Add]** button will be placed. |
| ![2](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg) | The sprites added to the timeline are played sequentially, and you can check the animation playback position based on the green crosshairs.<br>The Scale slider at the top can be used to zoom in or out.  |
| ![3](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg) | Used to save edited and created resources. <ul><li>Name: Name is a required value. It cannot be saved without a name. </li><li>Category: Select the category where the animation clips will be saved. When saving, the selected category is saved in Resource Storage.</li><li>Description: You can write a description of the animation clip to be saved.</li></ul> |
| ![4](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg) | A timeline window where animations are listed in order. Play order is from left to right.<br>You can change the order of sprites or copy them. You can change the playback position (Offset) and control the playback speed. |
| ![5](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg) | <ul><li>Copy RUID: Copies the RUID of the saved animation clip. </li><li>Upload: Saves edited and created animation clips.</li></ul> |

# Load Resources

| Resource Storage | AimationClip Editor |
| ---------------- | ------------------- |
| <ol><li>Select **Resource Storage** or Panels - Resource Storage next to Scene.</li><li>In MSW Resource - animationclip, open the context menu of the desired animation clip.<br>![03](https://mod-file.dn.nexoncdn.co.kr/bbs/1656418775124de43bfb325a942cab8e9a51e7c2be9c9.png%7B%22width%22:%22240px%22%7D)</li><li>Select Make Animation Clip to open the AnimationClip Editor and load all sprites of the selected animation clip.</li></ol> | <ol><li>Click the **Window - Animation Clip Editor - Open New** button.<br>![04](https://mod-file.dn.nexoncdn.co.kr/bbs/1645696224908f637e6221fe64268b998d7c5f1441f03.png%7B%22width%22:%22240px%22%7D)</li><li>Select the animation clip you want to load.</li><li>Loads all sprites of the selected animation clip.</li></ol> |

> <span style="color: #86c1b9">**Tip**
> When an animation clip is selected in the **Resource Picker**, the animation plays when the mouse cursor is over the image.</span>
> ![02](https://mod-file.dn.nexoncdn.co.kr/bbs/16456955254226df46c48334d46d3ba67812d1387132b.gif)

# Adjusting Offset
The offset positions of animation clips provided by MapleStory Worlds are different.
For some animation clips, the RUID value is sometimes placed in a different place from where it was supposed to be. This is because each resource has a different offset value. If you use a particular animation clip frequently, you can edit the animation to center the offset position. Changing the offset to center can make placing entities easier.

1. Select one of the images on the timeline and adjust the **Offset** value to match the intersection of the green horizontal and vertical lines.
2. Select the first image and press **Shift**.
3. Move the scrollbar to select the last image.
4. Enter <span style="color: #dc9656">**-70**</span> for Y value of **Offset**.
5. Press ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/AnimationClip_Editor/icon_anim_play.png "play") **[Play]** to see if the image plays at the intersection of the lines.
6. Enter a name for the edited animation and click **[Upload]** to save.
![001](https://mod-file.dn.nexoncdn.co.kr/bbs/1689290777042f5a94f531c2c4e9ba9aba655904df826.gif "001")

Offset can also be used to display unusual movements. You can create a jump-like animation by changing the Y value of given frame offsets instead of changing the entire frame.

# Adjusting the Frame Speed
You can control the playback speed of animations created with the same sprite by adjusting the duration of one frame.
The larger the play speed value is, the slower the playback.

| 0.3 | 2.0 |
| --- | --- |
| ![0.3](https://mod-file.dn.nexoncdn.co.kr/bbs/1647569448836918d836f9ef04519bab4799a6bc03399.gif "0.3") | ![2.0](https://mod-file.dn.nexoncdn.co.kr/bbs/1647569468680d2f96a8611664549b55bddc28babe088.gif "2.0") |
1. Select the first image in the timeline and press **Shift**.
2. Move the scrollbar to select the last image.
3. Enter <span style="color: #dc9656">**0.3**</span> for the **Delay** value. 
4. Press ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/AnimationClip_Editor/icon_anim_play.png "play") **[Play]** to see the animation playback speed.
5. Enter a name for the edited animation and click **[Upload]** to save.<br>
![002](https://mod-file.dn.nexoncdn.co.kr/bbs/1689299519528e874a0e831b04b028175d0f44ee42afd.gif "002")

# Adjusting Frame
A frame is a single sprite (image) when the images to be animated are arranged. In the timeline, the leftmost sprite becomes the first frame, and the rightmost sprite becomes the last frame.
You can adjust the animation playback order by selecting frames. You can also add MSW sprites to the MSW clip resource or add your own sprites to create unique animations that are different from the original ones.

Frames can be adjusted by selecting a sprite and dragging it to the desired location or pressing the button.
The frame adjustment icons are as follows.
![17](https://mod-file.dn.nexoncdn.co.kr/bbs/1645703702067ee94dc82642b43f4bfd7039e24970f47.png "17")

| Icon | Description |
| --- | --- |
| ![duplicate](https://mod-file.dn.nexoncdn.co.kr/storage/icons/AnimationClip_Editor/icon_duplicate.png "duplicate") | Duplicate the selected sprite and add it to the end of the timeline.|
| ![forward](https://mod-file.dn.nexoncdn.co.kr/storage/icons/AnimationClip_Editor/icon_move_forward_01.png{"width":"12px"} "forward") | Moves the selected sprite to the front. |
| ![forward](https://mod-file.dn.nexoncdn.co.kr/storage/icons/AnimationClip_Editor/icon_move_forward_02.png "forward") | Move the selected sprite one space (frame) forward. |
| ![backwards](https://mod-file.dn.nexoncdn.co.kr/storage/icons/AnimationClip_Editor/icon_move_backwards_01.png{"width":"12px"} "backwards") | Move the selected sprite back one space (frame). |
| ![backwards](https://mod-file.dn.nexoncdn.co.kr/storage/icons/AnimationClip_Editor/icon_move_backwards_02.png{"width":"12px"} "backwards") | Move the selected sprite to the very back. |


#### Creating Transforming Animations
Let's add a single animation clip and several sprites to create an animation of a transforming monster.
![30](https://mod-file.dn.nexoncdn.co.kr/bbs/164578012706467425729618e4962bbc03bb827cbf967.gif "30")
1. Click **[Open New]** and select **MSW Resource - monster - animationclip-4440**.
<br>
2. Click **[Add]** and select **monster - sprite-171514, 171518, 171541, 171544**. 
<br>
3. Add a sprite and drag to move the frame to the desired location.
    ![003](https://mod-file.dn.nexoncdn.co.kr/bbs/1689294966234e70238ff30c74c5bbc6c42c0ee62b076.gif "003")
<br>
4. <span style="color: #dc9656">Enter the **name**</span>, change <span style="color: #dc9656">**Category**</span> to **NPC**, and select **[Upload]**.
<br>
5. See the added animation clip from **Resource Storage - animationclip**.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1689299814441db559f684d9d4fdcb61dc2446c450b38.png "3")
<br>


# Making My Animation
Create your own sprites to create your own unique animation clips.
If you do not have your own sprite, download the file below and follow along.
$$attachFile
{"name":"Frame.zip","url":"https://mod-file.dn.nexoncdn.co.kr/storage/DOCs/Frame.zip"}
$$

1. Select **Resource Storage - My Resources - sprite - npc**.
The creator's sprites are saved to the folder you choose.
<br>
2. Click the **[+]** button on the left side of the search bar to load the file.
    ><span style="color: #7cafc2">**Tip**
    > Multiple files cannot be loaded at once.</span>
<br>
3. Open the **AnimationClip Editor** window via Window and select **[Add]**.
<br>
4. Select and add sprites one by one from **Resource Picker - My Resources - sprite - npc**.
    ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/16893001407289c394805a7554de291f015ca115e2d24.gif "11")
<br>
5. Select the added sprite and add it to the timeline. 
<br>
6. Press ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/AnimationClip_Editor/icon_anim_play.png "play") **[Play]** to see the animation.
    ![005](https://mod-file.dn.nexoncdn.co.kr/bbs/168930039655358decfac3af84d658e8a18fcbbcf34b9.gif "005")
<br>
7. Enter the <span style="color: #dc9656">**name**</span>, change <span style="color: #dc9656">**Category**</span> to NPC, and select Upload.
    ![15](https://mod-file.dn.nexoncdn.co.kr/bbs/16893005211903635451de29d4a9598420363cd213774.png "15")



