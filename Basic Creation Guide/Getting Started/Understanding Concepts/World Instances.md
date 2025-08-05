# Course Introduction
Let's learn about the concept, creation, and destruction of world instances. To fully understand world instances, it is recommended that you have a firm grasp of rooms first. Related reading: [Creating Instance Maps].
# World Instances
A world instance is a real entity based on world information created by the Maker. World instances are executed independently, and static rooms and instance rooms are configured according to the creator's production method.

><span style="color: #7cafc2">**Tip**
> All world instances within attached to a given world will use one DataStorage.</span>

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1679291060113a0eff84f08214ed49d8a58ab3ba25664.png "1")
#### Communication Between World Instances
Since each World instance is executed individually, if you need to communicate between World instances or forward an event, you must use [RoomService](/apiReference/Services/RoomService{"target":"_self"}) and [WorldInstanceService](/apiReference/Services/WorldInstanceService{"target":"_self"}). For details on how to use it, refer to [World Instance Communication].

# Creating and Destroying World Instances
During a world's launch phase, creators set the world's **max number of players**. This number is one of the criteria for creating new world instances. For example, if 100 users connect to a world where the maximum number of players is set to 10, then 10 world instances will be created. 
The criterion for destroying a world instance is when a **user leaves**. If all users quit playing and there are none remaining, the world instance is automatically destroyed. 
After a certain amount of time has elapsed since the world instance is created, it **prepares for expiry**. This means new users will not be able to connect to this instance. The world instance will continue to operate with the currently connected users until they all quit playing.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1679291087805feeafc80d6894e21bb6fa7acd50caf64.png "3")


#### User Assignment of World Instances
Which world instance a new user is assigned to is predicated on the **number of people in one world instance**. When there are no world instances active, a new world instance is created and incoming users will be sent to that world instance until it is full. Once the first world instance is full, a new instance will be created and the cycle continues. 
If you are running multiple World instances and users leave the World, vacancies will occur. At this point, a new World instance is no longer created when a new user connects. Instead, new users are sent to the World instance that has the most users among the available World instances. World instances that have started preparing for retirement are not included, even if the number of users is the highest.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/167929120294711f23765789448a4a0b99bacfb91db09.png "2")