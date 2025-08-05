# Course Introduction
Particles refer to many small elements, such as snow, rain, and dust. You can control the properties and state via ParticleComponent.
Let's learn more about the concept of particles and how to use particle components.

##### API Reference
[BaseParticleComponent](/apiReference/Components/BaseParticleComponent{"target":"_self"})
[AreaParticleComponent](/apiReference/Components/AreaParticleComponent{"target":"_self"})
[BasicParticleComponent](/apiReference/Components/BasicParticleComponent{"target":"_self"})
[SpriteParticleComponent](/apiReference/Components/SpriteParticleComponent{"target":"_self"})
[UIBaseParticleComponent](/apiReference/Components/UIBaseParticleComponent{"target":"_self"})
[UIAreaParticleComponent](/apiReference/Components/UIAreaParticleComponent{"target":"_self"})
[UIBasicParticleComponent](/apiReference/Components/UIBasicParticleComponent{"target":"_self"})
[UISpriteParticleComponent](/apiReference/Components/UISpriteParticleComponent{"target":"_self"})
[ParticleEmitEndEvent](/apiReference/Events/ParticleEmitEndEvent{"target":"_self"})
[ParticleEmitStartEvent](/apiReference/Events/ParticleEmitStartEvent{"target":"_self"})
[ParticleLoopEvent](/apiReference/Events/ParticleLoopEvent{"target":"_self"})
[ParticleService](/apiReference/Service/ParticleService{"target":"_self"})

# Using Particles
The movements of various particles can be expressed using particles.
![00](https://mod-file.dn.nexoncdn.co.kr/bbs/166330355094795e601dda1164aac848ae79d1496e768.gif "00")

There are 3 ways to use particles. The creator can use particles in the method of their choice.

| Place in Preset List | Add particle component to entity | Add from Workspace |
| --- | --- | --- |
|In ![preset](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_asset.png "preset") Preset List - Particle Effect, place the particles you want in the ![scene](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene.png "scene") Scene.|Add a particle component to the ![property](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_inspector.png "property") property of the entity to generate particles|In ![workspace](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_workspace.png "workspace") Workspace, select the particle model, and place it in the ![scene](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_scene.png "scene") Scene.|
| ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16606313000388e7d4aa6e5374d6196f17bf2166b1b3e.png "1") | ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16606314685787f65edd3774b494cb32c9f285a4abdbc.png "2") | ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1660285664348a8db102a53b142f9b04a4e043ab2bb09.png "3") |

# Types of Particle Components
Particle components are largely classified for world and for UI, and there are 3 particles.
* Particle Component for World: [BasicParticleComponent](/apiReference/Components/BasicParticleComponent{"target":"_self"}), [SpriteParticleComponent](/apiReference/Components/SpriteParticleComponent{"target":"_self"}), [AreaParticleComponent](/apiReference/Components/AreaParticleComponent{"target":"_self"})
* Particle Component for UI: [UIBasicParticleComponent](/apiReference/Components/UIBasicParticleComponent{"target":"_self"}), [UISpriteParticleComponent](/apiReference/Components/UISpriteParticleComponent{"target":"_self"}), [UIAreaParticleComponent](/apiReference/Components/UIAreaParticleComponent{"target":"_self"})

