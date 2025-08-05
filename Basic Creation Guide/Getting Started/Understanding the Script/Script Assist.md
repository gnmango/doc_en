# Course Introduction
The Script Editor of MapleStory Worlds supports several functions to write code more accurately and quickly. Script Assist is the collective term for these features.
This course will guide you through the various features of Script Assist.

# Navigation
The Navigation function supports quick and easy movement to the desired location in the script.

### Find References
The Find References feature makes finding references easy.
Click **Show References** in the symbol's context menu to open the **References** window.
The Find References feature finds references to the current symbol throughout the entire project.
![01](https://mod-file.dn.nexoncdn.co.kr/bbs/167297348163458165b0bcf8a4f80a73d2dbc09d0db98.png "01")
![02](https://mod-file.dn.nexoncdn.co.kr/bbs/1673251019057e79f96ce9e9a4f9f9d7977acfc1b5794.png "02")

### Go to Definition
If you click **Go to Definition** in the symbol's context menu, you will move to the symbol's definition.
![03](https://mod-file.dn.nexoncdn.co.kr/bbs/16729741775763a6b33cef7374dbfae12640036d4eee6.png "03")

### Go Back/Go Forward
Using the shortcuts below, you can move to a previously recorded location or move back to the most recent location.
 - Navigate Back: Pressing `Ctrl(L)` + `-` will move to the previous location.
 - Navigate Forward: Pressing `Ctrl(L)` + `=` key moves you from the previous location to the latest location.
![navigate](https://mod-file.dn.nexoncdn.co.kr/bbs/1678676733191965cd0d90c1c4abd81eea6cec1638e52.gif "navigate")

# Code Inspection
Code Inspection is a function that checks the code to catch mistakes that the user was not aware of and prevent coding errors in advance.
For example, as shown below, an underline is displayed when an error occurs, and detailed information appears when you mouse over it. You can use this as a reference to quickly correct errors.
![04](https://mod-file.dn.nexoncdn.co.kr/bbs/16732510875278c77b508a29640aa88a82771566c5632.png "04")

Code Inspection can typically detect the following errors.
| Error | Description |
| :---: | --- |
| UnavailableMethodCall | You used Dot for a function where Colon is recommended. |
| TooManyParameter | You wrote more parameters than required. |
| ParameterTypeMismatch | The type of parameter you wrote does not match the required type. |
| AssignTypeMismatch | The type of value you assigned does not match the type of the variable. |
| TableKeyTypeMismatch | The table key types do not match. |
| NotRecommendedAssignment | You wrote an unrecommended assignment statement. |
| ReturnValueFromVoidFunction | You wrote a return statement in a function without a return value. |
| AssignToReadonlyProperty | You have assigned it to a read-only property. |
| IntroduceGlobalVariable | You declared a global variable. |
| ObsoleteAPIUsed | You used a deprecated API. We recommend using a different API. |
| UnbalancedAssignment | The left and right sides of an assignment statement have different lengths. |
| UnreachableCode | You wrote unreachable code. |
| UnresolvedSymbol | Symbol not found. |
| UnresolvedMember | Member not found. |
| UnresolvedFunction | Function not found. |
| DuplicateLocal | You have a local variable declared redundantly. |
| AnnotationNotFound | Annotations not found. |
| AnnotationTypeNotFound | The type used in the annotation not found. |
| ReturnTypeMismatch | The return type does not match. |
| NotEnoughArgument | You wrote fewer arguments than required. |
| DuplicateFunction | You have defined the function redundantly. |

# Highlighting
This is a function that helps to easily recognize the code by highlighting the grammar in various ways, using the text color and background color of the code.

### Symbol Reference Highlight
Symbol Reference Highlight is a feature that highlights multiple symbols that reference a selected symbol.
Selecting a specific symbol, as shown below, also highlights the background color of other symbols that refer to it.
![05](https://mod-file.dn.nexoncdn.co.kr/bbs/167323895521158cef8fc84a04a3cae102bbdbb564d69.png "05")


### Syntax Highlight
Syntax Highlight is a function to emphasize grammar.
For example, by analyzing the code, if the symbol is a variable, it is displayed in color 	#1F377F, in color #F17D80 if it is a string, and in color #648064 if it is an annotation type.
![07](https://mod-file.dn.nexoncdn.co.kr/bbs/167331313032494bf43f0f61f49d3a0258a84940b35f3.png "07")

In addition to the examples given, a variety of grammar highlighting is provided.

### Related Token Highlight
Related Token Highlight is a feature that highlights related tokens.
For example, if **if** is selected, **then** and **end**, which are used together in **if statement**, are also highlighted.
![06](https://mod-file.dn.nexoncdn.co.kr/bbs/1673239167792975ab6338cd647329a2cc5e0b19a34e7.png "06")

# Quick Info
Quick Info is a feature that quickly and easily displays additional code information when you hover over a function, variable, parameter, etc.
![quickinfo](https://mod-file.dn.nexoncdn.co.kr/bbs/1668064363104cfff013b14cd49a6bb3b7d90a79f90cf.png "quickinfo")

# Code Completion
By analyzing the context, Code Completion suggests symbols that can be created at the current position.
Use the top and down arrow keys to find an appropriate suggestion and then press the **Enter** or **Tab** key to insert the code phrase.
![code](https://mod-file.dn.nexoncdn.co.kr/bbs/16680557704632d17b97ae55d402ab873ed774fd19e92.png "code")

If you want to bring up code completion again, press `Ctrl` + `Space` on the symbol.
![code2](https://mod-file.dn.nexoncdn.co.kr/bbs/167867626381462b21a82b8e442b69c56c425c1b65957.gif "code2")

Code Completion, which also offers the names of functions or variables the user has declared, prevents typing mistakes and makes it easy to reuse codes.

# Signature Helper
Signature Helper is a feature that helps you write parameters by displaying the function signature when writing a function.
Use the up and down arrow keys to find the intended signature and get help in writing functions.
![signature](https://mod-file.dn.nexoncdn.co.kr/bbs/166963495531574f3fb62127b4fc99058e2630eb54577.png "signature")

If you want to load the Signature Helper again, press `Ctrl` + `Shift` + `Space`.
![signature2](https://mod-file.dn.nexoncdn.co.kr/bbs/16843976134234c31e23de23d48c58444e0b85cc6bdf2.gif "signature2")

# Annotation
Annotation is a function to help improve the script assist function.

> <span style="color: #585858">**Learn more**
> For more details, please refer to the [Utilizing Annotation](/docs/?postId=824{"target":"_self"}) guide.</span>

# Code Generation
Code Generation is a feature that allows you to write code quickly by automatically adding common lines.

### Auto Bracket Pair Completion
AutoBracketCompletion, as the name suggests, will automatically complete bracket pairs.

### Auto Statement Pair Completion
AutoStatementPair automatically pairs the statements when you press the Enter key while writing a sentence.
![001](https://mod-file.dn.nexoncdn.co.kr/bbs/168439212479329d2c516a9c44e54932c198c11f3fab7.gif "001")

# Comment Shortcuts
You can use keyboard shortcuts to comment on or remove comments from codes.

### Whole Line Comment
Press `Ctrl`(L) + `/` to comment on the code.
Add a comment to the code in the 'line where the cursor is located' or 'in multiple lines selected by dragging'.
Press 'Ctrl(L)' + '/' again in the commented part to remove the comment.
![t1](https://mod-file.dn.nexoncdn.co.kr/bbs/1676422686186f6925f46e5cf45dc993bb712979ba9be.gif "t1")

### Block comment
Press `Ctrl(L)` + `Shift(L)` + `/` to apply block comments to the selected code.
You can also apply block comments to just a part of a whole line and not the whole line.
Press `Ctrl(L)` + `Shift(L)` + `/` again in the block-commented part to remove the comment.
![t2](https://mod-file.dn.nexoncdn.co.kr/bbs/167642308489426b09acc998840a59ef5c097f9a0c5e4.gif "t2")

# Other Convenience Features
Here are some additional quality-of-life features you can utilize.

## Main Shortcuts
### Transparentize
Press `Ctrl(L)` key to make Code Completion, Quick Info, and Signature Helper transparent.
You can use the Transparentize feature to see your code more clearly.
![t3](https://mod-file.dn.nexoncdn.co.kr/bbs/16786770174103e1c8fd8108740a891fea3be0b0ac988.gif "t3")

### Duplicate
You can duplicate the selected line or area by pressing the `Ctrl(L)` + `D` keys.
 - When you duplicate a line, the duplicated content appears on the line immediately below it. 
 - When you duplicate an area, it follows immediately after the area you selected.
![duplicate](https://mod-file.dn.nexoncdn.co.kr/bbs/1678676463146908e86c91d9a47559246ef5be0195b68.gif "duplicate")

> <span style="color: #585858">**Learn more**
> You can see the full list of available shortcuts in the Script Editor section of [MSW Shortcuts](/docs/?postId=813{"target":"_self"}).</span>

## Settings
You can go to **Edit - Preferences** to set your Script Assist preferences.
![002](https://mod-file.dn.nexoncdn.co.kr/bbs/168437690187617eb3133ef694b9f826d4ce89b6be5ac.png{"width":"600px"} "002")
Once you've changed the settings on each tab, you should press the **[Save]** button to save your changes.
<br>

The settings include the following options:
| Items | Detailed items | Desc |
| --- | --- | --- |
| Code Completion | Enable Code Completion | Enables the **Code Completion feature**. |
| Signature Helper | Enable Signature Helper | Enables the **Signature Helper feature**. |
| @cols=1:@rows=2:Code Generation | Enable AutoBracketCompletion | Enables the **Auto Bracket Pair Completion feature**. |
| Enable AutoStatementPair | Enables the **Auto Statement Pair Completion feature**. |
| @cols=1:@rows=4:Code Inspection | Enable Code Inspection | Enables the **Code Inspection feature**. |
| Error Level | Sets whether it is enabled or disabled for each error level. |
| Warning Level | Sets whether it is enabled or disabled for each warning level. |
| Info Level | Sets whether it is enabled or disabled for each info level. |
| @cols=1:@rows=3:Highlighting | Syntax Highlighting | Enables the **Syntax Highlight feature**.<br>You can set the highlight color for each item. |
| Symbol Highlighting | Enables the **Symbol Highlight feature**.<br>You can set the highlight color for each item. |
| Related Token Highlighting | Enables the **Related Token Highlight feature**.<br>You can set the highlight color for each item.   |