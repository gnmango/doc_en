# Course Introduction
In this course, we'll learn about the major functions of MovementComponent, including jumping, player movement, setting entity coordinates, and the basic mechanisms of MovementComponent.
# Introduce MovementComponent
<span style="color: #dc9656">**MovementComponent**</span> offers various features, setting characters' movement speed and jump force and forcing an entity with RigidbodyComponent to jump, move, or stop.
You can dynamically move an entity by controlling TransformComponent in a script. However, if the entity has RigidbodyComponent, it might not move as you intended as RigidbodyComponent also has control over TransformComponent.
MovementComponent checks if an entity has RigidbodyComponent and controls TransformComponent or RigidbodyComponent to move the entity. Therefore, the creator can just use MovementComponent to control entity movement without having to check for the existence of RigidbodyComponent.

> <span style="color: #7cafc2">**Tip**
> You can control **KinematicbodyComponent** and **SideviewbodyComponent** in the same way.</span>

# About Player Movement
#### Setting Player Movement Speed
You can use <span style="color: #dc9656">**InputSpeed**</span> to set players' movement speed.
The property determines the amount of power to move the entity upon pressing the key (arrow key), only valid for player entities. The bigger the input value, the faster the player entity.

| InputSpeed = 1.2 | InputSpeed = 3 | InputSpeed = 5 |
| ---------------- | -------------- | -------------- |
| ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16560435039321a16b7ea2c7148098f921cc915250d8e.gif "1") | ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/165604352590254034248ce564039be047668cf8da65b.gif "2") | ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1656043549528b8d89f6ae69347248beb409c73e868e0.gif "3") |
#### Checking Pause on Ladder
You can use the <span style="color: #dc9656"> **IsClimbPaused**</span> property to check if the player stopped moving while climbing the ladder. The property checks if the entity is in pause status while on a ladder or rope, only valid for player entities. IsClimbPaused is a boolean type, and it can get true when the player entity paused while climbing the ladder. Since it is a ReadOnly property, it can only be read.
```lua
[server only]
void OnUpdate(number delta)
{
    local isClimbPaused = self.Entity.MovementComponent.IsClimbPaused
    log(isClimbPaused)
}
```
![isclimbpaused](https://mod-file.dn.nexoncdn.co.kr/bbs/1656043847301b2708b6a8c8740c5b3fd687fee8dd93a.gif "isclimbpaused")
#### Confirming the Direction the Player Faces
You can use the <span style="color: #dc9656">**IsFaceLeft**</span> function to check the direction the player is facing. The function is only for player entities, and it returns true if the player entity looks to the left, and false if not.
```lua
[serive: InputService]
HandleKeyUpEvnet(KeyUpEvent event)
{
	-- Parameters
	local key = event.key
	--------------------------------------------------------
	--Outputs true on the console window if the player is facing left whenever pressing the left and right arrow keys.
	if key == KeyboardKey.LeftArrow or key == KeyboardKey.RightArrow then
		log(self.Entity.MovementComponent:IsFaceLeft())
	end
}
```
![isfaceleft](https://mod-file.dn.nexoncdn.co.kr/bbs/1656043891044e47a017f5f0746648ec1081bf5829c3c.gif "isfaceleft")
# About Entity Jump
#### Entity Jump
MovementComponent's <span style="color: #dc9656">**Jump**</span> function makes an entity jump according to the power input in JumpForce. As a boolean type, it returns true if the entity jumped. It requires RigidbodyComponent in the entity to work properly. It returns false if the entity does not have RigidbodyComponent. In general, you are recommended to call it in server for synchronization, but in case of a player entity, it should be called from client.
```lua
[server only]
void OnUpdate(number delta)
{
    -- In case of a player entity, change the execution control space to client or client only.
	if self._T.accTime == nil then 
		self._T.accTime = 0 
	end
	
	self._T.accTime = self._T.accTime + delta
	
	--Jumps entity every 3 seconds.
	if self._T.accTime >= 3 then
		self.Entity.MovementComponent:Jump()
		self._T.accTime = 0
	end
}
```
![jump](https://mod-file.dn.nexoncdn.co.kr/bbs/1656043915462a19ef1f74b3a4006ba2e0f38f492f7d1.gif "jump")
#### Setting Jump Force
<span style="color: #dc9656">**JumpForce**</span> property determines how high an entity can jump. JumpForce value is reflected when MovementComponent's Jump function is called.

| JumpForce = 1 | JumpForce = 3 | JumpForce = 5 |
| ------------- | ------------- | ------------- |
| ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/16560437341534e08dd0a7a2d4eeba225a5b7b7e99282.gif "4") | ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1656043756640d0205b1f8b7f4b5f95a941c61ca310f7.gif "5") | ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/16560437769096cd53f7210b74a1f8a08694f05ddeb9b.gif "6") |
#### Down Jump
<span style="color: #dc9656">**DownJump**</span> function moves an entity to the foothold below, if there is another foothold below the one the entity is standing on.
It requires RigidbodyComponent to work properly. It is ignored if there is no RigidbodyComponent. The return type is boolean, returning true if entity moved successfully. If you add the component below to an entity with MovementComponent and RigidbodyComponent, you'll see the entity going down to the below foothold every 3 seconds. Just like Jump, in case of a player entity, you have to call the function from client.

```lua
[server only]
void OnUpdate(number delta) 
{
    -- In case of a player entity, change the execution control space to client or client only.
	if self._T.accTime == nil then 
		self._T.accTime = 0 
	end
	
	self._T.accTime = self._T.accTime + delta
	
	--Down-jumps entity every 3 seconds.
	if self._T.accTime >= 3 then
		self.Entity.MovementComponent:DownJump()
		self._T.accTime = 0
	end
}
```
![downjump](https://mod-file.dn.nexoncdn.co.kr/bbs/165604394157048172dcceb554c5f99f7bca5f20aca41.gif "downjump")
> <span style="color: #585858">**Learn More**
> Unlike components in other entities, player entity's RigidbodyComponent is synchronized from client to server. It is best to control RigidbodyComponent in client. </span>
# Entity Movement
#### Entity Movement by Coordinates
MovementComponent uses SetPosition function to set coordinates for entities.
The `SetPosition()` function sets the position of TransformComponent by default, but if it includes the RigidbodyComponent entity, sets the position through RigidbodyComponent. It takes the Vector2 value as a parameter. The entity will move to the coordinates corresponding to this value.
```lua
[server only]	
void OnUpdate(number delta)
{
    local deltaX = 1
    local pos = self.Entity.TransformComponent.Position
    pos.x = pos.x + deltaX * delta
    local deltaPos = Vector2(pos.x, pos.y)
    self.Entity.MovementComponent:SetPosition(deltaPos)
}
```
![setposition](https://mod-file.dn.nexoncdn.co.kr/bbs/16560439621203295e712472d457299cb202c757c42ef.gif "setposition")