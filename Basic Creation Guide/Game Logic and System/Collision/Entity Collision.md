# Course Introduction
Let's learn about collision events and how to process collisions.
##### Reference Guide
[Event System](https://mod-developers.nexon.com/docs?postId=73{"target":"_self"})
[Entity Event System](https://mod-developers.nexon.com/docs?postId=176{"target":"_self"})
# Collision
<span style="color: #dc9656">**Collision**</span> refers to when the collision areas of the entity containing the colliding body intersect each other.
If [TriggerComponent](/apiReference/Components/TriggerComponent{"target":"_self"}) or [HitComponent](/apiReference/Components/HitComponent{"target":"_self"}) are added to an entity, colliding bodies are automatically created.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16564849378616ff1f365457f40cca4aaccfdcca2ea41.png "1")

# Editing Colliding Bodies
Attributes such as size, position, and shape of the colliding body can be edited as properties of the components that can control the colliding body.
Typically, they are [TriggerComponent](/apiReference/Components/TriggerComponent{"target":"_self"}) and/or [HitComponent](/apiReference/Components/HitComponent{"target":"_self"}).
**TriggerComponent** uses a colliding body when setting a **collision range**, and **HitComponent** uses a colliding body when setting a **hit range**.

#### Enable Editing Mode
In the Property Editor, click the **[Edit]** button to activate editing. When activated, the collision area turns green.

| Collider Edit Deactivated | Collider Edit Activated |
| --- | --- |
| ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1647419996497c2317d38c37c4266a041e44a7962be00.png "2") | ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16474200145049bb170d059a94a9ebe454f0c79ede8e6.png "3") |
| ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1650933070591cc3f53205067484db1c6e758a07eda52.png "4")| ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1650933106203672a48645eb14948b2f02f2b115be9ff.png "5") |

#### Editing Size
When editing is enabled, you can pull the point and edit it to the desired size.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1647420159680d2bd972d40cd407cbacb51c32f411bdd.png "7") 

#### Edit Offset
You can set **ColliderOffset** by dragging the point of the colliding body.
 ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1647420135543c8ede53b0cf54ae9bb8ab318c12134e9.png "6") 

# ColliderType
There are three shapes of colliding bodies: <span style="color: #dc9656">**Box, Circle, Polygon**</span>.

#### Box
Sets the colliding body to a box. The X is the horizontal size, and the Y is the vertical size.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/16751561869387728a07df928485bafa17874b7bedb30.png "9")

#### Circle
Sets the colliding body to a circle. CircleRadius is the radius value. You can set the size of the circle with the value of CircleRadius.
![9-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1675156219035249c8c3667a646f8bfbc7b128c1fe38d.png "9-1")

#### Polygon
Sets the colliding body to a polygon. Add as many PolygonPoints as needed to create any colliding body shape you want.
![9-2](https://mod-file.dn.nexoncdn.co.kr/bbs/16792881867493ee65f9122fe47dfbb775527e2ea19ff.png "9-2")

# TransformComponent and Collider
The colliding body is affected by the **Scale** and **Rotation** values of **TransformComponent**.
* When the entity scales by a value of **Scale**, the colliding body size also changes. 
* If the entity rotates by a value of **Rotation**, the colliding body will also rotate.

Below is an example that turns yellow when an entity detects a collision. 
The **Transform**'s **Scale** and **Rotation** values change and turn yellow whenever it collides with another entity.

**Example of Transform.Scale change**
$$youtube 
4Ri0CpWg6RY
$$

**Example of Transform.Rotation change**
$$youtube
xox3H77CDNg
$$

All entities are organized in a hierarchical structure. Therefore, the **TransformComponent of the extended entity** is always affected by the **TransformComponent of the parent entity**. Similarly, colliding bodies are also affected by the **TransformComponent of parent entities**.
Let's create four entities by extending a parent entity and see how they are affected. If you rotate the parent entity as shown below, the four extended entities will also rotate, and you will see that they collide with other entities.
$$youtube
j2ijQUSA3Qg
$$

# Adding Action After Collision
After the collision event, the action uses <span style="color: #dc9656">**Entity Event System**</span> or <span style="color: #dc9656">**extended TriggerComponent**</span>.
Suppose there are two entities in the world, A and B, which contain **TriggerComponent**. When A and B collide, a collision event occurs. However, there will be no significant changes after that. This is because there is no further action to take after the collision event occurs.
**TriggerComponent** allows you to add an action that will happen when a collision event occurs. For example, if A and B collide, A and B's health will be reduced, or A or B may be eliminated.

![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1656485418269b988142433d34bb69e3de8e4da44a1fe.png "10")

# Use Entity Event System
#### Structure
Add handlers (or listeners) to the entity to perform actions when a collision event occurs. After that, when a collision event occurs in the **TriggerComponent** of the entity, it is a structure that calls the handler's function. 
For this structure to work, you need to add **TriggerComponent** and **Script Component** (reactors) to the entity. For example, if you want to reduce A and B's HP when entities A and B collide, you need to add a **TriggerComponent** and script component to A and B, respectively.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/16577115670559954f9e64a574fcc965846f902236144.png "11")

#### Adding Handlers
A **handler** that will perform the action can be added through **Entity Event Handler** of the script component (reactor).
Open the script component and click the **[+]** button of **Entity Event Handler** to add the **Handler**.
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/1656485699517a16e81436edd4343b354db1f31290178.png "12")

