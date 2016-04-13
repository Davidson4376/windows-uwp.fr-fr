---
Générer le rendu de la scène avec un test de profondeur
Créez un effet d’ombre en ajoutant un test de profondeur à votre nuanceur de vertex (ou géométrie) et votre nuanceur de pixels.
ms.assetid: bf496dfb-d7f5-af6b-d588-501164608560
---

# Générer le rendu de la scène avec un test de profondeur


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Créez un effet d’ombre en ajoutant un test de profondeur à votre nuanceur de vertex (ou géométrie) et votre nuanceur de pixels. Partie 3 de la [Procédure pas à pas : implémenter des volumes d’ombre à l’aide de tampons de profondeur dans Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## Inclure une transformation pour un tronc de cône lumineux


Votre nuanceur de vertex a besoin de calculer la position transformée de l’espace lumineux pour chaque vertex. Indiquez le modèle d’espace lumineux, l’affichage et les matrices de projection à l’aide d’un tampon constant. Vous pouvez également utiliser ce tampon constant pour indiquer la position et la normale de la lumière pour les calculs d’éclairage. La position transformée dans l’espace lumineux est utilisée lors du test de profondeur.

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    float4 modelPos = mul(pos, model);
    pos = mul(modelPos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    // Transform the vertex position into projected space from the POV of the light.
    float4 lightSpacePos = mul(modelPos, lView);
    lightSpacePos = mul(lightSpacePos, lProjection);
    output.lightSpacePos = lightSpacePos;

    // Light ray
    float3 lRay = lPos.xyz - modelPos.xyz;
    output.lRay = lRay;
    
    // Camera ray
    output.view = eyePos.xyz - modelPos.xyz;

    // Transform the vertex normal into world space.
    float4 norm = float4(input.norm, 1.0f);
    norm = mul(norm, model);
    output.norm = norm.xyz;
    
    // Pass through the color and texture coordinates without modification.
    output.color = input.color;
    output.tex = input.tex;

    return output;
}
```

Ensuite, le nuanceur de pixels utilise la position de l’espace lumineux interpolée indiquée par le nuanceur de vertex pour tester si le pixel est dans l’ombre.

## Tester si la position est dans le tronc de cône lumineux


Tout d’abord, vérifiez que le pixel se trouve dans le tronc de cône de l’affichage de la lumière en normalisant les coordonnées X et Y. Si les deux se trouvent dans la plage \[0, 1\], alors il est possible que le pixel soit dans l’ombre. Sinon, vous pouvez ignorer le test de profondeur. Un nuanceur peut le tester rapidement en appelant [Saturate](https://msdn.microsoft.com/library/windows/desktop/hh447231) et en comparant le résultat à la valeur d’origine.

```cpp
// Compute texture coordinates for the current point's location on the shadow map.
float2 shadowTexCoords;
shadowTexCoords.x = 0.5f + (input.lightSpacePos.x / input.lightSpacePos.w * 0.5f);
shadowTexCoords.y = 0.5f - (input.lightSpacePos.y / input.lightSpacePos.w * 0.5f);
float pixelDepth = input.lightSpacePos.z / input.lightSpacePos.w;

float lighting = 1;

// Check if the pixel texture coordinate is in the view frustum of the 
// light before doing any shadow work.
if ((saturate(shadowTexCoords.x) == shadowTexCoords.x) &&
    (saturate(shadowTexCoords.y) == shadowTexCoords.y) &&
    (pixelDepth > 0))
{
```

## Test de profondeur par rapport au mappage d’ombre


Utilisez une fonction de comparaison d’exemples (soit [SampleCmp](https://msdn.microsoft.com/library/windows/desktop/bb509696), soit [SampleCmpLevelZero](https://msdn.microsoft.com/library/windows/desktop/bb509697)) pour tester la profondeur du pixel dans l’espace lumineux par rapport au mappage de profondeur. Calculez la valeur de la profondeur de l’espace lumineux normalisé, à savoir `z / w`, puis passez la valeur à la fonction de comparaison. Puisque nous utilisons un test de comparaison LessOrEqual pour l’échantillon, la fonction intrinsèque renvoie zéro quand le test de comparaison est satisfaisant ; cela indique que le pixel est dans l’ombre.

```cpp
// Use an offset value to mitigate shadow artifacts due to imprecise 
// floating-point values (shadow acne).
//
// This is an approximation of epsilon * tan(acos(saturate(NdotL))):
float margin = acos(saturate(NdotL));
#ifdef LINEAR
// The offset can be slightly smaller with smoother shadow edges.
float epsilon = 0.0005 / margin;
#else
float epsilon = 0.001 / margin;
#endif
// Clamp epsilon to a fixed range so it doesn't go overboard.
epsilon = clamp(epsilon, 0, 0.1);

// Use the SampleCmpLevelZero Texture2D method (or SampleCmp) to sample from 
// the shadow map, just as you would with Direct3D feature level 10_0 and
// higher.  Feature level 9_1 only supports LessOrEqual, which returns 0 if
// the pixel is in the shadow.
lighting = float(shadowMap.SampleCmpLevelZero(
    shadowSampler,
    shadowTexCoords,
    pixelDepth + epsilon
    )
    );
```

## Calculer l’éclairage avec ou sans l’ombre


Si le pixel n’est pas dans l’ombre, le nuanceur de pixels doit calculer l’éclairage direct et l’ajouter à la valeur du pixel.

```cpp
return float4(input.color * (ambient + DplusS(N, L, NdotL, input.view)), 1.f);
```

```cpp
float3 DplusS(float3 N, float3 L, float NdotL, float3 view)
{
    const float3 Kdiffuse = float3(.5f, .5f, .4f);
    const float3 Kspecular = float3(.2f, .2f, .3f);
    const float exponent = 3.f;

    // Compute the diffuse coefficient.
    float diffuseConst = saturate(NdotL);

    // Compute the diffuse lighting value.
    float3 diffuse = Kdiffuse * diffuseConst;

    // Compute the specular highlight.
    float3 R = reflect(-L, N);
    float3 V = normalize(view);
    float3 RdotV = dot(R, V);
    float3 specular = Kspecular * pow(saturate(RdotV), exponent);

    return (diffuse + specular);
}
```

Sinon, le nuanceur de pixels doit calculer la valeur du pixel à l’aide de l’éclairage ambiant.

```cpp
return float4(input.color * ambient, 1.f);
```

Dans la partie suivante de cette procédure pas à pas, découvrez comment [prendre en charge les mappages d’ombre sur une gamme de matériel](target-a-range-of-hardware.md).

 

 






<!--HONumber=Mar16_HO1-->


