# Course Introduction 
We'll learn about how to save the value of data changed during gameplay in GlobalDataStorage and import it.

# Creating and Importing Data
GlobalDataStorage is storage where you can save and load data. We use this storage whenever we need to save or import data from the DB.
GlobalDataStorage can be created or loaded through `_DataStorageService:GetGlobalDataStorage`. The value of the name passed onto GetGlobalDataStorage as a parameter determines if either a new GlobalDataStorage is created and returned, or if the key value is already registered. Then, it returns the initially created GlobalDataStorage. You can create multiple GlobalDataStorages depending on the key value. You can save to different GlobalDataStorages according to the property or intended use of the data. In addition, `_DataStorageService:GetGlobalDataStorage` is a Server Only function, so it is recommended to load data from the server space.
```lua
void FunctionExample()
{
    -- Create a GlobalDataStorage named MyData to save data
    local MyData = _DataStorageService:GetGlobalDataStorage("MyData") 
}
```

# Saving Data
Data can be saved by using SetAsync from GlobalDataStorage.
`SetAsync` receives key and value as parameters and a Callback function. If there is any data to save, it is saved by passing the value as key and value parameters.
The key changes every time and is used for the purpose of calling the saving value. Therefore, for the key value, inserting the property name, or entering a name that tells you what the saved value means, is a good habit to get into. For example, when saving the value of a property (or stat) called "Power," insert "Power" for the key. For the value, convert the value of Power into a string and enter it. If it was saved as the same key value before, the current value will replace the previous value. 
```lua
Property: 
[Sync]
number Power = 100
 
Method: 
[server only]
void SetData()
{
     local data = _DataStorageService:GetGlobalDataStorage("data") 
     data:SetAsync("Power", tostring(self.Power), nil) 
     --Saves the value of property Power as a key called "Power". The value is saved as a string.  
}
```
<br>
The Callback function is called when data is saved. For parameters, the errorCode and key values are passed on.
When a callback function is called, the value is passed on to the parameter internally. If you don't want to register the callback function, pass on nil. Since saving the data via SetAsync is done asynchronously, the value isn't saved while SetAsync is running. Simply put, SetAsync sends a request for saving values while the DB, which receives the request and saves the requests in chronological order starting with the oldest one.
This means that the point at which the value will be saved isn't guaranteed. In this case, handling isn't added after calling SetAsync. Instead, handling is processed from the moment of saving values by passing the callback function as a parameter.

```lua
Property: 
[Sync]
number Power = 100

Method: 
[server only]
void FunctionExample()
{
    -- Compares the log of the moment when SetAsync ended and the moment when the log of callback function is printed.
    -- As callback function is called when the value is actually saved, the log of the callback function is printed later than the log shown after the completion of SetAsync.
    local data = _DataStorageService:GetGlobalDataStorage("data")
    self.Power = 20
    local callBack = function (errorcode, key)
        log(key.."Value saved.")
    end
    data:SetAsync("Power", tostring(self.Power), callBack)
    log("SetAsync Completed")
}
```

# Importing Data
`GetAsync` is a function to import data, which gets the value via the key value saved together when saving the value.
Just like SetAsync, GetAsync also requests a value for the key value. The requested value isn't directly returned from within the function, but it is returned as the parameter for the callback function of GetAsync with the key value. This is because GetAsync is an asynchronous function. When a request to import is sent, the exact moment when the value will be sent isn't guaranteed. Therefore, as GetAsync must be handled when the value was received, unlike SetAsync, registration of a Callback function is required.
```lua
Property: 
[Sync]
number Power = 0
 
Method:
[server only]
void OnBeginPlay()
{
    local data = _DataStorageService:GetGlobalDataStorage("Data") 
    local callBack = function(errorcode, key, value) 
        if key == "Power" then 
            self.Power = tonumber(value)
        end 
    end 
    data:GetAsync("Power", callBack)
}
```
<br>
If you import when there isn't any value saved as a key value, then nil will be imported. When importing, you may check whether the value is nil in order to process the unsaved value.

```lua
Property: 
[Sync]
number Power = 0
 
Method: 
[server only]
void OnBeginPlay()
{
    local data = _DataStorageService:GetGlobalDataStorage("Data")
    local callBack = function(errorcode, key, value)
        --Adds processing if value is nil.
        if value == nil then value = 0 end
            if key == "Power" then
                self.Power = tonumber(value)
            end
    end
    data:GetAsync("Power", callBack)
}
```
# Resetting Data Storage
Data saved during test play does not go away after exiting the game. To reset this data, you can go to Settings - Create - Data Storage Setting.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/165656279468265d7796bbba149e583187ad5263b5d8a.png "2")

##### Reference Guide
* [Utilizing DataStorage](/docs/?postId=692{"target":"_self"})