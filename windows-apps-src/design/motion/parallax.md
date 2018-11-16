---
author: mijacobs
Description: Use the ParallaxView control to add depth and movement to your app.
title: Utilisation du contrôle ParallaxView pour ajouter de la profondeur et du mouvement à votre application.
ms.assetid: ''
label: Parallax View
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: conrwi
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 987202969f22a5b1b34efc9089ea7f7f08282d8c
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6856206"
---
# <a name="parallax"></a>Parallaxe

Le parallaxe est un effet visuel où les objets proches de l'observateur se déplacent plus rapidement que ceux situés en arrière-plan. Parallaxe crée une impression de profondeur, de perspective et de mouvement. Dans une application UWP, vous pouvez utiliser le contrôle ParallaxView pour créer un effet parallaxe.  

> **API importantes**: [classe ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview), [propriété VerticalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift), [propriété HorizontalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/ParallaxView">ouvrez l’application et voir l'effet ParallaxView en action </a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>Le parallaxe et le système Fluent Design

 Le système de conception Fluent vous aide à créer une interface utilisateur moderne et audacieuse qui incorpore la lumière, la profondeur, le mouvement, les matières et la notion d'échelle. Le parallaxe est un composant du système Fluent Design qui ajoute du mouvement, de la profondeur et la notion d'échelle à votre application. Pour plus d’informations, voir [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md).

## <a name="how-it-works-in-a-user-interface"></a>Fonctionnement dans une interface utilisateur

Dans une interface utilisateur, vous pouvez créer un effet parallaxe en déplaçant les objets à différentes vitesses lorsque vous faites défiler ou que vous développez l'interface utilisateur. <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> Pour illustrer cette idée, examinons deux couches de contenu, une liste et une image d’arrière-plan.  La liste est placée au-dessus de l’image d’arrière-plan, ce qui donne déjà l'impression que celle-ci est plus proche de l'utilisateur.  Maintenant, pour obtenir l’effet parallaxe, nous voulons que l’objet le plus proche de nous se déplace «plus rapidement» que l’objet plus éloigné.  Lorsque l’utilisateur fait défiler l’interface, la liste se déplace à un rythme plus rapide que l’image d’arrière-plan, ce qui crée l’illusion de profondeur.

 ![Exemple de parallaxe avec une liste et une image d’arrière-plan](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>Utiliser le contrôle ParallaxView pour créer un effet parallaxe

Pour créer un effet parallaxe, vous devez utiliser le contrôle [ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview). Ce contrôle relie la position de défilement d’un élément au premier plan, par exemple une liste, à un élément en arrière-plan, comme une image. Tandis que vous faites défiler l’élément au premier plan, celui-ci anime l’élément en arrière-plan pour créer un effet parallaxe. 

Pour utiliser le contrôle ParallaxView, vous devez fournir un élément Source, un élément en arrière-plan et les propriétés [VerticalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift) (pour le défilement vertical) et/ou [HorizontalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift) (pour le défilement horizontal) sur une valeur supérieure à zéro. 
* La propriété Source fait référence à l’élément au premier plan. Pour que l’effet parallaxe se produise, le premier plan doit être un [ScrollViewer](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) ou un élément qui contient un ScrollViewer, comme un [ListView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.listview) ou un [RichTextBox](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.RichEditBox). 

* Pour définir l’élément en arrière-plan, vous devez ajouter cet élément en tant qu’enfant au contrôle ParallaxView. L’élément en arrière-plan peut être n'importe quel [UIElement](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement), comme une [Image](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Image) ou un panneau qui contient les éléments d’interface utilisateur supplémentaires. 

Pour créer un effet parallaxe, l'élément ParallaxView doit être situé derrière l’élément au premier plan. Les panneaux [Grille](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.grid) et [Canevas](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.canvas) vous permettent de superposer les éléments les uns sur les autres, afin qu'ils fonctionnent efficacement avec le contrôle ParallaxView.  

Cet exemple crée un effet parallaxe pour une liste:
 
```xaml
<Grid>
    <ParallaxView Source="{x:Bind ForegroundElement}" VerticalShift="50"> 
    
        <!-- Background element --> 
        <Image x:Name="BackgroundImage" Source="Assets/turntable.png"
               Stretch="UniformToFill"/>
    </ParallaxView>
    
    <!-- Foreground element -->
    <ListView x:Name="ForegroundElement">
       <x:String>Item 1</x:String> 
       <x:String>Item 2</x:String> 
       <x:String>Item 3</x:String> 
       <x:String>Item 4</x:String> 
       <x:String>Item 5</x:String>  
       <x:String>Item 6</x:String> 
       <x:String>Item 7</x:String> 
       <x:String>Item 8</x:String> 
       <x:String>Item 9</x:String> 
       <x:String>Item 10</x:String>     
       <x:String>Item 11</x:String> 
       <x:String>Item 13</x:String> 
       <x:String>Item 14</x:String> 
       <x:String>Item 15</x:String> 
       <x:String>Item 16</x:String>     
       <x:String>Item 17</x:String> 
       <x:String>Item 18</x:String> 
       <x:String>Item 19</x:String> 
       <x:String>Item 20</x:String> 
       <x:String>Item 21</x:String>        
    </ListView>
</Grid>
``` 

ParallaxView ajuste automatiquement la taille de l’image pour qu’elle fonctionne avec l’opération de parallaxe, afin que vous n'ayez pas à vous soucier que l’image défile hors de la vue.

## <a name="customizing-the-parallax-effect"></a>Personnalisation de l’effet parallaxe 

Les propriétés VerticalShift et HorizontalShift vous permettent de contrôler le degré de l’effet parallaxe.

* La propriété VerticalShift spécifie le degré de rotation verticale de l'arrière-plan pendant toute l'opération parallaxe. Une valeur de 0 signifie que l’arrière-plan ne se déplace pas du tout.
* La propriété HorizontalShift spécifie le degré de rotation horizontale de l'arrière-plan pendant toute l'opération parallaxe. Une valeur de 0 signifie que l’arrière-plan ne se déplace pas du tout.

Les valeurs élevées créent un effet plus important. 

Pour obtenir la liste complète des méthodes de personnalisation de l'effet parallaxe, consultez la classe ParallaxView. 

## <a name="dos-and-donts"></a>À faire et à ne pas faire

- Utilisez l’effet parallaxe dans les listes dotées d'une image d’arrière-plan
- Envisagez d’utiliser parallaxe dans ListViewItems lorsque ListViewItems contient une image
- N’utilisez pas cet effet partout, car une utilisation excessive peut réduire son impact

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Classe ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [Fluent Design pour UWP](../fluent-design-system/index.md)
- [De la science dans le système: Fluent Design et la Profondeur](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
