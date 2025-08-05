# # Course Introduction
You can create a world more efficiently by using a model's properties. Let's learn about the model editor you can use to edit a model.

# Intro to Model Editor
You can create and edit models more efficiently using the model editor. You can isolate and edit models in a space separate from the map or UIGroup by entering the model editor.
Map editing and test play becomes unavailable while using the model editor.

#### Entering the Model Editor
The Scene panel changes, and the Hierarchy window changes to display the hierarchical structure of the model being edited when entering the model editor. 
There are two ways to enter the model editor.

1. **Entering from Workspace**

    1. Create a new model from **Workspace**.
    2. Open the model editor by double-clicking.

2. **Entering from Hierarchy**

    1.Create an entity in **Hierarchy** and open the context menu, select **Create Model from Entity**, and turn the entity into a model.
    2. Select the created model and select **context menu - Edit Model**.

#### Screen Intro
Activating the model editor changes the Scene panel to the model editing screen, and the main button's function changes.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/175211224021236c5261d9da34c7d8f44febbb48c0e14.png "ModelEditor")

|Number | Name | Description|
|---|---|---|
|![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_01.jpg "1") | Scene | Save the model that's being created and return to Scene.|
|![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_02.jpg "2") | NewModel | The name of the model that is being edited. |
|![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_03.jpg "3") | Discard | Discard the edits made before saving.|
|![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_04.jpg "4") | Save | Save the edited model.|
| ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_05.jpg "5")| UI Button | Used when making a UI model. |
| ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_06.jpg "6")|Property Window | Add and delete components within the model that's being edited and change the property value. You can view the extension information for extended models.<br><ul><li>**Model Extension Info**: View a model's extension structure at a glance. When you need to edit a model that's on another level from the one being worked on, you can access it by selecting **Context Menu - Find Model**.</li></ul>|
|![[object Object]](https://mod-file.dn.nexoncdn.co.kr/storage/numbers/NO_07.jpg "7")|Hierarchy Window | View a model's hierarchical structure from this window. You can add new child entities or change the hierarchical structure. |

# Editing Models
You can edit 3 types of models from the model editor.

1. Models created by the creator
2. Models with extended native models
3. DefaultPlayer

#### Adding/Deleting Components
You can add or delete new components from the property window.

#### Adding Child Entities/Changing Hierarchical Structures
You can create models with a hierarchal structure from the model editor. You can drag parent-child relationships in the **Hierarchy** window to set them.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17521081010150fea6ccf5f314ccea8df8b2181341c3e.png "3")

> <span style="color: #585858">**Learn More**
> You can change hierarchal structure from a scene that's being edited and use Create Model From Entity to create a model with a hierarchal structure. Please confirm from Hierarchy.</span>

#### Notes for Editing
Models placed in a Scene are treated as individual entities. Placed entities aren't dependant on models, which must be noted. Therefore, entities that are placed on a map aren't affected even when the original model is edited and has its content changed. If you want to apply the edited model to the model-based entity that has been placed, you can **Revert** the entity to apply it in the same way.

If a created model is being edited after being placed, newly added entities and changed hierarchal structures aren't applied to the placed model-based entity. Only the added/deleted components and property values from the model editor are changed.

> <span style="color: #7cafc2">**Tip**
> You can delete components of a model placed in a Scene or change its hierarchal structure only from the model editor.</span>