# Course Introduction
We recommend studying this guide in parallel with [Saving and Importing Data DB].
In "Saving and Importing Data DB", we learned how to create, save, and load data conveniently.
This guide will teach you how to utilize data according to the purposes of creators, such as increasing data using various functions of DataStorageService or loading pages.
##### Reference Guide
* [Learning DataStorage Use Limits]
* [Utilizing DataStorage Use Limits]
# DataStorage Types and Introduction
DataStorage is divided into 4 types, and the creator can choose and use it according to the scope and purpose. DataStorage is independent, and it does not distinguish identifiers. If there are two different types of 'Data A' data storage, they are treated as separate data storage.
GlobalDataStorage, UserDataStorage, and CreatorDataStorage can only use string-type values, and SortableDataStorage can only use int type values. You must identify the type as you use them. If CreatorDataStorage is used in cooperation, the co-group itself holds DataStorage.

><span style="color: #7cafc2">**Tip**
> If you want to use a value of a type other than a string value, you must change it to a string using `_UtilLogic:TableToString()`.</span>

Different functions are available for each of the 4 data storages. Please check each API Reference for details.

* <span style="color: #dc9656">**GlobalDataStorage**</span>
For data storage used in one world, the data is not shared with other worlds. When importing a datastore, you can give it a name, and you can create and have multiple data storages. Note that only string-type values can be used. [GlobalDataStorage](/apiReference?postId=708{"target":"_self"}) (check the API Reference).
* <span style="color: #dc9656">**UserDataStorage**</span>
You can have one UserDataStorage per user. For data storage used in one world, the data is not shared with other worlds. Data storage is fetched using userId. Note that only string-type values can be used. This data storage can be used to save the access time of the user or to retrieve the item information in the world currently owned. [UserDataStorage](/apiReference?postId=716{"target":"_self"}) (check the API Reference).
* <span style="color: #dc9656">**CreatorDataStorage**</span>
You can have one CreatorDataStorage per creator. You can share data with other worlds. Note that only string-type values can be used. [CreatorDataStorage](/apiReference?postId=701{"target":"_self"}) (check the API Reference).
* <span style="color: #dc9656">**SortableDataStorage**</span>
For data storage used in one world, the data is not shared with other worlds. You can create and have multiple data storages, and you can name them. Note that only int type values can be used, and sort and increment functions can be used. [SortableDataStorage](/apiReference?postId=710{"target":"_self"}) (check the API Reference).


#### ErrorCode
When you perform various data-related operations such as saving, deleting, loading, incrementing, and traversing data using the functions of DataStorage, you can check whether the operation succeeded or failed with an error code. When the function request is successful, the error code is 0.
For example, you need to write errorCode together as `local errorCode, successKeys = globalDataStorage:BatchSetAndWait(keyValues)`.

| ErrorCode | Error Name | Desc |
| --- | --- | --- |
| 0 | Ok | Your request has been completed.  |
| 1000000 | Canceled | Your request has been cancelled. |
| 1000001 | InternalError | An internal error has occurred.  |
|1000002  | NotFound | An error occurred because the element was not found. |
| 1000003 | BadRequest | An error occurred due to an invalid request. |
| 1000004 | TimedOut | An error occurred because the request took too long to process. |
| 1000005 | ResourceExhausted | An error occurred because the number of calls exceeded the limit. |
| 1000006 | PartialFailure | An error occurred due to request failures. |
| 2000000 | UpdateApiFailed | An error occurred due to failed update requests. |
| 2147483647 | Unknown | An unknown error has occurred. |

#### Version
As the version of the value, you can use Version to get the value at a desired point in time or roll back. Versions can be explicitly specified by the creator and are prioritized according to the time when the stated version was first created. If Version is not specified, the default version operates. If at least one previously saved version exists, it operates as the last saved version. For example, if you add version A, then add version B, then add version A again, the latest version becomes version B. This is because version A already has a created history.

#### Tag
Tag is used to get the list as an additional identifier. You can also use tags to get the values of a desired group selectively. You may not specify a tag value, but in this case, you will not get a list of values you do not specify. 
Since tag does not separate groups, if TagA tag value is saved in AKey key value and TagB tag value is saved in AKey key value, the tag is overwritten with TagB.

# DataStorageKeyInfo
[DataStorageKeyInfo](/apiReference?postId=704{"target":"_self"}) is an object for embedding more information in the data. DataStorageKeyInfo can specify Key, Tag, and Version.

