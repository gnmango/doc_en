# Course Introduction
Let's learn how to use `FastVector3` and how to utilize it for world optimization.
# Fast Type
The `FastVector2`, `FastVector3`, and `FastColor` types are written with the Lua Table, enabling it to supplement `Vector2`, `Vector3`, and `Color`. Both types offer the same function, but types with the Fast prefix can be used to optimize worlds when there is an overload.

# FastVector3 Characteristic
FastVector3 has the following 5 characteristics.

1. `FastVector3` is implemented as a **Lua Table**, so it is lighter and more flexible compared to `Vector3`.
    ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/174401387084131f34bede9bb4db18195d9de68fcd798.png{"width":"1080px"} "FastVector3")
2. `FastVector3` has all of Vector3's properties and functions, and supports an auto conversion to Vector3.
3. `FastVector3` **doesn't support synchronization.** So it can't be used as a parameter for an execution function in another space.
4. `FastVector3` uses the **8 Byte real world number values**, and `Vector3` uses 4 Byte real world number values. This can cause in different valid ranges and operation results between the two types.
5. `FastVector3` references the original when approaching static properties such as zero and forward, while Vextor 3 returns copied values.

# Using FastVector3

#### Changing Generators
You can change the `Vector3` generator to the `FastVector3` generator. The more calculations, the more the difference in performance between `FastVector3` and `Vector3`.

Vector3 is used in the following way.
```lua
-- When using Vector3
local newPosition = Vector3(0, 0, 0)
newPosition.x = 1
newPosition.y = 2
self.Entity.TransformComponent.Position =  newPosition
```

FastVector3 is used in the following way.
```lua
local newPositionFast = FastVector3(0, 0, 0)
newPositionFast.x = 1
newPositionFast.y = 2
self.Entity.TransformComponent.Position = newPositionFast
```

#### Using the Static Property
You must be cautious when using the Static property for `FastVector3`. Because `FastVector3` **returns the reference to the original when accessing the Static property**, when directly accessing the the Static property, all of the values in that Static property may be changed.
The `Clone()` function must be used to copy the values as shown below.

```lua
local vt = FastVector3.zero:Clone() 
vt.x = 1
```

`FastVector3.zero` will change when the property value of `FastVector3.zero` is changed for `FastVector3`.

```lua
local vt = FastVector3.zero
vt.x = 1 -- FastVector3.zero : FastVector(1,0,0)
```

`Vector3.zero` won't change even if the propery value of `Vector3.zero` changes for `Vector3` like below.

```lua
local vt = Vector3.zero
vt.x = 1 -- Vector3.zero : Vector(0,0,0)
```

#### Setting Parameter Types
`FastVector3` can't be set as a **property type or function variable**. So when using `FastVector3` as a type or a variable, the **any, table** must be used.
Especially when using **mLua**, the  **any** should be used alongside Annotations.

```lua
---@type FastVector3
property any vt = FastVector3(0,0,0)
 
---@param vt FastVector3
method void NewMethod(any vt)
    -- ...
end
```

#### FastVector3 Function Expansion
The function `FastVector3` can be expanded by adding functions or properties. You can also change the previous function or property values.

```lua
-- Add static Property
FastVector3.two = FastVector3(2, 2, 2)
 
-- Add Function
local v3mt = getmetatable(FastVector3.zero)
v3mt.NewFunction = function(self)
    -- ...
end
```

# Using FastVector3 Effectively
You can reduce the creation of reference entities using the `FastVector3` type.
When accessing a Native type in Lua, a reference entity is created in MapleStory Worlds. This is to prevent the destruction of the entity being referenced. When the created reference entity is no longer needed, the memory for the Lua's Garbage Collector will be cleaned first, and then it will be removed from the memory after another cleaning of Native's Garbage Collector.

#### How to Use Reference Entities without Creating Them
`FastVector3` is used for optimization instead of `Vector3` because it doesn't **create a reference entity** when generating or reading a value.
Reference entities are needed for Lua script to use `Vector3`. A reference entity is automatically created and destroyed when using a `Vector3` entity in MapleStory Worlds. But this increases processing time and memory usage, requring optimization.

