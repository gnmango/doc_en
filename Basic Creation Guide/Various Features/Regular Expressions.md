# Course Introduction
Regular expressions allow you to process text flexibly and efficiently. In addition to basic Lua string pattern matching, MapleStory Worlds supports regular expressions. The pattern-matching notation of regular expressions allows you to parse large amounts of text to perform the following things:
* Find specific character patterns
* Test the text effectiveness to see if it matches a predefined pattern
* Extract, edit, replace, and delete substrings

However, regular expressions can be complex, so it's better to use Lua string pattern matching for simple regular expression operations. For more complex operations, you can use the following reference guide to get started using regular expressions.

##### API Reference
[Regex](/apiReference/Misc/Regex{"target":"_self"})

##### Reference Guide
[Regular Expression Syntax]

# Creator
You can use `Regex(String pattern)` to create a regular expression entity with a specific string pattern.
For example, the following code will create a regular expression entity that finds "word".
```lua
-- A regular expression entity to find "word"
local regex = Regex("word")
```
<br>
When creating a regular expression entity, you can also use `RegexOption` to specify options.
For example, you can set `RegexOption.IgnoreCase` as shown below to find a specified string pattern case without being case-sensitive.
```lua
-- Search for "word" without being case-sensitive
local regex = Regex("word", RegexOption.IgnoreCase)
```

# Property
You have a **boolean** type **IsRightToLeft** property. 
You can use this property to check for items that match the regular expression from right to left.

# Function
### IsMatch
`IsMatch(String origin)` checks whether an item matching a specified regular expression pattern exists in the origin string.
* Returns true if something matches the regular expression pattern.
* Returns false if nothing matches the regular expression pattern or if the regular expression fails due to a timeout. 
```lua
-- Common
local pattern = "^[A-Z0-9]\\d{2}[A-Z0-9](-\\d{3}){2}[A-Z0-9]$"
local regex = Regex(pattern, RegexOption.IgnoreCase)
 
log(regex:IsMatch("1298-673-4192"))    --  true
log(regex:IsMatch("A08Z-931-468a"))    --  true
 
log(regex:IsMatch("_A90-123-129X"))    --  false
log(regex:IsMatch("12345-KKA-1230"))    --  false
log(regex:IsMatch("0919-2893-1256"))    --  false
```
<br>
`IsMatch(String origin, Int32 startIndex)` checks for any item that matches a specified regular expression pattern, starting at the startIndex(startIndex) of the origin string.
```lua
-- Specifying startIndex
local pattern = "[a-zA-Z0-9]\\d{2}[a-zA-Z0-9](-\\d{3}){2}[A-Za-z0-9]$"
local regex = Regex(pattern)

log(regex:IsMatch("Part Number: 1298-673-4192", 10))  -- true
log(regex:IsMatch("Part Number: 1298-673-4192", 15))  -- false
```

### Match
`Match(String origin)` searches for the first item in an origin string that matches a regular expression pattern.
```lua
local input = "One car red car blue car"
local pattern = "(\\w+)\\s+(car)"
     
local regex = Regex(pattern, RegexOption.IgnoreCase)
 
local match = regex:Match(input)
 
while match.Success do
    log(match.Text)
    match = match:NextMatch()
end
 
-- (Result)
-- One car
-- red car
-- blue car
```
<br>
`Match(String origin, Int32 startIndex)` searches for the first hit that matches a regular expression pattern, starting at the startIndex of the origin string. If the regular expression fails, returns nil.
```lua
-- Specifying startIndex
local origin = "123456789"
local pattern = "\\d"
 
local regex = Regex(pattern)
 
log(regex:Match(origin, 1).Text)      -- 1
log(regex:Match(origin, 3).Text)      -- 3
log(regex:Match(origin, 5).Text)      -- 5
```
<br>
`Match(String origin, Int32 startIndex, Int32 length)` searches for the first hit that matches a regular expression pattern within the specified length of the string, starting at the startIndex of the origin string. If the regular expression fails, returns nil.
```lua
-- Specifying startIndex and length
local origin = "123456789"
local pattern = "\\d+"
 
local regex = Regex(pattern)
 
log(regex:Match(origin, 1, 5).Text)   -- 12345
log(regex:Match(origin, 3, 3).Text)   -- 345
log(regex:Match(origin, 5, 1).Text)   -- 5
```