# Storage Item
Objects with the Item suffix are classified into two types for use.
[DataStorageItem](/apiReference?postId=702{"target":"_self"}) represents the stored data in GlobalDataStorage, UserDataStorage, CreatorDataStorage. KeyInfo and Value are included, and the type of Value is string.
[SortableDataStoragePages](/apiReference?postId=713{"target":"_self"}) means the stored data in SortableDataStorage. KeyInfo and Value are included, and the type of Value is int. 

# Storage Pages 
[GlobalDataStoragePages](/apiReference?postId=709{"target":"_self"}), [SortableDataStoragePages](/apiReference?postId=713{"target":"_self"}), [UserDataStoragePages](/apiReference?postId=717{"target":"_self"}), [DataStorageItemPages](/apiReference?postId=703{"target":"_self"}), [SortableDataStorageItemPages](/apiReference?postId=712{"target":"_self"}), [DataStorageVersionPages](/apiReference?postId=706{"target":"_self"}) is an object for retrieving information related to DataStorage. All page objects provide the IsLastPages property and GetCurrentPageDatas() and MoveToNextPageAndWait() functions. You can use these two functions to get a list of data or move to the next page. Please check each API Reference for details.

# Use Example
Data storage is mainly used together with DataStorageService. DataStorageService provides functions to access multiple data storage. You can use data in various ways, such as saving, deleting, loading, and viewing data. Functions for utilizing data should be used according to the type of data storage. 

## Storing Data
In order to store data, the type of data storage must be clearly distinguished and used. This is because the type of value that stores data is different for each type of data storage. Global, User, Creator data storage can only store values as strings, and Sortable data storage can only store values as integers. For example, when you store the value A as string Grade in datastore "globalDS" and get value from GlobalDataStorage named "globalDS" via key value named "Grade", the value "A" is returned. 

#### GlobalDataStorage

```lua
[server only]
void SaveDataStorage()
{
    local globalDataStorage = _DataStorageService:GetGlobalDataStorage("globalDS")
    globalDataStorage:SetAndWait("Grade", "A")
}
```

#### SortableDataStorage
GetSortableDataStorage uses a different type, but the storage method is the same as described above.

```lua
[server only]
void SaveDataStorage()
{
    local sortableDataStorage = _DataStorageService:GetSortableDataStorage("sortableDS")
    sortableDataStorage:SetAndWait("Score", 90)
}
```

#### Storing Data with More Information
You can save by designating Tag and Version along with key values in DataStorage. DataStorageKeyInfo must be used to save the two new values. Key, Tag, and Version can be used as parameters like `DataStorageKeyInfo("key", "Tag", "Version1")`. If DataStorageKeyInfo is used without Tag or Version, it is the same as saving data using the SetAndWait and SetAsync functions. In addition, it is possible to save using DataStorageKeyInfo with the SetByInfoAndWait and SetByInfoAsync functions.

```lua
[server only]
void SaveDataStorage()
{
    local globalDataStorage = _DataStorageService:GetGlobalDataStorage("globalDS")
    local keyInfo = DataStorageKeyInfo("key", "Tag", "Version1") -- In this way, you can specify each value when creating it or fill in only the desired values after creating it with an empty constructor.
    keyInfo = DataStorageKeyInfo()
    keyInfo.Key = "Key"
    keyInfo.Version = "Version1"
     
    globalDataStorage:SetByInfoAndWait(keyInfo, "Value")
}
```

## Data Import
When retrieving saved data, GetGlobalDataStorage, GetUserDataStorage, GetCreatorStorage, and GetSortableDataStorage are used depending on the data storage type. Since the functions to import data are Server Only, we recommend loading data in the server space. 

#### GlobalDataStorage 
When calling GlobalDataStorage, a name must be specified. This is because different types of data storage with the same name may exist depending on the creator. 

```lua
[server only]
void getDataStorage()
{
    local globalDataStorage = _DataStorageService:GetGlobalDataStorage("globalData")
}
```

#### UserDataStorage 
Since UserDataStorage is unique data for each user, you can load data by entering userId as a parameter.  Since it is meant to load the data the user has, the code below should be added to DefaultPlayer. 

```lua
[server only]
void getDataStorage()
{
    local userId = self.Entity.Name
    local userDS = _DataStorageService:GetUserDataStorage(userId)
}
```

