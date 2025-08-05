# Course Introduction

We'll look into three concepts of **Tween Component** to create action in entities.
You can use components to adjust the direction and range of the entity's movement.
##### Reference Guide
[Changing Location, Size, Rotation of Entities]

# Introducing Tween Component

You can use **Tween Component** to create movement in entities and adjust the movement in detail.
There are three different components depending on the type of movement. You can also combine them to your convenience.

Types of **Tween Component** are as follows.

| Tweener | Type of Movement |
| ------- | ----- |
| TweenFloatingComponent | Floating movement around original point |
| TweenLineComponent | Linear movement from original position to destination |
| TweenCircularComponent | Circular movement around original point |

> **<span style="color: #7cafc2">Tip</span>**
> <span style="color: #7cafc2">Using multiple components might lead to malfunction or a bug where only one component operates.</span>
> <span style="color: #7cafc2">We recommend one Tween component for an entity.</span>

# TweenFloatingComponent 

**TweenFloatingComponent** is used to make the entity float around a single location.
It moves up and down relative to the origin of the entity. By adjusting the amplitude and cycle time, you can control its distance and range.
![tween01](https://mod-file.dn.nexoncdn.co.kr/bbs/1656418837727e9f305cc0d504a878ddf7c19063705e5.png "tween01")
Let's take a look at the properties of **TweenFloatingComponent**.

| Name | Description |
| --- | --- |
| TweenType | Default value is Linear. Refer to the document below for more information of floating movement type. |
| OneCycleTime | Time spent in round-tripping amplitude value. The bigger the value, the slower the movement. |
| Amplitude | Sets the distance the entity travels up/down from the original point. The bigger the value, the further it will move. |
| AutoStart | If enabled, the movement begins automatically upon the game's start. |
| AutoDestroy | If enabled, the component will be automatically deleted after the movement is finished. |
| SyncType | Choose whether the component will be synchronized with server or client. <ul><li>Default: Operates in server, appears same in all Clients.</li></ul><ul><li>ClientOnly: Appears and operates only on that particular Client.</li></ul> |

#### Application

1. Add **TweenFloatingComponent** to the Entity. Enter each property value of **TweenFloatingComponent** in the Property Editor window as follows.<br>![tween02](https://mod-file.dn.nexoncdn.co.kr/bbs/16462833345885523a1977e4446a99b1974de5d14eb58.png "tween02")
2. Tap the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test. Make sure the entity floats up and down.<br>![tween03](https://mod-file.dn.nexoncdn.co.kr/bbs/16351278266829a1fa2002d524f72bbeedbf7d481793b.gif "tween03") 

> **<span style="color: #7cafc2">Tip</span>**
> <span style="color: #7cafc2">Amplitude Value 1: It's hardly visible to the human eye, but considering each grid is 0.6 high, you can estimate it as 2 grids.</span>

# TweenLineComponent
It is used to make linear movement from the original point to the destination.
You can set if the trip goes one way, as well as accelerating/decelerating, bouncing upon arrival, etc.
![tween12](https://mod-file.dn.nexoncdn.co.kr/bbs/1656418848585c69dc780ccb44ddf91083ec563dfc053.png "tween12")
Let's take a look at the properties of **TweenLineComponent**.

| Property Name | Description |
| ------ | --- |
| TweenType | Default value is Linear. You can choose movement in another type.<br>Refer to the document below for more information of linear movement type. |
| Duration | Indicates time taken to reach the destination. |
| StopType | Chooses stop type.<ul><li>Oneway: Entity goes one way and terminates movement upon reaching destination.</li><li>OneRoundTrip: Entity goes for a round-trip, considering destination as the halfway point. Stops movement after 1 round-trip.</li><li>RepeatRoundTrip: Entity repeats round-trip, considering destination as the halfway point.</li></ul> |
| UseReturnTweenType | If enabled, entity follows separate method after turning halfway point. |
| ReturnTweenType | Designate the post-halfway point trip type if you have chosen OneRoundTrip/RepeatRoundTrip as the stop method.<br>Refer to the document below for more information on return trip type. |
| ReturnDuration | Indicates time taken to move from the destination to the starting point. |
| DestinationCoordinateType | It's how the entity recognizes its destination. <ul><li>Relative: Recognizes destination in relative coordinates. </li></ul><ul><li>Absolute: The values entered for the destination are interpreted as absolute coordinates.</li></ul> |
| Positions | You can add several destinations in the moving direction. The number of destinations is not displayed in ![TabScene](https://mod-file.dn.nexoncdn.co.kr/bbs/163452458863504f49c7a23aa4a41af56b5b4611a6daf.png "TabScene") Scene.<br>Size: Refers to the number of destinations. The number starts from 0, and the value you entered in Size will be the number of destinations.<br>Number: Enter the destination's X and Y coordinates. Coordinates of [1] will be the entity's first destination. Then it moves to the next number. |
| Interpolation | Determines the point's interpolation method. Operation is valid when there are more than 2 points.<ul><li>None: Default value, does not interpolate.</li><li>SplineTypeA: Interpolate in SplineTypeA. Entity might not pass points in the middle.</li></ul> \*Interpolation: If the entity follows a straight line between several points, the movement does not look smooth. Interpolation compensates for this by making the movement more soft by smoothing the path into a curve. |
| AutoStart | If enabled, the movement begins automatically upon the game's start. |
| AutoDestroy | If enabled, the component will be automatically deleted after the movement is finished. |
| SyncType | Chooses whetherthe component will be synchronized with server or client.<ul><li>Default: Operates in server, appears same in all Clients.</li><li>ClientOnly: Appears and operates only on that particular Client.</li></ul> |

#### Application
1. Add **TweenLineComponent** to the Entity and set as follows.<br>![tween04](https://mod-file.dn.nexoncdn.co.kr/bbs/1646283379788c89d5799ffb74e478c8d62d6db6be1e2.png "tween04")
2. Tap the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and test.<br>![tween05](https://mod-file.dn.nexoncdn.co.kr/bbs/1635128047869e1e7391ce85e4e7989395243e2590395.gif "tween05")
# TweenCircularComponent
It is used to make circular movement around the original point.
You can adjust the number, speed, and range of rotation.

Let's take a look at the properties of **TweenCircularComponent**.

| Property Name | Description |
| --- | --- |
| Radius | Moves away from original point by input value before circular movement.<br>The bigger the value, the further from center. |
| Speed | The bigger the number, the faster the entity's movement. |
| LookAtCenter | If enabled, the topside of the entity's square edge will face the centerpoint. |
| Degree | Rotates by input angle and then stops. When this is set to 0, the object will move continuously without stopping. || Radius | Moves away from original point by input value before circular movement.<br>The bigger the value, the further from center. |
| IsClockwise | Defines the direction of rotation. Rotate clockwise if set to true.| Will be deleted. |
| AutoStart | If enabled, the movement begins automatically upon the game's start. |
| AutoDestroy | If enabled, the component will be automatically deleted after the movement is finished. |
| SyncType | Chooses whether the component will be synchronized with server or client.<ul><li>Default: Operates in server, appears same in all Clients.</li><li>ClientOnly: Appears and operates only on that particular Client.</li></ul> |

#### Application
1. Add **TweenCircularComponent** to the Entity and set as follows. <br> ![tween06](https://mod-file.dn.nexoncdn.co.kr/bbs/164628347830889af06e47ea34c718e77f29897772157.png "tween06")
2. Tap the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and run a test.<br>If you only enter a **Radius** value, the entity will only rotate along the radius.<br>If you apply **LookAtCenter**, the top edge of the entity's rectangle border is attached to the rotation radius and rotates toward the base point of the rotation.<br>

| Basic Rotation | Apply LookAtCenter |
| --- | --- |
| ![tween07](https://mod-file.dn.nexoncdn.co.kr/bbs/1635132897859b8a88490ede14cf3ab4961b279af2fbc.png "tween07") | ![tween08](https://mod-file.dn.nexoncdn.co.kr/bbs/16370579280376c43e57eec704eaea6bac377ce481b5c.png "tween08") |
| ![tween09](https://mod-file.dn.nexoncdn.co.kr/bbs/16351329567296ac3abbf46b64488b31bd416bd566412.gif "tween09") | ![tween10](https://mod-file.dn.nexoncdn.co.kr/bbs/16351329728073d33972360c648cd9e11a4c70eb1c05f.gif "tween10") |

# Reference - Ease Type
Imagine you wound up a clockwork toy and put it on the ground. It will slowly start, move at a constant speed, get slower, and then come to a complete stop. Most of the objects we encounter in our daily life move at different speeds. So a robot that keeps moving at the same speed looks rather awkward. In MapleStory Worlds, you can set different movement speeds and recoil of entities so that they move naturally. Maker offers 10 different types, and each of them has 3 variations. You can refer to the table below.

Types: **Quad, Expo, Cubic, Quart, Quint, Circ, Sine, Elastic, Bounce, Back**

Variation
* **Ease Out**: It starts moving fast, slows down at the end, and stops at its destination.
* **Ease In**: It starts to move slowly, accelerates, and stops at its destination.
* **Ease In-Out**: Starts moving slowly, accelerates in the middle, then decelerates and stops at its destination.

![easetype](https://mod-file.dn.nexoncdn.co.kr/bbs/1635132999893719a42ed301645e8afcf19bb1a302ea9.png{"width":"740px"} "easetype")
