# Course Introduction
Let's learn about how DateTime expresses date and time, and study various format specifiers you can use. 

# Learn DateTime
DateTime is an entity to indicate the time expressed in date and time. The value format indicates the dates and times with the values from 00:00:00 (midnight), 1 January 0001 to 11:59:59 pm, 31 December 9999 A.D. in the Gregorian calendar. DateTime is used to indicate special dates such as the time when users enter the map or the date when the account was created. You need the hour value to use DateTime, so people usually use this with TimeSpan.

# DateTime Constant
You can use the constant of DateTime as `DateTime.UtcNow`, and the value need not be assigned separately.

| Constant | Explanation |
| --- | --- |
| UtcNow | Gets the current date and time in UTC.  |
| MaxValue | The maximum value of DateTime. |
| MinValue |The minimum value of DateTime.  |

# DateTime Creator
Creators can enter from big time to small time, sometimes classifying the cultural area depending on the creator.

| Creator | Explanation |
| --- | --- |
| DateTime(int elapsed) | Resets the entity from the integer in milliseconds unit to UTC. |
| DateTime(int year, int month, int day, int hour, int minute, int second) | Resets the entity to years, months, days, hours, minutes, and seconds in UTC.  |
| DateTime(int year, int month, int day, int hour, int minute, int second, int millisecond) | Resets the entity to years, months, days, hours, minutes, seconds, and milliseconds in UTC.  |
| DateTime(string dateTimeString) | Resets the entity to the string.  |
| DateTime(string dateTimeString, string format) | Initializes an entity with a string.<br>dateTimeString must be exactly matched to the format, and the format is a string that includes the standard date and time format specifier and the user custom date and time format specifier. |
| DateTime(string dateTimeString, string format, string cultureName) | Initializes an entity with a string.<br> dateTimeString must exactly match the format, and the format is a string that includes the standard date and time format specifier and the user custom date and time format specifier.<br>cultureName provides cultural area-specific information about the format specifiers contained in format. cultureName conforms to the standards of BCP-47. |


# DateTime Property

| Property | Explanation |
| --- | --- |
| Elapsed | Integer in milliseconds unit |
| Day | Day |
| Month | Month |
| Year | Year |
| Millisecond | Millisecond |
| Second  | Second |
| Minute | Minute |
| Hour   | Hour |


# Example
#### Save and Load DateTime
This is an example of converting DateTime to the integer type Elapsed and saving it to DataStorage or importing Elapsed to restore DateTime.

```lua
local originDate =  DateTime(2022, 5, 17, 13, 21, 35)
local elapsed = originDate.Elapsed

local dataStorage = _DataStorageService:GetGlobalDataStorage ("dsName")
dataStorage:SetAndWait("date", tostring(elapsed))
 
local errorCode, dateFromDataStorage = dataStorage:GetAndWait("date")
local convertedDate = DateTime(tonumber(dateFromDataStorage))

log(convertedDate)
```

#### Send DateTime to Another Execution Space
You can use DataTime to DateElapsed and utilize DateTime in another execution space.

```lua
[server only]
void OnBeginPlay()
{
    wait(1)
    local date = DateTime(2022, 5, 17, 13, 21, 35)
    self:ServerToClient(date.Elapsed)
}
 
[client]
void ServerToClient(number dateElapsed)
{
    local date = DateTime(dateElapsed)
    log(date)
}
```

#### Convert DateTime to Local Time
DateTime is set based on UTC by default. If you want to convert this to the local time, you can move it to the Client space through execution control and convert it to the local time with the GetLocalTimeForm method of UtilLogic. 

```lua
[server only]
void OnBeginPlay()
{
    wait(1)
    local date = DateTime(2022, 5, 17, 13, 21, 35)
    self:ServerToClient(date.Elapsed)
}
[client]
void ServerToClient(number dateElapsed)
{
    local date = DateTime(dateElapsed)
    local localTime = _UtilLogic:GetLocalTimeFrom(date)
    log(localTime)
}
```

