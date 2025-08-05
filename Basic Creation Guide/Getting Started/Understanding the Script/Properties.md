# Course Introduction
In this course, we will learn about MapleStory Worlds' properties. We will also learn about property-related information such as declaring properties, setting defaults, and synchronizing properties.

# Properties in MSW
Properties are a component's member variable, and can have different values for each entity. Properties have three characteristics.

* Unlike a Lua variable, a MapleStory Worlds property sets its type upon declaration and can refer to or substitute a value.
* A property can be accessed from the outside within a component, and its value can be read or written in each function. 
* You can set the synchronization between the server and client by setting the property's synchronization.

# Property Declaration and Removal
Let's learn how to declare and delete properties.

#### Declaring Properties
1. In the context menu of **Workspace - MyDesk**, click **Create Scripts - Create Component** to generate a new script component.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1687496291799b05a128af08748b8bc69f2956c9de929.png "1")
<br>
2. Name the component. Double-click the new component to open the Script Editor.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1635329393515dacd9e3446ac472e92b77b2f787c160d.png{"width":"300px"} "2")
<br>
3. Click the **[+]** button to add a new property.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1635329401326a69e30631ae4401784a4862c7711aada.png "3")
<br>
4. Set the property name, type, default value, and synchronization status between server and client.
 ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1656400889882e4b1522943154fef8f0ff8ab49b71e29.png "4")

#### Deleting Properties
Click **[⋮] Menu - Remove** to delete the property.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1635329420391af78791adc6e4e3e97380dea88d3b4ad.png "5")

# Property Type
When making properties, you should set the property's type. However, the way to do so is a little different from the usual Lua grammar. You must set a type for transferring and synchronizing property values.
Click "Type" to the left of the property name to set the appropriate type.
![06](https://mod-file.dn.nexoncdn.co.kr/bbs/1673599399633ba4e45d5b2694a838e3454621f4b9d4c.png{"width":"400px"} "06")

#### string
Can accept a string value.

```lua
Property: 
[Sync]
string StringProperty = "Hello"

Method:
[Server Only]
void OnBeginPlay()
{
    self.StringProperty = self.StringProperty .. " World!!"
    log(self.StringProperty) --Output "Hello World!!" String in Console Window
    self.StringProperty = "Hello MSW!!"
    log(self.StringProperty) --Output "Hello MSW!!" String in Console Window
}
```

#### number
The number type can accept number values. It can accept any numeric value, whether integer or real.
```lua
Property:
[Sync]
number NumberProperty = 100

Method:
[Server Only]
void OnBeginPlay()
{
    self.NumberProperty = self.NumberProperty + 83
    log(self.NumberProperty) --Output the Number 183 in Console Window
    self.NumberProperty = 3.28
    log(self.NumberProperty) --Output the Number 3.28 in Console Window
}
```

#### integer
The integer type can accept integer values. When a real value is entered, the decimal is dropped.

```lua
Property:
[Sync]
integer IntegerProperty = 100

Method:
[Server Only]
void OnBeginPlay()
{
    self.IntegerProperty = self.IntegerProperty + 83
    log(self.IntegerProperty) --Output the Number 183 in Console Window
    self.IntegerProperty = 3.28
    log(self.IntegerProperty) --Output the Number 3 in Console Window
}
```

#### boolean
Can assign a true or false value to the property.
```lua
Property: 
[Sync]
boolean BooleanProperty = false

Method:
[Server Only]
void OnBeginPlay()
{
    self.BooleanProperty = true
    
    if self.BooleanProperty == true then
    self.BooleanProperty = false
    log("Hello MSW!!")
    end
}
```

#### table
This refers to Lua table. It is not synchronized (Sync).
```lua
Property: 
[None]
table TableProperty = {1,2,3,4,5}

Method:
[Server Only]
void OnBeginPlay()
{
    local tmpTable = self.TableProperty
    self.TableProperty[1] = 10
    log("tmpTable : "..tmpTable[1]) --Output "tmpTable : 10" in Console Window
    log("TableProperty : "..self.TableProperty[1]) --Output "TableProperty : 10" in Console Window 
    
    self.TableProperty = {100,200,300,400}
    log("tmpTable : "..tmpTable[1]) --Output "tmpTable : 10" in Console Window
    log("TableProperty : "..self.TableProperty[1]) --Output "tmpTable : 100" in Console Window
}
```

#### SyncTable< v >
A table format of Lua, but it's a restricted type and the `next` function cannot be used. Unlike a general table property, you can set only one type, and it can be <span style="color: #dc9656">**synchronized(Sync)**</span>. However, note that it is slightly slower compared to regular tables.
**v** refers to type. You can select the desired type from the list.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/168610513116799d1be6022794e1582c2b9227100a178.png "2")

#### SyncTable< k, v >
A table format of Lua, but it's a restricted type and the `next` function cannot be used. Unlike a general table property, you can set only the type specified for key and value, and it can be <span style="color: #dc9656">** synchronized (Sync)**</span>. However, note that it is slightly slower compared to regular tables.
**k, v** refer to type. You can select the desired type from the list.

