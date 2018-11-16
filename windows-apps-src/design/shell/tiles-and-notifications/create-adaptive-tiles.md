---
author: andrewleader
Description: Adaptive tile templates are a new feature in Windows 10, allowing you to design your own tile notification content using a simple and flexible markup language that adapts to different screen densities.
title: Créer des vignettes adaptatives
ms.assetid: 1246B58E-D6E3-48C7-AD7F-475D113600F9
label: Create adaptive tiles
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f0af7dc153f75826444a517d4958bfeba53b103d
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6972447"
---
# <a name="create-adaptive-tiles"></a>Créer des vignettes adaptatives

Modèles de vignette adaptative sont une nouvelle fonctionnalité dans Windows 10, ce qui vous permet de concevoir votre propre contenu de notification par vignette à l’aide d’un langage de balisage simple et flexible qui s’adapte à différentes densités d’écran. Cet article vous indique comment créer des vignettes dynamiques adaptatives pour votre application de plateforme Windows universelle (UWP). Pour obtenir la liste complète des éléments et attributs adaptatifs, voir [Schéma des vignettes adaptatives](../tiles-and-notifications/tile-schema.md).

(Si vous le souhaitez, vous pouvez toujours utiliser les modèles prédéfinis du [catalogue de modèles de vignette de package Windows8](https://msdn.microsoft.com/library/windows/apps/hh761491) lors de la conception de notifications pour Windows 10.)


## <a name="getting-started"></a>Prise en main

**Installez la bibliothèque Notifications.** Si vous préférez utiliserC# plutôt queXML pour générer les notifications, installez le packageNuGet [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (recherchez «notifications uwp». Les exemples de code C# indiqués dans cet article utilisent la version1.0.0 du package NuGet.

**Installez Notifications Visualizer.** Cette application UWP gratuite vous permet de concevoir des vignettes dynamiques adaptatives en fournissant un aperçu visuel instantané de votre vignette lorsque vous la modifiez, comparable au mode Création/Éditeur XAML de VisualStudio. Consultez [Notifications Visualizer](notifications-visualizer.md) pour plus d’informations, ou [téléchargez Notifications Visualizer à partir du Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="how-to-send-a-tile-notification"></a>Comment envoyer une notification par vignette

Lisez notre [Démarrage rapide sur l’envoi de notifications par vignette locales](sending-a-local-tile-notification.md). La documentation ci-dessous explique toutes les possibilités de l’interface utilisateur visuelle avec les vignettes adaptatives.


## <a name="usage-guidance"></a>Conseils d’utilisation


Les modèles adaptatifs sont conçus pour fonctionner dans différents facteurs de forme et types de notification. Les éléments tels qu’un groupe ou un sous-groupe, relient du contenu et n’impliquent pas de comportement visuel particulier. L’apparence finale d’une notification varie en fonction de l’appareil sur lequel elle s’affiche, selon qu’il s’agit d’un téléphone, d’une tablette, d’un ordinateur de bureau ou d’un autre appareil.

Les indications sont des attributs facultatifs qui peuvent être ajoutés à des éléments afin d’obtenir un comportement visuel spécifique. Elles peuvent être propres à un appareil ou à une notification.

## <a name="a-basic-example"></a>Exemple de base


Cet exemple montre ce que les modèles de vignette adaptative peuvent produire.

```xml
<tile>
  <visual>
  
    <binding template="TileMedium">
      ...
    </binding>
  
    <binding template="TileWide">
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </binding>
  
    <binding template="TileLarge">
      ...
    </binding>
  
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = ...
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Jennifer Parker",
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Photos from our trip",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Check out these awesome photos I took while in New Zealand!",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },
  
        TileLarge = ...
    }
};
```

**Résultat:**

![exemple de vignette rapide](images/adaptive-tiles-quicksample.png)

## <a name="tile-sizes"></a>Tailles des vignettes


Le contenu de chaque taille de vignette est spécifié individuellement dans des éléments [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) distincts au sein de la charge utile XML. Choisissez la taille cible en définissant l’attribut de modèle sur l’une des valeurs suivantes:

-   TileSmall
-   TileMedium
-   TileWide
-   TileLarge (uniquement pour les ordinateurs de bureau)

Pour une charge utile XML de notification par vignette, indiquez des éléments &lt;binding&gt; pour chaque taille de vignette que vous souhaitez prendre en charge, comme illustré dans cet exemple :

```xml
<tile>
  <visual>
  
    <binding template="TileSmall">
      <text>Small</text>
    </binding>
  
    <binding template="TileMedium">
      <text>Medium</text>
    </binding>
  
    <binding template="TileWide">
      <text>Wide</text>
    </binding>
  
    <binding template="TileLarge">
      <text>Large</text>
    </binding>
  
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileSmall = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Small" }
                }
            }
        },
  
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Medium" }
                }
            }
        },
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Wide" }
                }
            }
        },
  
        TileLarge = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Large" }
                }
            }
        }
    }
};
```

**Résultat:**

![tailles de vignettes adaptatives: petite, moyenne, large et grande](images/adaptive-tiles-sizes.png)

## <a name="branding"></a>Personnalisation


Vous pouvez contrôler la personnalisation en bas d’une vignette dynamique (nom d’affichage et logo d’angle) à l’aide de l’attribut branding de la charge utile de notification. Vous pouvez choisir de ne rien afficher (valeur «none»), d’afficher uniquement le nom (valeur «name»), d’afficher uniquement le logo (valeur «logo») ou d’afficher à la fois le nom et le logo (valeur «nameAndLogo»).

**Remarque**Windows Mobile ne prend pas en charge le logo d’angle, la «logo» c’est le cas et la valeur par défaut «nameandlogo» sont sur «name» sur un appareil Mobile.

 

```xml
<visual branding="logo">
  ...
</visual>
```

```csharp
new TileVisual()
{
    Branding = TileBranding.Logo,
    ...
}
```

**Résultat:**

![Vignettes adaptatives, nom et logo](images/adaptive-tiles-namelogo.png)

Vous pouvez appliquer une personnalisation à des tailles de vignettes spécifiques en procédant de l’une des deux manières suivantes:

1. En appliquant l’attribut sur l’élément [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding)
2. En appliquant l’attribut sur l’élément [TileVisual](../tiles-and-notifications/tile-schema.md#tilevisual) (qui affecte toute la charge utile de notification si vous ne spécifiez pas de personnalisation pour une liaison), la personnalisation fournie sur l’élément visuel est utilisée.

```xml
<tile>
  <visual branding="nameAndLogo">
 
    <binding template="TileMedium" branding="logo">
      ...
    </binding>
 
    <!--Inherits branding from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,

        TileMedium = new TileBinding()
        {
            Branding = TileBranding.Logo,
            ...
        },

        // Inherits branding from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**Résultat de la personnalisation par défaut:**

![Personnalisation par défaut des vignettes](images/adaptive-tiles-defaultbranding.png)

Si vous ne spécifiez aucune personnalisation dans votre charge utile de notification, les propriétés de base de la vignette déterminent la personnalisation. Si la vignette de base indique le nom d’affichage, alors la personnalisation est définie par défaut sur « name ». Si le nom d’affichage n’est pas indiqué, la personnalisation est définie par défaut sur «none».

**Remarque**  il s’agit d’un changement de Windows8.x, dans lequel la personnalisation par défaut était définie sur «logo».

 

## <a name="display-name"></a>Nom d’affichage


Vous pouvez remplacer le nom d’affichage d’une notification en entrant la chaîne de texte de votre choix avec l’attribut **displayName**. Comme pour la personnalisation, vous pouvez spécifier cela dans l’élément [TileVisual](../tiles-and-notifications/tile-schema.md#tilevisual), ce qui affecte toute la charge utile de notification. Vous pouvez également spécifier cela dans l’élément [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding), ce qui affecte uniquement les vignettes individuelles.

**Problème connu** Sur WindowsMobile, si vous spécifiez une ShortName pour votre vignette, le nom d’affichage fourni dans votre notification ne sera pas utilisé (seule la chaîne ShortName sera toujours affichée). 

```xml
<tile>
  <visual branding="nameAndLogo" displayName="Wednesday 22">
 
    <binding template="TileMedium" displayName="Wed. 22">
      ...
    </binding>
 
    <!--Inherits displayName from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,
        DisplayName = "Wednesday 22",

        TileMedium = new TileBinding()
        {
            DisplayName = "Wed. 22",
            ...
        },

        // Inherits DisplayName from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**Résultat:**

![Nom d’affichage des vignettes adaptatives](images/adaptive-tiles-displayname.png)

## <a name="text"></a>Texte


L’élément [AdaptiveText](../tiles-and-notifications/tile-schema.md#adaptivetext) permet d’afficher un texte. Vous pouvez utiliser des indications pour en modifier l’apparence.

```xml
<text>This is a line of text</text>
```


```csharp
new AdaptiveText()
{
    Text = "This is a line of text"
};
```

**Résultat:**

![Texte de vignette adaptative](images/adaptive-tiles-text.png)

## <a name="text-wrapping"></a>Habillage du texte


Par défaut, le texte n’est pas renvoyé à la ligne et déborde de la vignette. Utilisez l’attribut **hint-wrap** pour définir le renvoi de texte à la ligne sur un élément de texte. Vous pouvez également contrôler les nombres minimal et maximal de lignes à l’aide des éléments **hint-minLines** et **hint-maxLines**, qui acceptent des entiers positifs.

```xml
<text hint-wrap="true">This is a line of wrapping text</text>
```


```csharp
new AdaptiveText()
{
    Text = "This is a line of wrapping text",
    HintWrap = true
};
```

**Résultat:**

![Vignette adaptative avec renvoi de texte à la ligne](images/adaptive-tiles-textwrapping.png)

## <a name="text-styles"></a>Styles de texte


Les styles contrôlent la taille de police, la couleur et l’épaisseur des éléments de texte. Plusieurs styles sont disponibles, notamment une variation « subtile » de chaque style qui définit l’opacité sur 60 %, ce qui transforme généralement la couleur du texte en nuance de gris clair.

```xml
<text hint-style="base">Header content</text>
<text hint-style="captionSubtle">Subheader content</text>
```

```csharp
new AdaptiveText()
{
    Text = "Header content",
    HintStyle = AdaptiveTextStyle.Base
},

new AdaptiveText()
{
    Text = "Subheader content",
    HintStyle = AdaptiveTextStyle.CaptionSubtle
}
```

**Résultat:**

![Styles de texte des vignettes adaptatives](images/adaptive-tiles-textstyles.png)

**Remarque**le style par défaut, légende si hint-style n’est pas spécifié.

 

**Styles de texte de base**

|                                |                           |             |
|--------------------------------|---------------------------|-------------|
| &lt;text hint-style="\*" /&gt; | Hauteur de police               | Épaisseur de police |
| caption                        | 12 pixels effectifs (epx) | Normale     |
| body                           | 15 epx                    | Normale     |
| base                           | 15 epx                    | Semibold    |
| subtitle                       | 20 epx                    | Normale     |
| title                          | 24 epx                    | Semilight   |
| subheader                      | 34 epx                    | Maigre       |
| header                         | 46 epx                    | Maigre       |

 

**Variations de style de texte numérique**

Ces variations réduisent la hauteur de ligne afin que le contenu situé au-dessus et au-dessous soit beaucoup plus proche du texte.

|                  |
|------------------|
| titleNumeral     |
| subheaderNumeral |
| headerNumeral    |

 

**Variations de style de texte de sous-titre**

Chaque style dispose d’une variation de sous-titre qui octroie au texte une opacité de 60 %, qui transforme généralement la couleur du texte en nuance de gris clair.

|                        |
|------------------------|
| captionSubtle          |
| bodySubtle             |
| baseSubtle             |
| subtitleSubtle         |
| titleSubtle            |
| titleNumeralSubtle     |
| subheaderSubtle        |
| subheaderNumeralSubtle |
| headerSubtle           |
| headerNumeralSubtle    |

 

## <a name="text-alignment"></a>Alignement du texte


Le texte peut être horizontalement aligné à gauche, au centre ou à droite. Pour les langues qui s’écrivent de gauche à droite, par exemple l’anglais, le texte est aligné à gauche par défaut. Pour les langues qui s’écrivent de droite à gauche, telles que l’arabe, le texte est aligné à droite par défaut. Vous pouvez définir l’alignement manuellement en utilisant l’attribut **hint-align** dans des éléments.

```xml
<text hint-align="center">Hello</text>
```


```csharp
new AdaptiveText()
{
    Text = "Hello",
    HintAlign = AdaptiveTextAlign.Center
};
```

**Résultat:**

![Alignement du texte des vignettes adaptatives](images/adaptive-tiles-textalignment.png)

## <a name="groups-and-subgroups"></a>Groupes et sous-groupes


Les groupes vous permettent de déclarer au niveau sémantique que le contenu à l’intérieur du groupe est lié et qu’il doit être affiché dans son intégralité pour qu’il ait du sens. Par exemple, vous pouvez avoir deux éléments de texte, un en-tête et un sous-titre, et il ne serait pas logique de n’afficher que l’en-tête. En regroupant ces éléments à l’intérieur d’un sous-groupe, les éléments sont tous affichés (s’ils peuvent s’ajuster à la vignette) ou pas affichés du tout (s’ils ne peuvent pas s’ajuster à la vignette).

Pour que l’expérience soit la meilleure possible sur les appareils et les écrans, indiquez plusieurs groupes. L’indication de plusieurs groupes permet à votre vignette de s’adapter aux écrans plus grands.

**Remarque**le seul enfant valide d’un groupe est un sous-groupe.

 

```xml
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup>
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </subgroup>
  </group>
 
  <text />
 
  <group>
    <subgroup>
      <text hint-style="subtitle">Steve Bosniak</text>
      <text hint-style="captionSubtle">Build 2015 Dinner</text>
      <text hint-style="captionSubtle">Want to go out for dinner after Build tonight?</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            CreateGroup(
                from: "Jennifer Parker",
                subject: "Photos from our trip",
                body: "Check out these awesome photos I took while in New Zealand!"),

            // For spacing
            new AdaptiveText(),

            CreateGroup(
                from: "Steve Bosniak",
                subject: "Build 2015 Dinner",
                body: "Want to go out for dinner after Build tonight?")
        }
    }
}

...

private static AdaptiveGroup CreateGroup(string from, string subject, string body)
{
    return new AdaptiveGroup()
    {
        Children =
        {
            new AdaptiveSubgroup()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from,
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },
                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },
                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        }
    };
}
```

**Résultat:**

![Groupes et sous-groupes de vignettes adaptatives](images/adaptive-tiles-groups-subgroups.png)

## <a name="subgroups-columns"></a>Sous-groupes (colonnes)


Les sous-groupes vous permettent également de répartir les données en sections sémantiques au sein d’un groupe. Pour les vignettes dynamiques, cela se traduit visuellement par des colonnes.

L’attribut **hint-weight** vous permet de contrôler la largeur des colonnes. La valeur de l’attribut **hint-weight** est exprimée sous forme de pondération de l’espace disponible, qui est identique au comportement **GridUnitType.Star**. Pour obtenir des colonnes de largeur égale, définissez chaque pondération sur 1.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Pourcentage de largeur</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="even">
<td align="left">Pondération totale: 4</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![Sous-groupes avec des colonnes égales](images/adaptive-tiles-subgroups01.png)

Pour qu’une colonne soit deux fois plus large qu’une autre colonne, attribuez la pondération 1 à la colonne la moins large, et la pondération 2 à la colonne la plus large.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Pourcentage de largeur</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">33,3 %</td>
</tr>
<tr class="odd">
<td align="left">2</td>
<td align="left">66,7 %</td>
</tr>
<tr class="even">
<td align="left">Pondération totale: 3</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![Sous-groupes avec une colonne deux fois plus large que l’autre](images/adaptive-tiles-subgroups02.png)

Si vous souhaitez que les première et seconde colonnes occupent respectivement 20% et 80% de la largeur totale, définissez la première pondération sur 20 et la seconde sur 80. Si la totalité des pondérations est égale à 100, elles correspondent à des pourcentages.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Pourcentage de largeur</td>
</tr>
<tr class="even">
<td align="left">20</td>
<td align="left">20 %</td>
</tr>
<tr class="odd">
<td align="left">80</td>
<td align="left">80 %</td>
</tr>
<tr class="even">
<td align="left">Pondération totale: 100</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![Sous-groupes avec des pondérations totalisant 100](images/adaptive-tiles-subgroups03.png)

**Remarque**une marge de 8 pixels est automatiquement ajoutée entre les colonnes.

 

Si vous disposez de plus de deux sous-groupes, vous devez spécifier l’attribut **hint-weight**, qui accepte uniquement des entiers positifs. Si vous ne spécifiez pas l’attribut hint-weight pour le premier sous-groupe, la pondération 50 lui est attribuée. Le sous-groupe suivant pour lequel aucun attribut hint-weight n’est spécifié, se voit attribuer une pondération égal à 100 moins la somme des pondérations précédentes, ou une pondération égale à 1 si le résultat est zéro. La pondération 1 est attribué aux sous-groupes restants pour lesquels aucun attribut hint-weight n’est attribué.

Voici un exemple de code pour une vignette météo, qui montre comment obtenir une vignette avec cinq colonnes de largeur égale :

```xml
<binding template="TileWide" displayName="Seattle" branding="name">
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Tue</text>
      <image src="Assets\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-align="center" hint-style="captionsubtle">38°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Wed</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">59°</text>
      <text hint-align="center" hint-style="captionsubtle">43°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Thu</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">62°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Fri</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">71°</text>
      <text hint-align="center" hint-style="captionsubtle">66°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°"),
                    CreateSubgroup("Wed", "Sunny.png", "59°", "43°"),
                    CreateSubgroup("Thu", "Sunny.png", "62°", "42°"),
                    CreateSubgroup("Fri", "Sunny.png", "71°", "66°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Résultat:**

![Exemple d’une vignette météo](images/adaptive-tiles-weathertile.png)

## <a name="images"></a>Images


L’élément &lt;image&gt; permet d’afficher des images dans la notification par vignette. Les images peuvent être incorporées au sein du contenu de la vignette (par défaut), en tant qu’image d’arrière-plan derrière le contenu ou en tant qu’image furtive qui s’anime à partir du haut de la notification.

> [!NOTE]
> Des images peuvent être utilisées à partir du package de l’application, du stockage local de l’application ou du web. À partir de Fall Creators Update, les images Web peuvent atteindre 3Mo sur les connexions normales et 1Mo sur les connexions limitées. Sur les appareils qui n’exécutent pas encore Fall Creators Update, les images web ne doivent pas dépasser 200Ko.

 

Si aucun comportement supplémentaire n’est spécifié, les images sont uniformément réduites ou développées pour remplir la largeur disponible. Cet exemple présente une vignette utilisant deux colonnes et des images incorporées. Les images incorporées sont étirées pour remplir la largeur de la colonne.

```xml
<binding template="TileMedium" displayName="Seattle" branding="name">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Apps\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Apps\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionSubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°")
                }
            }
        }
    }
}
...
private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Résultat:**

