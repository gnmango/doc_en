# Course Introduction
**Annotation** is a kind of metadata that you can use by adding to a code.
While the Comment using the `--` symbol in the script serves only as a reference for writing the script. Writing an **Annotation** in the format of `---@` can enable the help of script assists such as **Quick Info**, **Signature Helper**, **Code Completion**, etc.
Let's take a look at the types of **Annotation** offered in MapleStory Worlds and how to utilize them through this course.

> <span style="color: #585858">**Learn more**
> Check out the **Script Assist** item in the [Script Editor Orientation](/ko/docs/?postId=56{"target":"_self"}) guide if you want details on Quick Info, Signature Helper, and Code Completion.</span>

# Specifying Variable Type: Type Annotation
Script Assist can provide support in the Script Editor when the variable type is explicitly specified as **Type Annotation**. **Type Annotation** is applied to an assigned line.

## Usage
**Type Annotation** is used as follows.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/166788442640966b092935ab7482d948c6fc39188d48c.png "1")

```lua
---@type string
local testValue = "test"
```

## Example
##### Lua Functions that Cannot Deduct Return Type
Likewise, use **Type Annotation** when you retrieve a resulting Lua function value that cannot deduct return type as follows.
```lua
local function TestFunction()
    return "strValue"
end

local strValue = TestFunction()
```
<br>

| Before the application of Type Annotation | After the application of Type Annotation |
| :---: | :---: |
| ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/166788677395339c9be52267845a1a38273071868699e.png "4") | ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1667886963732dbed1d6d12984f79bd1922548876854f.png "5") |
<br>
##### When Multiple Values are Assigned
Likewise, use **Type Annotation** when multiple values are assigned as follows.
```lua
local function TestFunction()
    return "strValue", 5
end

local strValue, intValue = TestFunction()
```
<br>
| Before the application of Type Annotation | After the application of Type Annotation |
| :---: | :---: |
| ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1667887186926acb2c0f5eac64f118c480078c23b2ac4.png "6") | ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1667892117117387cdcbbe65e4aa08d76b2fc548d48ad.png "7") |
<br>
You can specify partially when multiple values are assigned. Specifying a rear value alone is impossible because the specified type is applied from a front value.

| Before the application of Type Annotation | After the partial application of Type Annotation |
| :---: | :---: |
| ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1667887186926acb2c0f5eac64f118c480078c23b2ac4.png "6") | ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/166789228646766e78448a0794b3cb971c4492954f34f.png "8") |


## Specifying Generic Type
To specify a generic type, follow this example.
```lua
---@type TYPE
```

##### Usage Example
```lua
---@type string
local testValue = "test"
```
<br>
```lua
---@type string, integer
local strValue, intValue = "str", 5
```
<br>
Various types such as event, component, entity, and Enum besides simple primitive types can be specified as **Type Annotation**.

## Specifying Table Type
To specify the type of a Table, follow this example.
```lua
---@type table<KEY_TYPE, VALUE_TYPE>
```

##### Example
```lua
---@type table
local testTable = {}
```
<br>
```lua
---@type table<Entity>
local testTable = {}
```
<br>
```lua
---@type table<string, Entity>
local testTable = {}
```
<br>
For example, you can check related information in Quick Info when specifying **Type Annotation** as follows.
```lua
local function TestFunction()
    local testTable = {}
    testTable[1] = "A"
    testTable[2] = "B"
    testTable[3] = "C"
end

---@type table<integer, string>
local tableValue = TestFunction()
```

![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1667895925609146ce9442a4f4e6da96fc1aa6932f744.png "9")

## Specifying Function Type
To specify the type of a Function, follow this example.
```lua
---@type function(param_name: PARAM_TYPE):RETURN_TYPE
```

##### Example
```lua
---@type function()
local testFunction = function() end
```
<br>
```lua
---@type function(testParamA:string, testParamB:Entity)
local testFunction = function(testParamA, testParamB) end
```
<br>
```lua
---@type function(testParamA:string, testParamB:Entity):AttackComponent
local testFunction = function(testParamA, testParamB) return atkComponent end
```
<br>
```lua
---@type function(testParamA:string, testParamB:Entity):AttackComponent, string
local testFunction = function(testParamA, testParamB) return atkComponent, "strValue" end
```
<br>
```lua
---@type (function(testParamA:string, testParamB:Entity):AttackComponent, string), Entity
local testFunction, testEntity = function(testParamA, testParamB) return atkComponent, "strValue" end, self.Entity
```
<br>
For example, you can get help from Signature Helper when specifying **Type Annotation** as follows.
```lua
local function TestFunction()
    return function(paramA, paramB)
        return "strValue", false
    end
end

---@type function(paramA:string, paramB:AttackComponent):string, boolean
local test = TestFunction()
```

![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1669633527104fde7bdfed20d4bdb802bdec69d11e4fb.png{"width":"770px"} "10")
<br>
You can also check the specified type in Quick Info.
```lua
---@type function(paramA:string, paramB:AttackComponent):string, boolean
local test = TestFunction()
local strValue, boolValue = test("param", self.Entity.AttackComponent)
```

![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1667893299258103b46b267b745f882a40a4cbb70c568.png "11")
<br>
Divide each type into groups with parenthesis when you want to specify other values along with a function that returns multiple values.
```lua
---@type (function(paramA:string, paramB:Entity):AttackComponent,string), Entity
local testFunction, testEntity = function(testParamA, testParamB) return atkComponent, "strValue" end, self.Entity
```


# Specifying Parameter Type: Param Annotation
Script Assist can provide support when the parameter type of a target function is specified using **Param Annotation**. **Param Annotation** is applied to a function-defining line.

Use the following syntax for **Param Annotation**:
```lua
---@param param_name PARAM_TYPE
```

##### Usage Example
```lua
---@param testParamA Entity
local function TestFunction(testParamA)
end
```
<br>
```lua
---@param testParamA Entity
---@param testParamB integer
local function TestFunction(testParamA, testParamB)

end
```
<br>
Basically, type deduction is impracticable because one cannot know what type will be inserted into a parameter when defining Lua function. Let's use **Param Annotation** in this case.

| Before the application of Param Annotation | After the application of Param Annotation |
| :---: | :---: |
| ![12](https://mod-file.dn.nexoncdn.co.kr/bbs/166789361996144188e4993ee4a6a8a4d0bddeb7229ae.png "12") | ![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1667893634012f417287ece184076a6dae5259609f80e.png "13") <br>![18](https://mod-file.dn.nexoncdn.co.kr/bbs/1668671082158866bdb58d8bf4710b2df2fab3d00ce47.png "18") |

# Specifying Return Type: Return Annotation
Script Assist can provide support when the return type of a target function is specified using **Return Annotation**. **Return Annotation** is applied to a function-defining line.

Use the following syntax for **Return Annotation**.
```lua
---@return TYPE
```

##### Example
```lua
---@return Entity
local function TestFnction(testParamA, testParamB)
    return self.Entity
end
```
<br>
```lua
---@return Entity, string
local function TestFunction(testParamA, testParamB)
    return self.Entity, "strValue"
end
```
<br>
| Before the application of Return Annotation | After the application of Return Annotation |
| :---: | :---: |
| ![15](https://mod-file.dn.nexoncdn.co.kr/bbs/1667893987810443dd08fe02842e19a29e8d2b7d14b54.png "15") | ![16](https://mod-file.dn.nexoncdn.co.kr/bbs/1667894003787ca547259d8d0414294064f26fd6569ac.png "16") |
