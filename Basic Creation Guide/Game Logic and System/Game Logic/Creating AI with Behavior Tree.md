# Course Introduction
Let's understand the concepts of behaviour trees and BTNodeType and how to create the Action Node using AIComponent. [Creating Behaviour Tree Nodes] describes how to implement a behaviour tree and gives implementation examples.

##### Reference Guide
* [Creating Behaviour Tree Nodes]

##### API Reference
* [MODBTNode](/apiReference?postId=455{"target":"_blank"})
* [AIComponent](/apiReference?postId=361{"target":"_blank"})
* [ParallelNode](/apiReference?postId=456{"target":"_blank"})
* [RandomSelectorNode](/apiReference?postId=457{"target":"_blank"})
* [SelectorNode](/apiReference?postId=458{"target":"_blank"})
* [SequenceNode](/apiReference?postId=459{"target":"_blank"})

# Understanding Concepts
### Finite State Machines
**Finite State Machine (FSM)** is a model that converts to one of the finite number of states under condition or operates a certain action to match that state. FSMs are relatively easier to configure and more intuitive to design. It is primarily used for designing AI. FSM is composed of **State** and **Transition**. If certain conditions are met while performing an action for a state, it transitions to the next state. For example, if a player approaches a monster currently in waiting status, the monster changes to attacking status to perform actions and attack the player.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17168764448031671b4e8e02a4b5cb6a933c7a246c29d.png "FSM")

However, as there are more states and actions, it becomes difficult to connect and maintain the states. In addition, as scale increases, it becomes difficult to see and reuse the flow of state.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/171687700691298cc5dc178a644d7863dd656f705c7f6.png "FSM2")

### Hierarchical Finite State Machines
**Hierarchical Finite State Machines (HFSM)** is a concept that modularizes various states and transitions to remedy the shortcomings of FMS. The modulized flow in HFSM can be reused, and it is easier to understand the flow of the current state.
However, it is difficult to reuse individual states. Each subordinate state has to be transitioned from a certain state. So it is impossible to access an individual state from another group. For example, a state named 'HIT' may be subordinate to 'IDLE' or to 'BATTLE'. Therefore, you need to add a 'HIT' state under the 'IDLE' state and a 'BATTLE' state, respectively.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17168770274270c242d640e824e7a8de6e55f9e475212.png "HFSM")

### Behaviour Tree
**Behaviour Tree (BT)** is a collection of individual tasks called nodes. Unlike Finite State Machines, which consist of state and transition, each node in a behaviour tree is modularized with individual logic. It can be reused depending on the situation and purpose. It also makes it easy to connect and disconnect specific behaviours, as each node is executed sequentially in the order of binding.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17168770533307c70cdaacf4741df8155b0d942229b73.png "BT")

# How the Behaviour Tree Works
The behaviour tree starts at the Root node every frame. After that, they are executed sequentially. Execution order is from top to bottom and from left to right. Parent nodes execute child nodes, and the execution order follows the sequence registered in each parent.
Different parent nodes have different conditions of executing child nodes. Parents execute all child nodes in sequence, as long as they meet the condition. For example, if the parent node has the child nodes A, B, and C registered sequentially, node A is placed on the far left, and node C on the far right. Executing the parent node will execute child nodes A, B, and C in order. And if the child node A has its own child nodes D and F, then both the child nodes D and F will be executed before executing node B.

All nodes return a state value of [**BehaviourTreeStatus**](/apiReference/Enums/BehaviourTreeStatus{\"target\":\"_blank\"}) upon execution. It returns a total of three values: **Running, Success, and Failure**. Each parent node decides whether to execute the next child node depending on which value its child node returns.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/171774458675144ffe3ccb3334751afda1dcf151b5387.png "BTNode2")
# Understanding Behaviour Tree Nodes
Behaviour tree consists of an **Action Node** that defines action and a **Composite Node** that controls flow of action. Action Node must be created and used by the creator using BTNodeType, and Composite Node is provided by MapleStory Worlds.

## Action Node
**Action Node** defines an action of an entity. It is also called Leaf Node and is characterized by having no child nodes. Action Node defines the entity's actual action or condition of an action. Each action and condition are defined in individual Action Nodes, and each node should be registered as Composite's child node. For instance, it defines actions for a monster's 'waiting' state or 'attacking' state.

## Composite Node
**Composite Node** is the node that controls the flow and can have multiple child nodes. When a child node returns Success or Failure, certain rules determine whether the next child node should be executed.
There are four types of Composite Nodes, and each node has different rules for how child nodes execute and how state values are returned to the parent.
* **Sequence Node, Selector Node, Random Selector Node, Parallel Node**
#### Sequence Node
**Sequence Node** executes its child nodes in sequence, and if a child node returns Failure, it stops executing and returns Failure to its parent node. Returns Success to its parent node if all child nodes return Success.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1719214998441707cc625005241ec8bef19f44d51277a.png "Sequence Node")
#### Selector Node
**Selector Node** executes its child nodes in sequence, and if a child node returns Success, it stops executing and returns Success to its parent node. It returns Failure to its parent node if all child nodes return Failure.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17168858785873218b87041fd46df9756a267a30ce29e.png "SelectorNode")