If you enter <span style="color: #dc9656">**Trigger**</span> in the search box, you'll see handlers for collision events like the following.
Select the handler that will fit your action's purpose. Use the necessary event appropriately by referring to the occurrence time of each event.
* **TriggerEnterEvent**: Occurs once upon the first collision between entities.
* **TriggerStayEvent**: If entities are colliding, occurs every frame.
* **TriggerLeaveEvent**: Occurs once when the collision between entities is ended.

![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1656485878094a57bbd99ec4f4a2783cb0f99d1d5a674.png "13")

#### Setting Collision Event Sender
Selecting a handler will add a function. On the top of the handler, you can set the sender.
![14](https://mod-file.dn.nexoncdn.co.kr/bbs/16371498027637a7ba3588b494f30a2dfca16bedba70e.png "14")

Set the sender to **self** to receive collision events for entities containing that script component (reactor). To receive collision events from other entities, set the sender to **entity** and specify the entity you want.
![15](https://mod-file.dn.nexoncdn.co.kr/bbs/163714981203633674070dae14e1180ec0cc07e6c07ec.png "15")

#### Handler Configuration
Handler is called upon event occurrence, and it receives collision event as a parameter.
The event entered as a parameter contains the **entity where the event occurred** and the **collision partner entity's information**.
![16](https://mod-file.dn.nexoncdn.co.kr/bbs/165648590724409f8204419c64f98a14a79c5bc1bf198.png "16")

# Use the extended TriggerComponent
#### Structure
**TriggerComponent** has an internal function that is called when a collision event occurs to an entity. You can utilize this function to add an action after an entity collision event.
However, the functions of **TriggerComponent** cannot be edited directly. As such, you need to edit that function in the component that extends **TriggerComponent** to add collision processing. 
As extended components include the features from the original component, just add the **extended TriggerComponent** to the entity.
![17](https://mod-file.dn.nexoncdn.co.kr/bbs/1657699624889b386c531319b425d99274f1b11566ce2.png "17")

#### Extend TriggerComponent
In the **Workspace**, search and select **TriggerComponent**. Selecting **Extend** from the context menu will create an extended component of **TriggerComponent** in the **MyDesk** folder.
![18](https://mod-file.dn.nexoncdn.co.kr/bbs/1687754755702bcecc1f59a204f3ca931773a1849464d.png "18")

#### Function Override
Unlike other script components, the extended **TriggerComponent** can override some functions. 
These functions are called when a collision event occurs. If you add this function, you can override it by editing the definition of that function.
![19](https://mod-file.dn.nexoncdn.co.kr/bbs/1637149832831df2bfe1ef6384f3184ef8f84be296356.png "19")


There are three functions that can be overridden. The following is the call time for each function.
* **OnEnterTriggerBody**: Called once upon first collision between entities.
* **OnStayTriggerBody**: Called for every frame while the two entities overlap following a collision.
* **OnLeaveTriggerBody**: Called once when the collision between entities stops.

#### Parameter
The collision event comes in as a parameter. The event contains information about which partner entity collided with the entity.
![20](https://mod-file.dn.nexoncdn.co.kr/bbs/16371498432816cef0f0507714aac9e2f70d25fb3a5ea.png "20")