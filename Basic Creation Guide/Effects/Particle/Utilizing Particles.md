# Course Introduction
From the [Using Particles] guide, we learned the concepts and types of particle components. This time, we'll see examples of particle use by using scripts.

# Particle Scripts
## Controling Particle Properties
Let's see how to control particle properties through an example in which the play speed of particles changes according to the distance from the character.
1. Set a dark background so that the particles can be seen clearly, place the topography, and place the desired particles. In this example, <span style="color: #dc9656">**back-16**</span> is set as the Background and <span style="color: #dc9656">**Nova**</span> is placed as the particle.
![particle](https://mod-file.dn.nexoncdn.co.kr/bbs/1660613012572797db96a127d4a01a0ecd37f02c3ae02.png "particle")

2. Generate the new script component ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") ParticlePlaySpeed in ![workspace](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_workspace.png "workspace") Workspace - ![MyDesk](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_user_folder_no.png "MyDesk") MyDesk.

3. Add ![etc](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/Ect.png "etc") ParticlePlaySpeed to the particle Entity's ![property](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_inspector.png "property") Property placed in the map.

4. Open ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") ParticlePlaySpeed, add `OnUpdate()` function, and write the script as follows.
    ```lua
    [client only]
    void OnUpdate(number delta)
    {
        local positionA = Vector2(self.Entity.TransformComponent.Position.x,self.Entity.TransformComponent.Position.y)
        local positionB = Vector2(_UserService.LocalPlayer.TransformComponent.Position.x,_UserService.LocalPlayer.TransformComponent.Position.y)
        local distance = Vector2.Distance(positionA, positionB)
             
        self.Entity.BasicParticleComponent.PlaySpeed = distance
    }
    ```

5. Let's press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") Start and test. You can check the particle playing slowly as the character gets closer to the particle and faster as they move away.
    ![speed](https://mod-file.dn.nexoncdn.co.kr/bbs/16606131392543e4d5902b9d74e7ba57dadb7ad920fb5.gif "speed")

## Changing Particle Playing State
Let's see how to play a particle or stop a particle that is playing when a certain event occurs. You can make these functions via the `Play()` and `Stop()` functions. Let's look at an example that stops a particle from playing when a character collides with a specific entity.

1. In ![workspace](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_workspace.png "workspace") Workspace - ![DefaultPlayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_myavatar_no.png "DefaultPlayer") DefaultPlayer's ![property](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_inspector.png "property") Property, add BasicParticleComponent by pressing the "+" button.
![addcomponent](https://mod-file.dn.nexoncdn.co.kr/bbs/1660290482559d3eb4baea5eb46a3b16be66aba558091.png "addcomponent")

2. Set ParticleType to FireField.
![particletype](https://mod-file.dn.nexoncdn.co.kr/bbs/1660290572916bf1ddbdf34bb4d799dca05964b5c5712.png "particletype")

3. Generate the new script component ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") ParticleController in ![workspace](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_workspace.png "workspace") Workspace - ![MyDesk](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_user_folder_no.png "MyDesk") MyDesk.

4. Add ![etc](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/Ect.png "etc") ParticleController to ![DefaultPlayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_myavatar_no.png "DefaultPlayer") DefaultPlayer's ![property](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_inspector.png "property") Property.

5. Open ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") ParticleController, add ![event](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_mod_event_no.png "event") TriggerEnterEvent in Event Handler, and write as follows.
    ```lua
    Event Handler:
    HandleTriggerEnterEvent(TriggerEnterEvent event)
    {
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        --------------------------------------------------------
        self.Entity.BasicParticleComponent:Stop()
    }
    ```

6. Place Entity to activate a trigger and add ![trigger](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/collision.png "trigger") TriggerComponent. This example used ![preset](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_asset.png "preset") Preset List - <span style="color: #dc9656">**object-1814**</span>.
    ![trigger](https://mod-file.dn.nexoncdn.co.kr/bbs/16602924504597b005ed1a05d4844ae69349f39372d47.png "trigger")

7. Now press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") Start and test. You can see that the FireField particle attached to the character's body disappears when it hits the waterfall entity.
    ![firestop](https://mod-file.dn.nexoncdn.co.kr/bbs/16602925276347f42645d58414436a13cd40260c6744b.gif "firestop")

# Particle Services
Let's learn how to control particles using particle services. 
## One-Time Particle Generation
1. Set a dark background so that the particles can be seen clearly, place the topography, and place the desired entity. In this example, Background - <span style="color: #dc9656">**back-51**</span> is set, and Object - <span style="color: #dc9656">**object-174**</span> is placed in the ![preset](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_asset.png "preset") Preset List.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1660614857422a2b18c5225874b859938ae7d87d119e1.png "8") 

2. Generate the new script component ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") ParticleServiceTest in ![workspace](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_workspace.png "workspace") Workspace - ![MyDesk](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_user_folder_no.png "MyDesk") MyDesk.

3. Add ![etc](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/Ect.png "etc") ParticleServiceTest to the placed Entity (object-174)'s ![property](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_inspector.png "property") Property.

4. Add ![etc](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/Ect.png "etc") TweenCircularComponent to it and set as follows.
    ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/166061571192425e9d621c5c54d8f8d64fad989e65a7e.png "9")
    <br>
5. Open ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") ParticleServiceTest, add the `AddParticle()` function, and then write the script as follows.
    ```lua
    void AddParticle()
    {
        local option = {["Color"] = Color(0.25,0.5,0.5, 1), ["ParticleCount"] = 3}
        
        _ParticleService:PlayBasicParticle(BasicParticleType.Firework, self.Entity, self.Entity.TransformComponent.WorldPosition, 0, Vector3(1,1,1), false, option)
        -- Play the particles through ParticleService's PlayBasicParticle() function. Loop is set to false so that the particle would only play once.
    }
    ```
   
6. Add the `OnBeginPlay()` function and write as follows.
    ```lua
    [server only]
    void OnBeginPlay()
    {
        _TimerService:SetTimerRepeat(self.AddParticle, 2, 0) 
        -- Make the AddParticle function work once every 2 seconds.
    }
    ``` 
 
7. Press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") Start and test. You can see that the entity generates particles once every 2 seconds.
    ![particle2](https://mod-file.dn.nexoncdn.co.kr/bbs/1660617146781af026d51a0bc46e1acf08c4ab0ea2158.gif "particle2")

## Generation and Deletion of Repeating Particles
Let's learn how to generate and delete the repeatedly playing particles.
Let's create an exercise where particles get generated when a character touches a specific entity and then stop when the character arrives at a target point.

1. Place the entity that will emit particles and the target entity. In this example, the entity to emit particles is set as <span style="color: #dc9656">**object-24**</span> (vial), and the target Entity as <span style="color: #dc9656">**object-928**</span> (fire extinguisher).
    ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1660626870398bc48f56357f64b8896d961b26265f31a.png "11")
  
2. Add ![trigger](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/collision.png "trigger") TriggerComponent to all placed entities.
   
3. Generate the new script component ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") ParticleServiceTest02 in ![workspace](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_workspace.png "workspace") Workspace - ![MyDesk](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_user_folder_no.png "MyDesk") MyDesk.
  
4. Open ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") ParticleServiceTest02 and add the property as follows.
    ```lua
    Property:
    [None]
    Entity ResetEntity = "nil"    
    [None]
    SyncTable<number> SpawnList
    ```
    
5. Let's say the particles must be generated once the character touches the vial and disappear when it touches the fire extinguisher. Add ![event](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_mod_event_no.png "event") TriggerEnterEvent to ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") ParticleServiceTest02's Event Handler and write as below.
    ```lua
    Event Handler:
    HandleTriggerEnterEvent(TriggerEnterEvent event)
    {
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        --------------------------------------------------------
        if TriggerBodyEntity ~= self.ResetEntity then
           local serial = _ParticleService:PlayBasicParticleAttached(BasicParticleType.ToonTallFire, TriggerBodyEntity, Vector3.zero, 0, Vector3.one, true)
           table.insert(self.SpawnList, serial)
           -- When a character hits an entity other than the target entity, a particle will be generated from that entity via the PlayBasicParticleAttached() function.
        else
           for idx, serial in pairs(self.SpawnList) do
               _ParticleService:RemoveParticle(serial)
           -- When the character touches the target entity, all previously generated particles will be removed through the RemoveParticle() function.
           end
           table.clear(self.SpawnList)
         
        end
    }     
    ```
6. Add ![etc](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/Ect.png "etc") ParticleServiceTest02 to ![workspace](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_workspace.png "workspace") Workspace - ![DefaultPlayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_myavatar_no.png "DefaultPlayer") DefaultPlayer. Then specify the target Entity (ResetEntity).
    ![12](https://mod-file.dn.nexoncdn.co.kr/bbs/16606287624144a1602d085b94be9a8a2a82da2df2c2c.png "12")

7. Press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") Start and test. You can see that when the character's body touches the vial, the flame particles are played, and touching the fire extinguisher makes the flame particles disappear.
    ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/16606269436796b58e75202444fc39e8d8bb970970fe7.gif "10")

# Particle Events
You can use particle events to inflict damage or show direction whenever particles are repeatedly generated. Let's take a look at how to use particle events with an example where the attack function activates and damages the character whenever lightning strikes.

1. Place the ![preset](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_asset.png "preset") Preset List - <span style="color: #dc9656">**LightningStrikeSharp**</span> particle.
    ![15](https://mod-file.dn.nexoncdn.co.kr/bbs/16606310495644b54ab47cff543aaada7c7264539eadc.png "15")

2. Extend AttackComponent and generate the new script component ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") ExtendAttackComponent.
    ![14](https://mod-file.dn.nexoncdn.co.kr/bbs/16606293771586eb9e239b50d424d8d0c5f5d79e31eb5.png "14")
3. Open ![scriptcomponent](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "scriptcomponent") ExtendAttackComponent and add the `OnLightning()` function.
    ```lua
    void OnLightning()
    {
        self:Attack(Vector2(2, 2), Vector2.zero, nil)
        -- Use the Attack() function to determine the attack.
    }
    ```

4. In Event Handler, add ![event](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_mod_event_no.png "event") ParticleEmitStartEvent and write as follows.
    ```lua
    Event Handler:
    HandleParticleEmitStartEvent(ParticleEmitStartEvent event)
    {
        -- Parameters
        local ParticleEntity = event.ParticleEntity
        --------------------------------------------------------
        self:OnLightning()
        -- Make the OnLightning function work when starting particles.
    }
    ```
  
5. In Event Handler, add ![event](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_mod_event_no.png "event") ParticleLoopEvent and write as follows.
    ```lua
    Event Handler:
    HandleParticleLoopEvent(ParticleLoopEvent event)
    {
        -- Parameters
        local ParticleEntity = event.ParticleEntity
        --------------------------------------------------------
        self:OnLightning()
        -- Make the OnLightning function work when starting particles.
    }
    ```

6. Add ![etc](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/Ect.png "etc") ExtendAttackComponent to the initially placed LightningStrikeSharp particle's ![property](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_inspector.png "property") Property.
    
7. Press ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") Start and test. You can see that the character takes damage when struck by lightning.
    ![damage](https://mod-file.dn.nexoncdn.co.kr/bbs/166063078658715693caafe5c498cb4d1afb74ab490ca.gif "damage")

> <span style="color: #585858">**Learn More**
> [Attack and Hit] Let's try a variety of effects by referring to the guide.</span>


# Using Particles in UI
If you want to play particles in UI, you must use UIBasicParticleComponent, UIAreaParticleComponent, and UISpriteParticleComponent. Let's learn how to use particles in UI through an example using UIBasicParticleComponent.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1738820159173bac2118a2b8e4fb3939212dacdec4c30.gif "UIParticle")

#### Making UI
Let's make a UI that can play particles. 

1.  Add a new **UIGroup** to **Hierachy - World - ui**.
2. Set the **DefaultShow** propery under **UIGroupComponent** from the added UIGroup to **true**.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1738820526946c8b71422566e4a919660cc52294d2f78.png "DefaultShow")
3. From UIGroup, press **Context Menu - Create Empty - Create Button** to add a new **UIButton**.
4. From UIButton, select **Context Menu - Create Entity as Child - Create UIEmpty**. Change the created entity's name to **UIParticle**.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1738821689063de9df9de3eb24d1ca3aaab61c30a0d34.png "UIButton")
   

#### Creating Particles
1. Add **UIBasicParticleComponent** to **UIParticle**.

2. Set the ParticleType as **CircleBust** and adjust to the desired particle size, speed, etc.
3. Set the **Loop** for **UIBasicParticleComponent** as **false** to make the particle play only once when the button is pressed. Also set **PlayOnEnable** to **false** to ensure the particle doesn't play immediately upon entering a world. 
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1738824957555795b8fa96c3b4e31a73bd9d943735ea5.png "3")
4. Select **Workspace - Create Script - Create Component** to create a new **UIButtonParticle**.
5. Add the created **UIButtonParticle** to the **UIButton** entity.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17388237882156f14f1b12db546a1a445464f92d57fa1.png "UIButtonParticle")
6. Add a **particleEntity** property to UIButtonParticle. Set the property type to **Entity** and link the **UIParticle** entity.
   ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/173882498272778923333c4a64ed8ad7b1c6c67ddbb18.png "4")
   ```lua
   Property:
   [None]
   Entity particleEntity = /ui/UIGroup/UIButton/UIParticle
   ```

7. Add **ButtonClickEvent** to Event Handler.
Write the following so that the particle will play when pressing the button.

   ```lua
   Event Handler:
   [self]
   HandleButtonClickEvent(ButtonClickEvent event)
   {
        --------------- Native Event Sender Info ----------------
        -- Sender: ButtonComponent
        -- Space: Client
        ---------------------------------------------------------

        -- Parameters
        -- local Entity = event.Entity
        ---------------------------------------------------------
        local Entity = event.Entity
        ---------------------------------------------------------
        if self.particleEntity.UIBasicParticleComponent.IsEmitting == true then
            self.particleEntity.UIBasicParticleComponent:Stop()
        end
            self.particleEntity.UIBasicParticleComponent:Play()
   }
   ```

8. After adjusting the entity's size, press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button to see if the particle plays each time a button is pressed.