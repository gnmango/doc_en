# Course Introduction
To use DateTime, you need to use TimeSpan. It indicates the units of seconds, minutes, and hours.

# Introduce TimeSpan
TimeSpan indicates the time interval. It indicates only the general interval without any reference to a certain start point or end point, so it cannot indicate any variable year and month units.
You can use TimeSpan to calculate the time from a certain time to another time. You can use this to figure out the time that was taken to go through some stage or to save the time in the data storage. The standard format specifier or the user format specifier can be used depending on what suits the creator. The standard format specifier defines certain formats for each specifier with "c", "g", and "G". The user format specifier can be used depending on what suits the creator for the number of digits of the format showing time and how to indicate.

# Creator of TimeSpan
Creators can usually be entered from big time to small time, sometimes classifying the cultural area depending on the creator. 
* e.g., hours, minutes, seconds/days, hours, minutes, seconds, milliseconds

* <span style="color: #dc9656">**TimeSpan(int elapsed)**</span>: It resets the entity to the integer in unit of milliseconds. 
* <span style="color: #dc9656">**TimeSpan(int hours, int minutes, int seconds)**</span>: It resets the entity to hours, minutes, and seconds. 
* <span style="color: #dc9656">**TimeSpan(int days, int hours, int minutes, int seconds)**</span>: It resets the entity to days, hours, minutes, and seconds. 
* <span style="color: #dc9656">**TimeSpan(int days, int hours, int minutes, int seconds, int milliseconds)**</span>: It resets the entity to days, hours, minutes, seconds, and milliseconds. 
* <span style="color: #dc9656"> **TimeSpan(string timeSpanString)**</span>:  [ws][-]{ d | [d.]hh:mm[:ss[.ff]] }[ws] format time interval specifications are used as the strings to reset the entity.  
* <span style="color: #dc9656">**TimeSpan(string timeSpanString, string format)**</span> : [ws][-]{ d | [d.]hh:mm[:ss[.ff]] }[ws] format time interval specifications are used as the strings to reset the entity. <br>timeSpanString must be exactly matched to the format, and the format includes the standard format specifier and the user format specifier.
* <span style="color: #dc9656">**TimeSpan(string timeSpanString, string format, string cultureName)**</span> : [ws][-]{ d | [d.]hh:mm[:ss[.ff]] }[ws] format time interval specifications are used as the strings to reset the entity. <br>timeSpanString must be exactly matched to the format, and the format includes the standard format specifier and the user format specifier.<br>cultureName provides the information by each cultural area when the format is a standard specifier. The cultural area name follows the standard of BCP-47.

><span style="color: #7cafc2">**Tip**
> You can use the elements in the square brackets [ ] selectively. 
> As for the elements between braces { }, select and use the things classified by the vertical bar | .</span>

# Time Interval Symbol
This is a common symbol to indicate the time in TimeSpan.

| Symbol | Explanation |
| --- | --- |
| ws | Blank |
| ff | The number of digits for seconds, composed of one digit to seven digits|
| ss | Seconds between 0 and 59|
| MM | Minutes between 0 and 59 |
| hh | Hours between 0 and 23 |
| d | Days between 0 and 10675199 |
| - | A symbol to indicate TimeSpan negative number|
| . | Cultural area classification symbol to classify the days and hours<br>Cultural area classification symbol to classify the minutes and seconds |
| : | Hours classification symbol that is sensitive to the cultural area|

# Standard TimeSpan Format Specifier
The standard TimeSpan format string determines the text expression of the TimeSpan value that is created by format specification works using a single format specifier.

| Format Specifier | Name | Explanation |
| --- | --- | --- |
| "c" | Constant Format | It doesn't classify the cultural area and use the [-][d'.']hh':'mm':'ss['.'fff] format. |
| "g" | General Informality | It classifies the cultural area and outputs the required content. It uses the [-][d':']h':'mm':'ss[.FFF] format.  |
| "G" | General Long Format | It classifies the cultural area and outputs the number of days and the 7 digits of the decimal.  It uses the [-]d':'hh':'mm':'ss.fff format. |

><span style="color: #7cafc2">**Tip**
> It uses the format specifier only when the symbols between [ ] from the format have valid values.</span>


#### Constant Format Specifier
The constant format specifier "c" doesn't classify the cultural area. The format is [-][d'.']hh':'mm':'ss['.'fff].

| Symbol | Explanation |
| --- | --- |
| - | A symbol to indicate a negative number |
| fff | It indicates the decimal part of seconds. The value range is from "001" to "999". |
| ss | It shows the seconds of the range between "0" and "59". |
| mm | It shows the minutes of the range between "00" and "59". |
| hh | It shows the hours of the range between "00" and "23". |
| d | It indicates the number of days. |

```lua
local timeSpan1 = TimeSpan(7, 45, 16)
local timeSpan2 = TimeSpan(18, 12, 38)
local timeSpanString1 = timeSpan1:ToFormattedString("c")
local timeSpanString2 = timeSpan2:ToFormattedString("c")
local timeSpanString3 = (timeSpan1 + timeSpan2):ToFormattedString("c")
 
log(timeSpanString1) -- 07:45:16
log(timeSpanString2) -- 18:12:38
log(timeSpanString3) -- 1.01:57:54
```

