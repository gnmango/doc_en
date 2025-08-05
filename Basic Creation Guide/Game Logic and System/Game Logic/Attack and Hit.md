# Course Introduction
In MapleStory, the player becomes an adventurer that explores the Maple World and battles against monsters. During battle, the player either attacks or gets hit by monsters. Not only in MapleStory, but in various games, attacking and hitting are important concepts. In this course, we will learn how to implement attacks and hits.

##### Reference Guide/Reference
[AttackComponent](/apiReference/Components/AttackComponent{"target":"_blank"})
[HitComponent](/apiReference/Components/HitComponent{"target":"_blank"})
[Making Collision Groups]
[Entity Collision]
[Controlling Entity Status]
[Setting and Controlling Player]

# Learning About AttackComponent
Pressing the 'Ctrl' key plays the attack animation in MapleStory Worlds, but an actual attack isn't made. For the animation to also register as an attack, the <span style="color: #dc9656">**AttackComponent**</span> must be used.
A player can make various attacks in a game, and the range for each attack can be set differently. `AttackComponent` is used to create and implement attacks.

#### Setting Attack Ranges
You can use the AttackComponent's `Attack()` and `AttackFrom()` functions to set attack ranges. 

* `Attack()`: Set the attack range based on the character's position. The attack range form is set by the shape type.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16621061020831776c9b26fd14098bed69cf25a44963f.png{"width":"640px"} "1")
   
* `AttackFrom()`: Set a square attack area based on the world coordinates.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/166210611645871cd92bfcd6640b6b83e6fce31be8829.png{"width":"640px"} "2")

* `AttackFast()`: An Attack function with no return value. It can reduce the creation of unneeded table entities based on the character's position. The attack range form is set by the shape type.

#### Damage Settings
Damage can be made with the `CalcDamage()` function. Critical attack damage can be used by creating formulas using the `CalcCritical()` function. You can use the `GetCriticalDamageRate()` function to set how much more damage will be done compared to the default damage amount.

# Learning about HitComponent
A `HitComponent` is needed for an entity to get hit. `HitComponent` can set the entity's collider shape, size, and collider group.
If there is an entity that has a `HitComponent` within the valid attack range for an attack made using the `AttackComponent` function, that entity will be a hit target. `HitComponent` judges if the entity is a hit target, and determines the final damage the entity will take.

# Relationship Between Attacks and Being Hit
To create an organic attack and hit relationship, both `AttackComponent` and `HitComponent` must be used. In boxing, there are cases where opponents will dodge attacks or take minimal damage. If you wish to create a variety of outcomes even if an attack lands, each attack and hit must be implemented with the two components. 
#### Determining Attack Targets
You can use the `IsAttackTarget()` based on the creator's attack creation method. The `IsAttackTarget()` function makes it so the attacker isn't included in the attack box, so there is no need to set it so that the attacker doesn't get hit.
The **Entity** type's `defender` parameter receives inforamtion that determines if the collided entity is an attack target. If the return value is **false**, **AttackComponent** determines if it's a proper attack target and passes the decision to **HitComponent**. If you want to include the attacker as a potential target, change the return value to **true** as shown below.

``` lua
override boolean IsAttackTarget(Entity defender)
{
    return true
}
```

 Attacks and hits function in the following way. First the attacking entity's `IsAttackTarget()` function for `AttackComponent` returns **false** if the attack fails and **true** if the attack is a success. **false** ends the process due to attack failure. If you want to guarantee a hit, it can be changed to **true**. In this case, `IsHitTarget()` won't be called and will always deal damage.

