# Course Introduction 
This guide functions normally when continued from the world that was made following the instructions from [Object Movement Ⅰ](/docs?postId=123{"target":"_blank"}). 
A monkey is blocking the way of the taxi that's trying to get to Azalin. Move the taxi by using `Translate` from `TransformComponent` and the `Translate` function from `RigidbodyComponent` to push the monkey away.

![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/17479080177867bce6cbfb0bd4a4d9233d6f9f549cc6d.gif "3")

# Monkey Placement
1. Search for **monster-1415** from **Preset List - Monster** and place it on the map.
2. Change the entity's name to <span style="color: #dc9656">**Monkey**</span>.
   ![[object Object]](https://mod-file.dn.nexoncdn.co.kr/bbs/1747967206328c03a4eb0cf424e2ca5471b700991cefa.png "MonkeyEntity")
# Pushing the Monkey Away
Let's make it so that the taxi pushes the monkey out of the way using RigidbodyComponent. In order to push the monkey away, the monkey needs to confirm the taxi's location. And when the taxi approaches the monkey, it should affect the monkey's RigidbodyComponent.
 
1. Select **Workspace - MyDesk - Create Scripts - Create Component** to add a new script component and change the name to <span style="color: #dc9656">**Monkey**</span>.
    ![13](https://mod-file.dn.nexoncdn.co.kr/bbs/16354916990243da854927fa84124a5c9b80e0f2aa9d3.png "13")

2. Add the **Monkey component** to the **Monkey** entity.
    ![18](https://mod-file.dn.nexoncdn.co.kr/bbs/1659681951398202605997c4d41c7aca3d4776d71e806.png "18")

3. Add the following 4 properties to the **Monkey** component.

    * **MonkeyTransform**: Assigns the monkey's Transform component reference value
    * **TaxiTransform**: Assigns the taxi's Transform component reference value
    * **MonkeyRigidbody**: Assigns the monkey's Rigidbody component reference value
    * **IsHit**: Force is applied to the monkey in the Rigidbody when the monkey approaches the taxi, and that force is applied only once

    ```lua
    Property:
    Component MonkeyTransform = nil    
    Component TaxiTransform = nil
    Component MonkeyRigidbody = nil 
    boolean IsHit = false 
    ```

4. Add the `OnBeginPlay()` function and write the following script.
    
    ```lua
    Method:
    OnBeginPlay()
    {
        local taxi = _EntityService:GetEntityByPath("/maps/map01/Taxi")
        local monkey = self.Entity
        
        self.TaxiTransform = taxi.TransformComponent
        self.MonkeyTransform = monkey.TransformComponent
        self.MonkeyRigidbody = monkey.RigidbodyComponent
    }
    ```
    <br>
5. Add the `OnUpdate` function and write the following.
   
   * If the property **isHit** default value is **false**, it checks the distance between the monkey and the moving taxi.
   * If the x value of the taxiPos is larger than the x value of the monkeyPos minus 0.8, the monkey's TransformComponent requires force equal to `(8,8)` to psuh it out of the way.
   * If the monkey is pushed, change the **isHit** value to **true** so that it can't be repeatedly pushed.

    ```lua
    OnUpdate()
    {
        if self.IsHit == false then
            local taxiPos = self.TaxiTransform.Position
            local monkeyPos = self.MonkeyTransform.Position
            if taxiPos.x > monkeyPos.x -0.8 then
                self.MonkeyRigidbody:SetForce(Vector2(8,8))
                self.IsHit = true
            end
        end
    }
    ```