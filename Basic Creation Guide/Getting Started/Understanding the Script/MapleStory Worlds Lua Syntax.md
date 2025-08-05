# Course Introduction
We'll discuss how to use frequently used syntax when writing scripts with Lua in MapleStory Worlds.
We recommend reviewing [MapleStory Worlds Lua Basics](/docs?postId=822{"target":"_self"}) before reading this guide.

# Conditional Statements
Usually, if you look at the logic of a program, it is often made up of a combination of loops and conditional statements. As such, conditional statements and loop statements are the basic constructs of programming.
<br>
Let's look at the conditional statement first.
* **if**
    * This is a basic conditional statement. If the condition is **true**, the content between **then** and **end** are executed.
* **elseif**
    * This is a conditional statement added to **if**. A **basic if statement** is required to use it.
    * If the condition of **if** is **false**, then the condition of **elseif** is checked.
    * **elseif** is not a required element.
* **else**
    * A **basic if statement** is required to use it.
    * If the condition of **if, elseif** is **false**, then the content of **else** is executed.
    * **else** is not a required element.

<br>
Let's look at an example.
```lua
[server only]
void OnBeginPlay()
{
    local a = 7
    
    -- if condition then execution end
    if a > 10 then
        log("big")
    
    -- elseif condition then execution
    elseif a > 5 then
        log("middle")
    
    -- else execution
    else
        log("little")
    end
}
```

Looking at the above conditional statement, the condition is set as follows.
* If a is greater than 10, output **big**
* If a is less than 10 but greater than 5, output **middle**
* Otherwise (i.e. if a is less than 5) output **little**

Since a is 7, executing the above will output **middle**.

# Loop Statements
Let's take a look at the format of the loop statement frequently used in Lua.

## while Statements
The **while statement** evaluates the conditional expression first and if the condition is **false** or **nil** then the loop ends. If the condition is **true** then it will execute the looping code.
Let's look at an example.

```lua
-- while condition do loop content end

local a = 10

while(a <= 15)
do
    log(a)
    a = a + 2
end

--[[Result:
10
12
14
]]
```

## repeat Statements
**repeat - until loop** is a statement that repeats until the condition becomes true. A code block is executed at least once because it must be executed to determine if the conditional is true. 
Let's look at an example.
```lua
local countdown = 5

repeat
    countdown = countdown - 1
    log(countdown)
until countdown == 0

log("The countdown is over.")

--[[Result:
4
3
2
1
0
The countdown is over.
]]
```

## for Statements
**for statement** is an iterative search based on a number or number of items.

### Numeric For Loops
 **Numeric For Loops** define the initial value, final value, and incremental value. The incremental value is added and repeatedly calculated from the initial value to the final value.

Let's look at an example.
```lua
-- for variable = Initial value, final value, incremental value do end
-- If the incremental value is 1, 1 can be omitted.

for i = 10, 15
do
    log(i)
end

--[[Result:
10
11
12
13
14
15
]]

for i = 20, 10, -2
do
    log(i)
end

--[[Result:
20
18
16
14
12
10
]]
```


### Generic For Loops
**Generic For Loops** iterates over the items in a table that are not a series of numbers.
A generic for statement allows you to execute the code for each table, and makes it easy to use each item in your code.
A generic for statement requires either a function or an iterator. `ipairs()` returns an iteration of Array and `pairs()` returns an iteration of Dictionary.

#### Array
`ipairs()` returns the indices and values of an array iteratively. 
Let's look at an example.
```lua
local testArray = {"A", "B", "C", "D", "E"}
for index, value in ipairs(testArray) do
    log(index, value)
end

--[[Result:
1 A
2 B
3 C
4 D
5 E
]]
```

#### Dictionary
`pairs()` repeatedly returns **key - value** of the dictionary. Remember that the order in which a dictionary is returned is determined internally, so the creator's order is not guaranteed.
Let's look at an example.
```lua
local testdictionary = {name = "Tom", age = 20, favoriteGame = "MapleStory Worlds", job = "Programmer"}

for key, value in pairs(testdictionary) do
    print(key, value)
end

--[[Result:
favoriteGame MapleStory Worlds
name Tom
job Programmer
age 20
]]
```

## Stop Repeat
To stop an iteration, use the `break` command. 
Let's look at an example of stopping an infinite loop.
```lua
local secondsElapsed = 0
local timeout = 5

while true do
	log(secondsElapsed.."seconds")
	wait(1)
	secondsElapsed = secondsElapsed + 1

	if secondsElapsed == timeout then
		break
	end
end

log("5 seconds have passed. Go to the next map.")

--[[Result:
0 seconds
1 second
2 seconds
3 seconds
4 seconds
5 seconds have passed. Go to the next map.
]]
```