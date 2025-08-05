# Course Introduction
The main camera of the game always follows the player.
The camera component that controls the main camera is owned by the MapleStoryPlayer model. By using the Property Editor or script for this model, you can edit the main values.
In this course, we will learn about the full functions of the Camera component by editing it in the Property Editor.
# Approaching Camera Component
In Workspace, select DefaultPlayer model.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1635470229255887ebcf3484b4ceb8a43d4b77d8af063.png "1")
<br>
In the Property Editor, check CameraComponent.
CameraComponent offers several properties for the camera control.
![02](https://mod-file.dn.nexoncdn.co.kr/bbs/1653012043486ba0619b7c3984494b5a7091c43500689.png "02")
<br>
We will examine each of the features of the camera component in detail.
# DeadZone
If you watch the video below, the camera doesn't follow the movement of the player avatar when it moves slightly left or right.
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/1657019820558b44468bafaf543d09447717a4987613f.gif "12")
<br>
When it jumps, the player avatar moves up and down on the Y axis, but the camera doesn't move.
Likewise, while the player avatar stays in the DeadZone, the camera doesn't follow the avatar's movement.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/165702062238605bd4be1a9174ee48bc5ce6512c80f0f.png "3")
<br>
If the DeadZone value is changed to 0, the camera reacts to small movements as shown below.
![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1657019829316abef7f6dc9b94ec79cc6865a63e25d67.gif "13")
<br>
Depending on the game you are creating, when you don't have a DeadZone, players may tire of sensitive camera movement.
However, in a game that requires quick movement, if you set a big DeadZone, the responsiveness may suffer.
If you adjust this value in accordance with the style of game you are creating, it can greatly enhance the feel of the action.
# SoftZone
Outside of the DeadZone is SoftZone.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1657017680023250571fd27f94d1cbda001a2b372ae8c.png "4")
<br>
If a player avatar is located in the SoftZone, the camera will chase after the player avatar to ensure that it doesn't move off the screen.
When moving outside of the SoftZone, the camera chases after the player avatar even faster.
# Damping
Damping is a ratio that connects the before and after movements smoothly by stopping the fast movement of the camera when a player avatar escapes from the DeadZone.
If this value increases, the speed of the camera chasing the player avatar becomes smoother.
As an extreme example, if you greatly increase the values of the Damping and SoftZone, the speed of chasing the player avatar becomes very slow, allowing for situations where the avatar might escape from the camera.
![10](https://mod-file.dn.nexoncdn.co.kr/bbs/16570191045568fcfc3ae70714c339d1c00f8769f5b1a.gif "10")
# CameraOffSet
Edit the location where the camera views the player.
You can better understand if you see the difference between the default value and the edited value below. (Change x value)
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16570177781742ef034f8531742fb95c3726007ac09c2.png "5")![6](https://mod-file.dn.nexoncdn.co.kr/bbs/165701780364165f094a24a814c2ba6410832691f10d2.png "6")
# DutchAngle
Sets the rotation value of the camera.
If you enter 80 for the angle, it will look like the image shown below.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/16570178319148e3e4420a1164958a85a65787011d4a2.png "7")
# Screen Offset
Although you can edit the point of showing the player avatar similar to CameraOffset, this value is the proportion value of the screen based on the subject. You can enter a value between 0 and 1, where a value of 0.5 places the camera at the center. For example, an X value of 0.8 will place the Player Avatar on the right side of the screen as shown below.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/16570178556921cd2a75c27ca4ad1ab666cbadf106ba2.png "8")
# isAllowZoomInOut
In the Maker screen, you can zoom in or out on the screen by moving the mouse wheel up or down.
If this value is set to True, the player can also zoom in and out using the mouse wheel while playing.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/165701854542594cce980f29f4bc9b9c14dc75c766830.gif "9")
# UseCustomBound
You can choose to define and use the camera-restricted area. If the <span style="color: #dc9656">**UseCustomBound**</span> value is <span style="color: #dc9656">**True**</span>, you can use the <span style="color: #dc9656">**LeftBottom, RightTop**</span> property to define camera-restricted area. Press the <span style="color: #dc9656">**Collider Edit**</span> button to either visually check the camera-restricted area through the gizmo displayed in the Scene ![scene](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene.png "scene") or easily adjust the area using a handler.
![UseCustomBound](https://mod-file.dn.nexoncdn.co.kr/bbs/16570204046562c59667490614c61917b3a312c8579a5.gif "UseCustomBound")
If the CameraComponent - UseCustomBound value is <span style="color: #dc9656">**False**</span>, the map area is used as the camera-restricted area. If the MapComponent - UseCustomBound value of the map area is <span style="color: #dc9656">**False**</span>, the map area is set to its default state. When the map area is in the default state, the camera-restricted area is defined as the corrected area based on that default area. However, if the MapComponent - UseCustomBound value is <span style="color: #dc9656">**True**</span>, the map area set by the creator is used as the camera-restricted area. Here is a summary. 
|Component configuration|Camera-restricted area|
|--|--|
|CameraComponent - UseCustomBound - <span style="color: #dc9656">**True**</span>|Use the camera-restricted area defined by the creator|
|CameraComponent - UseCustomBound - <span style="color: #dc9656">**False**</span> <br>MapComponent - UseCustomBound - <span style="color: #dc9656">**True**</span>|Use the map area defined by the creator as the camera-restricted area|
|CameraComponent - UseCustomBound - <span style="color: #dc9656">**False**</span><br>MapComponent - UseCustomBound - <span style="color: #dc9656">**False**</span>|Use the area corrected based on the default map area as the camera-restricted area|