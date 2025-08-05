# Course Introduction
You can check whether it supports the gyroscope sensor function in mobile devices and measure values by utilizing `MobileGyroscopeService`.

# MobileGyroscopeService
By using `MobileGyroscopeService`, you can learn where and how much force the device received when it moved with measurement of gravity, acceleration of gravity, and rotary acceleration of mobile devices. For example, you can use this service to measure how much the cellphone moved upward when you toss it up.

`MobileGyroScopeService` can be tested only on mobile devices. Check whether a device supports gyroscope sensor. If it does not, you'll need a guide.
```lua
void GyroscopeCheck (boolean onoff)
{
    if _MobileGyroscopeService:IsHWSupported() then
    	if _MobileGyroscopeService:IsEnabled() then
    		_MobileGyroscopeService:StopAndWait()
    	else
    		_MobileGyroscopeService:StartAndWait()	
    	end
    else
    	_UIToast:ShowMessage("Failed to start gyroscope")
    end
}
```


# Usage example
Let's make the ball move in the direction force is applied when a mobile device is tilted in a certain direction.
![GyroExample](https://mod-file.dn.nexoncdn.co.kr/bbs/1669112286682bf20fdee201444148bdcf22b3e9c2a8c.gif "GyroExample")

#### Making a Wall
1. Select **Switch to RecTileMap** in the context menu from **Hierarchy - World - maps - map01**.
2. Add **PhysicsSimulatorComponent** to **Hierarchy - World - maps - map01**.
3. Generate a new <span style="color: #dc9656">**Wall**</span> Entity, and add **TransformComponent, SpriteRendererComponent, and CustomeFootholdComponent**.
4. Enter a wall-shaped RUID to SpriteRendererComponent - SpriteRUID and create the foothold as long as the sprite with **CustomFootholdComponent**.
    * SpriteRUID: 9b518d2cf3a549f4bdbbac8662fecd42

5. Add **PhysicsColliderComponent** and adjust the collision area to match the size of the sprite.
6. Add **PhysicsRigidbodyComponent** and set properties.

    * BodyType: Static
    * Friction: 0.5
    * Restitution 0.2
    * CollisionDetectionMode: Continuous

7. Create a rectangular enclosure by copying 3 created walls.

#### Making a Ball
1. Generate a new <span style="color: #dc9656">**TiltBall**</span> Entity and add **TransformComponent and SpriteRendererComponent**.
2. Enter a ball-shaped RUID into SpriteRendererComponent - SpriteRUID.
    * SpriteRUID: 777ee92d25c84f839a12739590d149b6

3. Add **PhysicsColliderComponent** to TiltBall and adjust the collision area to match the sprite after setting **ColliderType** to **Circle**.
4. Add **PhysicsRigidbodyComponent** and set the property as follows.

    * BodyType: Dynamic
    * Mass: 1
    * Friction: 0.2
    * Restitution: 1
    * CollisionDetectionMode: Discrete
    * GravityScale: 1
    * SleepingMode: StartAwake

#### Making a Ball Move
1. Create a new <span style="color: #dc9656">**GyroscopeLogic**</span>.
2. Add Property and specify TiltBall to BallEntityRef.

    ```lua
    Property:
    [None]
    EntityRef BallEntityRef = /maps/map01/TiltBall
    [None]
    number ForcePower = 100
    ```

3. Check whether a device supports gyroscope sensors and start value measurement by utilizing `MobileGyroscopeService:IsHWSupported()`.

    ```lua
    [client only]
    void OnBeginPlay
    {
        if _MobileGyroscopeService:IsHWSupported() then
        	local enabled = _MobileGyroscopeService:StartAndWait()
        	if enabled then
        		_UIToast:ShowMessage("Gyroscope Started!")
        	else
        		log_error("Failed to start gyroscope")
        	end
        end
    }
    ```

4. Write as follows so that it moves according to the force the device received and its direction.

    ```lua
    void OnUpdate (number delta)
    {
        if _MobileGyroscopeService:IsEnabled() then
        	local userAcceleration = _MobileGyroscopeService:GetUserAcceleration()
        	local userAccelDir = Vector2(userAcceleration.x, userAcceleration.y)
        	self:Tilt(userAccelDir)
        end
    }
    
    [server]
    void Tilt (Vector2 direction)
    {
        if direction.x > 0.2 or direction.y > 0.2 then
        	self.BallEntityRef.PhysicsRigidbodyComponent:ApplyForce(direction * self.ForcePower)	
        end
    }
    ```
  5. Release World, and check it on mobile.

##### Reference Guide
* [Making Footholds]
* [Utilization of RectTileMap]
* [Using Physics]