#### CreatorDataStorage 
Since we have one data storage per creator, we can load data without specifying a key.

```lua
[server only]
void getDataStorage()
{
    local creatorDS = _DataStorageService:GetCreatorDataStorage()
}
```

#### SortableDataStorage 
```lua
[server only]
void getDataStorage()
{
    local sortableDS = _DataStorageService:GetSortableDataStorage("sortableDS")
}
```
## Loading Data
Use the GetAndWait and GetAsync functions when importing data. GlobalDataStorage, UserDataStorage, and CreatorDataStorage can only get the string-type values.
If the data is converted to a string using the `_UtilLogic:TableToString()` function when saving data, it can be converted to an existing type using the `_UtilLogic:StringToTable()` function when retrieving data. SortableDataStorage can only get integer-type values. 

> <span style="color: #7cafc2">**Tip**
> Distinction between Async and AndWait
> The two suffixes in front of the functions in DataStorageService indicate whether the functions are asynchronous or synchronous.
> Async is performed asynchronously, and the function delivered as the callbackFunction parameter is called when the function's operation is completed.
> AndWait runs synchronously, stopping script execution until the function's task is complete.</span>

This is an example of getting the value of GlobalDataStorage named `globalDS` using a key value of "Grade."
```lua
local globalDataStorage = _DataStorageService:GetGlobalDataStorage("globalDS")
local errorCode, grade = globalDataStorage:GetAndWait("Grade") -- Return previously stored "A" value
```

#### Loading Data with More Information
When retrieving the value of DataStorage, you can specify the Version to get the value of a specific version. Unlike general data retrieval, you must create an entity with DataStorageKeyInfo and specify the entity as a parameter to retrieve a value with more information.
DataStorageKeyInfo can specify the value of Key, Tag, and Version. When importing data, even if the tag value is specified, it is not used and it does not affect the operation. If you use DataStorageKeyInfo without specifying the version, it will behave the same as getting the value using the GetAndWait and GetAsync functions, and the value of the last saved version (the latest version) will be retrieved.
It is possible to load using DataStorageKeyInfo by using the GetByInfoAndWait and GetByInfoAsync functions.

```lua
local globalDataStorage = _DataStorageService:GetGlobalDataStorage("globalDS")
local setterKeyInfo = DataStorageKeyInfo()
setterKeyInfo.Key = "Key"
setterKeyInfo.Version = "Version1"
globalDataStorage:SetByInfoAndWait(setterKeyInfo, "Value1")
 
setterKeyInfo.Key = "Key"
setterKeyInfo.Version = "Version2"
globalDataStorage:SetByInfoAndWait(setterKeyInfo, "Value2")
 
local errorCode, value = globalDataStorage:GetAndWait("Key")
log(value) -- Value2 The latest version of the value is fetched by default.
 
local getterKeyInfo = DataStorageKeyInfo()
getterKeyInfo.Key = "Key"
getterKeyInfo.Version = "Version1"
 
local errorCode, value = globalDataStorage:GetByInfoAndWait(getterKeyInfo)
log(value) -- Value1 You can import by specifying the specific version you want.
```

## Increasing Data
SortableDataStorage can increase the value using the IncreaseAndWait and IncreaseAsync functions. Each function increments or decrements the value of that parameter by the value of the delta input. It increases when the value of the parameter is a positive number and decreases when it is a negative number.
If the value to be increased does not exist in DataStorage, it is saved as default value 0 and increased by delta. The IncreaseAndWait and IncreaseAsync functions give the error code and the incremented value.

This is an example of creating and increasing a value that is not stored in DataStorage with the IncreaseAndWait function.

```lua
local sortableDataStorage = _DataStorageService:GetSortableDataStorage("sortableDS")
 
-- Assume that "Key" in sortableDataStorage has no stored history.
local errorCode, value = sortableDataStorage:IncreaseAndWait("Key", 5)
log(value) -- 5, returns 5 because it has been created with the default value of 0 and then increased by delta.
 
local errorCode, value = sortableDataStorage:IncreaseAndWait("Key", 10)
log(value) -- 15, returning 15 because the previously stored value of 5 is incremented by delta.
```

## Deleting Data
You can remove data with the DeleteAndWait or DeleteAsync function. 

This example removes the value corresponding to the key value named Grade from GlobalDataStorage named globalDS.

