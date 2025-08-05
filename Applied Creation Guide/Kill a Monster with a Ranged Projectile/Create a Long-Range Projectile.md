# Course Introduction
> <span style="color: #585858">This guide follows the order below.
> **▶ Part 1.** [**Create a long-range projectile**](/docs?postId=935{"target":"_self"})
> Part 2. [Character attacking a monster with a projectile](/docs?postId=937{"target":"_self"})</span>

This is the first step to create a World where monsters are killed by flying long-range projectiles. Let's look at an example of creating a long-range projectile model and writing the associated script component.

# World Template
For ease of creation, it is recommended to create a world using the "Default Template".
![0](https://mod-file.dn.nexoncdn.co.kr/bbs/1656419849546d61c1ab96b5d454a971009e8053fe3c1.png "0")

# Projectile Model
First, let's create a projectile model.

1. In the context menu of **Workspace - MyDesk**, click **Create Model** to make a new model. Enter <span style="color: #dc9656">**Model_Projectile_Arrow**</span> as the name.
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/166994502327614339c1a63774655a2339847ccd8525b.png "1")
    <br>
2. Add **SpriteRendererComponent** from the Property Editor of **Model_Projectile_Arrow**, and then change the properties below.
    * **PlayRate : <span style="color: #dc9656">2.5</span>**
    * **SpriteRUID : <span style="color: #dc9656">82013caa86844d32853f8433266ef25e</span>**
    <br>
    You are free to use **SpriteRUID** as you like. 
    In this example, arrow-shaped sprites are used.
    ![001](https://mod-file.dn.nexoncdn.co.kr/bbs/16702272716982b32e3b2a0b540b6befb662b8ceebdf6.gif "001")
    <br>
3. Add **TransformComponent** and change the following properties.
    * **Scale : X = <span style="color: #dc9656">0.3</span>, Y = <span style="color: #dc9656">0.3</span>**
    <br>
4. Add **TriggerComponent** and change the properties below.
    * **BoxSize : X = <span style="color: #dc9656">2</span>, Y = <span style="color: #dc9656">0.3</span>**
    <br>
5. In the **CollisionGroup** property of **TriggerComponent**, click the **[Open Panel]** button. In the **Collision Groups** panel, click the **[Add Collision Group]** button to add the **Projectile** group. Then, go to the **Matrix** tab and set it as shown below.
    ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/166994619267247363ed2f25843fca7aef047bf291548.png "2")
    <br>
6. Set the **CollisionGroup** of **TriggerComponent** as **Projectile**.
    ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1670219112140feba1dbd76e54493bd2cd951c5d3755d.png "6")

# Projectile Script Component
Let's create a projectile script component and connect it to the projectile model.

1. In the context menu of **Workspace - MyDesk**, click **Create Scripts - Create Component** to create a new script component. Enter <span style="color: #dc9656">**Projectile**</span> as the name.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1669946572981601adb06667c4da180580b8863e708ca.png "3")
    <br>
2. Add the **Projectile** component to projectile model **Model_Projectile_Arrow**.
    <br>
3. Open the **Projectile** script component and add properties as follows.
    ```lua
    Property:  
    [Sync]
    number DestroyAfterSec = 2 -- Time from launch to detonation (seconds)
    [Sync]
    number Speed = 7.5 -- Projectile speed
    [Sync]
    string ExplodeSpriteRUID = "47a613712de34f9ba2ea25ef40894756" -- Explosion effect RUID
    [Sync]
    Vector2 ExplodeEffectOffset = Vector2(0,0) -- Effect offset (modify only if needed)
    [Sync]
    number EffectScale = 1 -- Explosion effect scale
    [None]
    boolean ToLeft = false -- Direction of projectile
    [None]
    Entity Firer = nil -- Subject that launches the projectile
    ```
4.  The projectile should not move at first, but should move once fired. So, add the `OnBeginPlay()` function and write as follows. Set the execution space to **Not Used**.
    ```lua
    void OnBeginPlay()
    {
        -- Set it to false so that the projectile doesn't move until fired.
        self._T.Fired = false
    }
    ```
    <br>
5. Add the `OnUpdate()` function to process the projectile moving forward over time. Set the execution space to **Server Only**.
    ```lua
    void OnUpdate(number delta)
    {
        if not self._T.Fired then
        	return
        end
        
        local xDelta = self.Speed * delta
        if self.ToLeft then
        	xDelta = xDelta * -1
        end 
        
        self.Entity.TransformComponent:Translate(xDelta, 0)
    }
    ```
     <br>
6. Add the `Fire()` function to control the projectile firing.
    ```lua
    void Fire()
    {
        local destroySelf = function()
        	self:Explode()
        end
        
        -- Set timer to execute destroySelf after DestroyAfterSec time
        _TimerService:SetTimerOnce(destroySelf, self.DestroyAfterSec) 
        
        -- Redirect the projectile's sprite when firing to the left
        self.Entity.SpriteRendererComponent.FlipX = not self.ToLeft 
        
        self._T.Fired = true
    }
    ```
    <br>
