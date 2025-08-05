# Course Introduction
You can use data tables to add or modify data. In this guide, we will explore loading data using a script, creating data, and editing data.
# Creating Data and Opening the Data Editor
1. In the context menu of **Workspace - MyDesk**, click **Create DataSet**.
<br>
2. Double-click the newly created data to open the Data Editor.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16651306854177c11e2993eae457888ac58ec6a52b3f6.png "2")
# Data Editor Interface
![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16651308011396be2bc720c0e4deda2c6425fd184ff10.png "3")
<br>
| Number | Explanation |
| --- | --- |
| ![NO01](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541272181b5c1a55fcf3d49b19734d25913c38583.jpg "NO01")| **Column Cell**<br>Define the data name for each row, i.e. the column name.<br>You can load the data of each row via the column name. |
| ![NO_02](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541300837cb541c2f44e046a79bb1901a885aa8ac.jpg "NO_02")| **Cell**<br>Each set is an input text field. Enter the data value suitable for the definition of each row.<br>The entered data values are all saved as strings. |
| ![NO_03](https://mod-file.dn.nexoncdn.co.kr/bbs/163454131465069e090278448490f965207e9a4a10348.jpg "NO_03")| **Add row/column button**<br>When you click the button, add a new row or column at the end of the row/column. |
| ![NO_04](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541326353d8628c1473944497bf376acb7a65ca45.jpg "NO_04")| **Insert or delete row/column handler**<br>Add a new row/column around the row/column selected, or delete the selected row/column.<br>You can see the handler by selecting the head row and head column.<br> |
| ![NO_05](https://mod-file.dn.nexoncdn.co.kr/bbs/1634541338689678f574f21e54a6ca533737924124d7e.jpg "NO_05")| **Import data from the web**<br>You can import the data of GoogleSpreadSheet.<br>If you write data in GoogleSpreadSheet and put the URL of the data in the pop-up window,<br>it overwrites data in GoogleSpreadSheet over currently open data. <br>However, URLs in GoogleSpreadSheet can only use URLs posted on the web<br> ![NO_05_1](https://mod-file.dn.nexoncdn.co.kr/bbs/16577012857253f3f35d4a11f4cfbb9faaa60264c18b9.png "NO_05_1") |
| 	![NO_06](https://mod-file.dn.nexoncdn.co.kr/bbs/163454135201207284554a25b498380fff224cd767f6b.jpg "NO_06")| **Import CSV**<br>You can load a CSV file saved on your computer and replace the current data.<br>You can only use a CSV file saved in utf-8 format. |
<br>
# Load the Composed Data Script
1. First, write the contents of the image below into the <span style="color: #dc9656">**NewDataSet**</span> you created above.
    ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/16596783003508b1add63e564405d9f00a099a8496934.png{"width":"750px"} "6")
    <br>
2. Save the data and add a new script component called "<span style="color: #dc9656">**DataSet**</span>".
    ![newcomponent](https://mod-file.dn.nexoncdn.co.kr/bbs/16878457449028bebb7ed9b9141288f93dd176baca7c5.png "newcomponent")
    <br>
3. Open the **DataSet** component and add the `OnBeginPlay()` function.
    ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/16354715733367404d00c99aa45de86f5cc5cdfe101ec.png "8")
    <br> 
4. Write as follows.
    ```lua
    [server only]
    void OnBeginPlay()
    {
        --Retrieve the data with the name of the data created above.
        local dataSet = _DataService:GetTable("NewDataSet")    
        -- Enter the row and column number of the cell data that you are trying to retrieve as parameters.
        local cellValue = dataSet:GetCell(3,2)               
        --Output the cell value on the console window.
        log("cellValue : "..tostring(cellValue))             
    }
    ```
    <br>
5. Click **Add Component** in the context menu of **Hierarchy - map01**.
    <br>
6. Search for **DataSet** and add the **DataSet** component.
    ![8-3](https://mod-file.dn.nexoncdn.co.kr/bbs/1637219296068e486bb5b29b4418b9ef31c1647ad3323.png "8-3")

    ><span style="color: #585858">**Learn More**
    >**_DataService:GetTable(string name)**
    > Function of returning the data table as the name of the data.
    > Pass the data name as parameter.
    > <br>
    > **UserDataSet:GetCell(int row_1based, int col_based)**
    > Return the value of a particular cell from data as string type.
    > Enter column and row number where the cell is located as parameter.
    > Row and column start from 1.</span>
    <br>        
7. Press Play and check the Console window.
![9](https://mod-file.dn.nexoncdn.co.kr/bbs/165967905703060d30eec2560433d9a4922fefc9ed0db.png "9")