#### RandomSelector Node
**RandomSelector Node** randomly selects one of its child nodes and executes it. It returns Success to its parent node if a child node returns Success and returns Failure if a child node returns Failure.
RandomSelector Node draws a random number and selects a node with a probability in the range to which the random number belongs. For example, when the RandomSelector Node has 0.2 (20%), 0.3 (30%), and 0.5 (50%) probabilities of a child node being selected as shown below, the Action2 node will be executed if the random number is determined to be 0.4, and the Action3 node will be executed if the random number is determined to be 0.9.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17192150545723d6a5611d3c24cf5b75b12d37935a549.png "RandomeSelectorNode")
#### Parallel Node
**Parallel Node** executes all child nodes, and if all child nodes return Success, it returns Success to its parent node. On the other hand, if any of the child nodes returns Failure, it returns Failure to its parent node.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/171774512352277dc3a613298479ab8ac9fb4f430a259.png "ParallelNode")
## Decorator Node
A **Decorator Node** has one child and can determine whether to execute the child or process a return value. It can have a Composite Node or Action Node as a child. MapleStory Worlds provides for creators to create Decorator Nodes.
The Decorator Node is located halfway between the parent node and child node A and determines whether or not to execute node A based on conditions. It can also deliver a different value to the parent instead of the value returned by node A.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17177451607950c32f6a27e8d4ffd8c5c67eb1aeefb69.png "DecoratorNode")
# Nodes Run for Multiple Frames
When a child node returns Success or Failure, the parent node executes the next child node. However, if a child node returns Running, the parent node will re-execute the child node that returned Running on the next frame. You can use this nature to create nodes that behave for multiple frames.
For Sequence Node and Selector Node that have child nodes A, B, and C as shown below, node C is not executed while node B returns Running. RandomSelector Node continues to execute the selected nodes. Due to the nature of Parallel Node that executes its child nodes simultaneously, all of nodes A, B, and C are executed, and in the next frame, only nodes A and B which returned Running are executed, but node C is not. 

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1716887214774390c0a3ef79d41519e77b73dcc166b9b.png "2")
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1716887242595459c3c005ce24bdfab35c3742a2d36a1.png "4")

Using this nature of node, you can create a Wait node that waits for a certain amount of time as shown below.

```lua
Property:
[None]
number Time = 2
[None]
number ElapsedTime = 0

Method:
void OnInit()
{
    -- OnInit() is not called while Running is returned.    

    self.ElapsedTime = 0
}

any OnBehave(number delta)
{
    -- Prevents the next child node from executing by continuing to return Running until Time has elapsed.
    -- OnBehave() continues to return while Running is returned.

    self.ElapsedTime += delta
    if self.ElapsedTime < self.Time then
        return BehaviourTreeStatus.Running
    end

    return BehaviourTreeStatus.Success
}
```

# Creating Action Nodes and Defining Function
MapleStory Worlds allows you to create an Action Node using BTNodeType. You can use it by defining the behaviour of the node in BTNodeType and creating a node object with AIComponent.

## Defining in BTNodeType
BTNodeType provides `OnInit()` and `OnBehave()` as default event functions. Define the behaviour of the node in the default event function.

#### OnInit()
`OnInit()` is the function that is executed before calling `OnBehave()`. It is called whenever the node is executed, but if **Running** is returned, `OnInit()` is not called in the next frame.

#### OnBehave()
`OnBehave()` is a method called once whenever the corresponding Action node is executed. It must return a value of **BehaviourTreeStatus**. It is a method used to define AI actions or conditions to actions, so it must be added to Action node type.
It can be used to implement measurement of movement or time using the **delta**, the time per frame parameter.

```lua
Property : 
[Sync]
number Num = 0

Method: 
void OnBehave(number delta)
{
	self.Num = self.Num + delta
	
	if self.Num >= 3 then
		self.Num
		return BehaviourTreeStatus.Success
	else	
		return BehaviourTreeStatus.Running
	end
}
```

## Creating an Action Node
The entity to be used as an AI must contain **AIComponent or an extended AIComponent**, because AIComponent creates the Action Node as a separate object with the content defined in BTNodeType or gives AI to entities by setting the highest node of the created behaviour tree as the Root node.
When creating an Action Node, you can use AIComponent's `CreateNode()` and `CreateLeafNode()`.

#### CreateNode()
When creating a node object with **BTNodeType**, use the `CreateNode()` function of **AIComponent**. Passes the BTNodeType and name to be created as parameters.

```lua
local node = self.Entity.AIComponent:CreateNode("NewBTNode", "actionNode")
```

#### CreateLeafNode()
If you want to create a simple Action Node, you can define the action of the node by passing a function directly to it as shown below. Unlike `CreateNode()`, `CreateLeafNode()` does not require BTNodeType. As a parameter, deliver the name of the node that will be created as entity and the function that will be called upon executing the node.

```lua
local func = function() 
    log("ActionNode!") 
    return BehaviourTreeStatus.Success
end

local printLogNode = self.Entity.AIComponent:CreateLeafNode("NodeInstanceName", func)
```