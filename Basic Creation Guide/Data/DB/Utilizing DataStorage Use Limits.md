# Course Introduction
The storage limit and Credit limit must be checked when using DataStorage. Let's learn about DataStorage usage limits.

# DataStorage Storage Limit
The following is the DataStorage storage limits. The string value size is calculated as the number of bytes when converted to a byte array via UTF-8 encoding. 

|Category | Storage Limit|
| --- | --- |
| DataStorage name| 1 ~ 64 byte | 
| DataStorage key| 1 ~ 100 byte |  
| DataStorage tag| 0 ~ 64 byte |
| DataStorage version | 0 ~ 64 byte |
| update |0 ~ 50,000 byte |
| set | 0 ~ 300,000 byte |

# Credit
Credit is a unit calculated to limit the number of database requests. The minimum value for Credit is 1. Credits are consumed only on actual requests to the database. 
FunctionGroups differ from each other in the number of Credits paid every minute, the number of Credits accumulated, and the number of Credits consumed on request by the database. To see which FunctionGroup belongs to each function, check out [Learn DataStorage Use Limits].

FunctionGroup| Credits Paid per Minute  | Max Credit That Can Be Accumulated | Credits Consumed on Request  |
| --- | --- | --- | --- |
| List DataStorage | 10 + (Number of Connections * 2) | Credits Paid per Minute * 2 | 1 |
| Delete DataStorage |10 + (Number of Connections * 2)  | Credits Paid per Minute * 2 | 1 |
| List Sorted | 50 + (Number of Connections * 2) | Credits Paid per Minute * 2 | 1 |
| List | 10 + (Number of Connections * 2) |  Credits Paid per Minute * 2|1  |
| Delete |  50 + (Number of Connections * 2)| Credits Paid per Minute * 2 | 1 per 4,000 Bytes |
| Get| 100 + (Number of Connections * 10) | Credits Paid per Minute * 2 |  1 per 4,000 Bytes |
| Set| 100 + (Online Players * 10) | Credit Given per Minute * 2 |   1 per 4,000byte |

#### Credit Consumption Depending on Byte Size
<1 Credit is used <span style="color: #dc9656">**from 0 to 4000 Byte**</span>. Credit is also consumed when searching for values that don't exist. 
The size of string value is calculated as the number of bytes when converted to a byte array via UTF-8 encoding.

><span style="color: #7cafc2">**Tip**
> **Key, tag and version** sizes do not affect Credit.</span>

#### Credit Consumption in Batch-Type Functions
Batch-type functions such as BatchGetAsync can access multiple keys at once. **The number of Credits consumed by these functions depends on the number of keys** on request.
Even when two values are requested, the amount of Credit consumed changes based on the requested number of Bytes. For example, if a request had two values of 2,000 Bytes each, a total of 2 Credits will be consumed. Meanwhile, when the two values are 4,000 Bytes and 4,500 Bytes, the 4,000 Byte value will cosume 1 Credit and the 4,500 Byte value will consume 2 Credits.
When using the batch-type functions, it's important to consider **when Credits will be consumed**. Credits are consumed at the time of actual request to the database. If you pass 50 keys to the `BatchGetAndWait()` function, it wouldn't consume Credit for the 50 values in a single call. The immediate call consumes Credit for 25 values, and the subsequent returning page consumes Credit for the remaining 25 values as it moves to the next page via `MoveToNextPageAndWait()`. 

><span style="color: #7cafc2">**Tip**
> The `BatchSetAndWait()` function requests the number of keys directly passed.</span>

#### Credit Consumption in Transact-Type Functions
Transaction-affiliated functions work similarly to batch-affiliated functions. However, more processing is required to guarantee a transaction, so it consumes **double the Credit** compared to batch affiliated functions.

#### Functions That Don't Consume Credit
When you used a function, but another function has already loaded a page, it doesn't consume Credit. For example, the `MoveToNextPageAndWait()` and `LoadNextPageAndWait()` functions do not consume Credit, depending on the situations. 
For example, when calling the `MoveToNextPageAndWait()` function after pre-loading the next Page of the GlobalDataStoragePages using `LoadNextPageAndWait()`, since the Page was already loaded, a request isn't made to the database. Therefore, Credit is only consumed when calling the `LoadNextPageAndWait()` function. 