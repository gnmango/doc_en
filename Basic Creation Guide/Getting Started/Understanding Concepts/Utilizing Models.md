# Course Introduction
In this course, you'll learn how to extend a model, create a model, and edit model properties.
It would be helpful to refer to the [Model](https://mod-developers.nexon.com/docs?postId=55{"target":"_self"}) guide first.

# Model Extension
<span style="color: #dc9656">**Model**</span> can create an extended model that inherits its own properties, the same component and property values. At first glance, it may seem similar to the concept of copying, but there are important differences.

* <span style="color: #dc9656">**Copy**</span>
    * Original models and copied models exist independently, not affecting each other upon modification of properties. 
* <span style="color: #dc9656">**Extend**</span>
    * The relationship between the underlying model and the extended model is maintained.
    * Changing the properties of the underlying model also applies to the extended model.
    * There are editing restrictions, such as not being able to delete properties inherited from the extended model.


## How to Extend Models
After selecting the desired model in the **Workspace**, create an extended model by clicking **Extend** from the context menu.
Usually it extends **NativeModel**, or an already created model.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/168748927574808ba5739c2514974927e404e117fb58a.png "1")

You can also create a extended model from an Entity placed in **Scene**.
Click **Create Extended Model From Entity** from the context menu of the entity to create an extended model.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/1659699619694576dc357b7da4990b5690ff856b3ca3e.png "2")

Extended models are saved under **MyDesk**. If you look at the components and properties of the extended model, you can see the same thing as the underlying model.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16596999700401c39926d07ac4d4c96e15f28690ad8a4.png "3")

## Characteristics of Extended Models
An extended model inherits components and property default values from its underlying model. For instance, if **Model A** contains **components A, B, and C**, A's extended model, **A1**, also contains **components A, B, and C**, with the value of the property set to **the same initial value as the underlying model**.

![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1659937910290f4135be616534e058a42ea11f624d982.png "4")

><span style="color: #7cafc2">**Tip**
> After extension, the property value of the extended model can be set differently from the initial value of the underlying model.</span>

If adding a new component or deleting an existing component to a underlying model, the change is applied to all extended models from the underlying model.
<br>
Let's look at an example.
* After creating the extended **A1** including components A and B in **Model A**,
    * adding component C to **Model A** will also add component C to **A1**.
    * Likewise, deleting component A from **Model A** will delete component A from **A1**.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16599385674172dc29b11c0d34513b9f9d68582466546.png "5")

However, the extended model cannot delete components received from the underlying model.
For example, extended model **A1** that inherits **components A, B, and C** from **Model A** cannot delete **components A, B, and C**. If you'd like to delete them, you have to delete the components from **Model A**, not **A1**.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/16599392579591f653ee65ab24429960ab8db9df69f1d.png "6")

Unlike how components added to the underlying model are added to the extended model, components added to the extended model are not added to the underlying model. That is, if you add component C to the extended model **A1** of **Model A**, **A1** will include it, but **Model A** will not. You can use this to add a component that only the extended model can have, giving it different characteristics from the underlying model.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1659939801137ef0cf58931c24d0d8a8faff04f187d68.png "7")

## Checking Model Hierarchy Structure
You can check the relationship between models in the **Model Extension Info** from the Property Editor. It shows a hierarchical structure for all the underlying models of the selected model and the extended model at the level immediately below.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/165970035134839cc60633a0e497a91bb193aee9266b2.png "8")

From the context menu of a model in the hierarchy structure, click **Find Model** to select the model in the **Workspace**.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/16599220988603cbb965fd0cf49898454e6c0b7fb2657.png "9")

# Creating a New Model
When creating entities, utilize the **Preset List** model. Models in the **Preset List** inherit the characteristics of **NativeModel** by default. So, to change the model properties of the **Preset List**, you need to modify **NativeModel**. However, if you change the properties of **NativeModel**, all the model properties of the **Preset List** extended from the **NativeModel** will be changed. It's inconvenient because you have to modify the characteristics of the original model again when you need it in the operation process.
<br>
It would be convenient if you could create a new model that is not provided in **Preset List** without modifying **NativeModel**. The new model is independent of **NativeModel**, so it can be freely modified and transformed.
You can follow the steps below to create a new model.
* <span style="color: #dc9656">**Method 1**</span>
    * Click the **[+]** button - **Create Model** in **Workspace**.
* <span style="color: #dc9656">**Method 2**</span>
    * Click **Create Model** in the context menu of **Workspace**.

![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1687494524198d8a118789c98467984663edb5051443b.png "10")

