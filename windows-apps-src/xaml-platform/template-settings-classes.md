---
description: Classes de paramètres du modèle
title: Classes de paramètres du modèle
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0d68b31a1574d23dd66de37e2f708d8d77fc052
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371122"
---
# <a name="template-settings-classes"></a>Classes de paramètres du modèle


## <a name="prerequisites"></a>Prérequis

Il est supposé que vous savez ajouter des contrôles à une interface utilisateur, définir leurs propriétés et joindre des gestionnaires d’événements. Pour obtenir des instructions sur l’ajout de contrôles à une application, voir [Ajouter des contrôles et gérer les événements](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro). Nous partons également du principe que vous connaissez les bases de la définition d’un modèle personnalisé d’un contrôle en modifiant une copie du modèle par défaut. Pour plus d’informations, consultez [Guide de démarrage rapide : Modèles de contrôle](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10)).

## <a name="the-scenario-for-templatesettings-classes"></a>Scénario relatif aux classes **TemplateSettings**

Les classes **TemplateSettings** fournissent un ensemble de propriétés qui sont utilisées lorsque vous définissez un nouveau modèle pour un contrôle. Les propriétés possèdent des valeurs, par exemple des mesures en pixels, correspondant à la taille de certains éléments d’interface utilisateur. Ces valeurs sont parfois des valeurs calculées qui proviennent de la logique de contrôle qui n’est généralement pas facile à remplacer ni même accessible. Certaines propriétés se veulent des valeurs **From** et **To** qui contrôlent les transitions et les animations des parties. Les propriétés **TemplateSettings** appropriées sont donc fournies par paires.

Voici plusieurs classes **TemplateSettings**. Elles figurent toutes dans l’espace de noms [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives). Voici une liste des classes et un lien vers la propriété **TemplateSettings** du contrôle approprié. Cette propriété **TemplateSettings** indique comment accéder aux valeurs **TemplateSettings** du contrôle et établir des liaisons de modèle à ses propriétés :

-   [**ComboBoxTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings): valeur de [ **ComboBox.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox.templatesettings)
-   [**GridViewItemTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemTemplateSettings): valeur de [ **GridViewItem.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridviewitem.templatesettings)
-   [**ListViewItemTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemTemplateSettings): valeur de [ **ListViewItem.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem.templatesettings)
-   [**ProgressBarTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressBarTemplateSettings): valeur de [ **ProgressBar.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar.templatesettings)
-   [**ProgressRingTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressRingTemplateSettings): valeur de [ **ProgressRing.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring.templatesettings)
-   [**SettingsFlyoutTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.SettingsFlyoutTemplateSettings): valeur de [ **SettingsFlyout.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.settingsflyout.templatesettings)
-   [**ToggleSwitchTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleSwitchTemplateSettings): valeur de [ **ToggleSwitch.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.templatesettings)
-   [**ToolTipTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToolTipTemplateSettings): valeur de [ **ToolTip.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.tooltip.templatesettings)

Les propriétés **TemplateSettings** sont toujours destinées à être utilisées en XAML. Ce sont des sous-propriétés en lecture seule d’une propriété **TemplateSettings** en lecture seule d’un contrôle parent. Pour un scénario de contrôle personnalisé avancé, lorsque vous créez une classe basée sur [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) et pouvez donc influer sur la logique de contrôle, pensez à définir une propriété **TemplateSettings** personnalisée sur le contrôle pour communiquer des informations pouvant être utiles à toute personne qui recrée le modèle du contrôle. Pour la valeur de cette propriété en lecture seule, définissez une nouvelle classe **TemplateSettings** liée à votre contrôle qui possède des propriétés en lecture seule pour chacune des informations qui sont pertinentes pour les mesures de modèle, le positionnement d’animation, etc. et qui donnent aux appelants l’instance d’exécution de cette classe, qui est initialisée à l’aide de votre logique de contrôle. Les classes **TemplateSettings** sont dérivées de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) afin que les propriétés puissent utiliser le système de propriétés de dépendance pour les rappels de modification de propriété. Mais les identificateurs de propriété de dépendance pour les propriétés ne sont pas exposés comme API publique, car les propriétés **TemplateSettings** sont censées être en lecture seule pour les appelants.

## <a name="how-to-use-templatesettings-in-a-control-template"></a>Comment utiliser **TemplateSettings** dans un modèle de contrôle

Voici un exemple qui provient des modèles de contrôles XAML par défaut de départ. Cet exemple particulier provient du modèle par défaut [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) :

```xml
<Ellipse
    x:Name="E1"
    Style="{StaticResource ProgressRingEllipseStyle}"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

Comme le code XAML complet du modèle [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) contient des centaines de lignes, il s’agit là d’un extrait minuscule. Ce code XAML définit une partie d’un contrôle qui est l’un des 6 éléments [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse), qui dépeignent l’animation de rotation de progression indéterminée. En tant que développeur, vous n’aimez peut-être pas les cercles et vous souhaitez peut-être utiliser une autre primitive graphique ou une autre forme de base pour la progression de l’animation. Par exemple, vous pouvez composer à la place un **ProgressRing** utilisant un ensemble d’éléments [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) organisés dans un carré. Si tel est le cas, chaque composant **Rectangle** individuel de votre nouveau modèle peut ressembler à ceci :

```xml
<Rectangle
    x:Name="R1"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

Les propriétés **TemplateSettings** sont utiles ici, car ce sont des valeurs calculées à partir de la logique de contrôle de base de [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing). Le calcul se divise en [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) et [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) de **ProgressRing**, et alloue une mesure calculée pour chacun des éléments de mouvement dans ses modèles pour que les composants de modèle puissent s’adapter au contenu.

Voici un autre exemple d’utilisation des modèles de contrôles XAML par défaut, qui montre cette fois l’un des ensembles de propriétés qui sont le **From** et la **To** d’une animation. Il provient du modèle [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) par défaut :

```xml
<VisualStateGroup x:Name="DropDownStates">
    <VisualState x:Name="Opened">
        <Storyboard>
            <SplitOpenThemeAnimation
               OpenedTargetName="PopupBorder"
               ContentTargetName="ScrollViewer"
               ClosedTargetName="ContentPresenter"
               ContentTranslationOffset="0"
               OffsetFromCenter="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOffset}"
               OpenedLength="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOpenedHeight}"
               ClosedLength="{Binding RelativeSource={RelativeSource TemplatedParent},
                 Path=TemplateSettings.DropDownClosedHeight}" />
        </Storyboard>
   </VisualState>
...
</VisualStateGroup>
```

À nouveau, comme le code XAML contient de nombreuses lignes dans le modèle, il ne s’agit là que d’un extrait. Et ce n’est là que l’un des états et des animations de thème qui utilisent chacun les mêmes propriétés [**ComboBoxTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings). Pour [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox), l’utilisation des valeurs **ComboBoxTemplateSettings** par le biais des liaisons impose que les animations associées dans le modèle s’arrêteront et commenceront aux positions qui sont basées sur les valeurs partagées. La transition sera donc parfaite.

**Remarque**    quand vous utilisez **TemplateSettings** valeurs dans le cadre de votre modèle de contrôle, assurez-vous que vous définissez les propriétés qui correspondent au type de la valeur. Sinon, vous devrez peut-être créer un convertisseur de valeur pour la liaison afin que le type cible de la liaison puisse être converti à partir d’un type de source différent de la valeur **TemplateSettings**. Pour plus d’informations, voir [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).

## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide : Modèles de contrôle](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10))

