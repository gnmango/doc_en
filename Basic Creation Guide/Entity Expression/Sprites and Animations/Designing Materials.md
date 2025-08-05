# Course Introduction
 Material is the Entity that displays the method of rendering the Entity by various properties. Property of the Entity is altered according to an Entity rendering code.
Shader is the code (or a script) that is used when rendering objects. Since the property and code of the shader cannot be switched to dynamic, effects are individually expressed by each shader.
MapleStory Worlds provides a variety of shaders. Creators can create a Material and adjust the shader property for the desired aim.

# Creating Material
We'll take a look at how to create Material.

1. New Materials can be created with one of the three methods below.
    | 1. Workspace [+] button | 2. Workspace Context Menu | 3. Create Menu |
    | :---: | :---: | :---: |
    | ![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1686538286105d45a745926da4e4b812578f6b51690c2.png "1") | ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/16865382991887b1f0a30aae44c31bb97404200571ed7.png "2") | ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/168653831150808fff6cb82914b4691ae98c678a78ae6.png "3") |
    <br>
2. Create a Material, then specify a name.
    ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/166840323583571410bb4351f4e0badb9b51d500f8b81.png "4")


# Applying Materials to Entities
Let's see how to apply Materials to the Entity.

1. After selecting Entity, click the ![Common_Select](https://mod-file.dn.nexoncdn.co.kr/bbs/1668408907363c261c9faa43d4ee3b131624dd723e3a2.png "Common_Select") button in **Property editor - SpriteRendererComponent - MaterialID**.
    ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/166908403861011c64e140fd04f8f90bbdb8d5d57af59.png "5")
    <br>
2. Select the Material you want from the **Reference** window.
    ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1668409227396b253b59b89094a9c9ec4b914a1c29bb5.png "6")
    <br>
3. Specify **Shader** type.
    ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/168681313976760c074e2954f4814a93d511c9b9e3c12.png "7")
    <br>
    By utilizing the filter feature, you can narrow down and view only the shaders available for the selected component.
    ![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1727180781072360fe8c368824af7a5df25df3b8429d7.png "13")

# Shader Type
The rendering method differs by shader type, and adjustable properties also differ.
Let's take a look at the shader types offered in MapleStory Worlds.