#### Designate DateTime Format
You can designate and use the format that the creator wants by using various format specifiers.

```lua
local date = DateTime(2022, 5, 17, 13, 21, 35)
local dateString = date:ToFormattedString("'year is 'yyyy ")
log(dateString) -- year is 2022
```


# Creation Format that Used a String
You can use the format as follows when creating the DateTime entity via string: If you combine and use certain components as in the various formats below and there are no other components, it sets a certain hour, date, day, and year as the standard or assumes based on the present.

#### Date and Time
A component to use both date and time.

```lua
local dateAsString = "08/18/2022 07:22:16" 
local date = DateTime(dateAsString)
log(date) -- 08/18/2022 07:22:16
```

#### Date
A component with date only. If there is no time component, the time should be assumed as 00:00:00.

```lua
local dateAsString = "08/18/2022" 
local date = DateTime(dateAsString)
log(date) -- 08/18/2022 00:00:00
```

#### Month and Year
A component with month and year only. In the case of not using day, the day component should be assumed as the first day of the month.

```lua
local dateAsString = "8/2022" 
local date = DateTime(dateAsString)
log(date) -- 08/18/2022 00:00:00
```

#### Month and Day
A component with month and day only. In the case of not using year, the year component should be assumed as the current year.

```lua
local dateAsString = "8/18" 
local date = DateTime(dateAsString)
log(date) -- 08/18/2022 00:00:00
```

### Hour
In the case of the string having only time, not date component, it should be assumed as the current date.

```lua
local dateAsString = "07:22:16" 
local date = DateTime(dateAsString)
log(date) -- 5/9/2022 07:22:16
```

#### Time and AM/PM
In the case of the string having only time and AM/PM specifier, not date component, the date component should be assumed as the current date, and the time (minutes and seconds) should be assumed as 00:00.

```lua
local dateAsString = "7 PM" 
local date = DateTime(dateAsString)
log(date) -- 05/09/2022 19:00:00
```

#### ISO 8601 String
If it includes the standard time zone information and uses the string complying with ISO 8601, DateTime is set in UTC by default. Therefore, if you set in UTC, the time value will be sustained.
* Example: "2022-11-01T19:35:00.000Z"
    
```lua
local dateAsString = "2022-08-18T07:22:16.000Z"
local date = DateTime(dateAsString)
log(date) -- 08/18/2022 07:22:16
```

If you set the standard time zone offset information, it will be saved with the UTC value that changed as much as the offset.
* Example: "2022-11-01T19:35:00.000-07:00"

```lua
local dateAsString = "2022-08-18T07:22:16.000-07:00" 
local date = DateTime(dateAsString)
log(date) -- 08/18/2022 14:22:16
```

#### RFC 1123 Time format string
You can use the string complying with the RFC 1123 time format. At this time, you need to include GMT specifier.

```lua
local dateAsString = "Thu, 18 Aug 2022 07:22:16 GMT" 
local date = DateTime(dateAsString)
log(date) -- 08/18/2022 07:22:16
```

#### Standard Time Zone Offset Information
The standard date and time format string determines the string expression of the DateTime value using a single character as a format specifier.
The date and time specification that includes more than two characters including blank will be interpreted as the user custom date and time specification string.

```lua
local dateAsString = "08/18/2022 07:22:16 -5:00" 
local date = DateTime(dateAsString)
log(date) -- 08/18/2022 12:22:16
```

# Standard Date and Time Format Specifier
This determines and uses the standard date and time specification as the string of the DateTime value using the single character as a format specifier.
The date and time specification that includes more than two characters including blank will be interpreted as the user custom date and time format specifier, so you need to be cautious if there is a blank or not.

#### Format Specifier
This describes the standard format specifiers briefly. Please refer to the content below for the detailed explanation.

