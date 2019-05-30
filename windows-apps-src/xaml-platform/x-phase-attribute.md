---
title: Attribut xPhase
description: Utilisez xPhase avec l’extension de balisage {x:Bind} pour rendre les éléments ListView et GridView de façon incrémentielle et améliorer l’expérience de mouvement panoramique.
ms.assetid: BD17780E-6A34-4A38-8D11-9703107E247E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dc1ec762e5c6f69db608805ac58cfb9469114beb
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372269"
---
# <a name="xphase-attribute"></a>Attribut x:Phase


Utilisez **x:Phase** avec l’[extension de balisage {x:Bind}](x-bind-markup-extension.md) pour rendre les éléments [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) et [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) de façon incrémentielle et améliorer l’expérience de mouvement panoramique. **x:Phase** offre un moyen déclaratif d’obtenir le même effet que l’utilisation de l’événement [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) pour contrôler manuellement le rendu des éléments de liste. Voir aussi [Mettre à jour les éléments ListView et GridView de façon incrémentielle](../debug-test-perf/optimize-gridview-and-listview.md#update-items-incrementally).

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML


``` syntax
<object x:Phase="PhaseValue".../>
```

## <a name="xaml-values"></a>Valeurs XAML


| Terme | Description |
|------|-------------|
| PhaseValue | Valeur numérique indiquant la phase dans laquelle l’élément sera traité. La valeur par défaut est 0. | 

## <a name="remarks"></a>Notes

Si une liste est rapidement panomariquée avec une interaction tactile ou à l’aide de la roulette de la souris, selon la complexité du modèle de données, la liste peut ne pas être pas en mesure de rendre des éléments assez rapidement pour suivre la vitesse de défilement. Cela est particulièrement vrai pour un appareil mobile doté d’un processeur économe en énergie, tel qu’un téléphone ou une tablette.

L’exécution par phases permet un rendu incrémentiel du modèle de données afin que le contenu puisse être hiérarchisé, et les éléments les plus importants rendus en priorité. La liste peut ainsi afficher du contenu pour chaque élément en cas de mouvement panoramique rapide, et rendre davantage d’éléments de chaque modèle si le temps le permet.

## <a name="example"></a>Exemple

```xml
<DataTemplate x:Key="PhasedFileTemplate" x:DataType="model:FileItem">
    <Grid Width="200" Height="80">
        <Grid.ColumnDefinitions>
           <ColumnDefinition Width="75" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Image Grid.RowSpan="4" Source="{x:Bind ImageData}" MaxWidth="70" MaxHeight="70" x:Phase="3"/>
        <TextBlock Text="{x:Bind DisplayName}" Grid.Column="1" FontSize="12"/>
        <TextBlock Text="{x:Bind prettyDate}"  Grid.Column="1"  Grid.Row="1" FontSize="12" x:Phase="1"/>
        <TextBlock Text="{x:Bind prettyFileSize}"  Grid.Column="1"  Grid.Row="2" FontSize="12" x:Phase="2"/>
        <TextBlock Text="{x:Bind prettyImageSize}"  Grid.Column="1"  Grid.Row="3" FontSize="12" x:Phase="2"/>
    </Grid>
</DataTemplate>
```

Le modèle de données décrit 4 phases :

1.  Présente le bloc de texte DisplayName. Tous les contrôles sans phase spécifiée sont implicitement considérés comme faisant partie de la phase de 0.
2.  Affiche le bloc de texte prettyDate.
3.  Affiche les blocs de texte prettyFileSize et prettyImageSize.
4.  Affiche l’image.

L’exécution par phases est une fonctionnalité de [{x:Bind}](x-bind-markup-extension.md) qui opère avec des contrôles dérivés de [**ListViewBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), et traite de manière incrémentielle le modèle d’élément pour la liaison de données. Lors du rendu d’éléments de liste, **ListViewBase** rend une seule phase pour tous les éléments de la vue avant de passer à la phase suivante. Le travail de rendu étant effectué par lots correspondant à des tranches temporelles, à mesure que la liste défile, le travail nécessaire peut être réévalué et ne pas être effectué pour les éléments qui ne sont plus visibles.

L’attribut **x:Phase** peut être spécifié sur tout élément dans un modèle de données utilisant [{x:Bind}](x-bind-markup-extension.md). Quand un élément est associé à une phase différente de 0, l’élément est masqué (via **Opacity**, et non **Visibility**) jusqu’à ce que cette phase soit traitée et les liaisons mises à jour. Quand un contrôle dérivé de [**ListViewBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase) défile, il recycle les modèles d’élément à partir des éléments qui ne sont plus à l’écran pour rendre les éléments nouvellement visibles. Les éléments d’interface utilisateur dans le modèle conservent leurs anciennes valeurs jusqu’à ce qu’ils soient à nouveau liés à des données. L’exécution par phases ayant pour effet de retarder l’étape de liaison aux données, elle doit masquer les éléments d’interface utilisateur obsolètes.

Il se peut qu’une seule phase soit spécifiée pour chaque élément d’interface utilisateur. Dans ce cas, cela s’applique à toutes les liaisons sur l’élément. Si aucune phase n’est pas spécifié, la phase 0 est supposée.

Les numéros de phase ne doivent pas nécessairement être contigus, et sont identiques à la valeur de [**ContainerContentChangingEventArgs.Phase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.containercontentchangingeventargs.phase). L’événement [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) est déclenché pour chaque phase avant le traitement des liaisons **x:Phase**.

L’exécution par phases affecte uniquement les liaisons [{x:Bind}](x-bind-markup-extension.md), pas les liaisons [{Binding}](binding-markup-extension.md).

L’exécution par phases s’applique uniquement quand le modèle d’élément est rendu à l’aide d’un contrôle qui la prend en charge. Pour Windows 10, cela signifie que [ **ListView** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) et [ **GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView). L’exécution par phases ne s’applique pas aux modèles de données utilisés dans d’autres contrôles d’élément, ou pour d’autres cas de figure tels que [**ContentTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.contenttemplate) ou des sections de [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub). Dans ces situations, tous les éléments d’interface utilisateur sont liés aux données en une fois.