The target's `IsHitTarget()` function for `HitComponent` detemines if the attack landed. `HitComponent` calculates the target's evasion etc and the final damage amount. If the target's `IsHitTarget()` returns **true**, the damage is applied to the target.
![14](https://mod-file.dn.nexoncdn.co.kr/bbs/16621062233862d80a40f725a44b2bd383f597906dca9.png "14")

#### Damage Calculation
The damage value uses the attacker's `CalcDamage()` function for **AttackComponent**. This function returns attack damage.
The target's **HitComponent** `OnHit()` function can receive the `CalcDamage()` value as a parameter. The value can then be used as-is or can undergo additional calculations from the `OnHit()` function to determine the final damage value. For example, the attacker's damage and defender's evasion can be calculated by the `OnHit()` function to produce the final damage as a return value.

![15](https://mod-file.dn.nexoncdn.co.kr/bbs/1662106244457dbfb15dbce134b3c8acc56b4dd23dfc8.png "15")

#### HitEvent
The `OnHit()` function triggers a **HitEvent** when called. If you want to make a special process when one is attacked by monsters 100 times, you can check the event trigger count to implment it.
![16](https://mod-file.dn.nexoncdn.co.kr/bbs/166210626375732ffa55d90e0452ab96703bf0137e84a.png "16")

# Example
Let's make it so that someone can attack monsters by pressing the `Ctrl` key, and for monsters to increase in size by a set ratio each time they're hit.

#### Implementing Player Attacks
1. Select <span style="color: #dc9656">**Workspace - BaseEnvironment - NativeScripts - Component - AttackComponent**</span>.
    ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1687765469788eef28e43cf6e478aadce01fbf58c566b.png "1")
2. Expand the component by pressing **Extend** in the context menu. Change the expanded component's name to <span style="color: #dc9656">**CharacterAttack**</span>. 
3. Double-click the **CharacterAttack** compoonent to open the script editor. 
4. Add a new function from **Method** and change the name to <span style="color: #dc9656">**NormalAttack**</span>.
5. Define the attack range that will be used from the `Attack` function and write the following so it can recieve it as a parameter.

	```lua
    Method:
	void NormalAttack()
	{
		local attackSize = Vector2(1, 1)
		local attackOffset = Vector2(0,0)
		
		self:Attack(attackSize, attackOffset, nil)
	}
	```
6. Add **PlayerActionEvent** to **Event Handler** so it can receive a player's action change. Call the `NormalAttack()` function each time the player presses the `ctrl` key.

	```lua
	Event Handler:
	[self]
	HandlePlayerActionEvent(PlayerActionEvent event) 
	{
		--------------- Native Event Sender Info ----------------
		-- Sender: PlayerControllerComponent
		-- Space: Server, Client
		---------------------------------------------------------

		-- Parameters
		local ActionName = event.ActionName
		-- local PlayerEntity = event.PlayerEntity
		---------------------------------------------------------
				
		if ActionName == "Attack" then
			self:NormalAttack()
		end
	}
    ```

    > <span style="color: #7cafc2">**Tip**
    > You can undo the `local ActionName = event.ActionName` annotation under Parameters for use.</span>

7. Add the **CharacterAttack** component to **DefaultPlayer**.
8. Place the monster on the map and press [Start] to see if the monster can be attacked.


# Calculating Attack Damage
Use the `CalcDamage()` function for `AttackComponent` to calculate and output the damage value of an attack. This function returns damage when a collision is detected.

1. Add the `CalcDamage()` function to the **CharacterAttack** component.
    ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1635473274032dd346b758c4743be834dcc36af950302.png "6")

2. Set the attack damage value as 100 like below. 
   
	```lua
	override int CalcDamage(Entity attacker, Entity defender, string attackInfo)
	{
		return 100
	}
	```   
3. Press [Start] and attack the monster to see if **100** damage is triggered.

# Implementing Monster Hits
 When an entity with an **AttackComponent** attacks an entity with a **HitComponent**, the entity is hit without the need of a separate process. Since there is no additional processing, the damage skin plays with the default value of 1.
Damage can be dealt equal to the attack when implementing a hit, but the damage taken can be calculated by using formulas other than the attack. For example, an attack can deal 100 damage, but if the monster's defense is high, it can be made to deal half damage.
This time, let's implement the two examples below and see if they register hits and their final damage dealt.

* Change the damage dealt from the attack to 20.
* The size increases with each attack.

1. Select **Workspace - BaseEnvironment - NativeScripts - HitComponent** and select **Context Menu- Extend** to expand the component.
2. Change the name to <span style="color: #dc9656">**MonsterHit**</span>.
![19-2](https://mod-file.dn.nexoncdn.co.kr/bbs/16353330342954504d937277b4fada89371a5e3da7faa.png "19-2")

3. Select the **MoveMonster** model from **Workspace - BaseEnvironment - NativeModel**.
![17](https://mod-file.dn.nexoncdn.co.kr/bbs/1635473322027345d4d4d82df42d283fe2d7c9ef91747.png "17")

4. Delete **HitComponent** from the model by pressing **Context Menu - ![Common_menu](https://mod-file.dn.nexoncdn.co.kr/bbs/163453706197553d527cb3ea34392bc2829f15976f3d8.png "Common_menu") - Remove Component** under **HitComponent** from the property editor. This is because using the expanded HitComponent with the default HitComponent may cause an error.
    ![18](https://mod-file.dn.nexoncdn.co.kr/bbs/16353328810860b81ed55939d474aaaea5cd238480c92.png "18")

    > <span style="color: #7cafc2">**Tip**
    > Monsters already placed on the map don't have model property changes applied to them, so they must be deleted and placed again.</span>

5. Add the **MonsterHit** component to the placed monster.
6. Add the `OnHit()` function to the **MonsterHit** component.
    ![20](https://mod-file.dn.nexoncdn.co.kr/bbs/163547333521453f7d5d7ff6a4e20bd2f5c20a78ea254.png "20")
7. Change the **damage** value from the default text within the function to <span style="color: #dc9656">**20**</span>. This determines the damage value of the target.

	```lua
	method:
	override void OnHit(Entity attacker, int damage, boolean isCritical, string attackInfo, int hitCount)
	{
		__base:OnHit(attacker,20,isCritical,attackInfo,hitCount)
	}
	```

8. Add the following to the `OnHit()` function so that the monster that is hit gets larger. Increase the **TransformComponent.Scale** value by 1.3 times each time a **HitEvent** is triggered.
    
	```lua
	method:
	override void OnHit(Entity attacker, int damage, boolean isCritical, string attackInfo, int hitCount)
	{
		__base:OnHit(attacker,20,isCritical,attackInfo,hitCount)

		local scale = self.Entity.TransformComponent.Scale
		scale = scale * 1.3
		self.Entity.TransformComponent.Scale = scale
	}
	```
9. Press [Start] to see if **20** is output as the damage value and if the monster gets larger each time it is attacked.
![22](https://mod-file.dn.nexoncdn.co.kr/bbs/1662107300698572fbabb48b54d5d92f06e1ae6487794.png{"width":"640px"} "22")