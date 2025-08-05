# Course Introduction
Let's take a look at the configuration of avatars, and how to replace their equipment. You'll learn to understand the configuration of avatars to utilize them in brand new ways.
# Avatar
The Property Editor of **DefaultPlayer** includes **AvatarRendererComponent** and **CostumeManagerComponent**.
![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1639736344111420c42a85e1c4f678dbdd11dabf7ee64.png "8")
* **AvatarRendererComponent**
    * It takes the costume information and draws the avatar (rendering).
* **CostumeManagerComponent**
    * It has costume information.
    * If you do not specify a value for the property, the information of each user's equipped clothes is retrieved and displayed. 
    * If you want to fight with all the same weapons on the map created by the creator, specify the costume in **CostumeManagerComponent**. When the World is executed, **AvatarRendererComponent** gets the specified costume information and applies the same weapon to all users connecting to the world.

An **avatar** is divided into **Body** and **Face**, located below **AvatarRoot**. **Body** will automatically implement **AvatarBodyActionSelectorComponent** and **Face** will automatically implement **AvatarFaceActionSelectorComponent**. The two **Selector** components deliver which animation to play to **AvatarRendererComponent**.
![avatarroot1](https://mod-file.dn.nexoncdn.co.kr/bbs/163972976884316a24d6b6a37457aaa19b6fa1e57fdd7.png "avatarroot1")
#### Avatar Equipment Rules
You can change the property values of **CostumeManagerComponent** to equip avatars with a certain item.
* You can change the body, skin, hat, cape, coat, ear accessory, ears, eye accessory, face accessory, face, gloves, hair, long coat, 1H weapon, pants, shoes, sub-weapon, and 2H weapon. 
* However, the following rules apply to **equipment**, so some items cannot be equipped together.
    * Coat/pants and outfits cannot be equipped together.
    * A 1H weapon or sub-weapon cannot be equipped together with a 2H weapon.
    * If you enter items that cannot overlap, only long coats and 2H weapons will be output.

If the creator deactivated the **UseCustomeEquipOnly** property ![Editbox_Visible](https://mod-file.dn.nexoncdn.co.kr/bbs/16346176985962dcc6fd403f34978b50521f0a8329013.png "Editbox_Visible"), all users will equip changed costume from their avatars. To make all users enter with the same avatar, you have to activate **UseCustomeEquipOnly** ![Editbox_Check](https://mod-file.dn.nexoncdn.co.kr/bbs/16346176407708cb3de01eaaf48a68ab2dd6fe1b1183f.png "Editbox_Check"). If you put value only for long coat and differentiate the state of **UseCustomeEquipOnly**, it will be applied as shown below.
| UseCustomeEquipOnly Deactivated | UseCustomeEquipOnly Activated |
| :---: | :---: |
| ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/1656037430966b3ed208e362047fd96fc353a923d5694.png "9") | ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/165603717861326007c18880e4f3ab95ec987302d4b82.png "10") |
# Coloring Avatars
Use **AvatarRendererComponent**'s `SetColor` and **TriggerComponent** functions to make certain entities  turn red upon collision.