| Format Specifier | Explanation |
| --- | --- |
| "d" | Simple date pattern |
| "D" | Detailed date pattern |
| "f" | Whole date/simple time pattern |
| "F" | Whole date/detailed time pattern |
| "g" | General date/simple time pattern |
| "G" | General date/detailed time pattern |
| "M", "m" | Month/Day pattern |
| "O", "o" | Round-trip date/time pattern |
| "R", "r" |  RFC1123 Pattern|
| "s" | Date/Time pattern that can be arranged |
| "t" | Simple time pattern |
| "T" | Detailed time pattern |
| "u"  | UTC Date/Time pattern that can be arranged |
| "U" | UTC Date/Time pattern |
| "Y", "y" | Year Month Pattern |


#### "d" 
The "d" standard format specifier indicates the simple date format string of a certain cultural area.
If you don't designate the cultural area, the format string will use <span style="color: #dc9656">**"MM/dd/yyyy"**</span>.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("d")) -- 04/10/2022
log(date:ToFormattedString("d", "en-US")) -- 4/10/2022
```

#### "D" 
The "D" standard format specifier indicates the detailed date format string of a certain cultural area.
If you don't designate the cultural area, the format string will use <span style="color: #dc9656">**"dddd, dd MMMM yyyy"**</span>.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("D")) -- Sunday, 10 April 2022
log(date:ToFormattedString("D", "pt-BR")) -- domingo, 10 de abril de 2022
```


#### "f"
The "f" standard format specifier indicates the various patterns of strings combined with the detailed date ("D"), which is classified by blanks, and the simple time ("t").
If you don't designate the cultural area, the format string will use <span style="color: #dc9656">**"dddd, dd MMMM yyyy HH:mm"**</span>.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("f")) -- Sunday, 10 April 2022 00:00
log(date:ToFormattedString("f", "pt-BR")) -- domingo, 10 de abril de 2022 00:00
```


#### "F" 
The "F" standard format specifier indicates the whole date and detailed time format string of a certain cultural area.
If you don't designate the cultural area, the format string will use <span style="color: #dc9656">**"dddd, dd MMMM yyyy HH:mm:ss"**</span>.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("F")) -- Sundy, 10 April 2022 00:00:00
log(date:ToFormattedString("F", "pt-BR")) -- domingo, 10 de abril de 2022 00:00:00
```

#### "g"
The "g" standard format specifier indicates the various patterns of strings combined with the simple date ("d"), which is classified by blanks, and the simple time ("t").
If you don't designate the cultural area, the format string will be <span style="color: #dc9656">**"MM/dd/yyyy HH:mm"**</span>. If you designate the cultural area, the general date ("d") and the simple time ("t") format string of the cultural area will be indicated.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("g")) -- 04/10/2022 00:00
log(date:ToFormattedString("g", "pt-BR")) -- 10/04/2022 00:00
```

#### "G" 
The "G" standard format specifier indicates the various patterns of strings combined with the simple date ("d"), which is classified by blanks, and the detailed time ("T"). 
If you don't designate the cultural area, the format string will be <span style="color: #dc9656">**"MM/dd/yyyy HH:mm:ss"**</span>. If you designate the cultural area, the simple date ("d") and the detailed time ("T") format string of the cultural area will be indicated.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("G")) -- 04/10/2022 00:00:00
log(date:ToFormattedString("G", "pt-BR")) -- 10/04/2022 00:00:00
```

#### "O", "o" 
The "O" and "o" standard format specifier indicates a round-trip date, time format specifier. Round-trip refers to the format that can be converted and restored from the date to the string and from the string to the date. The "O" and "o" standard format specifier is the result string complying with ISO 8601. 
The "O" and "o" standard format specifier corresponds to the user custom format string of the DateTime values of <span style="color: #dc9656">**"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffK"**</span>.
Next is an example to make the "O" and "o" standard format specifier through the string and then restore it to the date entity again.

```lua
local date = DateTime(2022, 4, 10)
local formattedString = date:ToFormattedString("o")
log(formattedString) -- 2022-04-10T00:00:00.000Z
 
date = DateTime(formattedString)
log(date) -- 04/10/2022 00:00:00
```

