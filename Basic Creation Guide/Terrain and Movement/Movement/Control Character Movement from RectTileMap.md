# Course Introduction

You control the character's movement with KinematicbodyComponent in RectTileMap. A creator can make the character move and jump freely on the 2D plane through KinematicbodyComponent. The creator can also draw shadows.
Today let's learn about KinematicbodyComponent and look at use methods with simple examples.

##### API Reference

[KinematicbodyComponent](/apiReference/Components/KinematicbodyComponent{"target":"_self"})

> <span style="color: #585858">**Learn More**</span>
> Please refer to [Utilizing RectTileMap](docs?postId=589{"target":"_self"}) for details about RectTileMap.

# KinematicbodyComponent Introduction
Let's take a look at the properties of **KinematicbodyComponent** in the **Workspace - DefaultPlayer - Property** window.

| Property | Description |
| :---: | --- |
| Acceleration | This feature is deprecated. Please use SpeedFactor. |
| ApplyClimbableRotation| If it is True, the character on the rotating or slanted ladder will be affected by the shape of the ladder. If it is False, the character is not affected by the slant or rotation of the ladder. |
| EnableJump | If it is True, it uses the jump feature. |
| EnableShadow | If it is True, it uses shadow. |
| ShadowColor | The shadow color. |
| ShadowOffset | The shadow's position. |
| ShadowSize | The shadow's size. |
| ShadowScalingRatio | The rate of change of the shadow's size. The size varies depending on how high the Entity jumps. |
| EnableTileCollision | If it is True, it will collide with a square tile map. |
| JumpDrag | Controls the rate of the jump speed. The greater the value, the faster the fall. |
| JumpSpeed | It controls the speed of bouncing up when jumping. The greater the value, the higher the bounce. |
| SpeedFactor | When moving, this value is multiplied to the speed traveled in the X or Y direction. The greater the value, the faster the movement. |
| Enable | If it is True, it enables KinematicbodyComponent. |

# KinematicbodyComponent Use

## Create Speed Increase Tile

