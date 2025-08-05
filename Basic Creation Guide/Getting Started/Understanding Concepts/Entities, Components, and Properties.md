# Course Introduction
Let's examine the concepts of Entities, Components, and Properties.

# Entities
An Entity is an object existing within MapleStory Worlds.
What are the Entities in the image below?
![Entity\_Component\_Propoerty\_1](https://mod-file.dn.nexoncdn.co.kr/bbs/1634523787794b80552669cd948a59e89f799d0a016fe.png)
<br>
Objects that are marked in red are Entities.
That's not all, though. All of the things that are operating are Entities too, though not visible.
![Entity\_Component\_Propoerty\_2](https://mod-file.dn.nexoncdn.co.kr/bbs/16345239132331e370f40d8cb452aa323567c080660a0.png)
<br>
In MapleStory Worlds, there are actually countless placed Entities that are generated, changed, and deleted in real time.
When Entities operate in the actual game, they become visible, and the internal logic operates them.
<br>
So, what are these Entities made of?
# Components
Entities are composed of Components. Essentially, an Entity is a group of Components.
Components are in charge of their own individual elements and functionality.
<br>
Let's take pizza as an example.
![pizza](https://mod-file.dn.nexoncdn.co.kr/bbs/163547555559905a1c30c66a24b689e06baf34fc3a750.png "pizza")
<br>
Let's say there is a freshly-made pizza in front of you.
This pizza smells wonderful. You can eat it, too. This pizza is an Entity.
<br>
The above pizza is made with tomatoes, dough, sauce, and more.
And while you cannot see them, there are things like the recipe, too.
These are all Components.
<br>
* Tomato Component
* Dough Component
* Sauce Component
* Recipe Component
<br>
Each Component is in charge of its own functionality, and sometimes they combine with other Components for a different effect.
The reason we separate things into Components is that this gives us a high degree of reusability.
For example, let's say we want to make another pizza.
<br>
* Cheese pizza = dough + sauce + cheese + oven cooking
* Hawaiian pizza = dough + sauce + cheese + pineapple + oven cooking
* Frozen bulgogi pizza = dough + sauce + cheese + bulgogi + microwave cooking
<br>
Likewise, we can take previously created Components, and if we combine them in a clever way, they form something new.
<br>
In MapleStory Worlds, the Property Editor is in charge of managing Components.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1635475521115072c13f11f5e4c49abea1035b8234c82.png "4")
<br>
As stated earlier, Entities are composed of Components. If we open the Entity Editor, we can see Components as above.
Therefore, if you combine well-separated Components, you can create an Entity that operates in the way you want.
<br>
So, does this mean we can create the Entities we want by combining just the pre-determined Components?
# Properties
Properties establish the details of each Component.
Let's go back to our pizza example from the Component section.
<br>
Three kids want to eat pepperoni pizza.
Tom wants double pepperoni, and Emma wants cheddar instead of mozzarella cheese in her pizza.
And Harry wants to eat a bigger pizza.
<br>
We have the pre-existing dough Component, pepperoni Component, and cheese Component.
Then, what should we do?

For Tom, we add a pepperoni double Component.
For Emma, we add a cheddar cheese Component.
And for Harry, we add all the Components: dough double, pepperoni double, and cheese double.
<br>
So we can create pizzas, or in this case, Entities, in the form we want.
However, there's not enough reusability in the Components. We said earlier that using Components give us a high degree of reusability, but we ended up making everything individually.
<br>
What should we do instead, then?
<br>
This is when we turn to Property.
Properties refer to the main items that must be set on a component.
Therefore, if the same two entities use the same components, they operate differently if their property values are different.
<br>
For example, on the dough component, there is a "Size" property.
On the cheese component, there are "Size" and "Cheese Type" properties,
and on the pepperoni component, there is a "Size" property.
In this case, all three friends can make their own pizza (Entity) using the same components.
Of course, each of the Properties must be different.
<br>
Likewise, in MapleStory Worlds, you can determine the details of each Component using its Properties.
Shall we look at ChatBalloonComponent, which is used to create chat balloons?

| BalloonScale | FontSize |
| ------------ | -------- | 
|![Entity_Component_Propoerty_5](https://mod-file.dn.nexoncdn.co.kr/bbs/16345465139383799f3fe0d2a45f694d9126878968370.png "Entity_Component_Propoerty_5")<br>You can specify the size of the chat balloon. <br>Increasing the value will make the chat balloon bigger. | ![Entity_Component_Propoerty_6](https://mod-file.dn.nexoncdn.co.kr/bbs/1634546537630dd90d539b29b4f15bf01cdb6eec31a1a.png "Entity_Component_Propoerty_6")<br>You can specify the size of the text in the chat balloon. <br>Increasing the value will make the text bigger.|

This ChatBalloonComponent has two properties: Scale and FontSize. It can be configured differently for each entity depending on each property, and each functions differently.