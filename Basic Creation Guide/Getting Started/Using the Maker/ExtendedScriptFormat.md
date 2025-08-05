# Course Introduction 
Let's find out how to use extended script format and about the mlua file format.

##### Reference Guide 
* [Coproduction]
* [Coproduction Group Member's Level and Permission]
* [WorldConfig]
* [LocaWorkspace]

# Extended Script Format Introduction
If you enable **Extended Script Format**(UseExtendedScriptFormat) from your Local Workspace, an **mlua file** that pairs with the metadata file will be created.  Since the extended script format is plain text, it would be easier for you to find the source of the problem if a collision occurs when merging files.

><span style="color: #dc9656">**Caution!**
> **UseExtendedScriptFormat is a preview feature, and it may not be stable. There may be unintended bugs, and the feature may be removed via future updates. It is advised that you create a backup file or a backup World.**</span>

# Enabling Extended Script Format
1. Activate the feature via **WorldConfig - LocalWorkspace - UseExtendedScriptFormat**.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1718965223446aece1d91f1aa4b5289f10318668c8d55.png{"width":"450px"} "1")
2. Check if the Environment folder and mlua file have been created in the LocalWorkspace folder.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/172717900378801c962c5094344a8b3ae9802c6c9168a.png{"width":"540px"} "2")
# Disabling Extended Script Format
Once you activate UseExtendedScriptFormat, you cannot deactivate it. Instead, you'll need to disable Local Workspace.
You can disable Local Workspace as follows.

1. Revert the local file to the version where the problem had not occurred yet.
2. Select **ReimportAll** in MapleStory World Maker.
3. Select **File - Sync To Remote Workspace** to synchronize the current local files to Remote Workspace.
4. Open **Global - WorldConfig.config** in your local folder.
5. Change **"UseExtendedScriptFormat": true** to **"UseExtendedScriptFormat": false** and then save.
6. Select **ReimportAll** in MapleStory World Maker again.
7. Check if UseExtendedScriptFormat had been disabled in the Maker.
8. Deactivate **LocalWorkspace**.
9. Click **File - Revisions** and then select and revert to the previous version where the problem had not occurred yet.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1727179083087cc4b9b000bb54765b8250e4d35617d07.png{"width":"550px"} "3")

# Introduction to mlua File
Files in mlua format are defined in a specific way.

> <span style="color: #dc9656">**Caution!
> You should refrain from using features that are not supported in the Maker on mlua files. It may work at the current state, but it may be changed in future updates.**</span>

#### Script Entry Definition
Script entries can be inherited from one parent and only among script entries of the same type. Among the script entries, **Component, Item, BTNode, State, Logic** requires information of the parent. 
Scripts that can be defined via script entries are as follows.

* Component
* Item
* BTNode
* State
* Logic
* Event
* Struct

```lua
@Script Type
ScriptName extends Inheritance
end
```

This is an example of a script entry definition.
```lua
@Component
script NewComponent extends Component
end
```

#### Property Definition
The property definition composition is as follows.
```lua
PropertyType PropertyName = default value
```

This is an example of a property definition.
```lua
Property number NewProperty = 5
```


#### Method Definition
The method definition composition is as follows.

```lua
Method ReturnType MethodName (ParameterType ParameterName)
```

This is an example of a method definition.
```lua
method void TestMethod(float parameter)
```

#### Event Handler Definition
Event handler requires the EventSender attribute.

```lua
@EventSender("Self")
Handler HandlerName(EventType event)
```

This is an example of an event handler definition.
```lua
@EventSender("Self")
handler HandleButtonClickEvent(ButtonClickEvent event)
```

#### Attribute Definition
This can be omitted if the attribute's parameter is not required.

```lua
@Name()
@Name(Parameter)
```


This is an example of an attribute definition.
```lua
@EventSender()
@EventSender("Self")
```