```lua
local globalDataStorage = _DataStorageService:GetGlobalDataStorage("globalDS")
globalDataStorage:DeleteAndWait("Grade")
local errorCode, grade = globalDataStorage:GetAndWait("Grade") -- "Grade" cannot get a valid value because it has been removed.
```

## Loading Data Lists
You can query the list of DataStorage stored as DataStorageService. If you call the function, you can get GlobalDataStoragePages, SortableDataStoragePages, and UserDataStoragePages that can look up the list of the corresponding DataStorage. However, in the case of CreatorDataStorage, there is no function to get the list because there is only one. The function to get a list provides AndWait and Async, and you can use the desired form depending on the purpose.

```lua
local errorCode, globalDataStoragePages = _DataStorageService:GetGlobalDataStoragePagesAndWait()
```
```lua
local errorCode, sortableDataStoragePages = _DataStorageService:GetSortableDataStoragePagesAndWait()
```
```lua
local errorCode, userDataStoragePages = _DataStorageService:GetUserDataStoragePagesAndWait()
```

#### Traversing Pages
When receiving a DataStorage list or a data list, it receives a value in the form of Pages, an object that can search the list page by page.

```lua
local errorCode, globalDataStoragePages = _DataStorageService:GetGlobalDataStoragePagesAndWait()
 
while true do
 
    local globalDataStorages = globalDataStoragePages:GetCurrentPageDatas() --You can get a list of current pages with GetCurrentPageDatas().
 
    for _, globalDataStorage in pairs(globalDataStorages) do
        log(globalDataStorage.Name)
    end
     
    if globalDataStoragePages.IsLastPage == true then -- You can check if the current page is the last page through the IsLastPage property.
        break
    end
    globalDataStoragePages:MoveToNextPageAndWait() --MoveToNextPageAndWait() to move to the next page.
end
```

## Batch Request
You can make batch requests to store, retrieve, or remove data using the Batch prefix function. The `BatchSetAndWait()` and `BatchSetAsync()` functions provide a key table of error codes and values for which the request was successful. 
When storing, retrieving, or removing multiple values, you should utilize Batch-prefixed functions to make many requests quickly. Functions with the Batch prefix may only succeed in some of the requested actions. Therefore, you must check the result values and handle any unsuccessful actions properly.

The following is an example of saving data in batches.

```lua
local globalDataStorage = _DataStorageService:GetGlobalDataStorage("globalDS")
local keyValues = {}
keyValues["key1"] = "value1"
keyValues["key2"] = "value2"
keyValues["key3"] = "value3"
 
local errorCode, successKeys = globalDataStorage:BatchSetAndWait(keyValues)
```

## Atomic Batch Request
You can make atomic batch requests to store, retrieve, or remove data using the Transact prefix function. Function will not be called if any of the tasks in the function call fail. You can manipulate up to 20 keys at a time.
If all of the requests delivered at once don't succeed, they all fail, and the data before the request is retained. This simplifies the code related to the error handling. However, in this case, some complex processing should be done internally to ensure transactions. In addition, the credit is consumed twice as much as the Batch function.

> <span style="color: #585858">**Learn More**
> Transaction is a unit of task that should be processed atomically.</span>

The following is an example of saving data in batches if all tasks are successful. 

```lua
local transactDs = _DataStorageService:GetGlobalDataStorage(self.TransactionTestDsName)
local t1 = {}
t1["key1-1"] = "value1"
t1["key1-2"] = "value2"
t1["key1-3"] = "value3"

local t2 = {}
t2["key2-1"] = "value1"
t2["key2-2"] = "value2"
t2["key2-3"] = "value3"

local callback = function (code, keys)
    if code ~= 0 then
        error("! error. TransactSetAsync. code:"..tostring(code))
        return
    end
     
    for i, key in ipairs(keys)  do
        log("["..tostring(i).."] "..key)       
    end
end

-- ~AndWait
local code1, resultKeys1 = transactDs:TransactSetAndWait(t1)
for i, key in ipairs(resultKeys1) do
    log("["..tostring(i).."] "..key)
end
​
-- ~Async
transactDs:TransactSetAsync(t2, callback)
```

The following is an example of deleting data in batches if all tasks are successful.