## Default
Basic Material. It's the state without any effects.
|	@cols=2:@rows=1:Shader			|	Description	|	
| :---: | --- | --- |  
|	Default	|	![00](https://mod-file.dn.nexoncdn.co.kr/bbs/1668479786163abb0a4314fe34db0885f9d118d23236f.png "00")	|	Default is used when the Material is not specified or cannot be found. 	|	

## Outline
It's the effect that draws a contour line of the sprite.

|	@cols=2:@rows=1:Shader			|	Description	|	
| :---: | --- | --- |  
|	Outline	|	![01](https://mod-file.dn.nexoncdn.co.kr/bbs/166841095184460411e0fbd3d419cbe83b632bce31401.png "01")	|	Draws a contour line away from the sprite.	|	
|	InnerOutline	|	![2](https://mod-file.dn.nexoncdn.co.kr/bbs/166841143439790c697d7e2f24b368d3a3ad635d19020.png "2")	|	Draws a contour line to the inner sprite.	|	
|	ColorDiffOutline	|	![3](https://mod-file.dn.nexoncdn.co.kr/bbs/16684114971291ab4281d87cc4b9b87fd25f52356d572.png "3")	|	Draws a contour line by using the sprite color difference.	|	

## ColorEffect
It's the effect that displays various colors on the sprite.

|	@cols=2:@rows=1:Shader			|	Description	|	
| :---: | --- | --- |  
 | Rainbow | ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1668413469593d7bd1d3a1aa44677b8ac611864878d88.gif "4") | Blends with rainbow colors. |
| LineGradient | ![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1668413449478ceb1da170e9147d48914323930b9b404.png "5") | Gradationally blend 4 colors based on y axis. |
| VertexGradient | ![6](https://mod-file.dn.nexoncdn.co.kr/bbs/16684135009255cd62251f9bc4a8c90b0430dff483926.png "6") | Gradationally blend 4 vertex colors. |
| RadialGradient | ![7](https://mod-file.dn.nexoncdn.co.kr/bbs/16684135201985614fc9d040440078b52333b4e6f751a.png "7") | Gradationally blend center - external colors. |
| Gradient | ![8](https://mod-file.dn.nexoncdn.co.kr/bbs/1668413536391582e7b18622a46eeb8bc117524eead9b.gif "8")  | Displays a single color according to the time. |
| Reverse | ![9](https://mod-file.dn.nexoncdn.co.kr/bbs/16684135490414f64d3e9f5ac40ed9d8ccec8a7b5f49d.png "9") | Output colors opposite to target colors. |
| GrayScale | ![10](https://mod-file.dn.nexoncdn.co.kr/bbs/16684135604161d31df8cdb724a77b9b85129b1d5a79f.png "10")  | Output in gray tone. |
| Hologram | ![11](https://mod-file.dn.nexoncdn.co.kr/bbs/16684135717173629a30e711f41c79d6ce3bd85fa0baf.gif "11") | Output colored light effect to a certain direction.  |
| LimLight | ![12](https://mod-file.dn.nexoncdn.co.kr/bbs/1668413583454c85ff546505745bda50ecb4b3b1e1797.png "12") | Output bright light effect. |
| Posterize | ![13](https://mod-file.dn.nexoncdn.co.kr/bbs/1668413596725492a72e7ca45438ebb10abb856ef3701.png "13") | Restricts the number of rendering colors. |
| ColorOverride | ![14](https://mod-file.dn.nexoncdn.co.kr/bbs/1668413607254b9983afe45d44979bf13be197d61c2fe.gif "14")  | Changes to a single specified color. |
| ColorGlow | ![colorglow](https://mod-file.dn.nexoncdn.co.kr/bbs/1678961863541f622336dc27645c9b69ccace791963fb.png "colorglow") | Lightens areas of color similar to the color specified by GlowColor. |
| DropShadow | ![dropshadow](https://mod-file.dn.nexoncdn.co.kr/bbs/1678962067474c059773fcc6c490981a9faf032335085.png "dropshadow") | Applies a shadow effect to the color specified by TargetColor. |
| ConcentrationLine | ![concentrationline](https://mod-file.dn.nexoncdn.co.kr/bbs/1678959255414e67530f0ca6142cbbca85a7a939f3ea9.png "concentrationline") | Applies a saturated line effect towards the specified location. |

## UVEffect
It's the effect that distorts a coordinate of the sprite.

|	@cols=2:@rows=1:Shader			|	Description	|	
| :---: | --- | --- |  
| Noise | ![15](https://mod-file.dn.nexoncdn.co.kr/bbs/1668413949472f7b0b5a86aa4405ba3f4cccaec032045.gif "15") | The Entity is constantly shaking.|
| RoundWave| ![16](https://mod-file.dn.nexoncdn.co.kr/bbs/16684136490085c94218d5b2f4a618666646fea88d23c.gif "16") | Draw a concentric circle wave from a centerpoint. |
| UVScroll| ![18](https://mod-file.dn.nexoncdn.co.kr/bbs/16684137205168eccf5a92ada47f5883928a58ba1815c.png "18") | Moves the UV coordinates during sprite rendering. |
| Wave| ![19](https://mod-file.dn.nexoncdn.co.kr/bbs/1668413755673c476bd19b1774a808c070a6f1a37d7a4.gif "19") | Displays a circular wave at a certain point. |
| Glitch| ![20](https://mod-file.dn.nexoncdn.co.kr/bbs/166841389198235926a2352554e5fa7634e64c2ab2e7b.gif "20") | Randomly sample the other section of the sprite. |
| GrassMovement| ![20](https://mod-file.dn.nexoncdn.co.kr/bbs/16684139052283945063a36ea4c02b3f33b2dfcc0c1bc.gif "20") |  Swaying grass-like effect. |
| Distortion| ![21](https://mod-file.dn.nexoncdn.co.kr/bbs/166841391898321b73ca5b9884b8aa4141328b906457d.gif "21")  | Distorts the Entity. |
| Ripple | ![ripple](https://mod-file.dn.nexoncdn.co.kr/bbs/1678959633804aacda73d12264dc0be37ffb71af134dd.gif "ripple") | Applies a ripple effect. |

## Blurry
It's the effect that blurs the sprite.

|	@cols=2:@rows=1:Shader			|	Description	|	
| :---: | --- | --- |  
| Pixel| ![22](https://mod-file.dn.nexoncdn.co.kr/bbs/16684141311201c1f06b39e27416b96255925150be57c.png "22") | Pixelates the sprite. |
| ChromaticAberration| ![23](https://mod-file.dn.nexoncdn.co.kr/bbs/16684144518044c88ee2c6581467cbd882eab77e2bed6.gif "23") |  Applies chromatic aberration effect. |
| Blur | ![23](https://mod-file.dn.nexoncdn.co.kr/bbs/16684144693599a45ff2fd5c34efb8bba4ee8d9ecdf7b.png "23") |  Applies a blur effect to the sprite. |
| MotionBlur| ![24](https://mod-file.dn.nexoncdn.co.kr/bbs/16684144858279a9c29b9aee64a65a3271537c4835c99.png "24")   | Applies an orientable blur effect to the sprite. |
| RadialBlur | ![radialblur](https://mod-file.dn.nexoncdn.co.kr/bbs/16789605975780c74d541a0824bfea9f28e699737e860.png "radialblur") | Applies a centered blur effect. |
| KuwaharaFilter | ![KuwaharaFilter](https://mod-file.dn.nexoncdn.co.kr/bbs/16789606314994a9b29416cc14c9ab2072cfd7e3f5a99.png "KuwaharaFilter") | Applies the kuwahara filter. <br>You can create the feeling of watercolor by adjusting intensity.<br>It is difficult to notice a big difference when used on a dot image. |

## AlphaMask
It's the effect that makes the part of the sprite invisible.

|	@cols=2:@rows=1:Shader			|	Description	|	
| :---: | --- | --- |  
| AlphaCutoff | ![28](https://mod-file.dn.nexoncdn.co.kr/bbs/1668414746608f412644c0aae4f7d913cd7ccfda21ac8.png "28") | Does not render the part with low transparency.|
| AlphaRound| ![29](https://mod-file.dn.nexoncdn.co.kr/bbs/1668414760640a654dd3c8bc6429594372014855d9166.png "29")  | Processes the alpha value as 0 if it is lower than the transparency and 1 if it is higher. |
| Clipping| ![30](https://mod-file.dn.nexoncdn.co.kr/bbs/16684260189626d960534962e4a0589215d868f611bf0.png "30") |  Cut the part of the sprite. |
| Mask| ![31](https://mod-file.dn.nexoncdn.co.kr/bbs/1668482058357b63335cd7bfb40ae87c30a952e404255.png "31") |  Receive sprites as the parameter and specify the masking area.<br>e.g. **MaskTexture**: <span style="color: #dc9656">**0005fdee112f4ab8a59764021ac1dda0**</span> Application <br>![312](https://mod-file.dn.nexoncdn.co.kr/bbs/16684822611015fb0223e1b4844738039ffd237b746a5.png "312") |

## BlendColor
It's the effect that alters methods of the application of sprite colors to various forms.

|	@cols=2:@rows=1:Shader			|	Description	|	
| :---: | --- | --- |  
| BlendDarken| ![32](https://mod-file.dn.nexoncdn.co.kr/bbs/16684260934885895cb89035b485189cc3c2987d86870.gif "32") | Makes it dark by using the smaller value of each RGB channel.|
| BlendMultiply| ![33](https://mod-file.dn.nexoncdn.co.kr/bbs/166848152367162fef2866b0b475aac9f3635770d6466.gif "33") |  Displays the outcome from multiplying two colors. |
| BlendColorBurn| ![34](https://mod-file.dn.nexoncdn.co.kr/bbs/16684261191845675af047a21421aa335ef5ae24e488c.gif "34") |  Makes the chroma high, increases contrast, and makes the basic color dark.  |
| BlendLinearBurn| ![35](https://mod-file.dn.nexoncdn.co.kr/bbs/1668426133282cddb3c0ee4fe4191bfecf3739a8c0677.gif "35") | Keeps the color contrast of the basic sprite and makes the color dark.   |
| BlendDarkerColor | ![36](https://mod-file.dn.nexoncdn.co.kr/bbs/1668426170398e831fc11796e4fafbd329e11a1744bad.gif "36") |  Applies the darker color between the **Color** and the sprite.  |
| BlendLighten | ![37](https://mod-file.dn.nexoncdn.co.kr/bbs/16684261855583ea181a43a0c4925b6212596777e1fd5.gif "37") |  Makes it bright by using the larger value of each RGB channel.  |
| BlendScreen | ![38](https://mod-file.dn.nexoncdn.co.kr/bbs/1668426201094510bec9a701d434fa499d049c9506565.gif "38") | Makes it bright by multiplying the mixed color and the inverted color of the basic color.   |
| BlendColorDodge | ![39](https://mod-file.dn.nexoncdn.co.kr/bbs/1668426214619ccd1f984bfac4ff8b6c62e2cc92bfabe.gif "39")   | Makes the chroma high, increases contrast, and makes it bright. |
| BlendLinearDodge | ![40](https://mod-file.dn.nexoncdn.co.kr/bbs/166842622931445eada41c5154dee92f5b5944142249f.gif "40")   | Makes it bright by adding two colors. |
| BlendOverlay | ![41](https://mod-file.dn.nexoncdn.co.kr/bbs/16684262452622aa6ab0bbb9d432e94226ea8fe61e8e6.gif "41") | Applies the Multiply effect to a dark color and the Screen effect to a bright color. |  
| BlendSoftLight | ![42](https://mod-file.dn.nexoncdn.co.kr/bbs/1668426262251183016eda21c4e07b9e21e6879ca2b3f.gif "42") | It's similar to the Overlay, but has smaller contrast and color difference compared to the Overlay. |
| BlendHardLight | ![43](https://mod-file.dn.nexoncdn.co.kr/bbs/166842627472882eb382dd2184fd88abf98d4ade02ef9.gif "43") | Similar to SoftLight but image contrast is strong and bright.  |
| BlendVividLight | ![44](https://mod-file.dn.nexoncdn.co.kr/bbs/166842629152549952cd63e774a36a4d9f5a3321b2086.gif "44") | Applies the Color Burn effect if the color to blend is dark and applies the Color Dodge effect if the color to blend is bright.  |
| BlendLinearLight | ![44_2](https://mod-file.dn.nexoncdn.co.kr/bbs/1673418964186d00f27d5a60b4bd2939245ab8eb9f498.gif "44_2") | Apply the combination of the LinearBurn and LinearDodge effects. <br>It is similar to VividLight but without increased contrast compared to VividLight. |
| BlendPinLight | ![45](https://mod-file.dn.nexoncdn.co.kr/bbs/1668426308466df7b6151174b4c91bff2ff8069026131.gif "45") | Applies the Darken effect if the color to blend is dark and applies the Lighten effect if the color to blend is bright. |  
| BlendHardMix | ![46](https://mod-file.dn.nexoncdn.co.kr/bbs/1668426325065c2d5f6d9609e4eb3b025e6a01f55aeb8.png "46")   | Mixes two images with strong colors. <br>The blending result is unconditionally displayed in only 8 colors. |
| BlendDifference | ![47](https://mod-file.dn.nexoncdn.co.kr/bbs/166842634679204790bae607048f1b20b6577182fd0b1.gif "47")   | Returns a difference of two colors. <br>If it is the same color, it will be black. |
| BlendExclusion | ![48](https://mod-file.dn.nexoncdn.co.kr/bbs/1668426366592d90b1f754849452d9adb34b139062a36.gif "48")   | Calculates Exclusion for two colors. <br>If it is the same color, it will be grey. In contrast to <br>Difference, the color contrast of the original is weak. |
| BlendSubtract | ![49](https://mod-file.dn.nexoncdn.co.kr/bbs/166842638191501ba6d72ea244bf0b9825d54d2611095.gif "49")   | Returns the result that deducted colors from the original. |
| BlendDivide | ![50](https://mod-file.dn.nexoncdn.co.kr/bbs/1668426394581cb26db187efd4041905dbcd8b20c0465.gif "50") | Returns the result of the color values being divided from the original. |  
| BlendHue | ![51](https://mod-file.dn.nexoncdn.co.kr/bbs/16684264113953037882a6fc040878105a8508905e558.png "51") | Keep the brightness and chroma, and change it so that it fits a new color.<br>Make the original with achromatic colors if a new color is an achromatic color. |  
| BlendSaturation | ![54](https://mod-file.dn.nexoncdn.co.kr/bbs/1668426455903b9564b1fd8b84ac589fc78bf614869c6.png "54")   | Maintains the brightness and tone and changes to fit the chroma of the new color. <br>Make the original with achromatic colors if a new color is an achromatic color. |
| BlendColor | ![52](https://mod-file.dn.nexoncdn.co.kr/bbs/16684264237827483efddb1634d42bf77a1ca0b247de2.png "52") | Maintains the brightness and changes to fit the tone and chroma of the new color. |  

## AlphaBlend
It's the effect that alters the transparency of shaders.

|	@cols=2:@rows=1:Shader			|	Description	|	
| :---: | --- | --- |  
| Additive| ![53](https://mod-file.dn.nexoncdn.co.kr/bbs/1668515969651ddbdb3c1da904a82b6c8184c7347ce38.png "53") | Applies an Additive effect when rendering in front of the object. |
| SoftAdditive| ![55](https://mod-file.dn.nexoncdn.co.kr/bbs/1668515996851bead4bcb2bfe4ceeabae9240247da0f8.png "55") | Applies a SoftAdditive effect when rendering in front of the object.	  |

## Screen
This effect is applied to the entire screen. It can only be applied in **CameraComponent**.

|	@cols=2:@rows=1:Shader			|	Description	|	
| :---: | --- | --- |  
| LensDistortion | ![lensdistortion](https://mod-file.dn.nexoncdn.co.kr/bbs/167896118717416bf94656d5344f18cae0a12cb3def94.png{"width":"300px"} "lensdistortion") <br>![lensdistortion](https://mod-file.dn.nexoncdn.co.kr/bbs/1678961600478271874df2997410684f8ba6c5f80c965.png{"width":"300px"} "lensdistortion") | Applies a lens distortion effect. <br>(Convex lens if intensity is positive number, concave lens if negative number) |
| Vignette | ![vignette](https://mod-file.dn.nexoncdn.co.kr/bbs/1678961508666ec8dd0224f3d431293b7b24a161305f6.png{"width":"300px"} "vignette") |  Applies a colored vignette effect.  |