#### "R", "r"
The "R" or "r" standard format specifier indicates the user custom format string for the hour value based on the IETF RFC 1123 specifications. (<span style="color: #dc9656">**"ddd, dd MMM yyyy HH':'mm':'ss 'GMT'"**</span>)
The single quotations of user custom format strings corresponds to the literal. It is used to distinguish between hyphen, colon, and development characters. Therefore, they do not appear in the result string.
Time is indicated in UTC according to the IETF RFC 1123 standard, so you need to check if the value is UTC before using the format specifier.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("r")) -- Sun, 10 Apr 2022 00:00:00 GMT
```

#### "s" 
The "s" standard format specifier indicates the user custom format string. (<span style="color: #dc9656">**"yyyy'-'MM'-'dd'T'HH':'mm':'ss"**</span>)
The "s" format specifier is a format specifier for creating the result string that is arranged in ascending and descending order depending on the date and time values.
In the user custom format string, the single quotation marks that classify the hyphen, colon, and development character correspond to the literal. The single quotation marks in the result string are not indicated.
The date and time values are indicated in the same format, but the values are not modified depending on the types (UTC, Local) of DateTime. 
```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("s")) -- 2022-04-10T00:00:00
```

#### "u" 
The "u" standard format specifier indicates the user custom format string. (<span style="color: #dc9656">**"yyyy'-'MM'-'dd HH':'mm':'ss'Z"**</span>)
In the user custom format string, the single quotation marks that classify the hyphen, colon, and development character correspond to the literal. The single quotation marks in the result string are not indicated.
The time is indicated in UTC in the result string. However, the value is not modified depending on the types (UTC, Local) of DateTime, so you need to check if the value is in UTC before using the format specifier.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("u")) -- 2022-04-10 00:00:00Z
```

#### "U" 
The "U" standard format specifier has the same pattern as the "F" standard format specifier, but the "U" standard format specifier converts the value of DateTime to UTC.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("U")) -- Sunday, 10 April 2022 00:00:00
log(date:ToFormattedString("U", "pt-BR")) -- domingo, 10 de abril de 2022 00:00:00
```

#### "t" 
The "t" standard format specifier indicates the simple time format string of a certain cultural area. If you don't designate the cultural area, the format string will be <span style="color: #dc9656">**"HH:mm"**</span>.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("t")) -- 00:00
log(date:ToFormattedString("t", "en-US")) -- 12:00 AM
```

