#  Course Introduction
Let's create a collision group and learn how to set collision between the entities. You can make it collide between certain groups by using a collision group, or ignore the collision entirely.

# Collision Group Features
A collision group is a feature to bundle up the multiple colliding bodies in MapleStory Worlds by certain group units and control the interactions between those groups. There are **Default, TriggerBox, and HitBox** for the default collision groups, and you can create **up to 15**, including the collision groups the creator made. 

* **Default**: This will be set as **Default** when you cannot find the group, the collision group designated as Default when specifying a collision group.
* **TriggerBox**: A collision group that is designated as default to **TriggerComponent**.
* **HitBox**: A collision group that is designated as default to **HitComponent**.

If you use a collision group, you need not write the collision detection conditions using the collision-detecting component for each script, so it will help you create optimized worlds. 

![ex](https://mod-file.dn.nexoncdn.co.kr/bbs/16636400041968e29e517aa4a4d1fbebd96e76548c348.png{"width":"760px"} "ex")

If you want to allow players to grab items using **TriggerEnterEvent**, you must write the following condition. However, if you utilize collision groups, you don't have to write the following condition. You just need to designate different collision groups for each entity and set only two groups to collide.

```lua
Event Handler:
[server only] [self]
HandleTriggerEnterEvent(TriggerEnterEvent event)
{
    -- Parameters
    local TriggerBodyEntity = event.TriggerBodyEntity
    --------------------------------------------------------
    if not TriggerBodyEntity.PlayerComponent then
        return
    end
}
```

You can also use the characteristic that can collide between the desired groups only and apply the collision group to the physics system. Please refer to [Apply Physics](/docs?postId=757{"target":"_self"}) for the details of physics.

#### Collider
A Collider is required to make the entity induce collision. A collider can use Circle and Box forms. You can control the collider size and offset location that the creator made. Please refer to [Entity Collision](/docs/?postId=175{"target":"_self"}) for the details.

![ColliderType](https://mod-file.dn.nexoncdn.co.kr/bbs/1658731042366f8819537a3d742f99d669f5070219681.png{"width":"760px"} "ColliderType")

#### Create and Delete Collision Group
1. Select **Panels - Collision Groups**.
![panel](https://mod-file.dn.nexoncdn.co.kr/bbs/165879872581169fd5a2cac2641078636ce0eb587c817.png "panel")
<br>
2. Create a new collision group by pressing **[Add Collision Group]** and then change the name.
![AddCollision](https://mod-file.dn.nexoncdn.co.kr/bbs/16584743118836fa224b41e3445d79cdc7f15cf2a3886.png{"width":"460px"} "AddCollision")
<br>
3. Erase the collision group to delete by selecting the **[-]** button on the right.

#### Designate Collision Group
You can designate collision groups for each component that generates or detects collisions. Typically, the collision groups are used from **TriggerComponent, HitComponent, and PhysicsColliderComponent**. Let's look at how to designate collision groups from **HitComponent**.

1. Add the **HitComponent** or the **extended HitComponent** to the entity where you will set the collision groups.
2. The collision groups are set from the **CollisionGroup** Property. <br> ![CollisionPlayer](https://mod-file.dn.nexoncdn.co.kr/bbs/16644408634092cedbdbd0396471b85eb4a2b71e7547c.png "CollisionPlayer")

><span style="color: #7cafc2">**Tip**
>Next to **CollisionGroup**, click the ![openpanel](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_open_collision.png{"width":"16px"} "openpanel") **[Open Panel]** button to open the **CollisionGroup** panel.</span>

#### Collision Group Matrix
Collision Group Matrix (Matrix) is used to decide whether to allow the collisions between collision groups. Once you create a collision group, a new collision group will be automatically generated in the matrix. You can simply control whether to allow the collisions between collision groups by selecting the check box. You should enable the ![checkbox](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/checkbox.png{"width":"16px"} "checkbox") check box to check the relations between the collision groups written in the Matrix and allow the collision. 

1. Select **Panels - Collision Groups - Matrix**.<br>![Matrix](https://mod-file.dn.nexoncdn.co.kr/bbs/1658474513854b268965254e847a4933ae4b9a65b9c19.png{"width":"460px"} "Matrix")
2. When you check the horizontal and vertical group name and then allow the collision, the check box ![checkbox](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/checkbox.png{"width":"16px"} "checkbox") will be and allow the collisions.

# Example
Let's create a situation where the player character can collide with fallen coins, but not with monsters. 

![collision](https://mod-file.dn.nexoncdn.co.kr/bbs/165872039921972bcfdc780bd46d3858e48a54e7f080c.gif "collision")

><span style="color: #7cafc2">**Tip**
> You can find the omitted attack and hit implementation of the Player and the Monster on **Create - Create New - Default**.
>![new](https://mod-file.dn.nexoncdn.co.kr/bbs/16651315251702782341c1ca8470eb874b9b8a69f21bf.png "new")</span>

1. Create **Player, Monster, Item** for the new collision groups, and then set the Matrix as below.
![CollisionMatrix](https://mod-file.dn.nexoncdn.co.kr/bbs/1658799000407ba127c7e1c444484b8b1c91e8a0892cf.png "CollisionMatrix")

2. In **DefaultPlayer**, set **CollisionGroup** of **TriggerComponent** and **PlayerHit** as <span style="color: #dc9656">**Player**</span>.
![PlayerCollision](https://mod-file.dn.nexoncdn.co.kr/bbs/16587303515703643b6e021f0420084a25b41f08825ea.png "PlayerCollision")

3. Select the desired model from **Preset List - Monster**, and then open the context menu and press **Add to Workspace**.

4. In the selected Monster model, change **CollisionGroup** of **TriggerComponent and HitComponent** into <span style="color: #dc9656">**Monster**</span>.
![monster](https://mod-file.dn.nexoncdn.co.kr/bbs/16587300227457fa8146e6bd3466bb179b865a3a40ca1.png "monster")

5. Add **TriggerComponent** to the Coin model, assign **CollisionGroup** as <span style="color: #dc9656">**Item**</span>.
![itemCollision](https://mod-file.dn.nexoncdn.co.kr/bbs/165873019590583fa187d2e8a4b6ba01551f193284baf.png "itemCollision")

6. Create a new script component **ItemComponent** and add it into Coin model. Make coins disappear only when hitting with **PlayerComponent** as below.

    ```lua
    Event Handler:
    [server only] [self]
    HandleTriggerEnterEvent(TriggerEnterEvent event)
    {
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        --------------------------------------------------------
        self.Entity:Destroy()
    }
    ```