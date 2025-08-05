# Course Introduction
Let's see how to set the trip type of specific entities in a map that uses physics. You can use various JointComponents to create entities that perform various movements and use them to create Worlds.

# Joint
A joint refers to a specific link section that connects the particular entities on a map that applies the laws of physics, so that they can influence each other.
Like the image below, the link section that allows Entity A and Entity B to influence each other is Joint. Once you set a Joint attachment location (LocalAnchor A) on Entity A, the standard entity, and set another Joint attachment location (LocalAnchor B) on the entity to connect to (TargetEntityRef), Entity B, Entity A and Entity B become connected.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1663837353395619ae9a35f0a47a481baf3f797bb5446.png "1")

The Anchor's location sets the distance relative to the entity's ColliderOffset. Entities linked to each other may or may not collide, depending on the creator's intent. 

* <span style="color: #dc9656">**Entity A**</span>: You can use another entity as TargetEntityRef by adding Joint as an entity with JointComponent.
    * <span style="color: #dc9656">**ColliderOffset**</span>: An Entity's PhysicsColliderComponent's ColliderOffset. It is used as the origin of the LocalAnchor location.
    * <span style="color: #dc9656">**LocalAnchorA**</span>: Set Anchor for Entity A with JointComponent. Take ColliderOffset of Entity A as the origin.

*  <span style="color: #dc9656">**Entity B**</span>: Specify TargetEntityRef in JointComponent of Entity A as the entity to connect the Joint to.
    * <span style="color: #dc9656">**TargetEntityRef**</span>: The Entity to connect the Joint to.
    * <span style="color: #dc9656">**LocalAnchorB**</span>: Set the Anchor of Entity B, which is the Entity to connect the Joint to. Take ColliderOffset of Entity B as the origin.

![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16638373662946c98c73c26ec4d78933f46034b0179c2.png "2")
#### Anchor
This is the reference point at which the joint operates. The entity's ColliderOffset location is used as the Anchor's origin. For example, as shown in the figure below, even if the values of LocalAnchor A and B are the same (3,0), the base entities' offset locations are different. LocalAnchor is located elsewhere, and the origin point of Entity A and B is the foothold.

The base Offset of LocalAnchor A is Entity A, and the base Offset of LocalAnchor B is Entity B. 

![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1663839068005e7743548cf8e44349be58cdf0cbf9300.png "3")