```lua
-- Reference Entity Created when Reading Position Value
local position = self.Entity.TransformComponent.Position
 
-- Reference Entity Created when Creating a Vector3 Entity
local vt = Vector3(0, 0, 0)
```

```lua
-- Reference Entity Created
local newPosition = Vector3(0, 0, 0)
-- Reference Entity Not Created
newPosition.x = 1
newPosition.y = 2
self.Entity.TransformComponent.Position =  newPosition
``` 

The following is a use example of `FastVector3` that doesn't create a reference entity.

```lua
-- Reference Entity Not Created
local newPositionFast = FastVector3(0, 0, 0)
 
-- Reference Entity Not Created
newPositionFast.x = 1
newPositionFast.y = 2
-- The Reference Entity is not created until you are substituting FastVector3 to Native Property
self.Entity.TransformComponent.Position =  newPositionFast
```

The property type of `Position` and `WorldPosition` for `TransformComponent` is `Vector3`. Therefore a reference entity is created when reading the value. If you wish to use the two properties without creating reference entities, you can use the `PositionAsFastVector3()` and `WorldPositionAsFastVector3()` functions.

><span style="color: #7cafc2">**Tip**
>You can use the `PositionAsFastVector3()` and `WorldPositionAsFastVector3()` functions to utilize it in the same way for `UITransformComponent` too.</span>

Let's compare the performance of the code that changes the **Position** value for `TransformComponent`.

```lua
local transform = self.Entity.TransformComponent
  
-- Average 1.113 us
local position1 =  transform:PositionAsFastVector3() -- Reference Entity Not Created

position1.x += 0.1
position1.y += 0.1
transform.Position = position1
 
-- Average 2.217 us
local position2 =  transform.Position --  Reference Entity Created

position2.x += 0.1
position2.y += 0.1
 
-- Average 1.876 us
local position3 =  FastVector3(transform.Position) -- Reference Entity Created

position3.x += 0.1
position3.y += 0.1
transform.Position = position3
```

Substituting `FastVector3` in the `Vector3` script property will **automatically convert** to `Vector3`. This auto conversion may cause `FastVector3` to not function as intended by the creator, so the following usages should be avoided.
The type needs to be set to **any or table** and not Vector3 to prevent issues from the automatic conversion.

```lua
Property:
Vector3 vt3 = Vector3(0, 0, 0)

Method:
void Test()
{
    -- FastVector3 Automatically Converted to Vector3
    self.vt3 = FastVector3(1, 1, 1)

    -- Reference Entity Created as Local Variable References Vecrtor3
    local vt = self.vt3
}
``` 

# Using FastVector3 and Index
`FastVector3` can be accessed using index as well. You will notice through the table below that accessing through the index is slightly faster than accessing it using a key.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17453667314987383595f55d84737a7cb12f62068050b.png{"width":"1080px"} "FastVector3Index")

The following is an example of accessing `FastVector3` using the index. X corresponds to 1, Y corresponds to 2, and Z corresponds to 3.

```lua
local v = FastVector3(1, 2, 3)
print(v.x, v.y, v.z)      -- Output: 1 2 3
print (v[1], v[2], v[3])  -- Output: 1 2 3
```

Index access can help with optimizing a world's performance optimization. However the readability will be lower compared to accessing the x, y, and z members directly. Therefore index access is recommended only when max optimization is needed for a world. For example, when the `Normalize()` function is being called too many times, using index access as shown below can improve performance.

```lua
local v3mt = getmetatable(FastVector3.zero)
 
-- Edit the Normalize Function for FastVector3
v3mt.Normalize = function(self)
    local x, y, z = self[1], self[2], self[3] -- self.x , self.y , self.z
    local length = (x*x + y*y + z*z) ^ 0.5
 
    if length == 0 then
        return FastVector3(x, y, z)
    end
 
    return FastVector3(x / length, y / length, z / length)
end
```