# Course Introduction
You can use [TweenLogic](/apiReference?postId=700{"target":"_self"}) to enlarge, reduce, rotate, and move entities. See [Tweener](/apiReference?postId=714{"target":"_self"}) to learn more.

# TweenLogic Concept
**TweenLogic** provides various functions to give animation effects on an entity. You can change the location and size of an entity or rotate it over several frames.
You can make the entity Tween using various functions and produce an animation by calling the function once.
#### A Difference with TweenComponent
TweenLogic and TweenComponents seem like a similar action in the World, however, they are different functions. You must select what is suitable for the creator's production plan.
 **TweenLineComponent, TweenCircularComponent and TweenFloatingComponent** are recommended for the times you wish to give animation effects to an entity without writing a script. Since three components are optimized for the network, we can expect performance improvement when we make the multiple entities Tween in a multi-player environment.
 **TweenLogic** can be used for Dynamic Tweens. The Tweener entities that TweenLogic function returns include Reverse and Delay features. If you terminate a Tween using the SetOnEndCallback function, you can define the operation to run once it's done.

#### EaseType
When you use the TweenLogic function, you can decide the movement shape by setting up **EaseType**. You can select the desirable type from EaseType and create different movements for each entity. If you used each different EaseType, even though there are two entities toward the same location for a constant time, some entities would bounce similarly to a ball as their movement. Some other entities will start to move slowly and approach the destination faster.
For example, the `MoveTo(Entity entity, Vector2 destination, number duration, and  EaseTypetype)` function of TweenLogic assigns a certain point as a destination and moves the entity. Once you call the function, it gradually moves to the destination through several frames and arrives. At this time, the entity's movement shape will differ depending on whether <span style="color: #dc9656">**EaseType**</span> is used.
EaseType needs to understand the Easing function. The Easing function's x-axis refers to time, and the y-axis refers to movement location. You can find out the x and y axis's origins and end values by assuming the entity moves from (1,1) to (2,2) for 3 seconds. The x's origin value will be 0, and its end value will be 3. The y-axis is the movement location, so the origin value will be (1,1) and the end value will be (3,3). During this time, it moves in different shapes depending on the function, and the acceleration changes will also differ.
Please find the reference material in [Entity Section Movement](/docs/?postId=122{"target":"_self"}) for the Easing function graph.

#### Ease Function
The <span style="color: #dc9656">**Ease function**</span> does not create Tweener entities, and it is not automatically updated for each frame. So, if there is a movement required, the creator should directly call the `Ease()` function for each frame. In contrast, the Tweener entity created with `MakeTween()` to call Tweener's `Play()` function, or the Tweener entity created with `PlayTween()` to play automatically, is automatically updated for each frame by TweenLogic. 

You can use the function below to figure out the return value from a certain progress rate. The x-axis in the Easing function graph is a value that divides tweenTime by duration with Tweener's progress rate. The y-axis is a return value between startValue and endValue. 

```lua
number Ease(number startValue, number endValue, number duration, EaseType type, number tweenTime)
```