#### Vector2
Expresses a 2D vector and location. This is provided in a table type and is referred to when assigned to a property or a variable. Usually it is used to express the location coordinates in a 2D space, which includes the member variables x and y, indicating x and y coordinates.
Please refer to the following descriptions about how to create and refer to values and access member variables x, y:
```lua
Property: 
[Sync]
Vector2 Vector2Property = Vector2(0,0)

Method:
[Server Only]
void OnBeginPlay()
{
    local tmpVector2 = Vector2(10,10) --Create Vector and Assign to Local Variable tmpVector2
    self.Vector2Property = tmpVector2 --Assign tmpVector2 Value to Property Vector2Property
    log(tmpVector2) --Output as (10.000, 10.000) in Console Window
    log(self.Vector2Property) --Output as (10.000, 10.000) in Console Window
    
    local calcu = self.Vector2Property
    calcu.x = 20 --Set Value of Member Variable x
    calcu.y = 50 --Set Value of Member Variable y
    log(calcu) --Output as (20.000, 50.000) in Console Window
}
```

#### Vector3
Expresses a 3D vector and location. Just as **Vector2**, it is referred when assigned to a property or a variable. Member variable z is added in **Vector2**.
**Vector2** is commonly used, except in cases when you need to assign the **Position** of a **TransformComponent**. Please refer to the following descriptions about how to create and refer to values and access member variables x, y, z:

```lua
Method:
[Server Only]
void OnBeginPlay()
{
    local tmpVector3 = Vector3(10,10,10)  --Create Vector and Assign to Local Variable tmpVector3
    local Vt1 = tmpVector3
    local Vt2 = tmpVector3
    log(Vt1)  --Output as (10.000, 10.000, 10.000) in Console Window
    log(Vt2)  --Output as (10.000, 10.000, 10.000) in Console Window  
    
    tmpVector3.x = 20
    tmpVector3.y = 20
    tmpVector3.z = 20
    
    log(tmpVector3)  --Output as (20.000, 20.000, 20.000) in Console Window
    log(Vt1)  --Output as (20.000, 20.000, 20.000) in Console Window
    log(Vt2)  --Output as (20.000, 20.000, 20.000) in Console Window
}
```

#### Vector4
Expresses a 4D vector and location. Member variable w is added in **Vector3**.
Please refer to the following descriptions about how to create and refer to values and access member variables x, y, z, w:
```lua
Method:
[Server Only]
void OnBeginPlay()
{
    local tmpVector4 = Vector4(10,10,10,10)  --Create Vector and Assign to Local Variable tmpVector3
    local Vt1 = tmpVector4
    local Vt2 = tmpVector4
    log(Vt1)  --Output as (10.000, 10.000, 10.000, 10.000) in Console Window
    log(Vt2)  --Output as (10.000, 10.000, 10.000, 10.000) in Console Window  
    
    tmpVector4.x = 20
    tmpVector4.y = 20
    tmpVector4.z = 20
    tmpVector4.w = 20        

    log(tmpVector4)  --Output as (20.000, 20.000, 20.000, 20.000) in Console Window
    log(Vt1)  --Output as (20.000, 20.000, 20.000, 20.000) in Console Window
    log(Vt2)  --Output as (20.000, 20.000, 20.000, 20.000) in Console Window
}
```

#### Entity/EntityRef
This property points to an entity. Entity and EntityRef have the same basic usage but have different reference methods.

* Entity: Directly reference an entity. If you move to another map and return, the reference is severed, making the property not usable.
* EntityRef: Holds the entity’s key value and then searches for the entity when it is accessed. It can access the entity it is pointing to without issue even after returning after going to another map. EntityRef is slower than Entity, since it searches for the entity each time an entity is accessed.

```lua
Property:
[Sync]
Entity EntityProperty = EntityPath
[None]
EntityRef EntityRefProperty = EntityPath

Method: 
[Client Only]
void OnBeginPlay()
{
    log(self.EntityProperty.Name)
    log(self.EntityRefProperty.Name)
}
```

#### Component/ComponentRef
This property points to a component. Select the Component property type, then click a button to set a specific component to a type. Component and ComponentRef have the same basic usage but have different reference methods.

* Component: Directly references a component. If you move to another map and return, the reference is severed, making the property unusable.
* ComponentRef: Holds the component’s key value and then searches for the component when it is accessed. It can access the component it is pointing to without issue even after returning after going to another map. ComponentRef is slower than Component, since it searches for the component each time a component is accessed.


    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1635329448310478cc03be91a4882900e39683a801fd7.png "7")

```lua
Property:
[None]
TransformComponent ComponentProperty = EntityPath
[None]
TransformComponentRef ComponentRefProperty = EntityPath

Method:
[client only]
void OnBeginPlay()
{
    log(self.ComponentProperty.Position)
    log(self.ComponentRefProperty.Position)
}
```