7. Add the `Explode()` function to set the projectile's attack area and play the explosion effect.
    ```lua
    void Explode()
    {
        local shape = BoxShape(Vector2.zero, Vector2.one, 0)
        
        -- Get the size of the explosion effect sprite and set it as the attack area
        _ResourceService:PreloadAsync({self.ExplodeSpriteRUID}, function()
        	if not isvalid(self.Firer) and not isvalid(self.Firer.PlayerAttack) then
        		return
        	end
        						
        	local clip = _ResourceService:LoadAnimationClipAndWait(self.ExplodeSpriteRUID)
        	local firstFrameSprite = clip.Frames[1].FrameSprite
        	local firstSpriteSizeInPixel = Vector2(firstFrameSprite.Width, firstFrameSprite.Height)
        	local ppu = firstFrameSprite.PixelPerUnit
        			
        	local spriteSize = firstSpriteSizeInPixel / ppu
        	local positionOffset = (firstSpriteSizeInPixel / 2 - firstFrameSprite.PivotPixel:ToVector2()) / ppu
        		
        	-- Correct offset
        	local effectOffset = self.ExplodeEffectOffset
        	if self.ToLeft then
        		effectOffset.x = -effectOffset.x
        	end
    }
    ```
    <br>
8. In order to process the attack as much as the area of the explosion effect, write the content below following the content of the `Explode()` function above.
    ```lua
    -- Attack as much as the area of the explosion effect
	local transform = self.Entity.TransformComponent
	local worldPosition = transform.WorldPosition
	local scale = self.EffectScale
	local radian = math.rad(transform.ZRotation)
	local offsetX = math.cos(radian) * positionOffset.x * scale - math.sin(radian) * positionOffset.y * scale
	if not self.ToLeft then
     offsetX = -offsetX
    end
	local offsetY = math.sin(radian) * positionOffset.x * scale + math.cos(radian) * positionOffset.y * scale
	shape.Size = Vector2(spriteSize.x * math.abs(scale), spriteSize.y * math.abs(scale))
	shape.Position = Vector2(worldPosition.x + offsetX + effectOffset.x, worldPosition.y + offsetY + effectOffset.y)
	shape.Angle = transform.ZRotation
		
	self.Firer.PlayerAttack:AttackFast(shape, nil, CollisionGroups.Monster)		
    ``` 
      <br>
9. In order to play the explosion effect and destroy the projectile, write the content below following the content of the `Explode()` function above.
    ```lua
    -- Play explosion effect
	local effectPosition = Vector3(transform.Position.x + effectOffset.x, transform.Position.y + effectOffset.y, transform.Position.z)
	local options = { ["FlipX"] = not self.ToLeft }
	_EffectService:PlayEffect(self.ExplodeSpriteRUID, self.Entity, effectPosition, 0, Vector3.one * self.EffectScale, false, options)	
	
	-- Remove projectile	
	self.Entity:Destroy()
    end)
    ```

      <br>
    The complete `Explode()` function is as follows.
    ```lua
    void Explode()
    {
        local shape = BoxShape(Vector2.zero, Vector2.one, 0)
        
        -- Get the size of the explosion effect sprite and set it as the attack area
        _ResourceService:PreloadAsync({self.ExplodeSpriteRUID}, function()
        	if not isvalid(self.Firer) and not isvalid(self.Firer.PlayerAttack) then
        		return
        	end
        						
        	local clip = _ResourceService:LoadAnimationClipAndWait(self.ExplodeSpriteRUID)
        	local firstFrameSprite = clip.Frames[1].FrameSprite
        	local firstSpriteSizeInPixel = Vector2(firstFrameSprite.Width, firstFrameSprite.Height)
        	local ppu = firstFrameSprite.PixelPerUnit
        			
        	local spriteSize = firstSpriteSizeInPixel / ppu
        	local positionOffset = (firstSpriteSizeInPixel / 2 - firstFrameSprite.PivotPixel:ToVector2()) / ppu
        		
        	-- Correct offset
        	local effectOffset = self.ExplodeEffectOffset
        	if self.ToLeft then
        		effectOffset.x = -effectOffset.x
        	end
        			
        	-- Attack as much as the area of the explosion effect
        	local transform = self.Entity.TransformComponent
        	local worldPosition = transform.WorldPosition
        	local scale = self.EffectScale
        	local radian = math.rad(transform.ZRotation)
        	local offsetX = math.cos(radian) * positionOffset.x * scale - math.sin(radian) * positionOffset.y * scale
        	if not self.ToLeft then
        	    offsetX = -offsetX
        	end
        	local offsetY = math.sin(radian) * positionOffset.x * scale + math.cos(radian) * positionOffset.y * scale
        	shape.Size = Vector2(spriteSize.x * math.abs(scale), spriteSize.y * math.abs(scale))
        	shape.Position = Vector2(worldPosition.x + offsetX + effectOffset.x, worldPosition.y + offsetY + effectOffset.y)
        	shape.Angle = transform.ZRotation
        		
        	self.Firer.PlayerAttack:AttackFast(shape, nil, CollisionGroups.Monster)		
        		
        	-- Play explosion effect
        	local effectPosition = Vector3(transform.Position.x + effectOffset.x, transform.Position.y + effectOffset.y, transform.Position.z)
        	local options = { ["FlipX"] = not self.ToLeft }
        	_EffectService:PlayEffect(self.ExplodeSpriteRUID, self.Entity, effectPosition, 0, Vector3.one * self.EffectScale, false, options)	
        	
        	-- Remove projectile	
        	self.Entity:Destroy()
        end)
    }
    ```
      <br>
10. Add **TriggerEnterEvent** to the event handler to process the projectile exploding when it hits the monster. Set the execution space to **Server Only**.
    ```lua
    [server only] [self]
    HandleTriggerEnterEvent(TriggerEnterEvent event)
    {
        -- Parameters
        local TriggerBodyEntity = event.TriggerBodyEntity
        --------------------------------------------------------
        if isvalid(TriggerBodyEntity.Monster) == false then
        	return
        end
          self:Explode()
    }
    ```

So far, we've created projectile-related scripts. In the next lesson, let's have the character attack the monster with a projectile.
[Character Attacking a Monster with a Projectile](/docs?postId=937{"target":"_self"})