# Use TweenLogic
#### Move
You can move entities by using `MoveTo()`.
![Movement](https://mod-file.dn.nexoncdn.co.kr/bbs/1657616366795ce5a3fc175fa498e8e3a9a096db18941.gif "Movement")

1. Create a new component and add it to a certain entity.
2. You write destination, time, and EaseType for the entity to move.
3. You write together whether you will process the returning Tweener's delay time, repetition type, number of times, and automatic destroy.
If you set automatic destroy to false or set LoopCount to -1 to keep executing the tween, you may unintentionally accumulate Tweener entities that no longer function. Having too many of these Tweener entities can cause performance degradation in the World, so tweens need to be destroyed at the right time. We recommend destroying the component by calling the Tweener's Destroy() function in OnEndPlay of the component or when the target entity to be tween is destroyed.
    ```lua
    Property:
    [None]
    any tween = nil
    [Sync]
    number LoopCount = 0
    [Sync]
    number Delay = 0
    
    Method:
    [server only]
    void OnBeginPlay()
    {
        self.tween = _TweenLogic:MoveTo(self.Entity, Vector2(0.5, self.Entity.TransformComponent.Position.y), 4, EaseType.BounceEaseOut)
        self.tween.Delay = self.Delay
        self.tween.LoopCount = self.LoopCount
        self.tween.LoopType = TweenLoopType.PingPong
        self.tween.AutoDestroy = false
    }
    
    [server only]
    void OnEndPlay()
    {
        self.tween:Destroy()
    }
    ```
#### Rotate
You can make the entity rotate using `RotateTo()` and make it rotate based on a certain entity's offset using `TweenLogic:RotateAroundOffset()`. 

1. Create a new component and add it to a certain entity.
2. Set **angle** as 360 degrees and write to make the entity rotate once.
    ```lua
    Method:
    void OnBeginPlay()
    {
        self._T.tween = _TweenLogic:RotateTo(self.Entity, 360, 5, EaseType.Linear)
        self._T.tween.LoopCount = -1 
    }
       
    [server only]
    void OnEndPlay()
    {
        self._T.tween:Destroy()
    }
    ```
3. Create a component and add it to a certain entity.
4. Add new **RotateCenter** to the property and select an entity to set as a centerpoint on the property editor window.
5. Write as below to make it rotate around the center entity.
    ```lua
    Property:
    [Sync]
    Entity RotateCenter = nil
    
    Method:
    [server only]
    void OnBeginPlay()
    {
        if self.RotateCenter == nil then
            do return end
        end
            
        local offset = self.RotateCenter.TransformComponent.Position - self.Entity.TransformComponent.Position
        self._T.tween = _TweenLogic:RotateAroundOffset(self.Entity, 360, offset:ToVector2(), true, 5, EaseType.Linear)
        self._T.tween.LoopCount = -1
    }
    
    [server only]
    void OnEndPlay()
    {
        self._T.tween:Destroy()
    }
    ```

#### Change Entity Size
You can make an entity's size large or small using `ScaleTo()`.

![Size Adjustment](https://mod-file.dn.nexoncdn.co.kr/bbs/1657616386668608a20252d60459599b9d78530594d46.gif "Size Adjustment")

1. Create a new component and add it to a certain entity.
2. Enter the entity size you want to enlarge or reduce as `Vector2()` value.
    ```lua
    Method:
    [server only]
    void OnBeginPlay()
    {
        local tween = _TweenLogic:ScaleTo(self.Entity, Vector2(2.5, 2.5), 4, EaseType.QuadEaseInOut)
        tween.LoopCount = -1
        tween.LoopType = TweenLoopType.PingPong
    }
    ```

#### Adjust Transparency
You can adjust the **Alpha** value from `MakeTween()` to make a certain entity fade.

![Alpha](https://mod-file.dn.nexoncdn.co.kr/bbs/16576164032874b4be3d0e5384da9a8858e5d8eacc361.gif "Alpha")

1. Create a new component and add it to a certain entity.
2. Designate the startvalue of MakeTween() as a SpriteRenderer and write as below, designating the endvalue as 0 to make the entity transparent.

    ```lua
    Method:
    [server only]
    void OnBeginPlay()
    {
        local spriteRenderer = self.Entity.SpriteRendererComponent
        local tween -- Pre-declare the variable to assign the tweener.
        local tweenAlpha = function(tweenValue)
            if isvalid(spriteRenderer) == false then
                tween:Destroy()
            end
            spriteRenderer.Color.a = tweenValue
        end
         
        tween = _TweenLogic:MakeTween(spriteRenderer.Color.a, 0, 0.5, EaseType.Linear, tweenAlpha)
        tween.LoopCount = -1
        tween.LoopType = TweenLoopType.PingPong
        tween.AutoDestroy = false
        tween:Play()
         
        _TimerService:SetTimerOnce(function() self.Entity:RemoveComponent(SpriteRendererComponent) end, 5)
    }
    ```

# Example of Use
Let's make a ball bounce forward after throwing it from a high place.

![ball](https://mod-file.dn.nexoncdn.co.kr/bbs/165761643870906375319aaf64fcaa084c94c11f7b1b2.gif "ball")

1. Create a new ![model](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_model_no.png "model") Model_Ball model. Add ![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/Ect.png "component") TransformComponent and ![sprite](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/sprite.png "sprite") SpriteRendererComponent and use Ruid below.
    * SpriteRuid: d4dddd1f07d445939433f70cd1aa82fa

2. Create a new ![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/Ect.png "component") BallComponent, and add it to ![model](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_model_no.png "model") Model Ball.
3. To make a ball fly away while rotating, write as below using PlayTween() and RotateTo() together.

    ```lua
    Property:
    [None]
    number posX = 0
    [None]
    number posY = 0
    
    Method:
    [server only]
    void OnBeginPlay()
    {
        local transform = self.Entity.TransformComponent
        self.posX = transform.Position.x
        self.posY = transform.Position.y
    
        local yDestination = 0.3
        local tweenDuration = 3
    
        local xTween = _TweenLogic:PlayTween(self.posX, self.posX + _UtilLogic:RandomIntegerRange(40, 100)/10, tweenDuration, EaseType.CubicEaseOut, function(val) self.posX = val end)
        local yTween1 = _TweenLogic:PlayTween(self.posY, yDestination, tweenDuration, EaseType.BounceEaseOut, function(val) self.posY = val end)
    
        local rotateTween = _TweenLogic:RotateTo(self.Entity, -1800 * _UtilLogic:RandomIntegerRange(95, 105)/100, tweenDuration, EaseType.QuartEaseOut)
    }
    
    [server only]
    void OnUpdate(number delta)
    {
        --  Assign the posX and PosY property values, which change for each frame, to TransformComponent.Position
        local transform = self.Entity.TransformComponent
        transform.Position = Vector3(self.posX, self.posY, transform.Position.z)
    }
    ```

     In the example above, `PlayTween()` receives Action <number> tweenFunction as its last parameter. tweenFunction is called each frame and receives the Ease result values as the parameter once Tween starts. If 1.2 seconds passes after the Tween starts, the Ease result value will be Ease (self.posY, yDestination, tweenDuration, EaseType.BounceEaseOut, 1.2). So, `function(val) self.posX = val end` receives the result value(val) of Ease function to assign it to self.posY property.
     
    > <span style="color: #b8b8b8">**Learn more**
    > If you call PlayTween twice, which receives Vector2 as the parameter when using PlayTween, and then implement as below to make one control x and another one to control y, it will not work properly. Vector2 uses x and y as a pair because it overwrites the Tween return value.</span>
    >```lua 
    > -- Assume throwing a ball to a location(8, 0, 0)
    >_TweenLogic:PlayTween(current location, Vector3(8, 0, 0), tweenDuration, EaseType.CubicEaseOut, function(val) self.Entity.TransformComponent.Position = val end)
    >_TweenLogic:PlayTween(current location, Vector3(8, 0, 0), tweenDuration, EaseType.BounceEaseOut, function(val) self.Entity.TransformComponent.Position = val end)
    >```


3. Create a ![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_component_no.png "component") SpawnBallOnAttack component and add it to ![player](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_myavatar_no.png "player") DefaultPlayer. When a player attacks as below, write to make it summon ![model](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_model_no.png "model") Model_Ball.
    ```lua
    Event Handler:
    [server only] [self]
    HandlePlayerActionEvent(PlayerActionEvent event)
    {
        -- Parameters
        local ActionName = event.ActionName
        local PlayerEntity = event.PlayerEntity
        --------------------------------------------------------
        if ActionName == "Attack" then
        	local parent = self.Entity.Parent
        	local spawnPosition = self.Entity.TransformComponent.Position + Vector3(0, 0.5, 0)
        	_SpawnService:SpawnByModelId("model://8474ac05-aac0-49bf-a171-84b7d8b9f42c", "Ball", spawnPosition, parent)
        end
    }
    ```

##### Reference Guide
* [Entity Section Movement](/docs/?postId=122{"target":"_self"})
* [Changing Location, Size, Rotation of Entities](/docs/?postId=82{"target":"_self"})
* [Object Movement Ⅰ](/docs/?postId=123{"target":"_self"})