#### "T" 
The "T" standard format specifier indicates the detailed time format string of a certain cultural area. If you don't designate the cultural area, the format string will be <span style="color: #dc9656">**"HH:mm:ss"**</span>.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("T")) -- 00:00:00
log(date:ToFormattedString("T", "en-US")) -- 12:00:00 AM
```

#### "M", "m" 
The "M" or "m" standard format specifier indicates the month format string of a certain cultural area. If you don't designate the cultural area, the format string will use <span style="color: #dc9656">**"MMMM dd"**</span>.

```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("m")) -- April 10
log(date:ToFormattedString("m", "pt-BR")) -- 10 de abril
```
#### "Y", "y" 
The "Y" or "y" standard format specifier indicates the year month format string of a certain cultural area. If you don't designate the cultural area, the format string will use <span style="color: #dc9656">**"yyyy MMMM"**</span>.
```lua
local date = DateTime(2022, 4, 10)
log(date:ToFormattedString("y")) -- 2022 April
log(date:ToFormattedString("y", "pt-BR")) -- abril de 2022
```

# User Custom Date and Time Format Specifier
The user custom format specifier is composed of at least one user custom date and time format specifier. It determines the text expression of the DateTime value that is created.
The string, not the standard date and time format specifier, will be interpreted as the user custom date and time format specifier.
#### Format Specifier
This describes the user custom format specifiers briefly. Please refer to the content below for the detailed explanation.

| Format Specifier | Explanation |
| --- | --- |
| "d" | A format specifier to indicate days from 1 to 31 |
| "dd" | Days from 01 to 31 |  
| "ddd" | Informal name of days |  
| "dddd" | Whole name of days |  
| "f" | 1/10 Seconds of the date and time values |  
| "ff" | 1/100 Seconds of the date and time values |  
| "fff" | 1/1000 Seconds of the date and time values |  
| "F" | 1/10 Seconds of the date and time values. Nothing will be indicated if the valid number of digits is all 0. |  
| "FF" |  1/100 Seconds of the date and time values. Nothing will be indicated if the valid number of digits is all 0.| 
| "FFF" | 1/1000 Seconds of the date and time values Nothing will be indicated if the valid number of digits is all 0. |  
| "g", "gg" | A.D. or Era |  
| "h" | Hours from 1 to 12 |  
| "hh" | Hours from 01 to 12 |
| "H" | Hours from 0 to 23 |  
| "HH" | Hours from 00 to 23 |  
| "K" | Time zone information |  
| "m" | Minutes from 0 to 59 |  
| "mm" | Minutes from 00 to 59 |  
| "M" | Month from 0 to 12 |  
| "MM" | Month from 01 to 12 |  
| "MMM" | Informal name of month |  
| "MMMM" | Whole name of month |  
| "s" | Minutes from 0 to 59 |  
| "ss" | Minutes from 00 to 59 |  
| "t" | AM/PM specifier's first character |  
| "tt" | AM/PM specifier |  
| "y" | Year from 0 to 99 |  
| "yy" | Year from 00 to 99 |  
| "yyy" | At least three-digit year |  
| "yyyy" | Four-digit year |  
| ":" | Time classification symbol |  
| "/" | Date classification symbol |  
| "string". 'string' | Literal string classification symbol |  
| % | A format specifier that defines the character coming behind to the user custom format specifier |  

#### "d" 
The "d" user custom format specifier indicates the day with a number from 1 to 31. If the day is a one-digit number, the ten-digit will be omitted. For example, if the day is 3, then it will be indicated as 3, not 03.
If only "d" is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.

```lua
local date = DateTime(2022, 8, 9, 19, 27, 15)
log(date:ToFormattedString("%d")) -- 9
```

#### "dd" 
The "dd" user custom format specifier indicates the day with a number from 01 to 31. If the day is a one-digit number, 0 will be indicated for the ten-digit.

```lua
local date = DateTime(2022, 8, 9, 19, 27, 15)
log(date:ToFormattedString("dd")) -- 09
```

#### "ddd" 
The "ddd" user custom format specifier indicates the day's informal name. If you designate the cultural area, the informal day corresponding to the cultural area will be displayed.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15)
log(date:ToFormattedString("ddd")) -- Mon
log(date:ToFormattedString("ddd", "fr-FR")) -- lun.
```

#### "dddd" 
The "dddd" user custom format specifier indicates the whole name of the day. If you designate the cultural area, the day corresponding to the cultural area will be displayed.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15)
log(date:ToFormattedString("dddd")) -- Monday
log(date:ToFormattedString("dddd", "fr-FR")) -- lundi
```

#### "f" 
The "f" user custom format specifier indicates 1/10 seconds from the date and time values.
If only "f" is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("%f")) -- 0
```

#### "ff"
The "ff" user custom format specifier indicates 1/100 seconds from the date and time values.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("ff")) -- 02
```

#### "fff" 
The "fff" user custom format specifier indicates 1/1000 seconds from the date and time values.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("fff")) -- 023
```

#### "F" 
The "F" user custom format specifier indicates 1/10 seconds from the date and time values. Nothing will be indicated if the valid number of digits is all 0. 
If only "F" is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("%F")) -- empty
```

#### "FF" 
The "FF" user custom format specifier indicates 1/100 seconds from the date and time values. Nothing will be indicated if the valid number of digits is 2 and the valid number of digits is all 0.
Only 2 valid numbers of digits will be indicated even if the valid number of digits is more than 3.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("FF")) -- 02
```