![anchor1](https://mod-file.dn.nexoncdn.co.kr/bbs/16638363593571f7fb2dde7e7440cb0c1b9447f4123e4.gif "anchor1")

#### Motor
Using Motor forces the Entity to move in a specific direction. It is used in RevoluteJointComponent, WheelJointComponent, and PrismaticJointComponent. If you set the MotorEnable property to true to use the motor, the entity will rotate according to the set target speed. Target speed means the value of MotorSpeed. For example, if MotorSpeed is 100, and the MotorSpeed value reduces to 10 once the speed reaches 100, the speed will decrease.

![motor](https://mod-file.dn.nexoncdn.co.kr/bbs/16638363279474aea43c2e0f849e78e12f9371b760269.gif "motor")
# Type of JointComponent
It includes six types: DistanceJointComponent, RevoluteJointComponent, PrismaticJointComponent, PulleyJointComponent, WeldJointComponent, and WheelJointComponent. Since each type has different characteristics, you should use the JointComponent that best fits what you want to create. 

#### DistanceJointComponent
DistanceJointComponent sets the distance-related limits between two entities. The distance (Length) between LocalAnchorA and B remains constant.

| Major Properties | Description |
| --- | --- |
| <span style="color: #dc9656">**Length**</span> | Sets the distance that will be maintained between two Entities connected by a Joint. |

![distance](https://mod-file.dn.nexoncdn.co.kr/bbs/1660634671012e4dbb294cfa94678b6af5dfc7804bc7f.gif "distance")

#### RevoluteJointComponent
RevoluteJoint sets the rotation-related limits between two entities. After activating UseLimits, you can control the rotation angle using LowerAngle and UpperAngle. 
When an entity rotates, the point where LocalAnchorA and B meet becomes the common Anchor. Also if you use Motor, you can receive the rotational power based on the Anchor. 

| Major Properties | Description |
| --- | --- |
|<span style="color: #dc9656">**UseLimits**</span> |  Determines whether to set limits on relative angles. |
|<span style="color: #dc9656">**LowerAngle**</span> | Sets the minimum relative angle. It's based on the Entity's Local Vector2 (1, 0). |
|<span style="color: #dc9656">**UpperAngle**</span>|  Sets the maximum relative angle. It's based on the Entity's Local Vector2 (1, 0). |
|<span style="color: #dc9656">**MotorSpeed**</span>|  Sets the target speed of Motor. If it is a positive number, it rotates in a CCW (counterclockwise) direction. |

![revolute](https://mod-file.dn.nexoncdn.co.kr/bbs/1660634686437edf5ce687bf04396952bf91d804c774c.gif "revolute")

#### PrismaticJointComponent
PrismaticJointComponent sets the entity to move linearly along the axis (LocalAxis).
You can set the LowerTranslation and UpperTranslation values to limit the maximum and minimum distance the entity moves. The location of LocalAxis is relative to LocalAnchor.

| Major Properties | Description |
| --- | --- |
| <span style="color: #dc9656">**UseLimits**</span> | Sets whether or not to limit the relative movement distance. |
|  <span style="color: #dc9656">**LowerTranslation**</span> | Sets the minimum relative travel distance.  |
| <span style="color: #dc9656">**UpperTranslation**</span> |  Sets the maximum relative travel distance. |
|<span style="color: #dc9656">**MotorEnable**</span>  | Sets whether to use Motor. If True, the attached Entity will receive a force in the direction of LocalAxis. |
|  <span style="color: #dc9656">**MotorSpeed**</span>| Sets the target speed of Motor. MotorEnable must be True to operate. |

![primatic](https://mod-file.dn.nexoncdn.co.kr/bbs/16606347060771a318324cf13474ba3e52d4b6f847f92.gif "primatic")
#### PulleyJointComponent
PulleyJointComponent maintains the sum of the total length of the distances between LocalAnchor and GroundAnchor. If one entity's line length (Length) gets shortened, the other entities' line length will get longer, maintaining the total length of the line. The total length is determined by the formula: Initial distance between Entity A and GroundAnchorA + Ratio * Initial distance between Entity B and GroundAnchorB.
<span style="color: #dc9656">**The lines of each entity**</span> are the distance between LocalAnchor and GroundAnchor, and the origin of GroundAnchor is the location of LocalAnchor. <span style="color: #dc9656">**Ratio**</span> determines the percentage by which the distance between the Entity and GroundAnchor decreases. 
If the Ratio is 3 and the line of Entity A is reduced by 3, even if the line with Entity B is increased by 1, the line of Entity A has 1/3 of the constraint force compared to the line of Entity B. 

| Major Properties | Description |
| --- | --- |
| <span style="color: #dc9656">**Ratio**</span> | Sets the constraint force ratio of both imaginary lines that form PulleyJoint. |

![pulley](https://mod-file.dn.nexoncdn.co.kr/bbs/1660634724636c6a1887dec234a97ad13994d906a7ebc.gif "pulley")
#### WeldJointComponent
Sets it so that connected entities move together. They move while maintaining a certain distance apart, as if the two ends were attached to a skewer. Connected entities are constrained to relative movement and rotation.

![weld](https://mod-file.dn.nexoncdn.co.kr/bbs/1660634741001b728a6793af4499e8f64070bcf551639.gif "weld")
#### WheelJointComponent
WheelJoint sets the constraint to make the connected entity behave like a wheel. TargetEntityRef using Motor uses the MotorSpeed value as the target speed. 
| Major Properties | Description |
| --- | --- |
| <span style="color: #dc9656">**MaxMotorTorque**</span> | Sets the maximum strength applicable to the motor of the Joint in order to reach the target motor speed. |
| <span style="color: #dc9656">**MotorEnable**</span> | Sets whether to use Motor. If True, the attached Entity will receive a force in the direction of LocalAxis.  |
| <span style="color: #dc9656">**MotorSpeed**</span> | Set the target motor speed of the Joint. MotorEnable must be True to operate.|
![wheel](https://mod-file.dn.nexoncdn.co.kr/bbs/16606347549715160d2650c9c4197a2b2b2ba189ecc6a.gif "wheel")
# Adding Joint
The way to add a JointComponent is the same. 

1. Generate two entities with PhysicsRigidbodyComponent and PhysicsColliderComponent added and place them.
2. Add the JointComponent you want to use. Enter the number of joints to generate in <span style="color: #dc9656">**Size**</span>.
![size](https://mod-file.dn.nexoncdn.co.kr/bbs/1690259361892e5e2bdd02e44452fb93da6ac7a30c4d6.png "size")
3. After selecting the desired entity by using <span style="color: #dc9656">**TargetEntityRef**</span>, set the location of LocalAnchor A and B.
![joint](https://mod-file.dn.nexoncdn.co.kr/bbs/1690259508600e65c1ca226c84bcfbdfa719299acb7a5.png "joint")

# Example
Let's make a simple game that bounces a ball using RevoluteJointComponent. 
![example](https://mod-file.dn.nexoncdn.co.kr/bbs/1660804708258db7c651cc14d4beb87fc23eee95cc3c8.gif "example")
#### Ready
1. Change the tile mode to **RectTile**.
    ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16902702934039459adc7e8234eb4950ba11f2d11493f.png "5")
2. Add **Hierarchy - World - maps - map01 - PhysicsSimulatorComponent**.
3. Activate **Hierarchy - World - maps - map01 - RectTileMap - RectTileMapComponent - PhysicsInteractable**. 
4. Place a **Static** type physics entity so that the ball can hit it. Place it in a trapezoid shape with a hole in the bottom.
    * SpriteRUID: <span style="color: #dc9656">**1ce2eff829ae4346bc89a3f0a357ac0e**</span>
![static](https://mod-file.dn.nexoncdn.co.kr/bbs/169026003093236f6d51367a14dd4a906324c452a0d7d.png "static")

#### Making a Ball
1. Create a new <span style="color: #dc9656">**Ball**</span> model and add **PhysicsRigidbodyComponent, PhysicsColliderComponent**.
    * Ball SpriteRUID: <span style="color: #dc9656">**164e94bea20441adac2af9c6f800bdf3**</span>
2. Change **PhysicsRigidbodyComponent**'s **BodyType** to <span style="color: #dc9656">**Dynamic**</span>.
3. Change **PhysicsColliderComponent**'s **ColliderType** to <span style="color: #dc9656">**Circle**</span> and adjust the <span style="color: #dc9656">**CircleRadius**</span> to fit the sprite size.
4. Place it in the air, allowing the ball to fall under gravity.

#### Creating Obstacles
1. Create a new <span style="color: #dc9656">**Obstacle**</span> model and add **PhysicsRigidbodyComponent, PhysicsColliderComponent**.
    * SpriteRUID: <span style="color: #dc9656">**3ada0e4cc7f44cb988932f1b183ec778**</span>
2. Change **PhysicsRigidbodyComponent**'s **BodyType** to <span style="color: #dc9656">**Static**</span>.
3. Change **PhysicsColliderComponent**'s **ColliderType** to <span style="color: #dc9656">**Circle**</span> and adjust the <span style="color: #dc9656">**CircleRadius**</span> to fit the sprite size.

#### Creating a Lever
1. Generate a new object Entity of <span style="color: #dc9656">**LeftEntity, LeftBar**</span> and add **PhysicsRigidbodyComponent, PhysicsColliderComponent**.
    * LeftEntity SpriteRUID: <span style="color: #dc9656">**e75cac80f719471fa482195531ed57cc**</span>
    * LeftBar SpriteRUID: <span style="color: #dc9656">**0308ac89b10843fcab6384aa43f6ae22**</span>
2. Add **RevoluteJointComponent** to the circular Entity, and enter <span style="color: #dc9656">**1**</span> for **Joint**. Specify LeftBar to TargetEntityRef, and assign the values of **LocalAnchorB, LowerAngle, UpperAngle, MotorSpeed, UseLimits** as follows.
![left](https://mod-file.dn.nexoncdn.co.kr/bbs/16902694608421c03f7e8bfc44e6eba274aefaef0bbf7.png "left")
4. Copy the two entities above to create the opposite side.
![right](https://mod-file.dn.nexoncdn.co.kr/bbs/1690269890755b731c1c566234f999b810610411cf45f.png "right")
5. Create the new <span style="color: #dc9656">**Swing**</span> script component and add it to **DefaultPlayer**.
6. Enter as follows to make it so that when N or M is pressed, the motor of RevoluteJoint is used, and when released, it is not used.

    ```lua
    Property:
    [Sync]
    Entity Left = /maps/map01/LeftEntity
    [Sync]
    Entity Right = /maps/map01/RightEntity
    
    Method:
    [server]
    void MoveLeft(boolean enable)
    {
        self.Left.RevoluteJointComponent:SetMotorEnable(1, enable)
    }
    
    [server]
    void MoveRight(boolean enable)
    {
        self.Right.RevoluteJointComponent:SetMotorEnable(1, enable)
    }
    
    Event Handler:
    [service: InputService]
    HandleKeyDownEvent(KeyDownEvent event)
    {
        -- Parameters
        local key = event.key
        --------------------------------------------------------
        if key == KeyboardKey.N then
        	self:MoveLeft(true)
        elseif key == KeyboardKey.M then
        	self:MoveRight(true)
        end
    }
    
    [service: InputService]
    HandleKeyReleaseEvent(KeyReleaseEvent event)
    {
        -- Parameters
        local key = event.key
        --------------------------------------------------------
        if key == KeyboardKey.N then
        	self:MoveLeft(false)
        elseif key == KeyboardKey.M then
        	self:MoveRight(false)
        end
    }
    ```

7. Press Start to see if the bar moves when you press N and M respectively.

##### Reference Guide
* [Using Physics](/docs?postId=757{"target":"_self"})
* [Create Collision Group](/docs?postId=754{"target":"_self"})
* [Applying Physics to Entities](/docs?postId=761{"target":"_self"})