![Exemple d’image](images/adaptive-tiles-images01.png)

Les images placées à la racine &lt;binding&gt; ou dans le premier groupe sont également étirées pour s’ajuster à la hauteur disponible.

### <a name="image-alignment"></a>Alignement d’images

Les images peuvent être définies pour s’aligner à gauche, au centre ou à droite à l’aide de l’attribut **hint-align**. Cela entraîne également leur affichage à leur résolution native, et non leur étirement pour remplir la largeur.

```xml
<binding template="TileLarge">
  <image src="Assets/fable.jpg" hint-align="center"/>
</binding>
```

```csharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveImage()
            {
                Source = "Assets/fable.jpg",
                HintAlign = AdaptiveImageAlign.Center
            }
        }
    }
}
```

**Résultat:**

![Exemple d’alignement d’images (à gauche, au centre, à droite)](images/adaptive-tiles-imagealignment.png)

### <a name="image-margins"></a>Marges d’images

Par défaut, une marge de 8pixels est insérée dans les images incluses entre le contenu et l’image située au-dessus ou au-dessous. Cette marge peut être supprimée à l’aide de l’attribut **hint-removeMargin** dans l’image. Toutefois, les images conservent toujours la marge de 8pixels à partir du bord de la vignette, et les sous-groupes (colonnes) conservent toujours le remplissage de 8pixels entre les colonnes.

