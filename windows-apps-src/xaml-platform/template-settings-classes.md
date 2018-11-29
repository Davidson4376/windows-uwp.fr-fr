---
description: Classes de paramètres du modèle
title: Classes de paramètres du modèle
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2b44aaf741c188658c7a639422b0d091f8db6e3e
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8189073"
---
# <a name="template-settings-classes"></a>Classes de paramètres du modèle


## <a name="prerequisites"></a>Prérequis

Il est supposé que vous savez ajouter des contrôles à une interface utilisateur, définir leurs propriétés et joindre des gestionnaires d’événements. Pour obtenir des instructions sur l’ajout de contrôles à une application, voir [Ajouter des contrôles et gérer les événements](https://msdn.microsoft.com/library/windows/apps/mt228345). Nous partons également du principe que vous connaissez les bases de la définition d’un modèle personnalisé d’un contrôle en modifiant une copie du modèle par défaut. Pour plus d’informations, voir [Démarrage rapide : modèles de contrôles](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374).

## <a name="the-scenario-for-templatesettings-classes"></a>Scénario relatif aux classes **TemplateSettings**

Les classes **TemplateSettings** fournissent un ensemble de propriétés qui sont utilisées lorsque vous définissez un nouveau modèle pour un contrôle. Les propriétés possèdent des valeurs, par exemple des mesures en pixels, correspondant à la taille de certains éléments d’interface utilisateur. Ces valeurs sont parfois des valeurs calculées qui proviennent de la logique de contrôle qui n’est généralement pas facile à remplacer ni même accessible. Certaines propriétés se veulent des valeurs **From** et **To** qui contrôlent les transitions et les animations des parties. Les propriétés **TemplateSettings** appropriées sont donc fournies par paires.

Voici plusieurs classes **TemplateSettings**. Elles figurent toutes dans l’espace de noms [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818). Voici une liste des classes et un lien vers la propriété **TemplateSettings** du contrôle approprié. Cette propriété **TemplateSettings** indique comment accéder aux valeurs **TemplateSettings** du contrôle et établir des liaisons de modèle à ses propriétés :

-   [**ComboBoxTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227752) : valeur [**ComboBox.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209364)
-   [**GridViewItemTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh738499) : valeur [**GridViewItem.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh738503)
-   [**ListViewItemTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh701948) : valeur [**ListViewItem.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br242923)
-   [**ProgressBarTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227856) : valeur [**ProgressBar.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227537)
-   [**ProgressRingTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh702248) : valeur [**ProgressRing.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh702581)
-   [**SettingsFlyoutTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/dn298721) : valeur [**SettingsFlyout.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/dn252826)
-   [**ToggleSwitchTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209804) : valeur [**ToggleSwitch.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209731)
-   [**ToolTipTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209813) : valeur [**ToolTip.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227629)

Les propriétés **TemplateSettings** sont toujours destinées à être utilisées en XAML. Ce sont des sous-propriétés en lecture seule d’une propriété **TemplateSettings** en lecture seule d’un contrôle parent. Pour un scénario de contrôle personnalisé avancé, lorsque vous créez une classe basée sur [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) et pouvez donc influer sur la logique de contrôle, pensez à définir une propriété **TemplateSettings** personnalisée sur le contrôle pour communiquer des informations pouvant être utiles à toute personne qui recrée le modèle du contrôle. Pour la valeur de cette propriété en lecture seule, définissez une nouvelle classe **TemplateSettings** liée à votre contrôle qui possède des propriétés en lecture seule pour chacune des informations qui sont pertinentes pour les mesures de modèle, le positionnement d’animation, etc. et qui donnent aux appelants l’instance d’exécution de cette classe, qui est initialisée à l’aide de votre logique de contrôle. Les classes **TemplateSettings** sont dérivées de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) afin que les propriétés puissent utiliser le système de propriétés de dépendance pour les rappels de modification de propriété. Mais les identificateurs de propriété de dépendance pour les propriétés ne sont pas exposés comme API publique, car les propriétés **TemplateSettings** sont censées être en lecture seule pour les appelants.

## <a name="how-to-use-templatesettings-in-a-control-template"></a>Comment utiliser **TemplateSettings** dans un modèle de contrôle

Voici un exemple qui provient des modèles de contrôles XAML par défaut de départ. Cet exemple particulier provient du modèle par défaut [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) :

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

Comme le code XAML complet du modèle [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) contient des centaines de lignes, il s’agit là d’un extrait minuscule. Ce code XAML définit une partie d’un contrôle qui est l’un des 6 éléments [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse), qui dépeignent l’animation de rotation de progression indéterminée. En tant que développeur, vous n’aimez peut-être pas les cercles et vous souhaitez peut-être utiliser une autre primitive graphique ou une autre forme de base pour la progression de l’animation. Par exemple, vous pouvez composer à la place un **ProgressRing** utilisant un ensemble d’éléments [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) organisés dans un carré. Si tel est le cas, chaque composant **Rectangle** individuel de votre nouveau modèle peut ressembler à ceci :

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

Les propriétés **TemplateSettings** sont utiles ici, car ce sont des valeurs calculées à partir de la logique de contrôle de base de [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538). Le calcul se divise en [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) et [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) de **ProgressRing**, et alloue une mesure calculée pour chacun des éléments de mouvement dans ses modèles pour que les composants de modèle puissent s’adapter au contenu.

Voici un autre exemple d’utilisation des modèles de contrôles XAML par défaut, qui montre cette fois l’un des ensembles de propriétés qui sont le **From** et la **To** d’une animation. Il provient du modèle [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) par défaut :

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

À nouveau, comme le code XAML contient de nombreuses lignes dans le modèle, il ne s’agit là que d’un extrait. Et ce n’est là que l’un des états et des animations de thème qui utilisent chacun les mêmes propriétés [**ComboBoxTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227752). Pour [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348), l’utilisation des valeurs **ComboBoxTemplateSettings** par le biais des liaisons impose que les animations associées dans le modèle s’arrêteront et commenceront aux positions qui sont basées sur les valeurs partagées. La transition sera donc parfaite.

**Remarque**  lorsque vous utilisez des valeurs **TemplateSettings** dans le cadre de votre modèle de contrôle, assurez-vous que vous définissez les propriétés qui correspondent au type de la valeur. Sinon, vous devrez peut-être créer un convertisseur de valeur pour la liaison afin que le type cible de la liaison puisse être converti à partir d’un type de source différent de la valeur **TemplateSettings**. Pour plus d’informations, voir [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903).

## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide: modèles de contrôles](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)