#### General Informality Format Specifier
The general informality format specifier "g" outputs the required content only and classifies the cultural areas. The format is [-][d':']h':'mm':'ss[.FFF].

| Symbol | Explanation |
| --- | --- |
| - | A symbol to indicate a negative number |
| . | It indicates the classification of the seconds decimal. |
| FFF | It indicates the decimal part of seconds. |
| ss | It shows the seconds of the range between "00" and "59".|
| mm | It shows the minutes of the range between "00" and "59". |
| hh | It shows the hours of the range between "00" and "23". |
| d | It indicates the number of days. |

```lua
local timeSpan1 = TimeSpan(7, 45, 16)
local timeSpan2 = TimeSpan(18, 12, 38)
local timeSpanString1 = timeSpan1:ToFormattedString("g")
local timeSpanString2 = timeSpan2:ToFormattedString("g")
local timeSpanString3 = (timeSpan1 + timeSpan2):ToFormattedString("g", "fr-FR")
 
log(timeSpanString1) -- 07:45:16
log(timeSpanString2) -- 18:12:38
log(timeSpanString3) -- 1:1:57:54
```

#### General Long Format Specifier
The general long format specifier "G" always outputs the number of days and the three digits of decimal and classifies the cultural areas. The format is [-]d':'hh':'mm':'ss.fff.

| Symbol | Explanation |
| --- | --- |
| - | A symbol to indicate a negative number |
| . | It indicates the classification of the seconds decimal. |
| fff | It indicates the decimal part of seconds. |
| ss | It shows the seconds of the range between "00" and "59".|
| mm | It shows the minutes of the range between "00" and "59". |
| hh | It shows the hours of the range between "00" and "23". |
| d | It indicates the number of days. |

```lua
local timeSpan1 = TimeSpan(7, 45, 16)
local timeSpan2 = TimeSpan(18, 12, 38)
local timeSpanString1 = timeSpan1:ToFormattedString("G")
local timeSpanString2 = timeSpan2:ToFormattedString("G")
local timeSpanString3 = (timeSpan1 + timeSpan2):ToFormattedString("G", "fr-FR")
 
log(timeSpanString1) -- 0:07:45:16.000
log(timeSpanString2) -- 0:18:12:38.000
log(timeSpanString3) -- 1:01:57:54,000
```

# User Custom TimeSpan Format Specifier
The user custom format string is composed of at least one user custom TimeSpan format specifier and an arbitrary number of literal characters.
The TimeSpan format string defines the text expression of the TimeSpan values created. The string, not a standard TimeSpan format string, is interpreted as a user custom TimeSpan format string.

#### "d" 
The "d" user custom format specifier outputs the value indicating the number of days. It outputs the total number of days even when the value is more than two digits. If the value is 0, the specifier outputs "0".
If only the "d" user custom format specifier is used, it designates "%d" so as not to be interpreted incorrectly with the standard format string. 
```lua
local timeSpan = TimeSpan(16, 4, 3, 17, 250)
local timeSpanString = timeSpan:ToFormattedString("%d")
 
log(timeSpanString) -- 16
```

#### "d" - "dddddddd" 
The "dd", "ddd", "dddd", "ddddd""dddddd", "ddddddd", and "dddddddd" user custom format specifiers output the values indicating the number of days.
The output string contains the minimum number of digits designated with the number of "d" character to the format specifier, and it is filled with 0 in the front as required. If the total number of days exceeds the number of "d" characters of the format specifier, the total number of days will be outputted in the result string.
```lua
local timeSpan = TimeSpan(365, 21, 19, 45)
  
log(timeSpan:ToFormattedString("dd'.'hh':'mm':'ss")) -- 365.21:19:45
log(timeSpan:ToFormattedString("dddd'.'hh':'mm':'ss")) -- 0365.21:19:45
```

#### "h" 
The "h" user custom format specifier outputs the value indicating the number of hours. It returns the one-digit string value if the hour value is 0-9 and the two-digit string if 10-23.
If only the "h" user custom format specifier is used, it designates "%h" so as not to be interpreted incorrectly with the standard format string. The string, which usually contains only one number, is interpreted as the number of days.
You can interpret the number string as the number of hours using the "h" user custom format specifier instead.
```lua
local timeSpan = TimeSpan(16, 4, 3, 17, 250)
local timeSpanString = timeSpan:ToFormattedString("%h")
 
log(timeSpanString) -- 4
```

#### "hh" 
The "hh" user custom format specifier outputs the value indicating the number of hours. If the hour value is 0-9, there will be 0 in front of the output string.
The string, which usually contains only one number, is interpreted as the number of days.
You can interpret the number string as the number of hours using the "hh" user custom format specifier instead.
```lua
local timeSpan = TimeSpan(365, 3, 14, 45)
  
log(timeSpan:ToFormattedString("dd'.'hh':'mm':'ss")) -- 365.03:14:45
```