```xml
<binding template="TileMedium" branding="none">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Numbers\4.jpg" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Numbers\3.jpg" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionsubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.None,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "4.jpg", "63°", "42°"),
                    CreateSubgroup("Tue", "3.jpg", "57°", "38°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Numbers/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

![Exemple d’indication de suppression de marge](images/adaptive-tiles-removemargin.png)

### <a name="image-cropping"></a>Rognage d’images

Les images peuvent être rognées dans un cercle à l’aide de l’attribut **hint-crop**, qui ne prend actuellement en charge que les valeurs «none» (par défaut) ou «circle».

```xml
<binding template="TileLarge" hint-textStacking="center">
  <group>
    <subgroup hint-weight="1"/>
    <subgroup hint-weight="2">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-weight="1"/>
  </group>
 
  <text hint-style="title" hint-align="center">Hi,</text>
  <text hint-style="subtitleSubtle" hint-align="center">MasterHip</text>
</binding>
```

```csharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    new AdaptiveSubgroup() { HintWeight = 1 },
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 2,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },
                    new AdaptiveSubgroup() { HintWeight = 1 }
                }
            },
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Title,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.SubtitleSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

**Résultat:**

![exemple de rognage d’image](images/adaptive-tiles-imagecropping.png)

### <a name="background-image"></a>Image d’arrière-plan

