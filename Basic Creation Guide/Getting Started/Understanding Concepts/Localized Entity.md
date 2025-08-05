# Course Introduction
Let's learn about localized entities that are only created in the client. 

##### Reference Guide
[Server and Client]
[Server Verification Example]
[Preparation for Packet Modulation]
[Effective MSW 2]
# Localized Entities
A <span style="color: #dc9656">**Localized Entity**</span> indicates entities that are only created in the client. In the Hierarchy window, it is displayed as an entity marked with ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1744353390721e6e7eb37353d4042aabf7d24c432ba27.png "Localized").
Entities in MapleStory Worlds are created in both the server and client by default. This is to help with easy multiplayer working and creating. However, the more the created entities on the server, the more load the server needs to handle. If an entity doesn't need to be managed by the server, it can be created as a Localized Entity that is only created in the client.
By localizing entities or data used as decorative elements for a world that don't have to be saved, less communication is required between the server and client for sychronization, making a more efficient world.

When using Localized Entities, you must keep 4 things in mind.

1. They are affected by **Parent-Child** relationships. Changing a parent entity to a localized entity will also change all of its child entities as well.
2. **Server Execution Control Functions** can't be used.
3. Since verification can't be done from the server, they are succeptible to falsification and hacking. Entities that play a crucial role in the world's ecosystem should not be localized entities.
4. The **PortalComponent** is unavailable for localized entities. Also some of the functions in  components that use server functions may not run as intended.

# Creating Localized Entities
There are 3 ways to use default entities that synchronize with the server as localized entities.

#### Activating the Localize Property
You can localize entites with the <span style="color: #dc9656">**Localize**</span> property. This property can only be activated in entities below world. When this function is activated, entities will only be created in the client, even when they are placed directly in Scene.
Activates `Localize` from **Property Window - Model Property** for entities placed under **Hierarchy - World - maps**.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/174433695073900c80e0cc0604735bd090b707991c89b.png "Localize")
#### Create as UI Entity
**UI Entities** created in **ui - UI Group** can only be created in the client. UI Entities have no Localize property in the Property Editor window, since the default value is localized.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1744354007080d2ad5568bf8c4085889573254f82f4ea.png "UI")
#### Create a Dynamic Using SpawnService
You can create Localized Entities with dynamics by calling `SpawnService` in the client space. When creating entities using dynamics, you must call SpawnService an equal number of times as the number of entities that will be created. This may decrease world creation efficiency, so it's recommended that you only use it when needed.