Let's create a tile to increase speed when stepping on it.
<br>
1. Set the tile map mode as RectTile.
    ![recttile](https://mod-file.dn.nexoncdn.co.kr/bbs/16582959899857d48535b46064d39a8b24d45268f5e02.png)
   <br> 
2. Press the **[+]** button on the upper left of the Tile Set Palette, then select **Create TileSet From Template - BnB** Tile Set.
    ![tileset](https://mod-file.dn.nexoncdn.co.kr/bbs/169034747395370bf20b37bcd4b86900f6f1b9c52284f.png "tileset")
    <br>
3. Set the name of the Tile Set as <span style="color: #dc9656">**BnB TileSet**</span>.
    ![BnB](https://mod-file.dn.nexoncdn.co.kr/bbs/1690347500526ff0abc6e49f34688b95fbe284a378453.png "BnB")
    <br>
4. Click the Edit button on the upper right of the Tile Set Palette to change to Tile Property Edit mode.
    ![tileedit](https://mod-file.dn.nexoncdn.co.kr/bbs/1658300856970ff2537ee3b9f46a7a74b1756bf4d48cb.png)
    <br>
5. Check if it is <span style="color: #dc9656">**movable**</span> from the editing target, then select a tile without making the character move. The green arrow icon refers to a movable tile, and the red arrow icon refers to an immovable tile.
    ![dontmove](https://mod-file.dn.nexoncdn.co.kr/bbs/1658368512967a68f8090145147d99c7dccea02d22577.gif)
    It removes the edition mode after editing.
    <br>
6. Select Layer1 from **Map Layer** and then create a map.
    ![map1](https://mod-file.dn.nexoncdn.co.kr/bbs/16582986986052598c60d8a5c4f97bd12a101ee3feaba.png%7B%22width%22:%22750px%22%7D)
    <br>
7. Create Layer2 by pressing the ![addlayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_swatches_add.png) button in **Map Layer**, then decorate the map by placing houses and trees.
    ![map2](https://mod-file.dn.nexoncdn.co.kr/bbs/16582993425778cfc274e817a4e18876539fddcf14ec2.png%7B%22width%22:%22750px%22%7D)
    <br>
8. Press the ![addlayer](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_swatches_add.png) button in **Map Layer** and add Layer3.
    ![layer](https://mod-file.dn.nexoncdn.co.kr/bbs/16589727962500e6f27c08a514189afcd6c49e7ad6047.png "layer")
    <br>
9. . Press the **[+]** button on the upper left of the Tile Set Palette, then select **Create Empty TileSet**. Add a new Tile Set with the name <span style="color: #dc9656">**Buff TileSet**</span>.
    <br>
10. Press the ![add](https://mod-file.dn.nexoncdn.co.kr/storage/icons/UI/icon_addtile_rect.png) button on the bottom left of the Tile Set Palette, then select **Add ResourceStorage Image**.
    ![addtile](https://mod-file.dn.nexoncdn.co.kr/bbs/1690349260069399d7e9a5a21497d9c52ec63ab26c4c2.png)
    <br>
11. Click the Sprite that has been searched with the RUID below from **Resource Picker - MSW Resource** to add it to the Tile Set.
    <span style="color: #dc9656">**Buff**</span>: 02652c247d9d40518702255bed1376c8
    <span style="color: #dc9656">**Trap**</span>: 510cf1b467634a3db7624041432c3de5
    ![buff](https://mod-file.dn.nexoncdn.co.kr/bbs/16810900445520aec35be4e2f446ba329e093a22f61ba.png{"width":"500px"} "buff") ![trap](https://mod-file.dn.nexoncdn.co.kr/bbs/168109006571334c6e0622b7347388c4fc77829c32f55.png{"width":"500px"} "trap")
    <br>
12. Switch to Tile Property Editing mode, then double-click the names to set them as <span style="color: #dc9656">**Buff and Trap**</span> for each.
    ![rename](https://mod-file.dn.nexoncdn.co.kr/bbs/1658369458607f8a560fe2565473b85ae6823a370224f.gif)
    It removes the edition mode after editing.
    <br>
13. Place the Buff tiles on the map while selecting Layer3 from the **Map Layer**.
    <br>
14. Under **MyDesk**, create a new script component <span style="color: #dc9656">**CheckSpeedBuffTile**</span> and add it to **DefaultPlayer**.
    <br>
15. Write **CheckSpeedBuffTile** as below.
    ```lua
    Method:
    [server]
    void IncreaseSpeedFactor()
    {
        local kb = self.Entity.KinematicbodyComponent
        local oldSpeedFactor = kb.SpeedFactor
        local newSpeedFactor = Vector2(oldSpeedFactor.x + 1, oldSpeedFactor.y + 1)
         
        kb.SpeedFactor = newSpeedFactor
    }
     
    Event Handler :
    [client only]
    HandleRectTileEnterEvent(RectTileEnterEvent event)
    {
        -- Parameters
        local Kinematicbody = event.Kinematicbody
        local TileInfo = event.TileInfo
        local TileMap = event.TileMap
        local TilePosition = event.TilePosition
        --------------------------------------------------------  
     
        -- When the Buff tile is stepped on, SpeedFactor is increased.
        if TileInfo.Name == "Buff" and Kinematicbody:IsOnGround() then
            self:IncreaseSpeedFactor()
        end
    }
    ```
       <br>
16. ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png) Let's watch the speed increase when you press the **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") button and step on the Buff tile.
    ![speedup](https://mod-file.dn.nexoncdn.co.kr/bbs/165830449805569dd931c42f04da08e73ac13afcbf882.gif%7B%22width%22:%22700px%22%7D)

## Avoid Obstacle Tile

Let's create obstacles that a character must jump over and set the character to die if they step on the obstacle.

1. Place Trap tiles on the map.
    <br>
2. Under **MyDesk**, create a new script component <span style="color: #dc9656">**CheckTrapTile**</span> and add it to **DefaultPlayer**.
    <br>
3. Write the **CheckTrapTile** as below.

    ```lua
    Property:
    [None]
    string CurrentTileName = ""
     
    Method:
    [client only]
    void OnUpdate(number delta)
    {
        local kb = self.Entity.KinematicbodyComponent
     
        -- It collides with tiles only when on the ground.
        kb.EnableTileCollision = kb:IsOnGround()
     
        -- The character dies when stepping on the Trap tile. Jump carefully!
        if self.CurrentTileName == "Trap" and kb:IsOnGround() then
            self.Entity.PlayerComponent:ProcessDead()
        end
    }
     
    Event Handler:
    [client only]
    HandleRectTileEnterEvent(RectTileEnterEvent event)
    {
        -- Parameters
        local Kinematicbody = event.Kinematicbody
        local TileInfo = event.TileInfo
        local TileMap = event.TileMap
        local TilePosition = event.TilePosition
        --------------------------------------------------------
     
        self.CurrentTileName = TileInfo.Name
    }
    ```
    <br>
4. ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png) Let's see if the character can jump over the Trap by pressing the **[Play]** ![play](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "play") button and die when stepping on the Trap by mistake.
    ![trap](https://mod-file.dn.nexoncdn.co.kr/bbs/16583053608016b9cb5709e7749bcb2a121412c7d3c2f.gif%7B%22width%22:%22700px%22%7D)