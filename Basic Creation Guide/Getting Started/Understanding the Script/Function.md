# Course Introduction 
We'll look into how to declare, define, and call functions, along with a brief introduction about functions.
# About Method
A **component** basically consists of a **property** and a **method**. Function is a term used in entity-oriented programming, meaning the collection of codes to perform a certain action.
Functions in MapleStory Worlds can be divided into two categories, **Default Event Function** and **User Defined Function**, depending on the agent calling the function.
Default event functions are called when a certain event occurs before, during, or after the game. It has a set timing for calling and name, being the entry point of all actions.
On the other hand, there is not set timing for calling user-defined functions, and you can rename them to match your purpose.

| **Default Event Method** | **User Defined Method** |
| --- | --- |
| Has set timing for call, and is called within system<br>Cannot be called from another user-defined function.<br>Cannot set function name, return type, and parameter.<br>Starting point of all actions in game. | Can freely set calling timing and name depending on purpose.<br>However, it is not called automatically by the system, so it has to be called from a default event function. |

> <span style="color: #585858">**Learn More**
> To learn more about default event functions, please refer to the guide below!
> [MSW Default Event Functions](https://mod-developers.nexon.com/docs?postId=163{"target":"_self"})</span>
# Declaring and Deleting Method
#### Declaring Method
You can declare functions in the Script Editor.
To learn about declaration, first create a new script component and open Script Editor.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1687496291799b05a128af08748b8bc69f2956c9de929.png "1")
<br>
Clicking on the **[+]** button next to **Function** outputs the list of functions.
Select **New** to add a User Defined Function, and select others to add a Default Event Function.
Click **New** to add a new User Defined Function.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16735978319009a6594de0ce841a687bb1bfc3de66e85.png{"width":"533px"} "2")
<br>
The new function is created with the name **Function1**, which can be renamed to suit your purpose.
Click the name of the function to change its name when text input is enabled.
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/163529625383210fdafccdf4d48389a20f83444d8da12.png "3")
<br>
#### Deleting Method
Click ![script_more](https://mod-file.dn.nexoncdn.co.kr/bbs/16345206995612a35d54577d8466da03a9fe452a5218c.png "script_more") **[Menu]** on the right of the function name, and click **Remove** to remove the function.
![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1637035145550329a2d443fec46c4963e19267a80fbdc.png "4")
<br>
# Setting Method Return Type
You need to set a return type for functions in MapleStory Worlds. Though it is different from the basic Lua syntax, you must set a return type to pass values within a script.
The function return type is set to **void** by default. Click on the return type text to bring up a list of types to choose from. You can select the type that matches your function to change the type.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1673598279122cc02c371fecc424e8c66e4783dc434d8.png{"width":"533px"} "5")
<br>
# Method Definition
You can define how the function will operate upon calling it.
Click the **[+]** button to the left of the function name to open the function implementation part. Write the capacity of the function in the implementation part as code.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1635296282345d1313e4452014e6ca076982525e7746f.png "6")
<br>
# Parameter
In general, a function can receive several types of values upon calling. You can also add a parameter for this.
In the implementation part, you can use a parameter like a local variable. Its default value is assigned when the function is called.
If you didn't put in any value for parameter, **nil** will be received.
#### Adding Parameters
Click **[Menu]** ![script_more](https://mod-file.dn.nexoncdn.co.kr/bbs/16345206995612a35d54577d8466da03a9fe452a5218c.png "script_more") on the right of the function name, and click **Add Parameter** to add a parameter to the function.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1637035161326431e96d64ef1404a80167ba28c6518ec.png "8")
<br>
You can set the type and name for the added parameter.
Click on the type text to open the type list. You can select the type of parameter value to be input.
![8-1](https://mod-file.dn.nexoncdn.co.kr/bbs/1673598346122b15e0c97880c46719660198439894ac4.png{"width":"633px"} "8-1")
<br>
Click the name of the parameter to change its name when text input is enabled.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1635296312284b23737fee9e14a94928f2350da9df274.png "9")
<br>
Adding a parameter once more will add a new parameter on the right side of the previous one.
![10](https://mod-file.dn.nexoncdn.co.kr/bbs/16370352064875922d8bb2fcd41088e3bb33edde931ed.png "10")
#### Deleting Parameters
You can delete added parameters.
Press ![script_more](https://mod-file.dn.nexoncdn.co.kr/bbs/16345206995612a35d54577d8466da03a9fe452a5218c.png "script_more") **[Menu]** on the right of the method name and click **Remove Parameter** to delete a parameter.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1637035223229386a07a202b04ebf99b8bbe5d31070bb.png "11")
<br>
If there are two or more parameters, they will be deleted in order starting from the right.
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/1637035232985d47aef4485fe42a29adeaa73f8a09678.png "12")
<br>
# Calling Method
You can call a different method within a method to execute that other function.
Basically, you can call a method in the format of <span style=\"color: #dc9656\">**ComponentEntity(or service):MethodName(Parameter)**</span>.
#### Calling a Method from the Same Component
When you call a method from the same component, use the format of <span style=\"color: #dc9656\">**self:MethodName(Parameter)**</span>.
(**self** refers to the component entity that includes the method currently operating.)
```lua
--The code is for explanation purposes. It may not operate successfully when applied to a component.
void OnBeginPlay()
{
    local param = "MethodCallTest"
    self:MethodPractice(param)          -- Calls a function with self:MethodName(Parameter)
}
 
void MethodPractice(string parameter)
{
    log(parameter)                     -- Outputs "MethodCallTest" to the Console window
}
```
#### Calling a Method from Another Component or Service
When you call a method from a different component, use the format of <span style=\"color: #dc9656\">**ComponentEntity:MethodName()**</span>.
In general, you can refer to another component entity via the entity that includes that component.
Likewise, you can call a service in <span style=\"color: #dc9656\">**ServiceName:MethodName()**</span> format.
To test the script below, add TestComponent, and connect it to **map01** using **Add component**.
```lua
--The code is for explanation purposes. It may not operate successfully when applied to a component.
void OnBeginPlay()
{
    local param = "MethodCallTest" 
    local mapEntity = _EntityService:GetEntityByPath("/maps/map01")     -- In case of calling a method of the service
    local componentInstance = mapEntity.TestComponent                   -- Refers other component entity
    componentInstance:MethodCallTest(param)                             -- In case of calling a method of a component entity
}
```
#### Exceptions in Calling Methods
Some methods from Lua script or **Static** method from several types can be called <span style=\"color: #dc9656\">**with '.', not ':'**</span>.
```lua
--The code is for explanation purposes. It may not operate successfully when applied to a component.
void OnBeginPlay()
{
    --Lua Function Example
    local tmpTable = {1,2,3,4,5}
    table.insert(tmpTable, 6)                   -- Calls function with '.', not ':'
    local len = string.len("Method Practice")     -- Calls function with '.', not ':'
 
    log(#tmpTable)                              -- Outputs 6 to the Console window
    log(len)                                   -- Outputs 15 to the Console window
    
    --Example of Static function of Vector2 type
    local positionA = Vector2(0,0)
    local positionB = Vector2(10,10)
    local distance = Vector2.Distance(positionA, positionB)  -- Calls function with '.' not ':'
    log(distance)                                       -- Outputs distance between positionA and positionB to the Console window
}
```