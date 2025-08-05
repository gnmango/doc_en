# Course Introduction
Let's learn how to apply the laws of physics to the MapleStory Worlds maps and entities. Using the laws of physics, you can make a variety of movements, from colliding and bouncing off each other, flying in a parabolic motion, or making entities move by connecting them to each other.
We recommend learning this in parallel with [Entity Collision] and [Create Collision Group].

# Applying the laws of physics to the map
The basic map follows the MapleStory map composition and movement principle, so the laws of physics are not applied by default. Therefore, a separate setting is required for a map you want to apply the laws of physics to. If you want to use physics features on a map, you must add <span style="color: #dc9656">**![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/move.png "component") PhysicsSimulatorComponent**</span> to the map.
You can set the gravity of the map using the ![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/move.png "component") PhysicsSimulatorComponent's Gravity property. Depending on the x and y values of Gravity, the direction and force of gravity on the map will change. If you want to create a map that applies the laws of physics where gravity acts in the left direction to all entities, specify a negative x value. If you want to create a map in which gravity acts upward, specify a positive y value. You can also create a zero gravity map by setting the x and y values to 0.

1.  Choose ![hierachy](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene_maker.png "hierachy") Hierarchy - ![world](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_world_no.png "world") World - ![maps](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_maps_no.png "maps") maps - ![map](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_map_no.png "map") map01.
2. Add PhysicsSimulatorComponent and enter a Gravity value.<br>![simulator](https://mod-file.dn.nexoncdn.co.kr/bbs/16602791075369a9cbb9197f24d2bacf24d88f506eba3.png "simulator")

| (-5,-10) | (0,4) |
| --- | --- |
| ![-5](https://mod-file.dn.nexoncdn.co.kr/bbs/1659508219042208f13fd27134e3992c526d914be0b51.gif "-5") | ![+4](https://mod-file.dn.nexoncdn.co.kr/bbs/16595082076157020bb152a804a76b314d6c9eedc6012.gif "+4")|

#### Making the Ground
Since the default World setup doesn't interact with the physics entities, the basic foothold will likewise pass through the physics entities without colliding. If you want physics entities to collide with tiles, activate ![map](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/map.png "map") TileMapComponent, ![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/Ect.png "component") CustomeFootholdComponent's <span style="color: #dc9656">**PhysicsInteractable**</span>.
Let's make a ground and wall role to which physics entities can collide possible by activating ![map](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/map.png "map") <span style="color: #dc9656">**TileMapComponent's PhysicsInteractable**</span>.

1. Choose ![hierachy](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene_maker.png "hierachy") Hierarchy - ![world](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_world_no.png "world") World - ![maps](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_maps_no.png "maps") maps - ![map](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_map_no.png "map") map01 - ![map](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/map.png "map") TileMap.
2. Activate ![mapcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/map.png "mapcomponent") TileMapComponent's PhysicsInteractable Property ![checkbox](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/checkbox.png "checkbox").
![interactable](https://mod-file.dn.nexoncdn.co.kr/bbs/1659508535605c9a75f4c314346f7a7917c8414ad7f19.png "interactable")

| PhysicsInteractable ![checkboxdisable](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/CheckboxOff.png "checkboxdisable") |   PhysicsInteractable ![checkbox](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/checkbox.png "checkbox") |
| --- | --- |
| ![off](https://mod-file.dn.nexoncdn.co.kr/bbs/16595852217680f7f0463ddd342b1bae1498907fbe9c8.gif "off") | ![on](https://mod-file.dn.nexoncdn.co.kr/bbs/16595852355591216ebd84358467e8a22501149d7856a.gif "on") |
# Applying the Laws of Physics to Entities
You can add <span style="color: #dc9656">**PhysicsRigidbodyComponent**</span> to an entity to adjust the entity's physics type, friction, etc. At this time, <span style="color: #dc9656">**PhysicsColliderComponent**</span> is added together, and you can set the shape and size of the colliding body. Entities that have two added components can collide and raise related Events. If you don't add <span style="color: #dc9656">**PhysicsRigidbodyComponent**</span>, you get a Static type entity with infinite mass. Physics entities are divided into three types and have the following characteristics.

* <span style="color: #dc9656">**Static**</span>: It is treated as an entity with an infinitely large mass and can collide with Dynamic types.
![static](https://mod-file.dn.nexoncdn.co.kr/bbs/1659576952722cf8c504f8d504c7e9086e3bec94a09c9.gif "static")
* <span style="color: #dc9656">**Dynamic**</span>: Entities behave according to the laws of physics. It can collide with other entities that can collide with physics.
![dynamic](https://mod-file.dn.nexoncdn.co.kr/bbs/1659576966548ee80da29541e440eac920f8b56b56064.gif "dynamic")
* <span style="color: #dc9656">**Kinematic**</span>: It is treated as an entity with an infinitely large mass, but unlike the Static type, it can move by specifying a speed.
![kinematic](https://mod-file.dn.nexoncdn.co.kr/bbs/165959362285423ac24e7dcdb4ca6b543886eee581004.gif "kinematic")
##### Reference Guide
* [Applying Physics to Entities]
* [Using Various Physics Joints]
* [Entity Collision]
