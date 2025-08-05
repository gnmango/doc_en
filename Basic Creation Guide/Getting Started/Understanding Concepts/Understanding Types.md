# Course Introduction
This course introduces the concept of types and how to use them.

# What is a Type?
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1687940593248ba72058dee8e4fb58ec8a1964a2a6663.png "1")
In MapleStory Worlds, <span style="color: #dc9656">**type**</span> refers to a user-defined datatype that combines several properties and functions to create a new datatype.
You can control and manage data of a similar nature all at once by using types, so it's good for reusing and maintaining code.

## Difference Between Types and Components
Types are similar to components, but with the following differences:
* Types are not components, so they cannot be added to entities.
* Types do not support execution control features.
* Types do not have event handlers.
* **Default event functions** are also provided for each intended use of the type.

# Characteristics and Categories of Types
Each type has its own usage. It's integrated with suitable native functions.
In addition, some types provide **default event functions** suitable for each type's intended use. How default event functions work is already defined in Native. As such, creators only need to take these default event functions, and implement only what is needed for their world.

Now, let's examine types, their characteristics, and how to use them.

## EventType
Used when defining a new **EventType** that is not defined in Native. **EventType** works with **Event System**. 
Let's see [Entity Event System](/docs/?postId=176{"target":"_self"}) for more detailed examples in utilizing **EventType**.

## ItemType
**ItemType** is literally used to classify items and define their characteristics. **ItemType** works with [ItemService](/apiReference/Services/ItemService{"target":"_self"}).
**ItemType** can be created in individual item units, but in general, we recommend creating them in groups of types with the same function. For example, there will be many items in each world. Let's assume that the types of this item can be roughly divided as follows.
* Weapon: has attack power
* Potion: has recovery power
* Armor: has defensive power
* Others: no special features

In this case, you can create **ItemType** as shown below.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16806724849204180b61d083646ccae04f255217b72c2.png "2")

You can use the default event function `OnCreate()` in the **ItemType** script.
`OnCreate()` is automatically called when an item is created, and data from the item table (UserItemDataSet) is transferred to the item entity. The data passed over to the item entity can be accessed with `self.ItemTableData:GetItem("column name")`.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1680672660749a9c4c02d07df4a938bb51c84d12a1b84.png "3")

See [Item Creation and Deletion](/docs/?postId=66{"target":"_self"}) for more detailed examples in utilizing **ItemType**.

## BTNodeType
Use [AIComponent](/apiReference/Components/AIComponent{"target":"_self"}) when applying AI to entities such as monsters or NPCs. **AIComponent** can make a behavior tree. When creating one of the nodes that configure this behavior tree, **action node**, use the **BTNodeType** script.

**Action node** defines an "action of an entity" or a "condition for an action." For example, it defines what action the monster will take when it is in the "waiting" state, or defines the conditions for converting from the "waiting" state to the "attack" state. If you create an action node with **BTNodeType**, you can easily create a node entity with `CreateNode()` of **AIComponent**.

You can use the default event functions `OnBehave()` and `OnInit()` in the **BTNodeType** script.

| Function | Description |
| :---: | --- |
| `OnBehave(number delta)` | This function is called once whenever the corresponding action node is executed. The function is used to define AI actions or conditions to actions, so if you created **BTNodeType**, you must add it. |
| `OnInit()` | The function is executed before `OnBehave()` is called.  |

![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1680766233591bda06f8923594e85abd6e71e451335bb.png "4")

For a more detailed description of each function and an example of using **BTNodeType**, refer to [Creating AI using Behavior Trees](/docs/?postId=562{"target":"_self"}).

## StateType
**StateType** is used when the user creates a new state. 
For example, the player character has several states: standing, prone, attacking, jumping, etc. When a creator wants to add a new state in addition to the default state, **StateType** can be added. **StateType** works with [StateComponent](/apiReference/Components/StateComponent{"target":"_self"}).

**StateType** can define the following.
* You can have a user-defined action performed when entering or exiting a state.
* You can define conditions under which the state will stop.

You can use the default event functions `OnEnter()`, `OnUpdate()`, `OnExit()`, and `OnConditionCheck()` in the **StateType** script.

| Function | Description |
| :---: | --- |
| `OnEnter()` | Executes the action when entering the state of the first time. |
| `OnUpdate()` | Executes an action while the state is maintained. |
| `OnExit()` | Executes when changing states. |
| `OnConditionCheck(string nextStateName)` | Specifies conditions to transition the state. You can use the <br>**nextStateName** parameter to return different result values depending on the connection state. |

![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16807662783116d11c7686e694f9197b6c6ab59f01ef0.png "5")

For a detailed explanation of **StateType**, refer to the [Controlling Entity State](/docs/?postId=686{"target":"_self"}) guide.

## StructType
**StructType** is used when the creator directly determines the usage and defines the shape accordingly.
It functions similarly to Structure in general programming languages. This is useful when grouping variables containing different data. For example, you can create a type called "Student" and put variables such as "name, student number, department" into it and manage it.