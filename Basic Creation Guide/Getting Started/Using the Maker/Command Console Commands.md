# Course Introduction
Let's learn about commands that can be used in the Command Console window.

# help 
Display a list of commands and their descriptions. 

```lua
help [command=all] [-out stream] [-err stream]
```
#### Parameter Variables
* **command**: The name of the command to output the description for. You can view descriptions for all commands by entering all.

#### Options
* **-out**: Set the stream to output the results to.
  * stream: A stream that will be output.
* **-err**: Set the stream to output errors to.
    * stream: A stream that will be output.

#### Example
```lua
help findentity
```

# echo 
Output the entered message to the results window as-is. 

```lua
echo message [-out stream] [-err stream]
```

#### Parameter Variable
* **message**: A message that will be output.
#### Options
* **-out**: Set the stream to output the results to.
  * stream: A stream that will be output.
* **-err**: Set the stream to output errors to.
  * stream: A stream that will be output.

  #### Example
```lua
echo "test"
```
# export
Sends the desired file to the set path. You can only send scripts, code blocks, data sets, and user data set entries. 

```lua
export src_path dest_path [-out stream] [-err stream]
```

#### Parameter Variable
* **src_path**: The workspace path for the entry that will be sent out.
* **dest_path**: The path to save the entry that was sent out. The path starts from MyDesk.
  
#### Options
* **-out**: Set the stream to output the results to.
  * stream: A stream that will be output.
* **-err**: Set the stream to output errors to.
  * stream: A stream that will be output.

#### Example
```lua
export "MyDesk/NewComponent" "C:\TestFolder"
```

# import 
Loads a file to the set path.
```lua
import src_path [dest_path=MyDesk] [-overwrite] [-out stream] [-err stream]
```
#### Parameter Variable
* **src_path**: The path of the entry that will be loaded.
* **dest_path**: The path to save the entry that was loaded. The path starts from MyDesk.

#### Options
* **-overwrite**: Allow overwriting.
* **-out**: Set the stream to output results to.
  * stream: A stream that will be output.
* **-err**: Set the stream to output errors to.
  * stream: A stream that will be output.

#### Example
```lua
import "C:\TestFolder/NewComponent.xml" "MyDesk/NewFolder"
```

# findentity 
Searches for a specific entity from the hierarchy. Entities of all maps can be targeted. 

```lua
findentity [-path path] [-name name [match_type=contains] [comparison_type=ordinal]] [-visibleonly] [-enableonly] [-modelonly] [-model_name name [match_type=contains] [comparison_type=ordinal]] [-model_id id] [-out stream] [-err stream]
```

#### Options
* **-path**: Set the the path for searching an entity.
    * path: Path for searching an entity.
* **-visibleonly**: Only search for Visible entities.
* **-enableonly**: Only search for Enabled entities.
* **-modelonly**: Only search for entities with expanded models.
* **-name**: Search for entities that include the set name.
    * name: The name that will be searched.
    * match_type: Method of searching for a name. Use either contains, equals, or regex.
      * contains: Search for entities with expanded models including the set name.
      * equals: Search for entities with expanded models that have the same set name.
      * regex: Search for matching entities using regular expressions.
    * comparison_type: Method of comparing names. Use either ordinal or ordinal_ignorecase.
      * ordinal: Execute byte comparison.
      * ordinal_ignorecase: Execute a byte comparison and ignore capitalization.
* **-model_name**: Search for entities with expanded models that include the set name.
    * name: The name that will be searched. 
    * match_type: Method for searching a name. Use either contains, equals, or regex.
      * contains: Search for entities with expanded models that include the set name.
      * equals: Search for entities with expanded models with the same set name.
      * regex: Search for matching entities using regular expressions.
    * comparison_type: Method of comparing names. Use either ordinal or ordinal_ignorecase.
      * ordinal: Execute byte comparison.
      * ordinal_ignorecase: Execute byte comparison and ignore capitalization.