### Matches
`Matches(String origin)` searches for any hit in the origin string that matches a regular expression pattern.
```lua
local origin = "Who writes these notes and uses our paper?"
local pattern = "\\b\\w+es\\b"
 
local regex = Regex(pattern)
 
local matches = regex:Matches(origin)
 
log(#matches)     -- 3
 
for i, match in pairs(matches) do
    log(match.Text)
end
 
-- (Result)
-- writes
-- notes
-- uses
```
<br>
`Matches(String origin, Int32 startIndex)` searches for any hit that matches a regular expression pattern, starting at the startIndex of the origin string.
```lua
-- Specifying startIndex
local origin = "Who writes these notes and uses our paper?"
local pattern = "\\b\\w+es\\b"
 
local regex = Regex(pattern)
 
local firstMatch = regex:Match(origin)
 
local matches = regex:Matches(origin, firstMatch.Index + firstMatch.Length)
 
log(#matches)     -- 2
 
for i, match in pairs(matches) do
    log(match.Text)
end
 
-- (Result)
-- notes
-- uses
```

### Replace
`Replace(String origin, String replacement)` changes any hit in the origin string that matches a regular expression pattern with a replacement string.
```lua
-- Common
local origin = "This is   text with   far  too   much   white space."
local pattern = "\\s+"
 
local regex = Regex(pattern)
 
local result = regex:Replace(origin, " ")
 
log(result) -- "This is text with far too much white space."
```
<br>
`Replace(String origin, String replacement, Int32 count)` changes all hits in the origin string that match a regular expression pattern with the replacement strings, up to the specified number of times. If the regular expression fails, returns nil.
```lua
local origin = "This is   text with   far  too   much   white space."
local pattern = "\\s+"
  
local regex = Regex(pattern)
  
local result = regex:Replace(origin, " ", 4)
  
log(result) -- "This is text with far  too   much   white space."
```
<br>
`Replace(String origin, String replacement, Int32 count, Int32 startIndex)` changes all hits in the origin string that match a regular expression pattern with the replacement string up to the specified number of times, starting at the startIndex of the string origin. If the regular expression fails, returns nil.
```lua
-- Set the maximum number and starting position
local origin = "1 3 5 7 9 h e l l o w o r l d"
local pattern = "\\s+"
 
local regex = Regex(pattern)
 
local result = regex:Replace(origin, "", 4, 11)
 
log(result) -- "1 3 5 7 9 hello w o r l d"
```

### ReplaceByEvaluator
`ReplaceByEvaluator(String origin, function<RegexMatch>→string evaluator)` changes any hit that matches a regular expression pattern to the string returned by the evaluator. If the regular expression fails, returns nil.
```lua
local origin = "letter alphabetical missing lack release penchant slack acryllic laundry cease"
local pattern = "\\w+  "
 
local regex = Regex(pattern, RegexOption.IgnorePatternWhitespace)
 
---@param match RegexMatch
---@return string
local function evaluator(match)
    return match.Text.."("..tostring(match.Text:len())..")"
end
 
local result = regex:ReplaceByEvaluator(origin, evaluator)
 
log(result)   
-- "letter(6) alphabetical(12) missing(7) lack(4) release(7) penchant(8) slack(5) acryllic(8) laundry(7) cease(5)"
```
<br>
`ReplaceByEvaluator(String origin, function<RegexMatch>→string evaluator, Int32 count)` replaces all hits that match a regular expression pattern with strings returned by the evaluator, up to the specified number of times. If the regular expression fails, returns nil.
```lua
local origin = "letter alphabetical missing lack release penchant slack acryllic laundry cease"
local pattern = "\\w+  "
   
local regex = Regex(pattern, RegexOption.IgnorePatternWhitespace)
   
---@param match RegexMatch
---@return string
local function evaluator(match)
    return match.Text.."("..tostring(match.Text:len())..")"
end
   
local result = regex:ReplaceByEvaluator(origin, evaluator, 5)
   
log(result)  
-- "letter(6) alphabetical(12) missing(7) lack(4) release(7) penchant slack acryllic laundry cease"
```
<br>
`ReplaceByEvaluator(String origin, function<RegexMatch>→string evaluator, Int32 count, Int32 startIndex)` replaces all hits in the origin string that match a regular expression pattern with the strings returned by the evaluator, up to the specified number of times, starting at the startIndex of the origin string. If the regular expression fails, returns nil.
```lua
local origin = "letter alphabetical missing lack release penchant slack acryllic laundry cease"
local pattern = "\\w+  "
  
local regex = Regex(pattern, RegexOption.IgnorePatternWhitespace)
  
---@param match RegexMatch
---@return string
local function evaluator(match)
    return match.Text.."("..tostring(match.Text:len())..")"
end
  
local result = regex:ReplaceByEvaluator(origin, evaluator, 5, 21)
  
log(result)   
-- "letter alphabetical missing(7) lack(4) release(7) penchant(8) slack(5) acryllic laundry cease"
```

