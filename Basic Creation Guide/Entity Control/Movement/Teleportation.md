# Course Introduction
Let's create a feature that moves a character to a different map every time a specific time is reached, and take a look at how to use the `TeleportToEntityPath` function of `TeleportService`.

##### API Reference
[TeleportService](/apiReference/Services/TeleportService{"target":"_self"})

# Creating the Destination Map
First, create the map the character will move to.

1. Select **Window - Map List**.
![Teleport_1](https://mod-file.dn.nexoncdn.co.kr/bbs/168775942033420815d67e57a4a5e8089b084e57856fa.png "Teleport_1")
<br>
2. Click **[Create New Map]** button in the **Map List** window.
Since we're going to make the teleport cycle through three maps, add two new maps.
![Teleport_2](https://mod-file.dn.nexoncdn.co.kr/bbs/1656418919196d6fe11492e9648bd8ce12f3dfa31a0b3.png "Teleport_2")
<br>
3. Decorate each map so that they can be distinguished from each other when you move to another map. Also, place an entity that will be the destination when you teleport to each map.
    
    | **Map** | **Map Creation Example** |
    | --- | ------- |
    | map01 | ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/165665674992362b0791572c143caac4932432ffabccd.png "1") |
    | map02 | ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16566567844498a1bd211546447a397a044a8bc9c5df1.png "2") |
    | map03 | ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1656656814159bbb5a8e0966b4ae388dda9382e40ec95.png "3") |
    <br>
4. Check the path for the destination entity.
Open the context menu of each entity in the **Hierarchy** and click **Copy Entity Path** to copy the path and record it separately.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/16862103427568f6a56e7203741289157a083b3cd4c20.png "6")
Paths for the destination entities in this example are as follows:
    
    | **Map** | **Destination Entity Path** |
    | --- | ------------ |
    | map01 | /maps/map01/object-19\_1 |
    | map02 | /maps/map02/object-6\_1 |
    | map03 | /maps/map03/object-42\_1 |
	
# Creating a Teleport Feature

In order to teleport, we'll need some scripts.
Create a new script component and add it to the character.

1. In the context menu of **Workspace - MyDesk**, click **Create Scripts - Create Component** to add a new component.
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1687756376627c84b1418872c42a2bb42529622985e1f.png "7")
    <br>
2. Name the component <span style="color: #dc9656">**Teleport**</span>.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1635423340528786df9745a5e455c96c59ebc7d949ce9.png "8")
    <br>
3. Add the **Teleport** component to **Workspace - DefaultPlayer**.
    ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1635423349810751f20bc31d24e4aa10027ab4d58f36e.png "9")
    <br>

Now let's get down to writing the script.

1. Open the **Teleport** script.
    ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/163542336059313a91256a8d64ff6bfb60b3534702e55.png "10")
    <br>
2. Add the `OnUpdate` function to the script and write as follows:
    ```lua
    [server only]
    void OnUpdate(number delta)
    {
        --Property synchronization is not necessary, so we use _T property
        if self._T.accTime == nil then self._T.accTime = 0 end
        if self._T.teleportCnt == nil then self._T.teleportCnt = 0 end
        
        --Declares table type property, and enters the Path of the destination entity placed on each map
        if self._T.destinationTable == nil then
            self._T.destinationTable = {}
            self._T.destinationTable[1] = "/maps/map01/object-19_1"
            self._T.destinationTable[2] = "/maps/map02/object-6_1"
            self._T.destinationTable[3] = "/maps/map03/object-42_1"
        end
         
        self._T.accTime = self._T.accTime + delta
         
        --Moves the character to a different map every 3 seconds
        if self._T.accTime >= 3 then
            self._T.accTime = 0
            self._T.teleportCnt = self._T.teleportCnt + 1
            local destinationNum = self._T.teleportCnt % 3 + 1
         
        --Uses the TeleportToEntityPath function of TeleportService
        --The path of the entity to be moved and that of the destination entity are passed as parameters
            _TeleportService:TeleportToEntityPath(self.Entity, self._T.destinationTable[destinationNum])
        end
    }
    ```
    <br>
3. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and test.
    Check if the character is transported to a different map every 3 seconds.
![teleport](https://mod-file.dn.nexoncdn.co.kr/bbs/16566569336433dc028f996c541839f32ad85efbd8755.gif "teleport")