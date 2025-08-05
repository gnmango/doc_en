# Course Introduction
You can draw various lines in the World freely by using PolygonRendererComponent.

# PolygonRendererComponent
`PolygonRendererComponent` can draw polygons in the World by connecting points. A polygon is drawn according to the order of the points added. If the lines intersect when connecting points, the polygon will not be drawn. If you need to check whether a polygon is drawn, use the `IsDrawable()` function.

If you enter the coordinates of the points as below, the polygon is not drawn because the line connecting (0,0) (3,2) and the line connecting (1,0) (2,2) intersect.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1692247765816f43f2f51cd72421fa5b6b38c8f1a1b4e.png "1")

The polygon is drawn if you enter the coordinates of points so that the lines don't intersect.
![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16922477424456fa94597546b46dfb64ac2eed48916c7.png "2")

The main properties of `PolygonRendererComponent` are as follows. 
* Click the **Points Edit**: [Edit] button to enable Edit. You can modify, add, and delete the point location in the Scene.
* **Color**: Specifies the color of the polygon.
* **Points**: The group of points that form a line.
    * **Size**: Indicates the number of points connecting lines. The number starts from 0, and the points will be added as much as the value you entered for Size.
    * **[Number]**: The number of the point. The X, Y values are the coordinates of the point.

# Usage Example
#### Draw Polygon
1. Create the new ![entity](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_entity_no.png "entity") EmptyEntity, and add it to **TransformComponent, PolygonRendererComponent**.
2. Select **Points** of the PolygonRendererComponent and add the number to the **Size**.
3. Enter the coordinates of the point, and specify the **Color**. 
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/16944121241288615fed41bbe4d32bc36b4eea6e4b651.png{"width":"840px"} "5")

> <span style="color: #7cafc2">**Tip**
> If there are more than 2,000 points, no more points are drawn.
> If the points are placed to intersect different sides, the polygon will not be drawn.

#### Draw Polygon with Material
You can use Material's **PolygonSprite** Shader to paint a polygon. For questions about how to use the Material, please refer to [Utilizing Material](docs/?postId=828{"target":"_self"}).
You can use both user resources and cloned MSW resources as the Material's sprites available for PolygonRendererComponent. To learn how to clone MSW resources, see [Managing Resources](docs/?postId=690{"target":"_self"}).

1. Select **Workspace - MyDesk - Create Material** to make a new Material.
2. In the Properties window of the Material you've created, select **Shader - PolygonRenderer - PolygonSprite**.
3. **Select the image you'd like to use for SpriteRUID**. If necessary, adjust the Offset and Scale values.
4. ![entity](https://mod-file.dn.nexoncdn.co.kr/storage/icons/workspace/icon_entity_no.png "entity") Create EmptyEntity and add PolygonRendererComponent.
5. In the **MaterialId** of PolygonRendererComponent, press ![open](https://mod-file.dn.nexoncdn.co.kr/storage/icons/common/icon_open_folder.png "open") and specify the Material.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1694413381584443e343db0c5476daf441bf8017867cf.png{"width":"840px"} "6")

><span style="color: #7cafc2">**Tip**
> Both the user resource and the cloned MSW resource must have their lab mode set to **Repeat**.</span>
> ![Repeat](https://mod-file.dn.nexoncdn.co.kr/bbs/168422631874542cfe7bb765e4d659b113e430d223b99.png{"width":"340px"} "Repeat")

><span style="color: #7cafc2">**Tip**
> If you increase the size of the polygon to which the material is applied, the size of the material will also increase.
> When the Rotate value of the polygon changes, the Material's sprites will change accordingly.</span>
> ![12](https://mod-file.dn.nexoncdn.co.kr/bbs/169225197319446744ec0a8594315ade9ee52da40ac70.png{"width":"340px"} "12")