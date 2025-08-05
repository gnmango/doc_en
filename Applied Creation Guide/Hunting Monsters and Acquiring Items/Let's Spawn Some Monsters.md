# Course Introduction
Let's design processes to spawn a monster, have the monster drop an item when defeated, and save the item in the inventory.
> <span style="color: #585858">This guide follows the order below.
> **▶ Part 1.** [**Let's Spawn Some Monsters**](/docs?postId=204{"target":"_self"})
> Part 2. [Creating Monster Drop Items and Inventory UI](/docs?postId=943{"target":"_self"})
> Part 3. [Writing an Inventory Script](/docs?postId=942{"target":"_self"})
> Part 4. [Writing an Inventory UI Script](/docs?postId=946{"target":"_self"})
> Part 5. [Writing an Item Script](/docs?postId=947{"target":"_self"})</span>

We'll start by outlining what this game will look like at the end of the course.
![game](https://mod-file.dn.nexoncdn.co.kr/bbs/1672128051320c8efeb0f27e4436bb7cc30156f965407.gif "game")

First, let's make a simple monster spawning system.
In the fields of MapleStory, no matter how many monsters you hunt, monsters continue to spawn up to their limit. You can set the type and number of monsters that spawn by each field unit because there is a spawn system that creates the monsters.
Let's spawn a monster using the example below.
<br>

# World Template
For ease of creation, we recommend creating a world using the "Default Template".
![0](https://mod-file.dn.nexoncdn.co.kr/bbs/1656419849546d61c1ab96b5d454a971009e8053fe3c1.png "0")

# Spawn Rules
The spawn rules we're going to use in this course are as follows:

* Number of spawns: Up to 10 for the map
* Respawn: After checking the number of monsters on the map every 5 seconds, respawns the number of missing monsters up to the maximum
* Spawn location: A random location within a defined area
* Monsters to spawn: Use the monsters you want from the Preset List

# Add Monsters to Spawn
Add the monster to spawn to the **Workspace** as follows.

1. In the **Preset List**, right-click the model you want to spawn and select **Add to Workspace**.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16600442197122526676a43b54c8b87db279545ec6cf1.png "3")
    <br>
2. Confirm that it has been added to the **Workspace**.
    ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1636441681835eec75b87dc70415c86afa51d3f5dca8f.png "4")

# Spawn Monsters
Now, let's create a monster spawn function with scripts.
First, we'll spawn monsters at a defined location and then gradually extend the function.

1. Generate the new script component <span style="color: #dc9656">**MonsterSpawn**</span>.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/16354075070807ead5f6f39d04bcd83c84cf04fcbb1b5.png "8")
    <br>
2. Open the **MonsterSpawn** script and add the `SpawnMonster()` function.
    <br>
3. Write the `SpawnMonster()` function as follows.
    ```lua
    void SpawnMonster()
    {
        local parent = _EntityService:GetEntityByPath("/maps/map01")
        _SpawnService:SpawnByModelId(
            "Entry ID", 
            "SpawnedMonster",
            Vector3(0,0,0),
            parent)
    }
    ```
    <br>
4. Right-click the monster model added to the **Workspace** above and press **Copy Entry ID** to copy the **Entry ID**. 
    ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/16708311988051c41e3516d2e4bde8d9ded17c148fb45.png "10")
    <br>
    Then, paste the **copied Entry ID** to **"Entry ID"** of the `SpawnMonster()` function. Even if you delete model://, it works normally.
    <br>
	> <span style="color: #585858">**Learn more**
	> `SpawnByModelId` is one of the spawn-related functions provided by `_SpawnService`.
	> To use `_SpawnService:SpawnByModelId`, the model that you'll create as an entity must be added to the **Workspace**, and the added **Entry ID** must be passed to the parameter.
	> In the context menu of the selected Model, press **Copy Entry ID** to copy **Entry ID** to the clipboard.
	> Please refer to the [Creating, Deleting, and Validating Entity Effectiveness](https://mod-developers.nexon.com/docs?postId=290{"target":"_self"}) for the details of the function.</span>
 <br>
5. To check **SpawnMonster**'s motion, add the `OnBeginPlay()` function and call the `SpawnMonster()` function as follows.
    ```lua
    [server only]
    void OnBeginPlay() 
    {
        self:SpawnMonster()
    }
    ```
    <br>
 6. In the Property Editor of **Hierarchy - common**, add the **MonsterSpawn** component.
    ![12](https://mod-file.dn.nexoncdn.co.kr/bbs/16595093911235aa8eff487344aeab5d2f65f28cfa2e9.png "12")
    <br>
7. Press **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play")  to test monster spawning.

# Setting Spawn Area
Let's place tiles on the map and then set up the spawn area.

1. From **Preset List**, select a tile you like and place it as follows.
    ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16600444158945ddd4856b7ca481e83e302d49635ef4f.png "5")
    <br>
2. Now, let's consider the monster's spawn area.
    Like the image below, from both ends of the bottom tile, press the Tab key and hover over it to check the coordinates.
    Record the x and y coordinates because the monsters will be spawned between the two endpoints.
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/165579543869079a43790709245ee92fd3c6b10bc2b13.png "7")

# Spawn at Random Locations in Specified Area
Let's add a function to spawn monsters at random locations within the designated area.
We will define the spawn area based on the coordinates we checked above, and spawn monsters in random locations within the area.

1. Open **MonsterSpawn** in the Script Editor and add a new function `GetRandomPosition()` of **Vector3** type.
    <br>
