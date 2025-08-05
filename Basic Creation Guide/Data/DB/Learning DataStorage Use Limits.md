# Course Introduction
Use of DataStorage above the threshold will be recorded in the critical report. In the future, limitations will be introduced to prevent usage above the threshold. We recommend creators maintain their data usage in advance. 
Use of DataStorage is limited to released Worlds only.
# Data Requests and Credit Consumption 
Data usage is calculated and applied at once across the entire World instance of a World built by the creator. 
DataStorage or similar functions use <span style="color: #dc9656">**1 Credit per request (Request)**</span> by default. However, you can also use multiple Credits if the size of a request is large. Also, the Credit consumption threshold depends on **FunctionGroup**. 
If you want to learn more about Credit, see [Utilizing DataStorage Use Limits]. 

# FunctionGroup
FunctionGroup is a standard for identifying functions that share Credit. 

#### List DataStorage
It is a group of functions that get a DataStorage list. It includes GetGlobalDataStoragePagesAndWait, GetUserDataStoragePagesAndWait, and more DataStorageService.

#### Delete DataStorage
This is a group of functions that remove DataStorage. It includes DeleteGlobalDataStorageAndWait, DeleteUserDataStorageAndWait, and DataStorageService.

#### List Sorted
It is a group of functions that get an arranged list of values. It includes GetSortedAndWait, GetPagesAndWait, and SortableDataStorage.

#### List
It is a group of functions that get a list of values. It includes GetPagesAndWait, GetVersionsAndWait, and GlobalDataStorage.

#### Delete
This is a group of functions that remove the values. It includes DeleteAndWait, BatchDeleteAndWait, and GlobalDataStorage.

#### Get
It is a group of functions that get the values. It includes GetAndWait, BatchGetAndWait, and GlobalDataStorage.

#### Set
This is a group of functions that save the values. It includes SetAndWait, UpdateAndWait of GlobalDataStorage, IncreaseAndWait of SortableDataStorage, and more.

#### None
None is a function that makes no request to the database.

# Guide to FunctionGroup

| Type | Functions | FunctionGroup |
| --- | --- | --- |
| @cols=1:@rows=23:DataStorageService | DeleteCreatorDataStorageAndWait  | Delete DataStorage | 
| DeleteCreatorDataStorageAsync | Delete DataStorage |
| DeleteGlobalDataStorageAndWait | Delete DataStorage |
| DeleteGlobalDataStorageAsync | Delete DataStorage |
| DeleteSortableDataStorageAndWait | Delete DataStorage |
| DeleteSortableDataStorageAsync | Delete DataStorage |
| DeleteUserDataStorageAndWait | Delete DataStorage |
| DeleteUserDataStorageAsync  | Delete DataStorage |
| GetAndWait | Get |
| GetAsync  | Get |
| GetCreatorDataStorage | None |
| GetDataStorage | None |
| GetGlobalDataStorage  | None |
| GetGlobalDataStoragePagesAndWait | List DataStorage |
| GetGlobalDataStoragePagesAsync | List DataStorage |
| GetSortableDataStorage  | None |
| GetSortableDataStoragePagesAndWait | List DataStorage |
| GetSortableDataStoragePagesAsync   | List DataStorage |
| GetUserDataStorage | None |
| GetUserDataStoragePagesAndWait  | List DataStorage |
| GetUserDataStoragePagesAsync | List DataStorage|
| SetAndWait | Set |
| SetAsync | Set |
| @cols=1:@rows=28:DataStorage Common | BatchDeleteAndWait | Delete|
| BatchDeleteAsync  | Delete |
| BatchGetAndWait   | Get  |
| BatchGetAsync | Get |
| BatchGetByInfoAndWait  | Get |
| BatchGetByInfoAsync | Get |
| BatchSetAndWait | Set |
| BatchSetAsync | Set |
| BatchSetByInfoAndWait | Set  |
| BatchSetByInfoAsync | Set |
| DeleteAndWait | Delete |
| DeleteAsync | Delete  |
| GetAndWait | Get  |
| GetAsync | Get  |
| GetByInfoAndWait | Get   |
| GetByInfoAsync | Get  |
| GetVersionsAndWait | List |
| GetVersionsAsync | List  |
| SetAndWait | Set  |
| SetAsync | Set  |
| SetByInfoAndWait | Set  |
| SetByInfoAsync | Set  |
| TransactSetAsync | Set |
| TransactSetAndWait | Set |
| TransactSetByInfoAsync | Set |
| TransactSetByInfoAndWait | Set |
| TransactDeleteAsync | Delete |
| TransactDeleteAndWait | Delete |
| @cols=1:@rows=6:GlobalDataStorage, UserDataStorage, CreatorDataStorage | UpdateAndWait | Set|
| UpdateAsync | Set |
| UpdateByInfoAndWait | Set |
| UpdateByInfoAsync  | Set |
| GetPagesAndWait  | List |
| GetPagesAsync  | List |
| @cols=1:@rows=6:SortableDataStorage | GetSortedAndWait | List Sorted |
| GetSortedAsync | List Sorted |
| IncreaseAndWait | Set |
| IncreaseAsync | Set |
| GetPagesAndWait | List Sorted |
| GetPagesAsync | List Sorted |