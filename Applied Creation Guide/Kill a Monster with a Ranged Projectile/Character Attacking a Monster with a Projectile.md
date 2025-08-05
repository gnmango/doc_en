# Course Introduction
> <span style="color: #585858">This guide follows the order below.
> Part 1. [Create a Long-Range Projectile]
> **▶ Part 2.** [**Character Attacking a Monster with a Projectile**]</span>

This is the last step in creating a world where monsters are killed by long-range projectiles. Let's have the character attack a monster with a projectile.

# Press the attack key to fire projectiles.
1. Open the **PlayerAttack** script and add the property as follows.
    ```lua
    Property:
    [Sync]
    Vector3 ProjectileOffset = Vector3(0, 0, 0)
    [None]
    string ProjectileModel = ""
    [Sync]
    number Damage = 10
    ```

2. In the Property Editor of **Workspace - DefaultPlayer**, modify the properties of the **PlayerAttack** component as follows.
    * **ProjectileOffset : X = <span style="color: #dc9656">0.4</span>, Y = <span style="color: #dc9656">0.4</span>**
    * **ProjectileModel: In the context menu of Model_Projectile_Arrow**, select **Copy Entry ID** to paste the copied value.
    <br>
    **Offset** value can be adjusted appropriately according to the sprite used in **Model_Projectile_Arrow**.
    <br>
3. Open the **PlayerAttack** script again and modify the content of the `AttackNormal()` function. Set the execution space to **Server Only**.
    ```lua
    [server only]
    void AttackNormal()
    {
        local playerController = self.Entity.PlayerControllerComponent
        local transform = self.Entity.TransformComponent
        if playerController and transform then
        	-- Modify to fire projectiles when the character presses the attack key.
        	local lookLeft = playerController.LookDirectionX == -1
        	local projectileOffset = self.ProjectileOffset:Clone()
        	if lookLeft then
        		projectileOffset.x = -projectileOffset.x
        	end
        	local projectileEntity = _SpawnService:SpawnByModelId(self.ProjectileModel, "Projectile", 
        						   transform.Position + projectileOffset, self.Entity.Parent)
        	local projectile = projectileEntity.Projectile
        	projectile.ToLeft = lookLeft
        	projectile.Firer = self.Entity
        	projectile:Fire()
        end
    }
    ```
    <br>
4. Modify the `CalcDamage()` function as follows.
    ```lua
    override int CalcDamage(Entity attacker, Entity defender, string attackInfo)
    {
        return self.Damage
    }
    ```

# Set Up Monster Collision Group
Set up a collision group to check collisions between projectiles and monsters.
1. In **Workspace - BaseEnvironment - NativeModel**, add each **TriggerComponent** to <span style="color: #dc9656">**StaticMonster, MoveMonster, ChaseMonster**</span> and set **CollisionGroup** to **Monster**.
    ![moster](https://mod-file.dn.nexoncdn.co.kr/bbs/1670223867214138f17c5c5b4411e9fb273e06a5e11e6.png "moster")
    <br>
2. Place the desired monsters on the map appropriately.

# Test
Let's press the **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button and run a test.
When you press the attack key, the projectile flies and explodes when it hits a monster or after a certain amount of time.

![play](https://mod-file.dn.nexoncdn.co.kr/bbs/16702265790943edc4c6330464a22afeb191890f81346.gif "play")

# Application
By changing the **SpriteRUID** of the projectiles and effects used, or by changing various properties such as damage and speed, the creator can implement the desired ranged attack. 
<br>
Refer to this guide any time to you need to create a ranged attack for your creations.