# Course Introduction
Scripts in an editor outside of MapleStory World maker must follow mLua syntax.
Let's learn about mLua syntax.
##### Reference Guide
[LocalWorkspace]
[ExtendedScriptFormat]
[mLua Extension]
[Coproduction]
# mLua
This is the programming language for expressing MSW maker's script entries.

# Definition Statement
## Script Definition Statement
You can define scripts by using script definition statements. The following is the script definition statement syntax.

```lua
script Type Name

end
```

If there is an inheritance relationship, you can attach extends to mark which type the inheritance comes from. The following is the definition statement syntax for script with extends.
```lua
script Type Name extends Type Name
 
end
```

## Property Definition Statement
You can define a script's property through a property definition statement. For property definition statements, the right-hand expression must be written with the exception of some types.
SyncTable types are unassignable, and the right-side expressions are omitted.

```lua
property TypeName Name = Expression
```

## Method Definition Statement
You can define methods in a script through a method definition statement. The following is the syntax for method definition statements.

```lua
method Return Type Name Method Name (Parameter Type Name Parameter Name)

end
```

**Use Example**
```lua
method string NewMethod(number num)
    return ""
end
```

## Event Handler Definition Statement
You can define event handlers in a script through an event handlerstatement. The following is the syntax for event handler definition statements.

``` lua
handler Handler Name(Event Type Name Parameter Name)

end
```

**Use Example**
```lua
handler HandleEvent(KeyDownEvent event)

end
```

## Attribute Statement
You can designate various functions to scripts and members using Attribute statements. You can use multiple Attribute statements for a single member. The following is the syntax for the Attribute statement.

```lua
@AttributeName
```

**Use Example**
```lua
@Sync
property string NewValue = nil
```

An Attribute can have parameters. The following is the syntax for an Attribute that has a parameter.

```lua
@AttributeName(Argument)
```

**Use Example**
```lua
@ExecSpace("Server")
method void OnBeginPlay()

end
```

# Attribute
An Attribute is required to apply functions like synchronization, execution control, min values, and max values to script members in mLua.
You can set multiple Attributes to a single member.

#### Sync Attribute
Sets the sync status on a property. The application target is **property**. The application target is  **a property with a Property type that can be synced**. The aplicable types are **Component, Logic, and BTNode**.

* Parameter: **None**

```lua
@Sync
Property string NewValue = nil
```
#### TargetUserSync Attribute
Sets the sync status of certain targets. The application targets are **properties and components with Property types that can be synced**.

* Parameter: **None**

``` lua
@TargetUsrSync
```

#### ExecSpace Attribute
Sets the execution control space for methods and event handlers. Only segments set as strings can be used as parameters. The application targets are **methods and event handlers**. The applicable types are **Component and Logic**.

* Parameter: **string** ("Server", "Client", "ServerOnly", "ClientOnly", "Multicast")

```lua
@ExecSpace("Client")
@ExecSpace("Server")
@ExecSpace("ServerOnly")
@ExecSpace("ClientOnly")
@ExecSpace("Multicast")
```

#### EventSender Attribute
Designates the event sending object of an event handler. The application target is **event handler**, and the EventSender Attribute must be used.
Only segments set as strings can be used as parameters.

* Parameter: **string** ("Self", "Entity", "Model", "LocalPlayer", "Service", "Logic"), **string**(optional)

> <span style="color: #7cafc2">**Tip**
> The value that can be used as the second parameter depends on the string used as the first parameter.
> * "Entity", "Model":  **Entity Id** must be entered as the second parameter.
> * "Service", "Logic":  **Type name** must be entered as the second parameter.
> * "Self", "LocalPlayer": The second parameter isn't used.</span>

```lua
@EventSender("Service", "InputService")
handler handleEvent(keyDownEvent event)

end
```
#### DisplayName Attribute
Sets the name that will be displayed in MSW maker's property editor. The application target is **property**.

* Parameter: **string**

```lua
@DisplayName("Test Value")
property string NewValue = nil
```

#### Description Attribute
Description that displays as a tooltip in MSW maker's property editor. The application target is **property**.

* Parameter: **string**