```lua
log("# Test_DataStorage_TransactDelete")
local transactDs = _DataStorageService:GetGlobalDataStorage(self.TransactionTestDsName)
local keys1 = {"key1-1", "key1-2", "key1-3"}
local keys2 = {"key2-1", "key2-2", "key2-3"}
local code1, resultKeys1 = transactDs:TransactDeleteAndWait(keys1)
log("code1: "..tostring(code1))
for _, item in pairs(resultKeys1) do
    log("- deleted key: "..item)
end
​
local callback = function (code, keys)
    if code ~= 0 then
        error("! error. TransactDeleteAsync. code:"..tostring(code))
        return
    end
     
    for i, key in ipairs(keys)  do
        log("- deleted key: "..key)
    end
end
​
transactDs:TransactDeleteAsync(keys2, callback)
```

## Sorting Data
SortableDataStorage can sort data with the GetSortedAndWait and GetSortedAsync functions. When sorting and importing data, you can specify whether to sort in ascending or descending order and sort and import a list of values in a range of your choice by specifying the minimum and maximum values.

This is an example of sorting and retrieving data stored in SortableDataStorage.

```lua
local sortableDataStorage = _DataStorageService:GetSortableDataStorage("sortableDS")
 
-- Only the values between the minimum value 0 and the maximum value 100 are sorted and retrieved.
local errorCode, pages = sortableDataStorage:GetSortedAndWait(SortDirection.Ascending, 0, 100)
 
while true do
 
    local items = pages:GetCurrentPageDatas()
    for _, item in pairs(items) do
        log(item.KeyInfo.Key)
        log(item.Value)
    end
 
    if pages.IsLastPage == true then
        break
    end
    pages:MoveToNextPageAndWait()
end
```

## Managing Versions
If Version is specified using DataStorageKeyInfo when saving the value of DataStorage, you can manage the version. Instead of overwriting the existing version, you can create a new version and even overwrite the old version if the latest version is not valid. You can check the list of existing versions with a function that returns a list of versions.

#### Create a New Version

```lua
local globalDataStorage = _DataStorageService:GetGlobalDataStorage("globalDS")
local keyInfo = DataStorageKeyInfo()
keyInfo.Key = "Key"
keyInfo.Version = "Version1"
 
globalDataStorage:SetByInfoAndWait(keyInfo, "Value")
 
-- Create a new version and save it instead of overwriting the "Value" of the existing version.
keyInfo.Key = "Key"
keyInfo.Version = "Version2"
globalDataStorage:SetByInfoAndWait(keyInfo, "Value2")
 
-- The "Value" is not overwritten, so it is still valid, and you can get the "Value" by specifying Version1.
keyInfo.Key = "Key"
keyInfo.Version = "Version1"
local errorCode, value = globalDataStorage:GetByInfoAndWait(keyInfo)
```

#### Returning to a Previous Version

```lua
local globalDataStorage = _DataStorageService:GetGlobalDataStorage("globalDS")
keyInfo = DataStorageKeyInfo()
keyInfo.Key = "Key"
keyInfo.Version = "Version1"
 
globalDataStorage:SetByInfoAndWait(keyInfo, "Value")
 
-- Create a new version and save it instead of overwriting the "Value" of the existing version.
keyInfo.Key = "Key"
keyInfo.Version = "Version2"
globalDataStorage:SetByInfoAndWait(keyInfo, "Value2")
 
-- The "Value" is not overwritten, so it is still valid and you can get "Value" by specifying Version1.
keyInfo.Key = "Key"
keyInfo.Version = "Version1"
local errorCode, oldValue = globalDataStorage:GetByInfoAndWait(keyInfo)
 
-- Get values from the previous version and overwrite the newer version. After that, calling the GetAndWait function returns "Value", not "Value2".
keyInfo.Key = "Key"
keyInfo.Version = "Version2"
globalDataStorage:SetByInfoAndWait(keyInfo, oldValue)
```

#### Importing Version List

```lua
local globalDataStorage = _DataStorageService:GetGlobalDataStorage("globalDS")
 
-- Get a list of versions from April 20th to May 20th only.
local minDate = DateTime(2022, 4, 20)
local maxDate = DateTime(2022, 5, 20)
 
local errorCode, pages = globalDataStorage:GetVersionsAndWait("Key", SortDirection.Ascending, minDate, maxDate)
 
while true do
 
    local versions = pages:GetCurrentPageDatas()
 
    for _, versionInfo in pairs(versions) do
        log(versionInfo.CreateTime) -- Tells you the time it was created.
        log(versionInfo.Version) -- Tells you the Version set by the user.
    end
 
    if pages.IsLastPage == true then
        break
    end
    pages:MoveToNextPageAndWait()
end
```