#### "m" 
The "m" user custom format specifier outputs the value indicating the number of minutes. It returns the one-digit string value if the minute value is 0-9 and the two-digit string if 10-59.
If only the "m" user custom format specifier is used, it designates "%m" so as not to be interpreted incorrectly with the standard format specifier.
The string, which usually contains only one number, is interpreted as the number of days.
You can interpret the number string as the number of minutes using the "m" user custom format specifier instead.
 
```lua
local timeSpan = TimeSpan(16, 4, 3, 17, 250)
local timeSpanString = timeSpan:ToFormattedString("%m")
 
log(timeSpanString) -- 3
```

#### "mm" 
The "mm" user custom format specifier outputs the value indicating the number of minutes. If the minute value is 0-9, there will be 0 in front of the output string.
The string, which usually contains only one number, is interpreted as the number of days. You can interpret the number string as the number of minutes using the "mm" user custom format specifier instead.

```lua
local timeSpan = TimeSpan(365, 21, 4, 45)
  
log(timeSpan:ToFormattedString("dd'.'hh':'mm':'ss")) -- 365.21:04:45
```

#### "s" 
The "s" user custom format specifier outputs the value indicating the number of seconds. It returns the one-digit string value if the second value is 0-9 and the two-digit string if 10-59.
The string, which usually contains only one number, is interpreted as the number of days. You can interpret the number string as the number of seconds using the "s" user custom format specifier instead.
If only "s" is used, it designates "%s" so as not to be interpreted incorrectly with the standard format specifier.

```lua
local timeSpan = TimeSpan(16, 4, 3, 17, 250)
local timeSpanString = timeSpan:ToFormattedString("%s")
 
log(timeSpanString) -- 17
```

#### "ss" 
The "ss" user custom format specifier outputs the value indicating the number of seconds. If the second value is 0-9, there will be 0 in front of the output string. 
The string, which usually contains only one number, is interpreted as the number of days. You can interpret the number string as the number of seconds using the "ss" user custom format specifier instead.

```lua
local timeSpan = TimeSpan(365, 21, 4, 5)
  
log(timeSpan:ToFormattedString("dd'.'hh':'mm':'ss")) -- 365.21:04:05
```

#### "f"
The "f" user custom format specifier outputs 1/10 seconds. In the format specification work, the remaining digits of decimal places will be cut.
If you create the TimeSpan entity with a string, the string must include the exact one-digit of the decimal place.
If only "f" is used, it designates "%f" so as not to be interpreted incorrectly with the standard format specifier.

```lua
local timeSpan = TimeSpan(876)
  
log(timeSpan:ToFormattedString("%f")) -- 8
```

#### "ff" 
The "ff" user custom format specifier outputs 1/100 seconds. In the format specification work, the remaining digits of decimal places will be cut. 
If you create the TimeSpan entity with a string, the string must include the exact two-digit decimal.

```lua
local timeSpan = TimeSpan(876)
  
log(timeSpan:ToFormattedString("ff")) -- 87
```

#### "fff" 
The "fff" user custom format specifier outputs milliseconds. In the format specification work, the remaining digits of decimal places will be cut.
If you create the TimeSpan entity with a string, the string must include the exact three-digit decimal.

```lua
local timeSpan = TimeSpan(876)
  
log(timeSpan:ToFormattedString("fff")) -- 876
```

#### "F"
The "F" user custom format specifier outputs 1/10 seconds. In the format specification work, the remaining digits of decimal places will be cut.
If the 1/10 seconds value is 0, the result string doesn't include this. If you create the TimeSpan entity with a string, the 1/10 seconds numbers need not be indicated.
If only "F" is used, it designates "%F" so as not to be interpreted incorrectly with the standard format specifier.
```lua
local timeSpan = TimeSpan(800)

log(timeSpan:ToFormattedString("%F")) -- 8
```


#### "FF"
The "FF" user custom format specifier outputs 1/100 seconds. In the format specification work, the remaining digits of decimal places will be cut.
If the 1/100 seconds value is 0, the result string doesn't include this. If you create the TimeSpan entity with a string, the 1/100 seconds numbers need not be indicated.

```lua
local timeSpan1 = TimeSpan(876)
local timeSpan2 = TimeSpan(800)
  
log(timeSpan1:ToFormattedString("FF")) -- 87
log(timeSpan2:ToFormattedString("FF")) -- 8
```

#### "FFF"
The "FFF" user custom format specifier outputs milliseconds. In the format specification work, the remaining digits of decimal places will be cut.
If the milliseconds value is 0, the result string doesn't include this. If you create the TimeSpan entity with a string, the milliseconds numbers need not be indicated.

```lua
local timeSpan1 = TimeSpan(876)
local timeSpan2 = TimeSpan(800)
  
log(timeSpan1:ToFormattedString("FFF")) -- 876
log(timeSpan2:ToFormattedString("FFF")) -- 8
```