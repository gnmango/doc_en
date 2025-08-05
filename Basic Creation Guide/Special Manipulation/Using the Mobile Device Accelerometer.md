# Course Introduction
You can check whether the acceleration sensor function in mobile devices is supported, and measure values by utilizing `MobileAccelerometerService`.

# MobileAccelerometerService
You can measure the slope of mobile devices by utilizing `MobileAccelerometerService`. MobileAccelerometerService is only supported for mobile devices with an accelerometer. Therefore, it is necessary to check whether the user who entered the World can play properly and provide a guide if the player's device is not compatible.

```lua
if _MobileAccelerometerService:IsHWSupported() then
    if _MobileAccelerometerService:IsEnabled() then
        _MobileAccelerometerService:Stop() 
    else
        _MobileAccelerometerService:Start()
    end
else
    _UIToast:ShowMessage("Failed to start Accelerometer")
end
```


# Example
Let's make a ball move in the direction the mobile device is tilted. 

![AccExample](https://mod-file.dn.nexoncdn.co.kr/bbs/1669116956573f5e430d1d6b546baa73dab2a0ecd76f5.gif "AccExample")
#### Making a Wall
1. Select **Switch to RecTileMap** in the context menu from **Hierarchy - World - maps - map01**.
2. Add **PhysicsSimulatorComponent** to **Hierarchy → World → maps → map01**.
3. Generate a new <span style="color: #dc9656">**Wall**</span> entity and add **TransformComponent, SpriteRendererComponent, and CustomeFootholdComponent**.
4. Enter a wall-shaped RUID to SpriteRendererComponent → SpriteRUID and create the foothold as long as the sprite with **CustomFootholdComponent**.
    * SpriteRUID: 9b518d2cf3a549f4bdbbac8662fecd42

5. Add **PhysicsColliderComponent** and adjust the collision area to match the size of the sprite.
6. Add **PhysicsRigidbodyComponent** and set properties.

    * BodyType: Static
    * Friction: 0.2
    * Restitution 0.5
    * CollisionDetectionMode: Continuous

7. Create a rectangular enclosure by copying three created walls.

#### Making a Ball
1. Generate a new <span style="color: #dc9656">**TiltBall**</span> entity and add **TransformComponent and SpriteRendererComponent**.
2. Enter a ball-shaped RUID into SpriteRendererComponent → SpriteRUID.
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
    
 
#### Accelerometer
1. Create a new <span style="color: #dc9656">**AccelerometerLogic**</span>. 
2. Add Property and specify TiltBall to BallEntityRef.
    ```lua
    Property:
    [None]
    EntityRef BallEntityRef = /maps/map01/TiltBall
    [None]
    number ForcePower = 100
    ```

3. Start acceleration measurement after checking whether the device supports acceleration sensor by utilizing `MobileAccelerometerService:IsHWSupported()`.
    ```lua
    Method:
    [clinet only]
    void OnBeginPlay ()
    {
        if _MobileAccelerometerService:IsHWSupported() then
        	_MobileAccelerometerService:Start()
        end
    }
    ```
4. To measure the acceleration and move the ball using the device's tilt, write the following.
    ```lua
    void OnUpdate (number delta)
    {
        if _MobileAccelerometerService:IsEnabled() then
        	local AccelerationDirection = _MobileAccelerometerService:GetLastAcceleration()
        	local direction = Vector2(AccelerationDirection.x, AccelerationDirection.y)
        	self:Tilt(direction)
        end
    }
    
    [server]
    void Tilt (Vector2 direction)
    {
        self.BallEntityRef.PhysicsRigidbodyComponent:ApplyForce(direction * self.ForcePower)
    }
    ```

##### Reference Guide
* [Making Footholds]
* [Utilizing RectTileMap]
* [Using Physics]