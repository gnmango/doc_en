# Course Introduction
MapleStory Worlds provides support for extended syntax which is not natviely supported by Lua but is commonly used in other programming languages. This course aims to introduce you to the Lua extension syntax and provide examples of its usage supported in MapleStory Worlds.

# Continue
The keyword **continue** is used to skip the remaining commands within the current iteration of a loop and move on to the next iteration.
```lua
local oddNumbers = {}
for i = 1, 10 do
    if i % 2 == 0 then
        continue -- Even numbers skip the remaining operation
    end
    print("odd number : ", i)
    table.insert(oddNumbers, i)
end
-- Output :
-- odd number : 1
-- odd number : 3
-- odd number : 5
-- odd number : 7
-- odd number : 9
```

# Compound Assignment Operators
Compound assignment operators can perform an operation and an assignment simultaneously.

#### Addition Assignment Operators
The addition assignment operator `+=` assigns the addition result.
```lua
self.Entity.TransformComponent.Position.x += 0.1 -- Adds x value of the Position by 0.1
local num = 1
num += 2 -- Assigns the result of adding 2 to num
print(num)
-- Output :
-- 3
```

#### Subtraction Assignment Operators
The subtraction assignment operator `-=` assigns the subtraction result.
```lua
self.Entity.TransformComponent.Position.x -= 0.1 -- Subtracts x value of the Position by 0.1
local num = 1
num -= 2 -- Assigns the result of subtracting 2 from num to num
print(num)
-- Output :
-- -1
```

#### Multiplication Assignment Operators
The multiplication assignment operator `*=` assigns the multiplication result.
```lua
local num = 2
num *= 3 -- Assigns the result of multiplying num by 3 to num
print(num)
-- Output :
-- 6
```

#### Division Assignment Operators
The division assignment operator `/=` assigns the division result.
```lua
local num = 5
num /= 2 -- Assigns the quotient of dividing num by 2 to num
print(num)
-- Output :
-- 2.5
```

#### Floor Division Assignment Operators
The floor division assignment operator `//=` assigns the quotient after performing the division operation.
```lua
local num = 5
num //= 2 -- Assigns the quotient of dividing num by 2 to num
print(num)
-- Output :
-- 2
```

#### Remainder Assignment Operators
The remainder assignment operator `%=` assigns the remainder after performing the division operation.
```lua
local num = 5
num %= 2 -- Assigns the remainder of dividing num by 2 to num
print(num)
-- Output :
-- 1
```

#### Exponentiation Assignment Operators
The exponentiation assignment operator `^=` assigns the exponentiation result.
```lua
local num = 5
num ^= 2 -- The square of num is assigned to num
print(num)
-- Output :
-- 25.0
```

#### String Concatenation Assignment Operators
The string concatenation assignment operator `..=` assigns the result of string concatenation.
```lua
local str = 'Hello, '
str ..= 'MapleStory '
str ..= 'World!'
print(str)
-- Output :
-- Hello, MapleStory World!
```

#### Bitwise Left Shift Assignment Operators
Bit shift is one of the bitwise operators used in programming that shifts the binary value to the left or right. The left bit shift `<<` operator shifts the binary value to the left and adds 0 on the right side. The bitwise left shift assignment operator `<<=` assigns the result of left-shifting the bits.
```lua
local num = 2    -- 0b0010

-- 0b is a notation used to indicate that the value is in binary form.
-- Shifts the binary representation of num to the left by 1.
num <<= 1       -- 0b0100
print(num)
-- Output :
-- 4
```

#### Bitwise Right Shift Assignment Operators
The bitwise right shift assignment operator `>>=` assigns the result of right-shifting the bits.
```lua
local  num = 2   -- 0b0010

-- Shifts the binary representation of num to the right by 1.
num >>= 1        -- 0b0001
print(num)
-- Output :
-- 1
```

#### Bitwise AND and OR Operators
The bitwise AND operator `&` compares two values and returns 1 for each corresponding bit position where both bits are 1, and returns 0 for all other cases. On the other hand, the bitwise OR operator `|` compares two values and returns 1 for each corresponding bit position where at least one of the bits is 1, and returns 0 for other cases.
Let's take an example.
```lua
local a = 10 -- binary 1010
local b = 7 -- binary 0111

local result = a & b -- Perform bitwise AND operation
print(result)
-- Output : 
-- 2 (binary 0010)

local result = a | b -- Perform bitwise OR operation
print(result)
-- Output : 
-- 15 (binary 1111)
```

The bitwise AND assignment operator `&=` assigns the result of performing a bitwise AND operation. Bitwise OR assignment operator `|=` assigns the result of performing a bitwise OR operation.
Let's take a look at an example.
```lua
local cardKeywords =
{
    Taunt = 1 << 0,
    BattleCry = 1 << 1,
    Deathrattle = 1 << 2,
    Windfury = 1 << 3,
    Immune = 1 << 4,
}

local function HasKeyword(card, keyword)
    return (card & keyword) == keyword
end

local newCard = cardKeywords.Taunt | cardKeywords.BattleCry
print("Taunt : ",HasKeyword(newCard, cardKeywords.Taunt))
-- Output :
-- Taunt : true

newCard &= ~cardKeywords.Taunt -- Remove Taunt from newCard
print("Taunt : ",HasKeyword(newCard, cardKeywords.Taunt))
-- Output :
-- Taunt : false

newCard |= cardKeywords.Immune -- Add Immune to newCard
print("Immune : ",HasKeyword(newCard, cardKeywords.Immune))
-- Output :
-- Immune : true
```

<br>
> <span style="color: #585858">**Learn more**
> When using compound assignment operators, keep in mind some limitations.
> First, it is not possible to perform multiple compound assignments simultaneously.</span>
> ```lua
> a , b += 1, 2 -- Error
> ```

> <span style="color: #585858">Additionally, compound assignments cannot be used as argument.
> ```lua
> print(a += 1) -- Error
> ```
> </span>