* **-model_id**: Search for models with a matching model id.
  * id : The id of the model to be searched for.
* **-out**: Set the stream to output results to.
  * stream: A stream that will be output.
* **-err**: Set the stream to output errors to.
  * stream: A stream that will be output.

#### Example
```lua
findentity -path "World/ui/DefaultGroup" -enableonly -name "Button" contains ordinal_ignorecase -out $"C:\TestFolder/result.txt"
```

# findcomponent 
Search for a specific component in the hierarchy. All maps can be targets of this search.

```lua
findcomponent [-path path] [-enableonly] [-name name [match_type=contains] [comparison_type=ordinal]] [-out stream] [-err stream]
```

#### Options
* **-path** Set the path  for searching a component. 
  * path: Path for searching components.
* **-enableonly**: Only search for components that are Enabled. 
* **-name**: Search for components with names including the set name.
    * name: The name that will be searched.
    * match_type: Method of searching for a name. Use either contains, equals, or regex.
        * contains: Search for components with names including the set name.
        * equals: Search for components with the same set name.
        * regex: Search for components with a matching name using regular expressions.
    * comparison_type: Method of comparing names. Use either ordinal or ordinal_ignorecase.
        * ordinal: Execute byte comparison.
        * ordinal_ignorecase: Execute byte comparison and ignore capitalization.
* **-out**: Set the stream to output results to.
  * stream: A stream that will be output.
* **-err**: Set the stream to output errors to.
  * stream: A stream that will be output.

#### Example
```lua
findcomponent -path "World/ui/DefaultGroup" -name "ButtonComponent" equals ordinal_ignorecase -out $"C:\TestFolder/result.txt"
```

# findproperty  
Search for specific properties in the hierarchy. All maps can be targets of this search.

```lua
findproperty [-path path] [-name name [match_type=contains] [comparison_type=ordinal]] [-component_name name [match_type=contains] [comparison_type=ordinal]] [-out stream] [-err stream]
```

#### Options
* **-path**: Set the path for searching properties. 
  *  path: Path for searching properties. 
* **-name**: Search for properties including the set name.
    * name: The name that will be searched. 
    * match_type: Method of searching for a name. Use either contains, equals, or regex.
        *  contains: Search for properties with names including the set name.
        *  equals: Search for properties with the same set name.
        *  regex: Search for properties with a matching name using regular expressions.
    * comparison_type: Method of comparing names. Use either ordinal or ordinal_ignorecase. 
        * ordinal: Execute byte comparison.
        * ordinal_ignorecase: Execute byte comparison and ignore capitalization.
* **-component_name**: Search for properties from components including the set name.
    * name: The name that will be searched. 
    * match_type: Method of searching for a name. Use either contains, equals, or regex.
        * contains: Search for components with names including the set name.
        * equals: Search for components with the same set name.
        * regex: Search for components with a matching name using regular expressions.
    * comparison_type: Method of comparing names. Use either ordinal or ordinal_ignorecase.
        * ordinal: Execute byte comparison.
        * ordinal_ignorecase: Execute byte comparison and ignore capitalization.
* **-out**: Set the stream to output results to.
  * stream: A stream that will be output.
* **-err**: Set the stream to output errors to.
  *  stream: A stream that will be output.

#### Example
```lua
findproperty -name "Color" -component_name "SpriteGUIRendererComponent"
```

# play 
Play in the World. Open in debug mode or add virtual players.

```lua
play [-debug] [-virtual_player player_number] [-out stream] [-err stream]
```

#### Options
* **-debug**: Play in debug mode.
* **-virtual_player**: Add virtual player 
  * player_number: Number of virtual players to be added. 
* **-out**: Set the stream to output results to.
  * stream: A stream that will be output.
* **-err**: Set the stream to output errors to.
  * stream: A stream that will be output.

#### Example

```lua
play -debug -virtual_player 3
```