## BasicParticleComponent
The [BasicParticleComponent](/apiReference/Components/BasicParticleComponent{"target":"_self"}) is a component controls the general particles.
![basic](https://mod-file.dn.nexoncdn.co.kr/bbs/16633078865839e5488d2c3574197a38db8ff29c04719.png "basic")

| Property | Description |
| :---: | --- |
| ParticleType | Sets the type of particle to be generated. <br> Supported properties vary depending on the particle type. |
| Color | Corrects the color of the particles to be rendered. <br> ![color](https://mod-file.dn.nexoncdn.co.kr/bbs/1660286156819e905e8074a604e01800aa653fb244c1c.gif "color") |
| Loop | Sets whether to play particles repeatedly. |
| ParticleCount | Sets the number of particles. <br> ![count](https://mod-file.dn.nexoncdn.co.kr/bbs/16602820708290573d1ebd7634d4491632ef67088c0f5.gif "count") |
| ParticleLifeTime | Sets the duration of particles. <br>![life](https://mod-file.dn.nexoncdn.co.kr/bbs/1660282419952c9bc08a01b6040ae929a90ddb867fc96.gif "life") |
| ParticleSize | Sets the size of particles. <br>![size](https://mod-file.dn.nexoncdn.co.kr/bbs/16602825979965f0f3af7b4304a6a9d3f220a9f645128.gif "size") <br> If you want to control the size of the entire particle, let's adjust the scale of TransformComponent.|
| ParticleSpeed | Sets the speed of particles. <br>![particlespeed](https://mod-file.dn.nexoncdn.co.kr/bbs/1660284180137e517a7338d2f4482ba095199640f0fc5.gif "particlespeed") |
| PlayOnEnable | Sets whether to play particles when the particle component is Enable. <br>Whether to repeat after playing should be set through the Loop property. |
| PlaySpeed | Sets the play speed of particles. If <br>PlaySpeed = 0, the particle will appear stopped. <br>PlaySpeed does not support negative numbers. Negative numbers are treated the same as 0. <br>![playspeed](https://mod-file.dn.nexoncdn.co.kr/bbs/166028475387470172905ee5b4714b327f18bf21dce0c.gif "playspeed") |
| Prewarm | If set as Enable, the maximum number of particles is loaded, and the particles play naturally. <br>From the moment the particle first starts playing, it will look natural as if the particle was already playing before that. |
| SortingLayer | When two or more entities overlap, the priority is determined according to the Sorting Layer. |
| OrderInLayer | It determines the priority within the same Layer. A greater number indicates higher priority. |
| IgnoreMapLayerCheck | It does not perform automatic replace when designating the Map Layer name to SortingLayer. |
| AutoRandomSeed | Sets whether to create a new random seed whenever particle emission begins. |

## SpriteParticleComponent
The [SpriteParticleComponent](/apiReference/Components/SpriteParticleComponent{"target":"_self"}) is a component generates particles via Sprite. 
For instance, if you set <span style="color: #dc9656">**c4839de6b7be438abf008b5f547cbe98**</span> in **SpriteParticleComponent - SpriteRUID**, the particles will be generated as heart-shaped sprites as below.
![spritegif](https://mod-file.dn.nexoncdn.co.kr/bbs/1663309554383b08e07180d384167a62f7383bcd4710b.gif "spritegif")
Let's look at the main properties.

| Property | Description |
| :---: | --- |
| ApplySpriteColor | Sets whether the Color property is applied to the Sprite to be used by the particle. Even if the property is false, the transparency value of Color is applied. |
| SpriteRUID | Sets the SpriteRUID to be used as the particle. |

## AreaParticleComponent 
[AreaParticleComponent](/apiReference/Components/AreaParticleComponent{"target":"_self"}) is a component that can set the particle's generation area.
 Even if the particle's generation area is changed, the same number of particles will be generated. When the ParticleCount is also increased with the particle's generation area, the particle density can be kept at previous levels, and if only the particle generation area is increased, then the particle density will decrease.
Let's look at the main properties.
 
| Property | Description |
| :---: | --- |
| Bounds | By pressing the Edit button, you can adjust AreaOffset and AreaSize in the Scene. When Editing, the visible rectangular bounds are the creation range, not the existence range of particles. Depending on the movement of the particle after creation, it may go out of the bounds of the rectangle. <br> ![area_bounds](https://mod-file.dn.nexoncdn.co.kr/bbs/166027977616209fce41236a24051a06ec41c99c44699.png "area_bounds") |
| AreaOffset | Sets the location of the centerpoint of the creation range relative to the entity. |
| AreaSize | Specifies the width and height of the particle creation range. |


## UIBasicParticleComponent
[UIBasicParticleComponent](/apiReference/Components/UIBasicParticleComponent{"target":"_self"}) is a component that controls normal particles. It can only be used in UI.
Let's look at the main properties.

| Property | Description |
| :---: | --- |
| ParticleType | Set the type of particle to generate. <br> The supported property changes based on the particle type. |
| Color | Corrects the color of the rednered particle.|
| LocalScale | The size of the particle. |
| PlaySpeed| Set the playing speed of the particle. <br>If PlaySpeed = 0, the particle will look like it's paused. <br>PlaySpeed doesn't support negative numbers, and negative numbers will be treated the same as 0.|
| ParticleSize | Set the particle size.|
| ParticleSpeed | Set the particle's speed. |
| ParticleCount | Set the number of particles.|
| ParticleLifeTime | Set the particle's duration.  |
| PlayOnEnable | When particle component is set to Enable, whether the particle will be played can be set. <br>Whether it repeats after playing can be set from the Loop property. |
| Loop | Set the particle's repeat play status. |
| Prewarm | When set to Enable, the max numer of particles is called and naturally plays the particle. <br>It will look natural as if it was being played before the particle was even first played. |
| AutoRandomSeed | Set if the random seed will be newly created each time the particle is released. |

## UIAreaParticleComponent
[UIAreaParticleComponent](/apiReference/Components/UIAreaParticleComponent{"target":"_self"}) is a component that can set the particle's generation area in the UI. Even if the particle's generation area is changed, the same number of particles will be generated in that area. 
Let's look at the main properties.

| Property | Description |
| :---: | --- |
| AreaSize| Sets the particle generation area.|
| AreaOffset | Set the center point of the generation area based on the Entity. |


## UISpriteParticleComponent
[UISpriteParticleComponent](/apiReference/Components/UISpriteParticleComponent{"target":"_self"}) is a component that creates particles using sprites in the UI. 

| Property | Description |
| :---: | --- |
| SpriteRUID | Sets the SpriteRUID that will be used as a particle. |
| ApplySpriteColor |  Sets if the the Color property will be used in the Sprite that will be used by the particle. Even if the property is set as false, the Color's transparency value will be applied. |
| LocalScale |Sets the size of the particle. |