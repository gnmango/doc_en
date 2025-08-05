# Course Introduction
In MapleStory Worlds, scripts are written using a language called **Lua**. 
This time, we'll look at the basic usage of **Lua**, and examples of using it in MapleStory Worlds.

# Variable
A variable is both a memory space that saves data and a container that holds values. 

## Range of Variables
Variables used in Lua are divided into the following two types.

| Variable | Description |
| --- | --- |
| Global variable | A variable that can be accessed from anywhere. If used without any declaration, it is a global variable. <br><span style="color: #dc9656">**However, using global variables is not recommended in MapleStory Worlds.**</span> <br>If you have a value you want to use like a global variable, use [Property] in the script.|
| Local variable | A variable that can only be accessed within the same function or statement.<br>It is valid only in the scope in which the variable is defined, and is destroyed when the execution of the scope is finished. |
<br>
![scope](https://mod-file.dn.nexoncdn.co.kr/bbs/16764300045452066dac334934dc2abbf59f6e9b9fea1.png "scope")
As shown in the picture above, **Property a** can be referenced throughout the script.
**Local variable b** is valid in **B area**, **local variable c** is valid only in **C area**, and is destroyed when the execution of the scope is finished.
<br>
Let's explore the scope of a variable through an example.
<br>
1. First, in the Maker, create a new script component called <span style="color: #dc9656">**LuaTest**</span>, and in **Hierarchy - common**, add the **LuaTest** component.
<br>
2. Add properties as follows.
    ```lua
    Property :
    [Sync]
    integer testP = 0
    ```
    <br>
3. Add the `OnBeginPlay()`, `testFunction()` functions and enter as follows.
The original Lua syntax uses the **print** command to output, but MapleStory Worlds uses the `log()` function that has processed this, so we will use the `log()` function in future examples as well.
<br>
Enter as follows and click the **[Start]** ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") button in the Console window to output **log**. You can see that each comment value is output.
    ```lua
    Property:
    [Sync]
    integer testP = 0
        
    Method:
    [server only]
    void OnBeginPlay()
    {
        log(self.testP) -- 0
        self.testP = self.testP + 100
        log(self.testP) -- 100
        
        -- Define the valid local variable a within the OnBeginPlay() function.
        local a = "A"
        log(a) -- A
        
        if true then
        	-- Change the value of local variable a.
        	a = 1
        	
        	-- Define the local variable b that is valid only within the conditional statement.
        	local b = "B"
        	log(b) -- B
        end
        
        log(a) -- 1
        
        --[[The local variable b, which was only valid at the scope of the conditional statement, is destroyed,
        so the value of b is nil in the scope of the function.]]
        log(b) -- nil
        
        -- Call the testFunction().
        self:testFunction()
    }
    
    void testFunction()
    {
        -- testP defined in Property can be accessed throughout the script.
        log(self.testP) -- 100
        
        --[[The local variable a, which was only valid at the scope of OnBeginPlay(), is destroyed,
        so the value of a is nil here.]]
        log(a) -- nil
    }
    ```

## Variable Type
Variables in Lua have types internally, but the type is not specified when creating a variable. The value put into a variable determines its type.

| Variable type | Description |
| --- | --- |
| nil | No value. <br>In logical operations, nil is false and nil in global variables and table keys are discarded. |
| boolean | Only true and false values. <br>It is not converted to numbers of 0, 1. |
| number | 64-bit real numbers. |
| integer | 64-bit integer numbers. |
| string | A string. <br> Use double quotation marks (") or single quotation marks ('). |
| table | A composite data type. <br>It can be used in both Array and Dictionary formats. |

You can check the type of a variable with the type() function.
For example, if you enter the following and output **log**, the value of the comments will be output respectively.

```lua
local a = 1
log(type(a)) -- number
	
a = 1234.5678
log(type(a)) -- number

a = "hello"
log(type(a)) -- string

a = (0 > 10)
log(type(a)) -- boolean

a= nil
log(type(a)) -- nil
    
a = {}
log(type(a)) -- table

a = {"Lua", "Test"}
log(type(a)) -- table
```

MapleStory Worlds provides a separate data type in addition to the types above. You can check the details from the **Property type** item of the [Property] guide.

# Table
In Lua, both Array and Dictionary formats are tables.
When dealing with lots of data, tables are usually used and declared in the format of **{ }**. 
```lua
local table = {}
```

## Array
Arrays are used quite a lot in Lua, and are useful for collecting and saving data.

### Making an Array
As an example, make it in the format below.
```lua
local testArray = {"A string", 1.234, self.Enable}
```

### Read Value
Arrays can be accessed by index [no].
Most programming languages use <span style="color: #dc9656">**0**</span> as the first index, but Lua uses <span style="color: #dc9656">**1**</span>.
```lua
local testArray = {"A string", 1.234, self.Enable}
log(testArray[1]) -- A string
log(testArray[2]) -- 1.234
log(testArray[3]) -- true
```

### Composite Value
To write or change a value in the array, access it by index [no] and enter the value.
```lua
local testArray = {11, 22, 33}

testArray[2] = "bb"
testArray[4] = "44"

log(testArray[2]) -- bb
log(testArray[3]) -- 33
log(testArray[4]) -- 44
```

### Add Value
Here are two ways to add values to an array.
* Use the `table.insert()` function.
* Use `array[#array+1]` syntax.

```lua
local testArray = {"A", 1.234}

table.insert(testArray, "C")
testArray[#testArray+1] = "D"

log(testArray[3]) -- C
log(testArray[4]) -- D
```

<br>
To add a value to the middle of the array, write the index value you want to add to the second argument of the `table.insert()` function.
```lua
local testArray = {"A", "B"}

-- Add "and" to index 2
table.insert(testArray, 2, "and")

log(testArray[1]) -- A
log(testArray[2]) -- and
log(testArray[3]) -- B
```

### Delete Value
To remove a value from an array, use the `table.remove()` function.
```lua
local array = {11, 22, 33, 44, 55}

-- Delete index 1
table.remove(array, 1)
log(array[1]) -- 22
```

<br>
> <span style="color: #585858">**Learn more**
> You can check various functions that can be used in tables in the Script Editor.
> ![02](https://mod-file.dn.nexoncdn.co.kr/bbs/16703984668191d7f941283f84d42b1b97146d6773bce.png "02")</span>

## Dictionary
A dictionary is a table of the format **key - value**. It is mainly used for fast retrieval of data from information structures.

### Making a Dictionary
When making a dictionary, define it in the format of `key = value` and separate it with a comma (,).
```lua
local myDictionary = {a = "myNumber", myNumber = 5, myString = "TTT", myBoolean = true}
```

### Read Value
The dictionary reads in the format shown below.
```lua
local myDictionary = {a = "myNumber", myNumber = 5, myString = "TTT", myBoolean = true}

log(myDictionary.myNumber) -- 5
log(myDictionary.myString) -- TTT
log(myDictionary["myBoolean"]) -- true
```

### Composite Value
**Key** is also used to define or rewrite values in a dictionary.
```lua
local myDictionary = {name = "Tom", age = 20}

-- Change value
myDictionary.name = "Katy"
myDictionary.age = 25

-- Key - Add value
myDictionary.favoriteGame = "MapleStory Worlds"

log(myDictionary.name) -- Katy
log(myDictionary.age) -- 25
log(myDictionary.favoriteGame) -- MapleStory Worlds
```

### Key - Delete Value
To delete **Key - Value**, set the value of **Key** to **nil**.
```lua
local myDictionary = {name = "Tom", age = 30, favoriteGame = "MapleStory Worlds", job = "Programmer"}

-- Key - Delete value 
myDictionary.job = nil

-- Iterate through the entire myDictionary and output the key and value
for key, value in pairs(myDictionary) do
    log(key, value)
end

-- [[Result:
age 30
name Tom
favoriteGame MapleStory Worlds
]]
```

> <span style="color: #585858">**Learn more**
> For the for statement used in the example above, refer to the **for statement** part of the [MapleStory Worlds Lua Syntax] guide.</span>

## Table Reference
When you save a table to a new variable, Lua does not make a copy of that table. Instead, the variable refers to the original table.
```lua
local original = {10, 100, 1000}
local reference = original

log(reference[1], reference[2], reference[3]) -- 10, 100, 1000

-- Modify the original value
original[1] = "A"
original[2] = "B"

log(reference[1], reference[2], reference[3]) -- A, B, 1000
```

# Operator
These are the operators available in Lua.

## Arithmetic Operator
This operator is used to perform arithmetic operations.

| Operator | Description |
| --- | --- |
| `+` | Add |
| `-` | Subtract |
| `*` | Multiply |
| `/` | Divide |
| `//` | Divide and take only the quotient |
| `%` | Divide and take the remainder |
| `^` | Quotient |

For example, if you enter the following and output **log**, the value of the comment will be output.
```lua
log(1 + 1) -- 2
log(1 - 1) -- 0
log(5 * 5) -- 25
log(13 / 5) -- 2.6
log(13 // 5) -- 2
log(13 % 5) -- 3
log(2 ^ 4) -- 16.0
```

## Relational Operator
An operator used to determine a relationship. The result is a boolean.

| Operator | Description |
| --- | --- |
| `=` | Declaration |
| `==` | Same |
| `~=` | Different |
| `>` | Greater |
| `<` | Less |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

For example, if you enter the following and output **log**, the value of the comment will be output.
```lua
local a = 1
local b = 2
log(a == b) -- false
log(a ~= b) -- true
log(a > b) -- false
log(a < b) -- true
log(a >= b) -- false
log(a <= b) -- true
```

## Logical Operator
Logical operators return true or false. Returns true if the argument is non-false and non-zero.

| Operator | Description |
| --- | --- |
| `and` | A and B. True if both A and B are true. |
| `or` | A or B. A or B must be true to be true |
| `not` | True if false, false if true |

## Other Operators
Here are some other useful operators.

| Operator | Description |
| --- | --- |
| `#` | Length of array |
| `..` | Concatenate strings|

For example, if you enter the following and output **log**, the value of the comment will be output.
```lua
local a = {"HELLO", "MapleStory Worlds"}

log(#a) -- 2
log(a[1].."! "..a[2]) -- HELLO! MapleStory Worlds
```

# Function
A function is a code block that can be executed multiple times as a command. It can be attached to an event or assigned as a callback. For more details, please refer to the [Function] guide.

# Comments and Annotations
Comments are shown below.

| Comment | Description |
| --- | --- |
| `--` | One line comment |
| `--[[]]` | Multi-line comment |
| `---@` | Annotation is a kind of metadata that you can use by adding to a code. <br>For more details, please refer to the [Annotation] guide. |

# Summary
We've examined the basic usage and examples of Lua. Next, let's learn how to use frequently-used syntax when writing scripts in Lua. Related information can be found in [MapleStory Worlds Lua Syntax].