### Split
`Split(String origin)` splits the origin string based on the hits that match a regular expression pattern. If the regular expression fails, returns nil.
```lua
local origin = "plum-bear-pear"
local pattern = "-"
     
local regex = Regex(pattern)
     
local texts = regex:Split(origin)
 
log(#texts) -- 3
 
for i, text in pairs(texts) do
    log(text)
end
 
-- (Result)
-- plum
-- bear
-- pear
```
<br>
`Split(String origin, Int32 count)` splits the origin string on the hits that match a regular expression pattern, but only up to the specified maximum count of split strings. If the regular expression fails, returns nil.
```lua
-- Specifying count
local origin = "apple-apricot-plum-pear-banana"
local pattern = "-"
     
local regex = Regex(pattern)
     
local texts = regex:Split(origin, 4)
 
log(#texts) -- 4
 
for i, text in pairs(texts) do
    log(text)
end
 
-- (Result)
-- apple
-- apricot
-- plum
-- pear-banana
```
<br>
`Split(String origin, Int32 count, Int32 startIndex)` splits the origin string based on the hits that match a regular expression pattern, up to the specified maximum number of split strings, starting at the startIndex of the origin string. If the regular expression fails, returns nil.
```lua
local origin = "apple-apricot-plum-pear-banana"
local pattern = "-"
 
local regex = Regex(pattern)
 
local texts = regex:Split(origin, 4, 7)
 
log(#texts) -- 4
 
for i, text in pairs(texts) do
    log(text)
end
 
-- (Result)
-- apple-apricot
-- plum
-- pear
-- banana
```

# Regular Expression Option
You can use `RegexOption` to set options for regular expression operations.

| Option | Description |
| :---: | --- |
| IgnoreCase | Enables case-insensitive searching for matches. |
| IgnorePatternWhitespace | Ignores the whitespace without escape characters (backslashes) in the pattern. |
| RightToLeft | Changes the direction of the search. Enables searching from right to left, rather than left to right.  |
| ExplicitCapture | Does not capture unnamed groups. Specifies that only explicitly named or numbered groups are valid captures. |
| Multiline | Enables the multiline mode. Changes the meaning of ^ and $ to match at the start and end of every line, not just the start and end of the entire string. |
																				 
| Singleline | Enables the single-line mode. Changes the meaning of the dot (.) to "match all characters", instead of all characters except the dot (.). |

Let's look at how to use each option with examples.

### IgnoreCase
Enables case-insensitive searching for matches.
```lua
-- Does not set IgnoreCase option
local pattern = "\\bthe\\w*\\b"
local input = "The man then told them about that event."
 
local noOptionRegex = Regex(pattern)
local matches = noOptionRegex:Matches(input)
log(#matches) -- 2
 
 
-- Sets IgnoreCase option
local optionRegex = Regex(pattern, RegexOption.IgnoreCase)
matches = optionRegex:Matches(input)
log(#matches) -- 3
```

### IgnorePatternWhitespace
Ignores the whitespace without escape characters (backslashes) in the pattern.
```lua
-- Does not set IgnorePatternWhitespace option
local pattern = "\\b \\(? ( (?>\\w+) ,?\\s? )+ [\\.!?] \\)? # Matches an entire sentence."
local input = "This is the first sentence. Is it the beginning of a literary masterpiece? I think not. Instead, it is a nonsensical paragraph."
 
local noOptionRegex = Regex(pattern)
local matches = noOptionRegex:Matches(input)
log(#matches) -- 0
 
-- Sets IgnorePatternWhitespace option
local optionRegex = Regex(pattern, RegexOption.IgnorePatternWhitespace)
matches = optionRegex:Matches(input)
log(#matches) -- 4
```

### RightToLeft
Changes the direction of the search. Enables searching from right to left, rather than left to right.
```lua
-- Does not set RightToLeft option 
local pattern = "\\bb\\w+\\s"
local input = "build11 band22 tab"
 
local noOptionRegex = Regex(pattern)
local matches = noOptionRegex:Matches(input)
 
log(noOptionRegex.IsRightToLeft)  -- false
 
for i, match in pairs(matches) do
    log(match.Text)
end
 
-- (Result)
-- build11
-- band22
 
-- Sets RightToLeft option 
local optionRegex = Regex(pattern, RegexOption.RightToLeft)
matches = optionRegex:Matches(input)
 
log(optionRegex.IsRightToLeft)    -- true
 
for i, match in pairs(matches) do
    log(match.Text)
end
 
-- (Result)
-- band22
-- build11
```

