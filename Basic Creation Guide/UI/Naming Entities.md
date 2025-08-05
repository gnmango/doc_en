# Course Introduction

One of the most effective ways to distinguish objects is to name them.
Names can be used to classify objects, NPCs, and users.
Here's how to use NameTagComponent and some examples.

# Naming

Select an entity, and add <span style="color: #dc9656">**NameTagComponent**</span>.
You can change the property value to make a name tag.
![NametagComponent](https://mod-file.dn.nexoncdn.co.kr/bbs/1633505174925b7f918183b8d46d996e32b3e84b3c15d.png)

The property composition of NameTagComponent is as follows.

| Name | Description |
| --- | --- |
| FontColor | Change the color of the letters. For more info on changing colors, refer to Sprite Color Adjustment. |
| FontSize | Change the name tag's name and size; the default is 1. The bigger the value, the bigger the tag. |
|
| Name | Change the name of the name tag. The default value is Null, and you can name it whatever you'd like. |
| NameTagRUID | You can use various Name Tag Sprites.<br>![name](https://mod-file.dn.nexoncdn.co.kr/bbs/1682065127446a1289f492f38401abbff1b9f791dbd13.png{"width":"500px"} "name") |
| OffsetY | Choose the location of the name tag; the starting point is 0, and the axis is set to Y by default. <br>If the value is 0, the name tag will be put on the starting point. A negative number will move the tag downwards, and a positive number will move it upwards. |

# Utilizing Name Tags

The example shows you how to put a name tag on an object to use it as an NPC.
If you'd like to use a monster or NPC from the same Sprite, you can use a name tag to distinguish them.
For example, you can place multiple cat objects and put a cat NPC with a name tag among them as the boss.

You can practice by entering the following value:

* FontColor: f5e38b
* FontSize: 1
* Name: Lost Cat
* NameTagRUID: ac88638649c3479bbb754e4b34bb3eac
* OffsetY: 0

![nametag cat](https://mod-file.dn.nexoncdn.co.kr/bbs/1656464315991005b3cae5aef4145a89c2fb8338abe15.jpg)