####  "FFF" 
The "FFF" user custom format specifier indicates 1/1000 seconds from the date and time values. Nothing will be indicated if the valid number of digits is 3 and the valid number of digits is all 0. 

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("FFF")) -- 023
```

#### "g", "gg" 
The "g" or "gg" user custom format specifier indicates the era. It shows A.D. (Anno Domini) by default. 
If only the "g" user custom format specifier is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("%g")) -- A.D.
```

#### "h" 
The "h" user custom format specifier indicates the hour with a number from 1 to 12. If the hour is a one-digit number, the ten-digit won't be indicated.
It is displayed in 12-hour format, which calculates the total hours after midnight or noon. It cannot classify between the hour after midnight and the hour after noon. 
If only "h" is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("%h")) -- 7
```

#### "hh"
The "hh" user custom format specifier indicates the hour with a number from 1 to 12. If the hour is a one-digit number, 0 will be indicated for the ten-digit.
It is displayed in 12-hour format, which calculates the total hours after midnight or noon. It cannot classify between the hour after midnight and the hour after noon.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("hh")) -- 07
```

#### "H" 
The "H" user custom format specifier indicates the hour with a number from 0 to 23. If the hour is a one-digit number, the ten-digit won't be indicated. It is displayed in a 24-hour format, which calculates the hours after midnight.
If only "H" is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("%H")) -- 19
```

#### "HH" 
The "HH" user custom format specifier indicates the hour with a number from 0 to 23. If the hour is a one-digit number, 0 will be indicated for the ten-digit.
It is displayed in a 24-hour format, which calculates the hours after midnight. 

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("HH")) -- 19
```

#### "K" 
The "K" user custom format specifier indicates the standard time zone information of the date and time values. 
If only the "K" user custom format specifier is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.
* If the type of DateTime is Local, this specifier outputs the result string including the local offset in UTC.
* If the type of DateTime is UTC, the result string will include and output the "Z" characters indicating the UTC date.

```lua
local date = DateTime(2022, 8, 29, 19, 27, 15, 023)
log(date:ToFormattedString("%K")) -- Z
```

#### "m"
The "m" user custom format specifier indicates the minutes with a number from 0 to 59. Minutes indicate the total number of minutes that have been passed after the last time. If the minute is a one-digit number, the ten-digit won't be indicated.
If only the "m" user custom format specifier is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.

```lua
local date = DateTime(2022, 8, 29, 19, 7, 15, 023)
log(date:ToFormattedString("%m")) -- 7
```

#### "mm" 
The "mm" user custom format specifier indicates the minutes with a number from 00 to 59. Minutes indicate the total number of minutes that have been passed after the last time.
If the minute is a one-digit number, 0 will be indicated for the ten-digit.
```lua
local date = DateTime(2022, 8, 29, 19, 7, 15, 023)
log(date:ToFormattedString("mm")) -- 07
```

#### "M" 
The "M" user custom format specifier indicates the month with a number from 1 to 12. If the month is a one-digit number, the ten-digit won't be indicated.
If only the "M" user custom format specifier is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.

```lua
local date = DateTime(2022, 8, 29, 19, 7, 15, 023)
log(date:ToFormattedString("%M")) -- 8
```

#### "MM" 
The "MM" user custom format specifier indicates the month with a number from 01 to 12. If the month is a one-digit number, 0 will be indicated for the ten-digit.

```lua
local date = DateTime(2022, 8, 29, 19, 7, 15, 023)
log(date:ToFormattedString("MM")) -- 08
```

#### "MMM" 
The "MMM" user custom format specifier indicates the month's informal name. If you designate the cultural area, the informal name of the month corresponding to the cultural area will be displayed.

```lua
local date = DateTime(2022, 8, 29, 19, 7, 15, 023)
log(date:ToFormattedString("MMM")) -- Aug
log(date:ToFormattedString("MMM", "it-IT")) -- ago
```

#### "MMMM" 
The "MMMM" user custom format specifier indicates the whole name of the month. 

```lua
local date = DateTime(2022, 8, 29, 19, 7, 15, 023)
log(date:ToFormattedString("MMMM")) -- August
log(date:ToFormattedString("MMMM", "it-IT")) -- agosto
```

