# Course Introduction
You can move from one world to another using the TeleportService function.

# Move to Another World
TeleportService can move users to a certain place, or move from one connected world to another. The movement is available when the connecting world and the world to move to have both been released. For this reason, it is not possible to test warp during production. 

| Current World | World to Move | Warp or Not |
| --- | --- | --- |
| Published | Published | <span style="color: #dc9656">**◯**</span> |
| Not Published | Published | <span style="color: #dc9656">**◯**</span> |
| Published | Not Published | X |
| Not Published | Not Published | X |

<br>When moving a user to another world, `WarpUserToWorldAndWait()` and `WarpUserToWorldAsync()` functions are used, and two pieces of data, such as <span style="color: #dc9656">**the user ID to move and the world ID to move to**</span>, are required. 
If you want to use the data that the user collected from the current world in the new world to move to when a user warps to another world, you can transfer the data when the user moves by using `WarpUserToWorldAndWait()` and then access and acquire the data with GetWarpRecord().
For detailed information, please refer to [TeleportService](/apiReference?postId=315{"target":"_self"}).<br>

><span style="color: #7cafc2">**Tip**
> You can warp to every world released, even if the creator has not made those worlds.</span>
# See World ID
You can see WorldID with the URL of the world information page on the MapleStory Worlds official website. The numbers at the end of the URL may be used as a World ID.
* https://maplestoryworlds.nexon.com/play/<span style="color: #dc9656">**Number**</span> 

![WorldID](https://mod-file.dn.nexoncdn.co.kr/bbs/1675147124079c0e09e99888c4f9bb0ed894fa8d6ba59.png "WorldID")
# Use Example
#### To Warp
Let's make it so a user can warp to another world through a portal.

![worldwarp](https://mod-file.dn.nexoncdn.co.kr/bbs/16751473183921f72083c0b094c9fb0a4aa7d407d2b8d.gif "worldwarp")

1. New: Create a new Warp ![component](https://mod-file.dn.nexoncdn.co.kr/storage/icons/component/Ect.png "component") component and add it to a portal.
2. Add WorldId to Property and enter World ID on the Property Editor window.<br>![warp](https://mod-file.dn.nexoncdn.co.kr/bbs/16577946836778dbd808244d6421e99a67c482d4aa1b8.png "warp")
3. To make it so a user can move to the world assigned, write the following portal event:

    ```lua
    Property:
    [Sync]
    string WorldId = ""
     
    Method:
    [server]
    void Warp(string userId)
    {
        local warpDataTable = {}
        warpDataTable.userId = userId
                 
        local serializedData = _UtilLogic:TableToString(warpDataTable)
                 
        _TeleportService:WarpUserToWorldAndWait(userId, self.WorldId, serializedData)
    }
     
    Event Handler:
    [self]
    HandlePortalUseEvent(PortalUseEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: PortalComponent
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        local PortalUser = event.PortalUser
        ---------------------------------------------------------
     
        self:Warp(PortalUser.Name)
    }
    ```

#### See If a User Entered into a World through a Warp
With `GetWarpRecord()`, you can see if a user entered a world through a warp. 

1. Create a new script component ![common](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_asset_no.png "common"), and add it to Common.
2. Write the following to see if a user entered the world via a warp:
    ```lua
    Event Handler:
    [server only] [service: UserService]
    HandleUserEnterEvent(UserEnterEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: UserService
        -- Space: Server
        ---------------------------------------------------------
        
        -- Parameters
        -- local ProfileCode = event.ProfileCode
        local UserId = event.UserId
        ---------------------------------------------------------
     
        local warpRecord = _TeleportService:GetWarpRecord(UserId)
        
        -- For a case of general world entry GetWarpRecord() returns nil.
        if warpRecord == nil then 
            return
        end
    }
    ```
#### Check the Warped-From World
With `GetWarpRecord()`, you can see which world the user who moved by Warp came from.

1. Create a new script component ![common](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_asset_no.png "common"), and add it to Common.
2. Write the following to see which world a user who moved using warp came from:
    ```lua     
    Event Handler:
    [server only] [service: UserService]
    HandleUserEnterEvent(UserEnterEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: UserService
        -- Space: Server
        ---------------------------------------------------------
        
        -- Parameters
        -- local ProfileCode = event.ProfileCode
        local UserId = event.UserId
        ---------------------------------------------------------
     
        local warpRecord = _TeleportService:GetWarpRecord(UserId)
        if warpRecord == nil then
            return
        end
         
        local currentWorldId = warpRecord.CurrentWorldId
        local prevWorldId = warpRecord.PreviousWorldId -- You can see the previous world.
    }
    ```

#### Deliver Data to Another World
You can deliver a user's data to the world the user moves to.

1. Create a new script component.
2. Add a portal to the map, then add the created script component to the portal.
3. You can deliver the data to another World by using `WarpUserToWorldAndWait()` and `UtilLogic` as below. 
    ```lua
    Property:
    [Sync]
    string WorldId = ""
     
    Method:
    [server]
    void Warp(string userId)
    {
        local warpDataTable = {}
        warpDataTable.testData = "test"
                 
        local warpData= _UtilLogic:TableToString(warpDataTable)
                 
        _TeleportService:WarpUserToWorldAndWait(userId, self.WorldId, warpData)
    }
     
    Event Handler:
    [self]
    HandlePortalUseEvent(PortalUseEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: PortalComponent
        -- Space: Client
        ---------------------------------------------------------
        
        -- Parameters
        local PortalUser = event.PortalUser
        ---------------------------------------------------------
        
        self:Warp(PortalUser.Name)
    }
    ```

#### Get Data Delivered from the Previous World
You can receive the values delivered from the previous world.

1. Create a new script component ![common](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_asset_no.png "common"), and add it to Common.
2. Write using `WarpUserToWorldAndWait()` and `UtilLogic` as below. 

    ```lua       
    Event Handler:
    [server only] [service: UserService]
    HandleUserEnterEvent(UserEnterEvent event)
    {
        --------------- Native Event Sender Info ----------------
        -- Sender: UserService
        -- Space: Server
        ---------------------------------------------------------
        
        -- Parameters
        -- local ProfileCode = event.ProfileCode
        local UserId = event.UserId
        ---------------------------------------------------------
         
        local warpRecord = _TeleportService:GetWarpRecord(UserId)
        if warpRecord == nil then
            return
        end
         
        local warpDataTable = _UtilLogic:StringToTable(warpRecord.Data)
        log(warpDataTable.testData)
    }
    ```


##### Reference Guide
* [Teleportation]