Pour définir une image d’arrière-plan, placez un élément image à la racine de &lt;binding&gt; et définissez l’attribut placement sur « background ».

```xml
<binding template="TileWide">
  <image src="Assets\Mostly Cloudy-Background.jpg" placement="background"/>
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    ...
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg"
        },

        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°")
                    ...
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Résultat:**

![exemple d’image d’arrière-plan](images/adaptive-tiles-backgroundimage.png)

### <a name="peek-image"></a>Image furtive

Vous pouvez spécifier une image qui défile furtivement à partir du haut de la vignette. L’image furtive utilise une animation qui glisse vers le bas/haut à partir du haut de la vignette, puis rebascule pour afficher le contenu principal de la vignette. Pour définir une image furtive, placez un élément image à la racine de &lt;binding&gt; et définissez l’attribut placement sur « peek ».

```xml
<binding template="TileMedium" branding="name">
  <image placement="peek" src="Assets/Apps/Hipstame/hipster.jpg"/>
  <text>New Message</text>
  <text hint-style="captionsubtle" hint-wrap="true">Hey, have you tried Windows 10 yet?</text>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Apps/Hipstame/hipster.jpg"
        },
        Children =
        {
            new AdaptiveText()
            {
                Text = "New Message"
            },
            new AdaptiveText()
            {
                Text = "Hey, have you tried Windows 10 yet?",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintWrap = true
            }
        }
    }
}
```

![Exemples d’images furtives](images/adaptive-tiles-imagepeeking.png)

**Rognage d’images furtives et d’arrière-plan dans un cercle**

Pour rogner les images furtives et d’arrière-plan dans un cercle, utilisez l’attribut suivant:

```xml
<image placement="peek" hint-crop="circle" src="Assets/Apps/Hipstame/hipster.jpg"/>
```

```csharp
new TilePeekImage()
{
    HintCrop = TilePeekImageCrop.Circle,
    Source = "Assets/Apps/Hipstame/hipster.jpg"
}
```

Le résultat se présente comme suit:

![Rognage d’image furtive et d’arrière-plan dans un cercle](images/circlecrop-image.png)

**Utilisation d’une image furtive et d’une image d’arrière-plan simultanément**

Pour utiliser une image furtive et une image d’arrière-plan sur une notification par vignette, spécifiez ces deux images dans la charge utile de notification.

Le résultat présentera l’aspect suivant:

![Utilisation combinée d’une image furtive et d’une image d’arrière-plan](images/peekandbackground.png)


### <a name="peek-and-background-image-overlays"></a>Superpositions d’une image furtive et d’une image d’arrière-plan

Vous pouvez définir une superposition noire sur vos images furtives et d’arrière-plan à l’aide de l’attribut **hint-overlay**, qui accepte des entiers compris entre 0 et 100, 0 correspondant à l’absence de superposition et 100 à une superposition noire complète. Vous pouvez utiliser la superposition pour vérifier que le texte sur votre vignette est lisible.

**Utilisation de l’attribut hint-overlay sur une image d’arrière-plan**

Votre image d’arrière-plan adopte par défaut une superposition à 20% tant que votre charge utile contient des éléments de texte (la superposition par défaut est de 0%).

```xml
<binding template="TileWide">
  <image placement="background" hint-overlay="60" src="Assets\Mostly Cloudy-Background.jpg"/>
  ...
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg",
            HintOverlay = 60
        },

        ...
    }
}
```

**Résultat de l’application de l’attribut hint-overlay:**

![Exemple d’image avec superposition](images/adaptive-tiles-image-hintoverlay.png)

**Utilisation de l’attribut hint-overlay sur une image furtive**

La version1511 de Windows10 prend en charge une superposition de votre image furtive, comme de votre image d’arrière-plan. Spécifiez l’attribut hint-overlay sur l’image furtive sous la forme d’un nombre entier compris entre0 et100. La superposition par défaut des images furtives est de0 (aucune superposition).

```xml
<binding template="TileMedium">
  <image hint-overlay="20" src="Assets\Map.jpg" placement="peek"/>
  ...
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Map.jpg",
            HintOverlay = 20
        },
        ...
    }
}
```

Cet exemple montre une image furtive avec une opacité de 20% (à gauche) et une opacité de 0% (à droite):

![Attribut hint-overlay sur une image furtive](images/hintoverlay.png)

## <a name="vertical-alignment-text-stacking"></a>Alignement vertical (empilement de texte)


Vous pouvez contrôler l’alignement vertical du contenu sur votre vignette à l’aide de l’attribut **hint-textStacking** à la fois sur l’élément [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) et sur l’élément [AdaptiveSubgroup](../tiles-and-notifications/tile-schema.md#adaptivesubgroup). Par défaut, tous les éléments sont alignés verticalement vers le haut, mais vous pouvez également aligner le contenu vers le bas ou le centre.

### <a name="text-stacking-on-binding-element"></a>Empilement de texte sur l’élément de liaison

Lorsqu’il est appliqué au niveau [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding), l’empilement de texte définit l’alignement vertical du contenu de la notification dans son ensemble, en s’alignant dans l’espace vertical disponible au-dessus de la zone de personnalisation/badge.

```xml
<binding template="TileMedium" hint-textStacking="center" branding="logo">
  <text hint-style="base" hint-align="center">Hi,</text>
  <text hint-style="captionSubtle" hint-align="center">MasterHip</text>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.Logo,
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Base,
                HintAlign = AdaptiveTextAlign.Center
            },

            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