#### Any
A type that can refer to any type. Instead of writing the basic types above, it is mostly used to refer to **Entity** or **Component**.
```lua
Property:
[None]
any EntityProperty = nil
any ComponentProperty = nil

Method:
[Server Only]
void OnBeginPlay()
{
    self.EntityProperty = _EntityService:GetEntityByPath("/maps/map01/entityA")
    self.ComponentProperty = self.EntityProperty.TransformComponent
    log(self.EntityProperty.Name) --Output Target Entity Name in Console Window
    log(self.ComponentProperty.Position) --Output Component’s Property Value in Console Window. (Position is a Vector3 type, and is output in the (x, y, z) format.)
}
```

# Setting the Default Value of a Property
You can set a property's default value from the script. For example, you can either enter a string value "Hello", or a numeric value such as 100, according to each property's type as shown below. In case of a Table-type property, you can also set the table as the default value. 
The default value of the property may also be the default value of the entity to which the component is applied. If you access each function without a property that points to a value or has a referenced value, it reads the default value of that property.

```lua
Property:
[None]
string StringProperty = "Hello!!"

Method:
[Server Only]
void OnBeginPlay()
{
    local getStringProperty = self.StringProperty 
    --Assign the self.StringProperty Value “Hello!!” to getStringProperty
    log(getStringProperty) 
    --Output “Hello!!” in Console Window
}
```
<br>
You can also set a different initial value for each entity. If you want to set a different default value for each entity, it's more efficient to do so via the Property Editor rather than a script. This is because the property value set in Property Editor takes priority over one set by a script. If you set the property default value in Property Editor after adding a component to an entity, each entity can have a different property default value.
![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1683001932653498b70b84e0242c7a8b8902a8643b144.png "10")

# Accessing Properties
%%Let’s learn how to access a property from functions. First, you can access the same component in the following way.%%
```lua
Property:
[None]
string StringProperty = "Hello!!"

Method:
[Server Only]
void OnBeginPlay()
{
    local stringProperty = self.StringProperty 
    --Load Target Property as “self.Property Name”
}
```
<br>
The properties of another component can be accessed in the following way. Let’s learn how to access them in the situation below.

* A situation where entities “A” and “B” exist, and each entity has "ComponentA" and "ComponentB" applied.
* "ComponentA" has a property named "PropertyA" declared and ComponentB" has a property named "PropertyB" declared.
* Then "ComponentB" reads the value of "PropertyA" property from "ComponentA" and assigns it to "PropertyB".

```lua
Property: 
[None]
string PropertyA = "HelloMSW!!"
```

```lua
Property:
[None]
string PropertyB = ""

Method:
[Server Only]
void OnBeginPlay()
{
    local entityA = _EntityService:GetEntityByPath("/maps/map01/A")
    local componentA = entityA.ComponentA
    self.PropertyB = componentA.PropertyA
    log(self.PropertyB) --Confirm that HelloMSW!! is output in the console window.
}
```

> <span style="color: #585858">**Learn More**
> For a more detailed explanation about referring to entities and components, review the the following guide.</span>
> [Referring to Entities and Components](https://mod-developers.nexon.com/docs?postId=164{"target":"_self"})

# _T Property
A property is a member variable within a component, which preserves value set in function and doesn't stop even when written and read in a function. Therefore, it can be used to contain a certain value through a function or accumulate values from repeated calls to a function.
The next example outputs "HelloMSW" to the Console window every 3 seconds.
```lua
Property:
[Sync]
number AccTime = 0

Method:
[Server Only]
void OnUpdate(delta)
{
    self.AccTime = self.AccTime + delta  
    --Each time OnUpdate is called, the delta value is added to the AccTime property.
        
    if self.AccTime >= 3 then  
    -- Check if the AccTime Property’s Total Value is 3 or Higher, Output "HelloMSW" in Console Window if 3 or Higher
        self.AccTime = 0
        log("HelloMSW")
    end
}
```
<br>
The total value can be placed inside the component like in the example above. However, it can be cumbersome, since it continues to synchronize and declare unneeded properties. To solve this issue, MapleStory World
provides the **_T Property**. The **_T Property** has the following characteristics.

* Unlike a general property, _T property is created when it is first written in a function.
* You can access it not only from within the component but also from outside of the component. 
* You can use it only in the same space as it cannot be synchronized. 
 
**_T Property** is used in the form of <span style="color: #dc9656">**"_T.PropertyName"**</span>. To help you understand, let's redo the example from above using _T Property.

```lua
Property:
[Sync]
number AccTime = 0 -- Use _T Property instead of this property.
 
Method:
[Server Only]
void OnUpdate(delta)
{
    if self._T.accTime == nil then self._T.accTime = 0 end  
    --_T Property is declared exactly at the moment of writing. As it was declared in the component, attach "self" to the front
 
    self._T.accTime = self._T.accTime + delta  
    -- Adds delta value to _T.accTime property whenever OnUpdate is called
     
    if self._T.accTime >= 3 then  
    -- Checks if accumulated value of _T.accTime is greater than or equal to 3; if so, it outputs "HelloMSW" on the Console window.
        self._T.accTime = 0
        log("HelloMSW")
    end
}
```