Since the new model does not contain any components, placing the model as an entity does not make much sense. Depending on the intended use of the new model you create, you need to add and use basic components. Let's add a basic component as follows according to whether you want to create a world entity or a UI entity through a new model.
| World Entity | UI Entity |
|---|---|
| <ul> <li>TransformComponent</li> <li>SpriteRendererComponent</li></ul> | <ul> <li>UITransformComponent</li> <li> SpriteGUIRendererComponent</li></ul> |

You can add an empty model from the **Workspace** and then add desired components, like above, but you can also turn a placed entity in **Scene** into a new model. It's convenient as there is no need for additional editing because the model consists of the entity with the component and property values already modified.
In the context menu of the placed entity, you can make an Entity into a model by clicking **Create Model From Entity**.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1659922427878a28dedc49ae74672b7c707d7cde5d520.png "11")

# Creating Model with UI Entity
You can make a UI Entity into a model.
For example, let's say we're making **Button01** and **Button02** in **NewUIGroup** into a model.
![uimodel01](https://mod-file.dn.nexoncdn.co.kr/bbs/1667281846129e4eac692fe634d5f94367e7eb8c080ff.png "uimodel01")

Click **Create Model From Entity** to create **Model_UISprite** in **Workspace - MyDesk** from the context menu of **UISprite**, a parent entity of **Button01** and **Button02**.
![uimodel02](https://mod-file.dn.nexoncdn.co.kr/bbs/1667281948281b5607277f7cd42eaa18987580c9e546a.png "uimodel02")

Modeling your UI like this allows you to include it in your MSW package, which can also be leveraged when moving your UI to another world.

> <span style="color: #585858">**Learn more**
>Please refer to the guide [Utilization of MSW Packages]](/docs/?postId=647{"target":"_self"}) to learn more about using packages. </span>

However, you need to be careful when modeling UI entities as there are some restrictions.
* Not all **UIGroup** entities can be modeled.
* **UIChat** and **UIJoystick** as the **Native** state within **Default Group** cannot be modeled.
* Some non-extensible models with **Break** in **Extension Info** cannot use the model extension function.

# Creating Model Entity
To make a new model or an extended model into an entity, drag the desired model from the **Workspace** to **Scene**.
![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1656034659249381775e32b3b4809aacebc9675b6a737.png "13")

# Editing Models
## NativeModel and Preset List
In the [Model] guide, we learned that entities are created from models, and that **Preset** from the **Preset List** is also another form of model.
Models in the **Preset List** are extended models of **NativeModel**, so they inherit properties of **NativeModel** by **Preset List** category. Therefore, if you change the **NativeModel** of each category, the change will apply to models in the **Preset List**.
For example, if you'd like to change a model's property from the **Preset List - Object** category, you can edit the property of **NativeModel - MapObject**. If you want to create an **Object** model twice as large as an entity, you just set **MapObject** model's **Scale** value to <span style="color: #dc9656">**2**</span>.

![14](https://mod-file.dn.nexoncdn.co.kr/bbs/166026562910074f2778c33374262853f0e05fa39d5bb.png "14")

## Model Property
At the top of the Property Editor of a model or entity are properties that do not belong to any component. This is the <span style="color: #dc9656">**Model Property**</span>. Model properties are like a bookmarked collection of properties that belong to each model or entity included in a component.
Model property is a projection of a component's property. If values change in the model property, the corresponding property value in the component will change as well. For instance, if you change the **Scale** value, one of the model properties of **Object-1** entities, to<span style="color: #dc9656">**x = 10, y = 10, z = 10**</span>, you'll see that the **Scale** values of **TransformComponent** change as well.
![16](https://mod-file.dn.nexoncdn.co.kr/bbs/1660265944245a95af2dcab5a4f26b5a3dc3b8793ec81.png "16")

Creators can edit the model property to their convenience.

1. Select a model you're going to edit and then click ≡ at the top of the model property.
2. From the menu, click **View Model Property** to shift to model property edit mode.
![17](https://mod-file.dn.nexoncdn.co.kr/bbs/166026617550958db5ba62369422292141e0fe5384465.png "17")

3. You can tick the checkbox on the right of each property to designate it as a model property.
![18](https://mod-file.dn.nexoncdn.co.kr/bbs/16393866635475d8a437cbd164248828822fe5ece17a3.png "18")

4. If you select "View Component Property" from the model setting menu, you'll see that the checked properties appear as model properties.
![19](https://mod-file.dn.nexoncdn.co.kr/bbs/16602672025747fe01c63b5fb40a19a5b048aaab4dfee.png "19")