1. Select **BaseEnvironment - NativeScripts -	Component - TriggerComponent**.
<br>
2. In the context menu of **TriggerComponent**, click **Extend**.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1687754755702bcecc1f59a204f3ca931773a1849464d.png "11")
<br>
3. Change the name of extended script, and add a component to the entity that will collide with the avatar.
<br>
4. Compose the script as follows.
```lua
Function:
override void OnEnterTriggerBody(TriggerEnterEvent enterEvent)
{
    __base:OnEnterTriggerBody(enterEvent)
     
    -- Gets target (player) that collided with the object.
    local player = enterEvent.TriggerBodyEntity
     
    -- If the target that collided with the object has no AvatarRendererComponent, color will not change.
    local avatarRenderer = player.AvatarRendererComponent
    if avatarRenderer == nil then
        return
    end
     
    -- Sets the color to use on a player upon collision with object (red).
    avatarRenderer:SetColor(1.0, 0.0, 0.0, 1.0)
}
```
![avatarcolour](https://mod-file.dn.nexoncdn.co.kr/bbs/1656037221622eb2a951c9c6a4efc8911926e1553490b.gif "avatarcolour")
><span style="color: #7cafc2">**Tip**
> Since we extended TriggerComponent, adding OnEnterTriggerBody function will add ![override](https://mod-file.dn.nexoncdn.co.kr/bbs/1640768458649f5cbcbca48d348adb5f4cb490510b822.png "override") at the front.</span>
# Changing Avatar's Costume 
You can use **CostumeManagerComponent**'s `SetEquip` property to change the avatar's costume when the avatar touches a certain entity.
Let's look at an example that changes the avatar's costume to the RUID below when it touches a specific entity.
| Parts | RUID |
| :---: | --- |
| Cap | 08346b77089b468baa34389b2bf299c7|
| Long coat (suit) | 0858e3dba9fb4fd3a20273cdeddff5ce|
| One-handed weapon | 2f921322114549da9ef5725e980a28c9 |

1.  Select **BaseEnvironment - NativeScripts - Component - TriggerComponent**.
2. In the context menu of **TriggerComponent**, click **Extend**.
![11](https://mod-file.dn.nexoncdn.co.kr/bbs/1687754755702bcecc1f59a204f3ca931773a1849464d.png "11")
3. Change the name of extended script, and add a component to the entity that will collide with the avatar.
4. Compose the script as follows.
```lua
{
    override void OnEnterTriggerBody(TriggerEnterEvent enterEvent)
    __base:OnEnterTriggerBody(enterEvent)
     
    -- Gets target (player) that collided with the object.
    local player = enterEvent.TriggerBodyEntity
     
    -- If the target that collided with the object has no AvatarRendererComponent, the costume will not change.
    local costumeManager = player.CostumeManagerComponent
    if costumeManager == nil then
        return
    end
    
     -- Enter RUID of the new equipment.
    costumeManager.CustomCapEquip = "08346b77089b468baa34389b2bf299c7"
    costumeManager.CustomLongcoatEquip = "0858e3dba9fb4fd3a20273cdeddff5ce"
    costumeManager.CustomOneHandedWeaponEquip = "2f921322114549da9ef5725e980a28c9"
}
```

![avatarchaing](https://mod-file.dn.nexoncdn.co.kr/bbs/16560372660016a0c0a7f65d346e1aa7d166ed839ec13.gif "avatarchaing")
# Other Ways to Utilize Avatars
You can create and use a new avatar with **AvatarRendererComponent** and **CostumeManagerComponent**. You can create monsters, portals, NPCs, or other special entities of your own. At this time, **TransformComponent, AvatarRendererComponent, and CostumeManagerComponent** are three essential components. Along with the mandatory components, you can add other native components or new components of your own.
#### Creating Avatar-shaped Portal
![avatar05](https://mod-file.dn.nexoncdn.co.kr/bbs/16397077408772b3142d25d454ec2811260ef4c5400d5.png "avatar05")
1. In the context menu of **Workspace - MyDesk**, click **Create Model** to make a new model.
<br>
2. Add **TransformComponent, AvatarRendererComponent, CostumeManagerComponent** to the new model.
![avatar02](https://mod-file.dn.nexoncdn.co.kr/bbs/16397058202122eac734eedb14d1fb88237289b257b05.png "avatar02")
<br>
3. Enter costume information RUID to **CostumeManagerComponent** to create an avatar that looks different from the creator's avatar.
    | Parts | RUID |
    | :---: | --- |
    | Body | 504244b76e654f5db23dc62693106bc8 |
    | Cape | 0c40fcbc25844082981d2f4ca04ee383 |
    | EyeAccessory | e3846cade85f4a04a603a9a8f5f705d4 |
    | Face | 0b67e20c585145b7b50bdce28386bef0 |
    | Glove | 43e864f8cd77466db4d0d7d342e72410  |
    | Hair | 0521135d1c234f10ab5c1c02b1aed813 |
    | Longcoat | 1141b91758e54052ac1f39cfb0680574 |
    | Shoes| ad81534ee2e34e4b98e312517ca3e192 |
    ![avatar3](https://mod-file.dn.nexoncdn.co.kr/bbs/16397063807041913c00c619e43c980832b999591973c.png "avatar3")
<br>
4. Add **PortalComponent** and **NameTagComponent**.
![avatar04](https://mod-file.dn.nexoncdn.co.kr/bbs/1639707712623152a28523dd74a3e8b5f5140fe31ba9b.png "avatar04")

##### Reference Guide
* [Editing Initial Character Properties]
* [Making a Portal to Move to Another Location]
* [Naming Entities]