#### "s" 
The "s" user custom format specifier indicates the seconds with a number from 0 to 59. If the second is a one-digit number, the ten-digit won't be indicated.
If only the "s" user custom format specifier is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.
Seconds indicate the total number of seconds that have passed after the last minutes.
```lua
local date = DateTime(2022, 8, 29, 19, 7, 5, 023)
log(date:ToFormattedString("%s")) -- 5
```

#### "ss"
The "ss" user custom format specifier indicates the seconds with a number from 00 to 59. If the second is a one-digit number, 0 will be indicated for the ten-digit.
Seconds indicate the total number of seconds that have passed after the last minutes. 

```lua
local date = DateTime(2022, 8, 29, 19, 7, 5, 023)
log(date:ToFormattedString("ss")) -- 05
````

#### "t" 
The "t" user custom format specifier indicates the first character of the AM/PM specifier. 
The AM specifier is used for all times from 0:00:00 to 11:59:59.999, and the PM specifier is used for all times from 12:00:00 to 23:59:59.999.
If only "t" is used, you need to use the "%" user custom format specifier so as not to be interpreted incorrectly with the standard format string.

```lua
local date = DateTime(2022, 8, 29, 19, 7, 5, 023)
log(date:ToFormattedString("%t")) -- P
```

#### "tt"
The "tt" user custom format specifier indicates the AM/PM specifier.
The AM specifier is used for all times from 0:00:00 to 11:59:59.999, and the PM specifier is used for all times from 12:00:00 to 23:59:59.999.
```lua
local date = DateTime(2022, 8, 29, 19, 7, 5, 023)
log(date:ToFormattedString("tt")) -- PM
```

#### "y" 
The "y" user custom format specifier indicates the year with one-digit or two-digit numbers. If the year exceeds two-digit, only the last two-digit numbers will be indicated for the result. If the ten-digit of the year is 0, then 0 won't be indicated. For example, if the year is 2008, then only 8 will be indicated.
If only "y" is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.
```lua
local date = DateTime(2022, 8, 29, 19, 7, 5, 023)
log(date:ToFormattedString("%y")) -- 22
```

#### "yy" 
The "yy" user custom format specifier indicates the year with two-digit numbers. If the year exceeds two-digit, only the last two-digit numbers will be shown for the result.
If the ten-digit of the year shown is 0, it will be shown as it is. 
```lua
local date = DateTime(2022, 8, 29, 19, 7, 5, 023)
log(date:ToFormattedString("yy")) -- 22
```

#### "yyy" 
The "yyy" user custom format specifier indicates the year with at least three-digit numbers. Even if the valid number of digits for the year is more than three-digit, it will be included in the result string. 
If the year is less than the number of three digits, it will be indicated in three-digit by filling with 0.

```lua
local date = DateTime(2022, 8, 29, 19, 7, 5, 023)
log(date:ToFormattedString("yyy")) -- 2022
```

#### "yyyy" 
The "yyyy" user custom format specifier indicates the year with four-digit numbers. If the year is less than the number of four digits, it will be indicated in four-digit by filling with 0.
```lua
local date = DateTime(2022, 8, 29, 19, 7, 5, 023)
log(date:ToFormattedString("yyyy")) -- 2022
```

#### ":"
The ":" user custom format specifier indicates the time classification symbol used to classify hour, minute, and second. 
If only the ":" user custom format specifier is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.
```lua
-- today 2022/05/17
 
local date = DateTime("3:32:22", "h:mm:ss")
log(date) -- 05/17/2022 03:32:22
```

#### "/" 
The ":" user custom format specifier indicates the date classification symbol used to classify year, month, and day.
If only the "/" user custom format specifier is used, you need to use the "%" user custom format specifier in front so as not to be interpreted incorrectly with the standard format string.

```lua
local date = DateTime("2022/6/21", "yyyy/M/dd")
log(date) -- 06/21/2022 00:00:00
```