```lua
@Description("Property Description")
property string NewValue = nil
```
#### HideFromInspector Attribute
Sets whether it will display in MSW maker's property editor. The application target is **property**.

* Parameter: **string**

```lua
@HideFromInspector
property string NewValue = nil
```
#### MinValue Attribute
Sets the minmum value in MSW maker's property editor. The application targets are **number, integer type properties**.

* Parameter: **number**

```lua
@MinValue(2)
property number NewValue = 3
```

#### MaxValue Attribute
Sets the max value in MSW maker's property editor. The application targets are **number, integer type properties**.

* Parameter: **number**

```lua
@MaxValue(10)
property number NewValue = 3
```

#### Delta Attribute
Sets the change value when pressing the left/right buttons in MSW Maker's property editor. The application targets are **number, integer type properties**.

* Parameter: **number**

```lua
@Delta(1)
property number NewValue =3
```

#### MaxLength Attribute
Sets the max length in MSW maker's property editor. The application targets are **string type properties**.

* Parameter: **integer**

```lua
@MaxLength(5)
property string NewVlaue = "test"
```

#### Component Attribute
Sets a script as a Component. The application targets are **scripts**.

* Parameter: **None **

```lua
@Component
script NewComponent extends Component
```

#### Event Attribute
Sets the script as an EventType. The application targets are **scripts**.

* Parameter: **None**

```lua
@Event
script NewEvent extends EventType

end
```

#### Item Attribute
Sets the script as an ItemType. The application targets are **scripts**.

* Parameter: **None**

```lua
@Item
script NewItem extends Item

end
```

#### BTNode Attribute
Sets the script as BTNodeType. The application targets are **scripts**.

* Parameter: **None**

```lua
@BTNode
script NewBTNode extends BTNode

end
```

#### State Attribute
Sets the script as StateType. The application targets are **scripts**.

* Parameter: **None**

```lua
@State
script NewState extends StateType

end
```


#### Struct Attribute
Sets the script as StructType. The application targets are **scripts**.

* Parameter: **None**

```lua
@Struct
script NewStruct

end
```

#### Logic Attribute
Sets the script as Logic. The application targets are **scripts**.

* Parameter: **None**

```lua
@Logic
script NewLogic extends Logic

end
```

# Annotation
Annotations are a type of metadata that is added to code. Writing annotations in the '---@' format can offer various assistance when writing actual code.

#### Type Annotation
Type Annotation types can be explicitly set. It can be used in assignment statements and property definition statements.

```lua
---@type TYPE
```

**Use Example**
```lua
---@type string
local testValue = "test"
```

```lua
---@type string, integer
local strValue, intValue = "str", 5
```

####  Param Annotation
Param Annotation can set the parameter type for targetted functions. Used in function definition statements.

```lua
---@param param_name PARAM_TYPE
```

**Use Example**
```lua
---@param testParamA Entity
local function TestFunction(testParamA)
end
```

```lua
---@param testParamA Entity
---@param testParamB integer
local function TestFunction(testParamA, testParamB)

end
```

#### Return Annotation
Return Annotations can set the return type of the targeted function. Used in function definition statements and method definition statements.
Return Annotations can be assisted by the script assist by setting the return type of the target function.
```lua
---@return TYPE
```

**Use Example**
```lua
---@return Entity
local function TestFunction(testParamA, testParamB)
    return self.Entity
end
```

```lua
---@return Entity, string
local function TestFunction(testParamA, testParamB)
    return self.Entity, "strValue"
end
```

####  Deprecated Annotation
Deprecated Annotations mark something as no longer in use. Used in script definition statements, property definition statements, method definition statements, and event handler definition statements.

```lua
---@deprecated
```

**Use Example**
```lua
---@deprecated
property any NewValue = nil
```

#### Sealed Annotation
Sealed Annotations mark members that will no longer be overridden. Used in property definition statements, method definition statements, and event handler definition statements.

```lua
---@sealed
```

**Use Example**
```lua
---@sealed
property any NewValue = nil
```

#### Description Annotation
Description Annotations are used for descriptions that are shown in tooltips. Used in script definition statements, property definition statements, method definition statements, and event handler definition statements.

```lua
---@description "Test"
```

**Use Example**
```lua
---@description "Test Value"
property any NewValue = nil