![Empilement de texte dans l’élément de liaison](images/adaptive-tiles-textstack-bindingelement.png)

### <a name="text-stacking-on-subgroup-element"></a>Empilement de texte dans l’élément de sous-groupe

Lorsqu’il est appliqué au niveau [AdaptiveSubgroup](../tiles-and-notifications/tile-schema.md#adaptivesubgroup), l’empilement de texte définit l’alignement vertical du contenu du sous-groupe (colonne), en s’alignant dans l’espace vertical disponible au sein du groupe tout entier.

```xml
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup hint-weight="33">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-textStacking="center">
      <text hint-style="subtitle">Hi,</text>
      <text hint-style="bodySubtle">MasterHip</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    // Image column
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 33,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },

                    // Text column
                    new AdaptiveSubgroup()
                    {
                        // Vertical align its contents
                        TextStacking = TileTextStacking.Center,
                        Children =
                        {
                            new AdaptiveText()
                            {
                                Text = "Hi,",
                                HintStyle = AdaptiveTextStyle.Subtitle
                            },

                            new AdaptiveText()
                            {
                                Text = "MasterHip",
                                HintStyle = AdaptiveTextStyle.BodySubtle
                            }
                        }
                    }
                }
            }
        }
    }
}
```

## <a name="related-topics"></a>Rubriquesassociées
* [Schéma du contenu de la vignette](../tiles-and-notifications/tile-schema.md)
* [Envoyer une notification par vignette locale](sending-a-local-tile-notification.md)
* [Modèles de vignette spéciaux](special-tile-templates-catalog.md)
* [UWP Community Toolkit - Notifications](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [Notifications Windows sur GitHub](https://github.com/WindowsNotifications)

 

 




