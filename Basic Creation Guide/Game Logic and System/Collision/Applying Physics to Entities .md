# Course Introduction

Let's apply the laws of physics to entities and make them collide with each other. For how to create physics in your world, see [Using Physics].

# PhysicsRigidbodyComponent

If you add a PhysicsRigidbodyComponent to an entity, it will be controlled by the physics engine. You can also set the values that influence physics calculation such as the entity's coefficient of resistance, coefficient of friction, mass value, etc.
As long as an entity has PhysicsRigidbodyComponent and PhysicsColliderComponent, it can collide with other entities that obey the laws of physics.
The main properties of PhysicsRigidbodyComponent are as follows.

| Property | Explanation |
| ---- | --- |
| BodyType | <ul><li>Static: Treated as an entity with an infinitely large mass and can collide with Dynamic types. </li></ul><ul><li>Dynamic: Entities behave according to the laws of physics. It can collide with other Entities that can collide with physics.</li><li>Kinematic: Treated as an entity with an infinitely large mass, but unlike the Static type, it can move by specifying a speed.</li></ul> |
| Mass | Sets the mass value. Calculates the density value using the values of the area and mass of the Collider. |
| Friction | Determines the coefficient of friction. The smaller the value is, the better it slides. |
| Restitution | Sets the Entity's coefficient of restitution. 0 is close to a perfectly inelastic collision, and 1 is close to a perfectly elastic collision. |
| AngularDamping | Sets the coefficient of resistance to changes in angular velocity (rotational speed) when an entity moves in a circle. The larger the value is, the more force is required to change the angular velocity. |
| LinearDamping | Sets the coefficient of resistance to linear velocity changes when performing linear motion. The larger the value, the more force is required for a linear velocity change. |
| CollisionDetectionMode | Sets the detection method for physics collisions.<ul><li>Discrete: Checks for collisions intermittently. Depending on the speed and size of the object, collisions may not be detected. The higher the speed or the smaller the size, the more likely it will pass without detecting a collision.</li><li>Continuous: Continually checks for collisions. Prevents objects from passing through without colliding. This computation requires more resources.</li></ul> |
| FixedRotation | If true, the object does not rotate due to physics interactions. |
| GravityScale | Sets how much the entity is affected by the gravity value of PhysiecsSimulatorComponent. When the Gravity value is (0, -10), if the GravityScale value is 1, the gravity value that the entity received is (0, -10), and if the GravityScale value is 2, the gravity value is (0, -20). |
| SleepingMode | <span style="color: rgb(34, 34, 34); font-family: Open Sans, Helvetica Neue, Helvetica, Arial, Nanum Barun Gothic, Malgun Gothic, sans-serif; font-size: 13px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: 0.25px; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 1px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; display: inline !important; float: none;">Sets the sleep mode.</span><ul><li>StartAwake: Starts out awake, but can convert to sleep.</li><li>StartAsleep: Starts in a sleeping state, but wakes up on impact.</li><li>NeverSleep: Unavailable to convert to sleep mode.</li></ul> |

> **<span style="color: #b8b8b8">Learn more</span>**
> * Coefficient of Restitution: The ratio of the relative velocities of two colliding objects before and after the collision.
> * Inelastic Collision: Kinetic energy is lost upon collision.
> * Perfectly Inelastic Collision: Kinetic energy is lost upon collision, and after collision, the two objects move as one.
> * Perfectly Elastic Collision: A collision in which kinetic energy is conserved upon impact.

# Function Description

Depending on the PhysicsRigidbodyComponent function, you can apply a force to an entity in a specific direction to make it move or perform a circular motion (rotational motion). The ApplyForce series function and the ApplyImpulse series function are affected by the mass value of the physical rigid body corresponding to the Entity. The velocity setting functions in the SetVelocity category can specify a velocity independent of the mass value.

#### Force-Applying Functions

The `ApplyForce()` and `ApplyTorque()` functions can apply a continuous force. This is when you want to continuously apply force in a specific direction by pressing a specific key. The parameter force means force. It is affected by the mass value of the physics rigid body corresponding to that Entity.

#### Impulse-Applying Functions

The `ApplyAngularImpulse()` and `ApplyLinearImpulse()` functions can temporarily apply short shocks. This is related to short-impact actions, such as firing a cannon or swinging a bat. It is affected by the mass value of the physics rigid body corresponding to that entity. The parameter Impulse refers to the amount of impulse and is used to temporarily apply a short shock.

#### Velocity-Setting Functions

The `SetAngularVelocity()` and `SetLinearVelocity()` functions can set the velocity of an entity independent of its mass value.

# PhysicsColliderComponent

