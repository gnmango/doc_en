# Course Introduction
MSW continues to update and modify various features to support creators in building their Worlds. MSW is working to avoid duplicate components in the future.
Before the feature support stops completely, we recommend referring to the guide in advance and checking for and correcting any duplicate components in your World to ensure smooth operation.
# Duplicate Component
When two or more of the same or similar components are added to an entity or a model, they are called duplicate components. Duplicate components often occur when you unintentionally add the same type of component to an entity multiple times in the production process, or when you add a component without knowing that it has been inherited from a parent-child model or an entity. 

#### Example of Duplicate Components
Let's take a look at the problems that arise when components are duplicated, along with examples.

'DoorComponent' has the 'Open' and 'Close' functions. 

```lua
Method:
void Open()
{
     log("Door Open")
}

void Close ()
{
    log("Door Close")
}
```

We've extended this component to create `RevolvingDoorComponent` and `AutomaticSlidingDoorComponent` and redefined the functions.
* `RevolvingDoorComponent`

    ```lua
    Method:
    override void Open()
    {
        log("revolving Door Open")
    }
    
    override void Close()
    {
        log("Revolving Door Close")
    }
    ```

* AutomaticSlidingDoorComponent
    ```lua
    Method:
    override void Open()
    {
        log("Automatic Sliding Door Open")
    }
    
    override void Close()
    {
        log("Automatic Sliding Door Close")
    }
    ```
We've added `AutomaticSlidingDoorComponent` to the Door1 entity and `RevolvingDoorOpenComponent` to the Door2 entity. You can then write the following logic to call the functions of the two components extended through `DoorComponent`. 

```lua
Property:
[None]
Entity Door1 = /maps/map01/Door1
[None]
Entity Door2 = /maps/map01/Door2

Method:
void OpenAll()
{
    self.Door1.DoorComponent:Open() -- Automatic Sliding Door Open
    Self.Door2.DoorComponent:Open() -- Revolving Door Open
}
```
    
However, let's consider a scenario wherein both components are added to a single TestEntity. If you call the function in TestEntity by using `self.TestEntity.DoorComponent:Open()`, it is not clear which component's Open function should be called. This is because both components use the extended forms of the `DoorComponent` function, and it's unclear which between `AutomaticSlidingDoorComponent` and `RevolvingDoorComponent` is more appropriate.
    
```lua
Property:
[None]
-- An entity with both AutomaticSlidingDoorComponent and RevolvingDoorComponent
Entity Door = /maps/map01/TestEntity

Method:
void OpenAll()
{
    -- Unclear which one to call for the Open() between AutomaticSlidingDoorComponent and RevolvingDoorComponent.
    self.Door.DoorComponent:Open() 
}
```
    
# Modifying Duplicate Components
If there are duplicate components, a related log message occurs in the <span style="color: #dc9656">**Console, Build Console window**</span>. These log messages are typically output when you first enter a World being created in Maker and when you make changes to the map. You can modify or remove components accordingly depending on the content and situation. Duplicate components typically occur in the following four situations. 
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/16844893719830c8cccd36923447fac8f6fc1cb46a27c.png "1")
#### Occurrence of Duplicate Components in Placed Entities on the Map
If there are duplicate components in entities placed on the map, log messages occur in the Console window when you first enter the Maker Pro mode or when you move to a map in Maker. The entities placed on the map can be divided into two categories, and you need to resolve the duplicate components accordingly.
1. **Entity Placed in the Model**
Delete the duplicate components. You should delete the duplicate components from the model or entity to align with the World's operation.

2. **Entity Placed on the Map**
Refer to the error messages and delete one of the duplicate components.

#### Occurrence of Duplicate Components in Dynamically-Created Entities
If there are duplicate components in dynamically-created entities, log messages occur in the Console window during test play and World play. This usually occurs in the following three cases. 
1. **Adding components using the `AddComponent()` function**
    During the process of dynamically adding entities using `AddComponent()`, component duplication can occur. Before adding a component, we recommend deleting and adding the components from the entity or model first.
2. **Creating the entity by moving to another map**
    Moving to another map will bring up the entities from the map you move to. Here, there may be entities with duplicate components on the map you want to move to. Based on the log messages that occur, find the entity with duplicate components and fix it.
3. **Spawning entities through the model**
    Check for duplicate components in the model and delete them. For more information, refer to the occurrence of duplicate components in the model.

#### Occurrence of Duplicate Components through Model Extend
It is possible to create multiple models in a parent-child relationship. In this case, duplicate components can occur because the child model inherits components from the parent model. Refer to the log messages in the Build Console window and delete the components. Duplicate components often occur in the following two cases:
1. **When completely identical Components are added**
In this case, duplicate components occur due to adding the same component to both the parent and child models. You can delete duplicate components from either the parent or child model to match the World's operation.
If you delete a component in the parent model, one of the components in the child model is deleted. Note that this may reset any remaining component values in the child model.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1684565806299564f6ae6ea59402eb33cfbb57fa921cc.png{"width":"740px"} "5")
2. **When components of the same type are added**
You may have added components that share a top-level parent at the same time. These components are identified as duplicate components because they have the same top-level parent. In this case, modify them to leave an appropriate component to match the World operation. 
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1684566220223902fa85801c94710a3b3d029922da031.png{"width":"740px"} "6")

#### Occurrence of duplicate components due to adding required components
Some components have prerequisite components that must be used in conjunction with them. This prerequisite component is automatically added when you add a specific component. This behavior can cause duplicate components to occur between related components, depending on the order in which they were added. In this case, delete the components you added, add them again, and modify them. This may require modifying the logic. You can check the components with required components in the table below.

![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1690333286437d3f1d73f2865486587ca983f6f6fee13.png)

| Added Components | Required Components |
| --- | --- |
| AIChaseComponent |@cols=1:@rows=2:StateComponent |
| AIWanderComponent | 
| DistanceJointComponent | @cols=1:@rows=6:PhysicsRigidbodyComponent, PhysicsColliderComponent |
| PrismaticJointComponent |  
| PulleyJointComponent |  
| RevoluteJointComponent |  
| WeldJointComponent |  
| WheelJointComponent |  
|PhysicsRigidbodyComponent  | PhysicsColliderComponent, TransformComponent |
| PhysicsColliderComponent | @cols=1:@rows=4:TransformComponent |
| KinematicbodyComponent |  
|  RigidbodyComponent|  
| SideviewbodyComponent |  