# Course Introduction
MapleStory Worlds provides a package function through which model, script components, and Workspace data sets can be distributed and shared externally as a file. You will learn how to export an entry from Workspace as a package file or import a package file into the World.

# About Package
Packages are files that bundled certain entries. You can share a portion of the created world with others or use packages shared by others in your world through package files. You can view the folder structure and heirarchy structure from the share and load package window.

* Folder Structure: Shows packages sorted in the order of their entries in Workspace.
* Hierarchy Structure: Shows the parent-child relationships of the entries. Useful when selecting and deselecting entries that have dependencies. 
 
| Folder | Hierarchy |
| --- | --- |
| ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17271629209637ae72caa69ec40dd93fe2106ba2fdfa7.png "folder")| ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/172716296091145b02203ddb7478a876b6a32fc498978.png "Hierachy")|

# Export package
1. In **Workspace - MyDesk**, select the entries to export as a package.
2. Click **Export To Package** in the context menu to open the Export window.
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1710308131839b0ccc1f6757d4c8497b97b6558157c70.png "exporttopackage")
3. After selecting the entries, click the **[Export]** button at the bottom. Change the name if necessary, and click the **[Save]** button.
![export](https://mod-file.dn.nexoncdn.co.kr/bbs/17271631294426b9c5d722e0a4858a4d87302041183ad.png{"width":"430px"} "export")

# Rules for Exporting
#### Resource Exporting
To include resource entries such as images or sounds in a package, you need to save the World. Save resource entries in the server when saving the World and distribute only resource entries in the server as a package. Because a package stores RUIDs of the resource entries, when you import that package from another World, it uses the RUIDs to import the resources stored on the server.

#### Entry Inheritance Relationship
If you export entries in a parent-child relationship as a package, the parent-child relationship is maintained. If you select only the expanded entries and export them as a package, the parent entries are automatically included in the package. For example, imagine that you created a Model_EmptyEntity_child that extends its parent Model_EmptyEntity. If you select only the Model_EmptyEntity_child and export it as a package, its parent is included too.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17271633453210707940d48834b2ea60474da9377acd9.png "export")

#### Limitations with Native Entries
**The native scripts and native models** provided by MapleStory Worlds cannot be exported as a package. Also, if you add a new component to the native model itself and extend that native model, it cannot be exported as a package. 
This is because not all information can be imported when the package is imported. For example, imagine that you added ComponentA in the native model and created its extended model. This extended model will inherit ComponentA.
When ModelB is imported from another World, it cannot be exported as a package, since the new World does not have a native model that contains ComponentA and ModelB may be added differently from the creator's intention.

![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1711520919542cc7ecbdb36ed4388a710eccb0248ac79.png{"width":"820px"} "1")

The extended native entries can be exported as a package. Since the extended entry has information from the native entry, it automatically finds the native model to inherit from the new World.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17115264454698f2775b2d21349e2adf74871b0d2cda5.png{"width":"820px"} "2")

To make a package by including new features into a model that inherits from a native model, we recommend either adding components to the extended model or placing them in the native model scene, editing them, and then creating them as Create Model From Entity.

![3](https://mod-file.dn.nexoncdn.co.kr/bbs/171152096321699e779d3b6e14078be21089b366ed7f3.png{"width":"820px"} "3")

# Importing Package
1.  Select **Import Package** and select the package you want to import. There are two ways to import a package.

    *  Click **Create - Import Package** and select the package you want to import.
    * Click **Workspace - Import From - Import Package** and select the package you want to import.

2. Select the entry you want to import and click the **[Import]** button. 
![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1710315987741322b1a2f76574f1795224144a943d481.png "import")

    > <span style="color: #7cafc2">**Tip**
    > When you import a package, the World automatically saves its previous state. You can see the automatically saved Worlds in **File - Rivisions**.</span>
    > ![revisions](https://mod-file.dn.nexoncdn.co.kr/bbs/17290697579923fd444be363345a8afa0da8586ead6b1.png{"width":"620px"} "revisions")

# Rules for Importing
#### Import Collision
When importing a package into the World, entries may collide. Depending on the nature of the colliding entries, the entries will either be unavailable, overwritten, or renamed. 
Entries that cannot be imported immediately as available will display the following icons. 

| Icon | Description |
| --- | --- |
| ![update](https://mod-file.dn.nexoncdn.co.kr/bbs/170495172860025bc9882ab1844618b4839224af7afb5.png{"width":"35x"} "update") | Entries with the same name in the World, but with different content. They will overwrite the existing entries.|
| ![rename](https://mod-file.dn.nexoncdn.co.kr/bbs/1704951745392fd9a4eef1a2a44beb5a7e7f811782655.png{"width":"23px"} "rename") | Renamed entries.   |
| ![NameComplict](https://mod-file.dn.nexoncdn.co.kr/bbs/17049517636351f8489cf0bdc424a8066cf85f6207987.png "NameComplict") | Entries with the same name and content.  |
| ![IdComplict](https://mod-file.dn.nexoncdn.co.kr/bbs/170495199953619842ee2757d465faee85fa3e285b1c3.png "IdComplict") | Entries with the same Id. |
| ![moved](https://mod-file.dn.nexoncdn.co.kr/bbs/1704952016010a8122bcebacc4831a18be2b30c2f6e21.png "moved") |  Entries with a different path from the current World. |
| ![equal](https://mod-file.dn.nexoncdn.co.kr/bbs/17049520316499abe1ca8a9944aafa17e610f838fd71c.png "equal") | Same entries in the current World.  |


# Official Package Download Guide
MapleStory Worlds offers various useful packages that will help creators in creating their worlds. You can freely edit the package's content to fit the world.
The official packages can be downloaded from [MSW Packages Github](https://github.com/MSW-Git/MSWPackages{"target":"_blank"}) - README.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17530626117338cdf85ebaa064b8b86ed9530baaabf94.png "download")