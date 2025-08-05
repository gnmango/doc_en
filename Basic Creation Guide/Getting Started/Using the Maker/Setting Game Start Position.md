# Course Introduction 
Here's how to set the starting position of the **Player Avatar**.
Let's examine setting the game's starting position, respawn, and test location during production.
# Player Avatar Default Position
The default position for the player avatar is the center of the screen.
In X and Y coordinates, both values are 0. Basically, the game starting position and respawn position follow the same settings. 
You can check the basic position at ![workspace_MyAvatar](https://mod-file.dn.nexoncdn.co.kr/bbs/16346008379922a9b756c3912461db7195808cb554abd.png "workspace_MyAvatar")<span style="color: #dc9656"> **DefaultPlayer's TransformComponent**</span>.
![transformcomponent](https://mod-file.dn.nexoncdn.co.kr/bbs/163515895824953612927a52740079e54f5fff52a4db3.png "transformcomponent")
# Setting Starting Position with SpawnLocation
If you've placed SpawnLocation in a starting map, this is where the player avatar first appears. If you've placed SpawnLocation in another map, it can be used where the avatar respawns or where it appears upon map entrance. 
1. Click <span style="color: #dc9656">**Special Model**</span> from the Preset List.
2. Place <span style="color: #dc9656">**SpawnLocation model**</span> in ![TabScene](https://mod-file.dn.nexoncdn.co.kr/bbs/163452458863504f49c7a23aa4a41af56b5b4611a6daf.png "TabScene")Scene.<br>![spawn80](https://mod-file.dn.nexoncdn.co.kr/bbs/1656036546711155b8385c0fb4001bda8ec16387d412e.png "spawn80")
# Respawn Location during Play
You can place multiple SpawnLocations in each map as necessary. If there are several SpawnLocations in one map,<span style="color: #dc9656"> **the distance between SpawnLocation and player avatar** </span>will be used to determine the respawn position.
In the example below, the player avatar died between the two spawn locations. Even though the avatar had been moving to the left, it will reappear at the SpawnLocation on the right, as it's closer.
![spawn01](https://mod-file.dn.nexoncdn.co.kr/bbs/1634957591109e98512448cae4044ae9a75e96b22221a.png "spawn01")
Place SpawnLocation appropriately to make a few checkpoints in the game. It will encourage users to replay, as they won't have to start the game over from the beginning.
# Setting Starting Position for Test
You might want to check a certain part of the game during production several times.
In this case, it would be much more efficient to start from a desired position, rather than starting from the beginning.
You can use ![Common_PlayHere](https://mod-file.dn.nexoncdn.co.kr/bbs/1634538307569eec2919eb0d84163aa7be63651bd165d.png "Common_PlayHere") <span style="color: #dc9656">**Play Here**</span> to set the starting position for the test.

1. ![TabScene](https://mod-file.dn.nexoncdn.co.kr/bbs/163452458863504f49c7a23aa4a41af56b5b4611a6daf.png "TabScene")Under the Scene tab, ![Common_PlayHere](https://mod-file.dn.nexoncdn.co.kr/bbs/1634538307569eec2919eb0d84163aa7be63651bd165d.png "Common_PlayHere")click on the <span style="color: #dc9656"> **Start Here** button</span>. <br>![spawn8](https://mod-file.dn.nexoncdn.co.kr/bbs/1740551206421b7111dfb4e224c79b380713b13c0bdcf.png{"width":"830px"} "spawn8")
2. Place it by moving to the position where the test will start (above a tile or platform).<br>![spawn9](https://mod-file.dn.nexoncdn.co.kr/bbs/1740551251453a53f6916f5d6499e86c8c453f7304fdc.png{"width":"830px"} "spawn9")
3. Press Start ![Tool_Play](https://mod-file.dn.nexoncdn.co.kr/bbs/163453086660754178e0ff96a45c58d1a580a4dfab9d1.png "Tool_Play") to test.
4. The player avatar will appear in the set location.

> <span style="color: #7cafc2">**Tip**</span>
> <span style="color: #7cafc2">For the test, "Play Here" takes priority over SpawnLocation.</span>

> <span style="color: #7cafc2">**Tip**</span>
> <span style="color: #7cafc2">If you set the starting point without any foothold or tile, the character will appear in the middle of the screen.</span>
> <span style="color: #7cafc2">Place the character above a foothold area if you want it to appear in a particular spot.</span>
##### Reference Guide
[Editing Initial Character Properties]