2. Write the `GetRandomPosition()` function as follows. Use the coordinates you recorded in the function above.
    ```lua
    Vector3 GetRandomPosition()
    {
        local leftX = -3.571 -- x coordinate on the left side of the spawn area
        local rightX = 2.688 -- x coordinate on the right side of the spawn area
        local y = 0.170 -- y coordinate of the spawn area
        local areaWidth = rightX - leftX
    
        local randomX = (_UtilLogic:RandomIntegerRange(0,100) / 100) * areaWidth + leftX
    
        return Vector3(randomX, y, 0)
    }
    ```
    <br>
3. Modify the `SpawnMonster()` function as follows.
    ```lua
    void SpawnMonster()
    {
        local parent = _EntityService:GetEntityByPath("/maps/map01")
        _SpawnService:SpawnByModelId(
            "Entry ID",
            "SpawnedMonster",
            self:GetRandomPosition(),
            parent)
    }
    ```
    <br>
4. Click **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play")  to check if monsters are spawned in a random location within the spawn area.

# Monster Spawn Cycle and Maximum Limit
This time, we will create a function to spawn an additional monster every 5 seconds, up to a maximum of 10 monsters.

1. Open **MonsterSpawn** and add the property.
    ```lua
    Property:
    [Sync]
    number MaxSpawnCount = 10
    [Sync] 
    number Time = 0
    [None]
    table MonsterArray = {}
    ```
    <br>
2. Modify the `OnBeginPlay()` function as follows.
    ```lua
    [server only]
    void OnBeginPlay() 
    {
        self.MonsterArray = {}
    }
    ```
    <br>
3. Modify the `SpawnMonster()` function as follows.
    ```lua
    void SpawnMonster()
    {
        local parent = _EntityService:GetEntityByPath("/maps/map01")
        local nextNum = #self.MonsterArray + 1
        
        self.MonsterArray[nextNum] = _SpawnService:SpawnByModelId(
            "Entry ID",
            "SpawnedMonster",
            self:GetRandomPosition(),
            parent)
    }
    ```
    <br>
4. Add the new `GetCurMonsterCount()` function as a **number** type and write as follows.
    ```lua
    number GetCurMonsterCount()
    {
        local i = 1
        while i <= #self.MonsterArray do
            if isvalid(self.MonsterArray[i]) == true then
                i = i + 1
            else
                table.remove(self.MonsterArray,i)
            end
        end
        
        return #self.MonsterArray
    }
    ```
    <br>
5. Add the `OnUpdate()` function and write as follows.
    ```lua
    [server only]
    void OnUpdate(number delta) 
    {
        self.Time = self.Time + delta
        
        if self.Time >= 5 then
            self.Time = 0
            local curMonsterCnt = self:GetCurMonsterCount()
            if curMonsterCnt == nil then
                return
            end
            if curMonsterCnt < self.MaxSpawnCount then
                self:SpawnMonster()
            end
        end
    }
    ```
    <br>
6. In the Property Editor of **common** entity, check if the **MaxSpawnCount** value of **MonsterSpawn** component is properly entered.
    If the value is set to 0, change it to 10.
    ![19](https://mod-file.dn.nexoncdn.co.kr/bbs/16354076592684e9f1701f1b349dc80c5df15763778a8.png "19")
    <br>
7. Press **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play")  to check if up to 10 monsters are spawned in different locations every 5 seconds.

# Spawning Replacements on Schedule
Now, let's modify it to check the current monster quantity every 5 seconds and then spawn the missing monsters, instead of spawning one by one every 5 seconds.

1. Add one parameter to the `GetRandomPosition()` function of **MonsterSpawn**.
    ![21](https://mod-file.dn.nexoncdn.co.kr/bbs/1655797605759f3d7410839674ad9b99040ab832c18fa.png "21")
    <br>
2. Set the parameter type to **number** and name to **spawnCount**, and change the return type to **table**.
    Modify the function as follows.
    ```lua
    table GetRandomPosition(number spawnCount)
    {
        local leftX = -3.571 -- x coordinate on the left side of the spawn area
        local rightX = 2.688 -- x coordinate on the right side of the spawn area
        local y = 0.170 -- y coordinate of the spawn area
        local areaWidth = rightX - leftX
        local positions = {}
    
        for i = 1, spawnCount do
            local randomX = (_UtilLogic:RandomIntegerRange(0,100) / 100) * areaWidth + leftX
            positions[i] = Vector3(randomX, y, 0)
        end
    
        return positions
    }
    ```
    <br>
3. Add a parameter to the `SpawnMonser()` function and set its type to **number** and its name to **spawnCount**. Modify the function as follows.
    ```lua
    void SpawnMonster(number spawnCount)
    {
        local parent = _EntityService:GetEntityByPath("/maps/map01")
        local nextNum = 0
        local positions = self:GetRandomPosition(spawnCount)
        
        for i = 1, #positions do
        	nextNum = #self.MonsterArray + 1
        	self.MonsterArray[nextNum] = _SpawnService:SpawnByModelId(
        	    "Entry ID",
        	    "SpawnedMonster",
        	    positions[i],
        	    parent)
        end
    }
    ```
    <br>
4. Finally, modify the content of the `OnUpdate()` function as follows.
    ```lua
    [server only]
    void OnUpdate(number delta) 
    {
        self.Time = self.Time + delta
        
        if self.Time >= 5 then
            self.Time = 0
            local curMonsterCnt = self:GetCurMonsterCount()
            if curMonsterCnt == nil then
                return
            end
            local spawnCount = self.MaxSpawnCount - curMonsterCnt
            if spawnCount > 0 then
                self:SpawnMonster(spawnCount)
            end
        end
    }
    ```
    <br>
5. Press **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play")  to check if the monsters are spawned up to the maximum quantity on schedule when the number of monsters decreases.