You can size the entity's colliding body and set an offset. It is mainly used with <span style="color: #dc9656">**PhysicsRigidbodyComponent**</span>, but entities used without PhysicsRigidbodyComponent are treated as a Static type. Refer to [Entity Collision] for how to set the type, size, and offset of the colliding body (/docs/?postId=175%7B%22target%22:%22_self%22%7D).

| Property | Explanation |
| ---- | --- |
| ColliderType | Depending on the classification, you can set the shape of the colliding body differently. <ul><li>Circle: Creates a circular colliding body. </li><li>Box: Creates a rectangular colliding body.</li><li>Polygon: Creates a polygonal colliding body.</li></ul> |
| Collider | Pressing the Edit button can set the size of the colliding body. |
| CollisionGroup | Sets up a collision group that can determine if there is a collision or not. You can set it up by creating a new collision group. |
| Density | Sets the density value to use when UseDensity of the attached PhysicsRigidbodyComponent is true. |
| UseCustomPhysicalProperties | Friction and Restituion values are applied to physics rigid bodies. If false, the value of the connected PhysicsRigidbodyComponent is used. |
| EnableContactEvent | Sets whether PhysicsContactBeginEvent and PhysicsContactEndEvent occur. If false, neither Event occurs. |
| IsSensor | If true, no physical interaction will occur, but a collision event will occur. |
| ClientOnly | If true, physics operation occurs in the Client, and functions and property changes are possible in the Client space. Physics calculation results are not synchronized with other Clients. If false, functions and property changes are possible in the Server space. Physics calculation results are synchronized with other Clients. |

#### Differences in Physics Calculations by Execution Space

Most of <span style="color: #dc9656">**PhysicsRigidbodyComponent**</span>'s Property and Function is divided into the available and accessible execution space depending on <span style="color: #dc9656">**PhysicsColliderComponent**</span>'s ClientOnly property value. They are computed in their respective physics Worlds according to ClientOnly, so they do not collide or affect each other.

* If **<span style="color: #dc9656">ClientOnly is True</span>**: The physics rigid body of the Entity is generated only in the Client, and the physics operation is calculated in each Client. Results are not synchronized with other Clients. It is possible to use related functions and change most of the properties only in the Client.
* If **<span style="color: #dc9656">ClientOnly is False</span>**: The physics rigid body of the Entity is generated in the Server and the Client, and the location of the physics rigid body is synchronized between the Server and all Clients according to the result of the physics calculation. It is possible to use related functions and change most properties only in the Server.

# Setting Collision Group

By setting a collision group, you can set whether or not entities to which the laws of physics apply collide with each other according to your needs. Specify the CollisionGroup of PhysicsColliderComponent and select whether or not the groups collide. When you start the world after creating gravity on the map and placing a dynamic type physics entity with various elasticity on top of the map, the entities will descend and bounce off each other. For details on how to set up a collision group, see [Creating Collision Groups].

# Example

You will need to apply physics to the world using PhysicsInteractable from PhysicsSimulatorComponent and TileMapComponent to see the example.

#### Throwing Objects on the Floor into the Air

Let's throw an entity placed on the ground into the air when the J key is pressed. Entities that are raised will fall to the bottom due to gravity.
![eg1](https://mod-file.dn.nexoncdn.co.kr/bbs/16601123764643cfe3adec2534fb68a22b98573a3713c.gif)

1. Place the object with PhysicsRigidbodyComponent and PhysicsColliderComponent added on the scene.
2. After generating a new script component, write the following to apply force to move the object to the entered location when J is pressed.

```lua
Method:
void Jump()
{
    self.Entity.PhysicsRigidbodyComponent:ApplyLinearImpulse(Vector2(0,10))
}

Event Handler:
[service: InputService]
HandleKeyDownEvent(KeyDownEvent event)
{
    --------------- Native Event Sender Info ----------------
    -- Sender: InputService
    -- Space: Client
    ---------------------------------------------------------
    
    -- Parameters
    -- local key = event.key
    ---------------------------------------------------------
    
    if key == KeyboardKey.J then
        self:Jump()
    end
}
```

#### Colliding Physics Entities with Collision Groups

1. Place several objects with PhysicsRigidbodyComponent and PhysicsColliderComponent added, and set each Mass and Restitution values differently.
2. Add a new physics group to CollisionGroup and set them to collide with each other as shown below.
    ![collisionmatrix](https://mod-file.dn.nexoncdn.co.kr/bbs/16601112357375799a6ba9d5e4f17b1d7096d2d0e02ba.png)
3. Click Start to see if entities collide with each other.
    ![eg2](https://mod-file.dn.nexoncdn.co.kr/bbs/16601137220417735c37efe64470ab00e710e1c460242.gif)

##### Reference Guide

* [Using Physics]
* [Creating Collision Groups]
* [Using Various Physics Joints]