### ExplicitCapture
Uses parentheses to define the groups to be captured by default. This option allows you to name a group in the form of `(?<name>...`) when defining the group. When you use the `ExplicitCapture` option, unnamed groups will not be captured. This option captures only the groups that are numbered or explicitly named in the form of `(?<name>...)`.
```lua
-- Does not set ExplicitCapture option
local pattern = "^(?<first>\\d{3})-(\\d{4})-(\\d{4})-(\\d{4})-(\\d{4})-(\\d{4})-(\\d{4})-(?<last>\\d{3})$"
local input = "010-1111-2222-3333-4444-5555-6666-777"
 
local noOptionRegex = Regex(pattern)
local match = noOptionRegex:Match(input)
 
---@type table<any, RegexGroup>
local groups = match:GetGroups()
     
for j, group in pairs(groups) do
    log(group.Name)
end
 
-- (Result)
-- 1
-- 2
-- 3
-- 4
-- 5
-- 6
-- 7
-- first
-- last
 
 
-- Sets ExplicitCapture option
local optionRegex = Regex(pattern, RegexOption.ExplicitCapture)
match = optionRegex:Match(input)
---@type table<any, RegexGroup>
groups = match:GetGroups()
     
for j, group in pairs(groups) do
    log(group.Name)
end
 
-- (Result)
-- 1
-- first
-- last
```

### Multiline
Enables processing origin strings that are composed of multiple lines. 
Originally, `^` and `$` mean the start and end of the entire origin string. However, the `Multiline` option enables the `^` and `$` to match the start and end of each line, rather than the entire origin string. Each line is identified by a `\n` (line break).
```lua
-- Multiline option disabled
local pattern = "^(?<Name>\\w+)\\s(?<Number>\\d+)$"
local input = "Joe 164\nSam 208\nAllison 211\nGwen 171\n"
  
local noOptionRegex = Regex(pattern)
local matches = noOptionRegex:Matches(input)
  
log(#matches) -- 0
  
for i, match in pairs(matches) do
      
    ---@type table<any, RegexGroup>
    local groups = match:GetGroups()
      
    local nameCapture = groups["Name"]:GetCaptures()[1]
    local numberCapture = groups["Number"]:GetCaptures()[1]
     
    log("Name : "..nameCapture.Text..", Number : "..numberCapture.Text)
end
  
-- (Result)
-- (None)
 
-- Multiline option enabled
local optionRegex = Regex(pattern, RegexOption.Multiline)
matches = optionRegex:Matches(input)
  
log(#matches) -- 4
  
for i, match in pairs(matches) do
      
    ---@type table<any, RegexGroup>
    local groups = match:GetGroups()
      
    local nameCapture = groups["Name"]:GetCaptures()[1]
    local numberCapture = groups["Number"]:GetCaptures()[1]
     
    log("Name : "..nameCapture.Text..", Number : "..numberCapture.Text)
end
  
-- (Result)
-- Name : Joe, Number : 164
-- Name : Sam, Number : 208
-- Name : Allision, Number : 211
-- Name : Gwen, Number : 171  
```

### Singleline
Processes an entered string as if it is composed in a single line.
By default, `Match()` ends when it meets `\n`, but with the `Singleline` option enabled, even if it meets `\n`, it continues.
```lua
-- Singleline option disabled 
local pattern = "^.+"
local input = "This is one line and\nthis is the second."
     
local noOptionRegex = Regex(pattern)
local match = noOptionRegex:Match(input)
 
log(match.Text)   -- "This is one line and"
 
 
-- Singleline option enabled 
local optionRegex = Regex(pattern, RegexOption.Singleline)
match = optionRegex:Match(input)
 
log(match.Text)   -- "This is one line and\nthis is the second."
```

### Applying Multiple Options
`RegexOption` allows you to apply multiple options at once.
```lua
local regex = Regex("line\\w+", RegexOption.Singleline | RegexOption.RightToLeft | RegexOption.IgnoreCase)
     
local matches = regex:Matches("lineFirst \n LineSecond \n LineLast")
     
for i, match in pairs(matches) do
    log(match.Text)
end
     
-- (Result)
-- LineLast
-- LineSecond
-- lineFirst
```