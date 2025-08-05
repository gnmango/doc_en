# Course Introduction
Have you heard of the Forest of Patience?
If not, you'll find many a harrowing tale if you search for it.
$$youtube
HXMM4Vbx8aA
$$
<br>
The Forest of Patience is a challenge where the player must jump from one foothold to another to move up toward the end.
<br>
But maybe you have different ideas about what would make the Forest of Patience more fun.
* The current Forest of Patience is way too easy.
* It may be more fun this way.
* Wouldn't it be better with rankings?
* I wish we could attack each other.

In MapleStory Worlds, you can make these ideas come true. If you are a fan of MapleStory, then perhaps you could expand a character's story through the Forest of Patience.
As shown below, the example uses a darker background, and includes the Black Mage. The change was simple, but the atmosphere changed a lot.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1636093094351729ce90b8f3e4fcfbf55585e7225343a.png "1")
<br>
In this guide, we will create the Forest of Patience and try a few things to create your own Forest of Patience.

# Decorating the Background
First, let's change the background.
1. Click **back-56** as the background of the Forest of Patience in **Preset List > Background**.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1660035281651d4afe23885ac457a8587cfb3aa0a9a7b.png "6")
<br>
2. You'll see the background has changed in **Scene**.
![7](https://mod-file.dn.nexoncdn.co.kr/bbs/1660035424150f41f0b35ee5242e4a52737c09038c024.png "7")
<br>You don't have to select **back-56**.<br>Choose whichever background you like!
<br>
# Placing Tiles
Now let's place tiles the players can walk around on.

1. **In Preset List - Tile**, select **tile-132**.
![10](https://mod-file.dn.nexoncdn.co.kr/bbs/1660035591312fa0c8c01d63a465ba40646a7e3479e92.png "10")
<br>
2. With **tile-132** selected, click the position you want to place the tile.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1660035656369af8021fb31874c4aaf32c355bfdaf187.png "11")
<br><br>Now, click and drag to place the tile.
![12](https://mod-file.dn.nexoncdn.co.kr/bbs/166003572116378a2cad79b154b478c81502158883c13.png "12")
<br>
3. Click the **[Box Fill]** button.
![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1683014163114615fa0accc994885b084f2da23d4a670.png "13")
<br>When you drag with Box Fill selected, the area will be selected as shown below. Let go of the mouse to place tiles in the selected area.<br>![14](https://mod-file.dn.nexoncdn.co.kr/bbs/1636092514103e87dce09603c476b969ed783676bc8ce.png "14")
<br>If you unclick Box Fill, tiles will be placed one by one, not in the selected area.
<br>
Now let's place the tiles as follows:<br>![16](https://mod-file.dn.nexoncdn.co.kr/bbs/16360925318318dbe599a114a4c829d0398538e036c5f.png "16")
<br>
4. Press the ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") **[Start]** button and test.<br>![17](https://mod-file.dn.nexoncdn.co.kr/bbs/163609254063076f491e458ed4541abb9c5133856e854.png "17")
<br>Every time you play, the contents will be auto-saved.<br>If you're saving for the first time, you'll see a pop-up that asks you name the world. Enter the name you want and click the [Confirm] button as shown below.<br>![18](https://mod-file.dn.nexoncdn.co.kr/bbs/1656934626750bb4f9e9f2a2c41ff948d1dc21e93cb28.png "18")
<br>Make sure that the tiles are placed correctly and that the game can be played as expected.<br>![19](https://mod-file.dn.nexoncdn.co.kr/bbs/1636092557762f09ec2e7140b4919af93dfe7b8635623.png "19")
<br>
> <span style="color: #585858">Learn more
> In MapleStory Worlds, you can directly **import maps from the MapleStory**.
> To do so, import maps via **Window - MapleStory Map**.</span>
> ![20](https://mod-file.dn.nexoncdn.co.kr/bbs/1656934655573a1a587f24db84fdb8ad9457b2f291cdc.png "20")

# Placing Entities
Let's place some Objects, Footholds, and NPCs on the tiles to decorate the map.

#### Placing Foothold
In **Preset List - Foothold**, select and place **foothold-1972**.
![22](https://mod-file.dn.nexoncdn.co.kr/bbs/166003635510122996c4d8e264df4ab29e67ab6d7c51b.png "22")
<br>
> <span style="color: #585858">**Learn more**
> Unlike general object entities, foothold entities have **CustomFootholdComponent** applied so characters can step on them. Add **CustomFootholdComponent** to general object entities as well and set the foothold, so you can change them to foothold entities.
> ![23](https://mod-file.dn.nexoncdn.co.kr/bbs/1636092598266fcc894e2b1c74141a3af370d3e4f138f.png "23")
> <br>
> For more information, please refer to the guide below. </span>
> [Making Footholds](https://mod-developers.nexon.com/docs?postId=71{"target":"_self"})

# Placing NPCs
1. In **Preset List - NPC**, select **npc-2412**.
    ![25](https://mod-file.dn.nexoncdn.co.kr/bbs/166003643766456517c0e13814359bb8310c76b4bdd1d.png "25")
    <br>
    > <span style="color: #7cafc2">**Tip**
    > If you can't find the NPC, use the Preset List's search box.
    > You can easily and quickly find the NPC name (npc-2412) by searching it.</span>
2. Place the NPC at the desired location.
    ![27](https://mod-file.dn.nexoncdn.co.kr/bbs/1636092640320394a6606f48848b89fdbc4e5c73fc9bf.png "27")
    <br>
3. Select the NPC you placed.
    ![28](https://mod-file.dn.nexoncdn.co.kr/bbs/163609265030356d046261b0a45baac597994993ce089.png "28")
    <br>
4. In the Property Editor of **npc-2412**, enter the following for **NameTagComponent - Name**:<br>
    ![30](https://mod-file.dn.nexoncdn.co.kr/bbs/16569347019776e807a60ea764ae38ca51cfd78528cb1.png "30")
    <br>
5. Check if the Name value you entered is shown on the name tag below the NPC.<br>![31](https://mod-file.dn.nexoncdn.co.kr/bbs/16569350989865bbe5095de9641b186c37eba3d9eacc3.png "31")

#### Placing Traps
Now let's place some traps to make our challenge more difficult.
1. In **Preset List - Trap**, select the Monkey Trap. This trap throws bananas to attack characters.
![33](https://mod-file.dn.nexoncdn.co.kr/bbs/1660036660663df86ba9f25b3408e8acdc844b7156499.png "33")
<br>Place additional tiles and Monkey Traps all over the map.<br>
![34](https://mod-file.dn.nexoncdn.co.kr/bbs/16360927069699f7be46079844c90bb4222bd8f12326d.png "34")
<br>
2. The Monkey Trap is designed to throw bananas left, so your only choice normally is to place the trap to right of the character. However, we can modify its properties so the bananas are thrown in the other direction.<br>Place the Monkey Trap on the left as shown below.<br>
![35](https://mod-file.dn.nexoncdn.co.kr/bbs/16360927154443da0f74814694fbe8430d837cccbaf50.png "35")
<br>
Select the Monkey Trap and click and tick the checkbox in **Property Editor - SpriteRendererComponent - FlipX**, so it appears ![Editbox_Check](https://mod-file.dn.nexoncdn.co.kr/bbs/16346176407708cb3de01eaaf48a68ab2dd6fe1b1183f.png "Editbox_Check") as checked.<br>
![36](https://mod-file.dn.nexoncdn.co.kr/bbs/1636092723032f4b70f1a759b4d81b958170c2e19ebe6.png "36")
<br>
In the Scene, you'll see that the Monkey Trap has be reoriented.
![37](https://mod-file.dn.nexoncdn.co.kr/bbs/163609273198592ca59a84fd14d34a230b9809ff33904.png "37")
<br>
> <span style="color: #585858">**Learn more** 
> To add the effect of being attacked upon the character touching the banana the monkey throws, you can refer to the following guide to implement the effect:</span>
> [Attack and Hit](https://mod-developers.nexon.com/docs?postId=206{"target":"_self"})

# Setting Game Start Position
Let's set the character's starting position.
Currently, characters spawn in the center of the map when you hit play. Let's set the start position so that the characters are spawned in the bottom-left position.
![38](https://mod-file.dn.nexoncdn.co.kr/bbs/1636092751698f23e24cabeff4901b01875e8bab3ee4f.png "38")
<br>
1. Select **Preset List - Special**.
![39](https://mod-file.dn.nexoncdn.co.kr/bbs/1660037369729769a1e38f0b1472ebab082fd8f63d2c1.png "39")
<br>
2. Click **SpawnLocation** and place it to the left of the bottom tile, as shown below.
![40](https://mod-file.dn.nexoncdn.co.kr/bbs/1636092769152e641a8b479b7486f8cfd2e5cf503b3d4.png "40")
<br>
3. ![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") Press the **[Start]** button and run a test.<br>Your characters should spawn at the **SpawnLocation** when you start the game.
![41](https://mod-file.dn.nexoncdn.co.kr/bbs/1636092778305c9d93c08fdfc40f39d9f519d96b418f8.png "41")
<br>

# Editing Character Properties
Increasing you character's jump force caused them to jump higher. Likewise, increasing movement speed causes your character jump further.
Let's modify the character's movement stats to adjust the difficulty of our game.
<br>
1. **Select Workspace - DefaultPlayer**.
![42](https://mod-file.dn.nexoncdn.co.kr/bbs/1636092791473d2f4ef1b25264e33aa03736e143f829d.png "42")
<br>
2. **In the Property Editor - MovementComponent**, check **InputSpeed, JumpForce**.
![43](https://mod-file.dn.nexoncdn.co.kr/bbs/1636092799102b630e690bb184702bbc1fd2028b8360e.png "43")
<br>**InputSpeed** property affects characters' movement speed, and **JumpForce** property affects characters' jump force.<br>Change the value for each property as follows:
![44](https://mod-file.dn.nexoncdn.co.kr/bbs/16360928082385428e930a6e649779bdbd54fd5c28aaf.png "44")
<br>
![start](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tool/icon_play.png "start") Press the **[Start]** button and test.<br>Check if the movement speed and jump force have changed.<br> Ensure that your game is completable with the new property values.

# Release
You can release the world you made so that other players can play, too.
1. Select **File - Release**.
![45](https://mod-file.dn.nexoncdn.co.kr/bbs/16838873916111a64161531c14b74a2b5fdb0bd0a08ba.png "45")
<br>
2. In the **Release** window, you can choose settings for your world and set a thumbnail image.<br>First, set the number of players, whether the world is public or just for you, and which platforms can access it. Then, write a helpful description of your world and set a thumbnail image which represents your world. Finally, give your world a check the title before clicking the **[Release]** button.
![46](https://mod-file.dn.nexoncdn.co.kr/bbs/16838881039953b88c88a42564cc9ab49e5f7f05ce48c.png "46")
<br>
3. You can the worlds you have released under <span style="color: #dc9656">**Play - My Worlds**</span>.
![47](https://mod-file.dn.nexoncdn.co.kr/bbs/168388871827091d6161436ac4e0d87391b9de1333035.png "47")
