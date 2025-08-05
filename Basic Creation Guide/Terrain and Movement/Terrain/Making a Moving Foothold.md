# Course Introduction

Let's make a moving foothold to add more fun.
You can use CustomFootholdComponent's IsDynamicFoothold property to create a moving foothold of your own.
Please refer to [Making Foothold] to learn the basics of creating footholds.

# Making a Moving Foothold
To make a moving foothold, you have to create one using CustomFootholdComponent. 
To make the basic CustomFootholdComponent move, you have to adjust the properties of IsDynamicFoothold and RigidbodyMovementOption.

| Property Name | Description |
| ------ | --- |
| IsDynamicFoothold | Activate to make a moving foothold. |
| RigidbodyMovementOption | Set what kind of inertia will be applied to player entity standing on the moving foothold.<ul><li>RelativeX: Apply inertia in x axis, not in y axis. If player moves in the opposite direction of the foothold, speed decreases. If it moves in the same direction, speed increases. </li><li>Absolute: Constant speed regardless of direction.</li></ul> |

| RelativeX  | Absolute |
| --- | --- |
| ![reletiveX](https://mod-file.dn.nexoncdn.co.kr/bbs/16466307010962640bc0dca2b4ba28d72e8138ee4a5cf.gif "reletiveX")| ![absolute](https://mod-file.dn.nexoncdn.co.kr/bbs/1646633877812d9fd5ab7b34d468abc8018984875cdf5.gif "absolute") |

#### Use Example
1. Add CustomFootholdComponent to the object you'll use as a foothold to create a foothold.
2. Activate CumstomFootholdComponent - <span style="color: #dc9656">**IsDynamicFoothold**</span>.
3. Select CumstomFootholdComponent, and set <span style="color: #dc9656">**RigidbodyMovementOption to RelativeX**</span>.<br>![01](https://mod-file.dn.nexoncdn.co.kr/bbs/16820538852880d9d1813790d4636b5b483c6f0b7e885.png "01")
4. Add a new component and add it to the foothold object.
5. Open the Script Editor of the new component, and write the following to set the time and range of movement.
     If you only put x value for `TransformComponent:Translate(0,0)`, the movement will be horizontal, and vertical if only a y value is entered. Entering both values will make the movement diagonal. 

```lua
Property:
[Sync]
number totalTime = 0
    
Method:
[Server only]
void OnUpdate(number delta)
{
    self.totalTime = self.totalTime + delta
    if self.totalTime > 0 and self.totalTime <= 5.0 then
        self.Entity.TransformComponent:Translate(0,0.009)
    elseif self.totalTime > 5.0 and self.totalTime <= 10.0 then
        self.Entity.TransformComponent:Translate(0,-0.009)
    elseif self.totalTime > 10 then
        self.totalTime = 0
    end
}
```