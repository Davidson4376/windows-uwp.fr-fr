---
title: Espacement et tailles
description: Les nouveaux styles de contrôle Standard Fluent et Compact garantir une expérience de l’utilisateur à l’aise, quel que soit la méthode de saisie et de périphérique.
keywords: UWP, Windows 10, contrôles, taille, densité, standard, compact
ms.date: 4/4/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 7b74e3dc2ad047d9e52509b71ef00b829ad63a0d
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249451"
---
# <a name="control-size-and-density"></a>Taille du contrôle et la densité

Utilisez une combinaison de taille et de densité pour optimiser votre application Universal Windows Platform (UWP) et fournir une expérience utilisateur qui convient le mieux pour les exigences de fonctionnalités et d’interaction de votre application.

Par défaut, les applications UWP sont rendues avec une faible densité (ou `Standard`) mise en page. Toutefois, depuis WinUI 2.1, une haute densité (ou `Compact`) l’option de disposition, pour plus d’informations l’interface utilisateur riche et des scénarios similaires spécialisés, est également pris en charge. Cela peut être spécifiée via une ressource de style de base (voir les exemples ci-dessous).

Lors de la fonctionnalité et le comportement n’a pas changé et reste cohérent entre les deux options de taille et la densité, la taille de police de corps par défaut a été mis à jour à 14px pour tous les contrôles prendre en charge ces options de densité de deux. Cette taille de police compatible avec les appareils et les régions et garantit le que reste de votre application à charge équilibrée et à l’aise pour les utilisateurs.

## <a name="fluent-standard-sizing"></a>Fluent dimensionnement Standard

*Dimensionnement Standard Fluent* a été créé pour fournir un équilibre entre le confort de densité et l’utilisateur des informations. En effet, tous les éléments sur l’écran alignent à une cible de pixels effective de 40 x 40 (epx), ce qui permet d’éléments d’interface utilisateur alignent sur une grille et l’échelle de manière appropriée en fonction de la mise à l’échelle au niveau du système.

**Dimensionnement standard est conçu pour prendre en charge tactile et le pointeur d’entrée.**

> [!NOTE]
>Pour plus d’informations sur les pixels efficaces et la mise à l’échelle, consultez [Introduction à la conception d’application UWP](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Pour plus d’informations sur la mise à l’échelle au niveau du système, consultez [alignement, marge, remplissage](../layout/alignment-margin-padding.md).

Pour le Kit de développement Windows 10 octobre 2018 Update (version 1809), le standard, la taille par défaut pour tous les contrôles UWP a été réduite pour faciliter l’utilisation pour tous les scénarios d’utilisation.

L’illustration suivante montre certaines du contrôle des changements de disposition qui ont été introduites avec Windows 10 octobre 2018 mettre à jour. Plus précisément, la marge entre un en-tête et le haut d’un contrôle a été réduite à partir de 8epx à 4epx, et la grille 44epx a été remplacée par une grille 40epx.

![Exemple de disposition de contrôle standard](images/standarddensity.png)

*Exemple de disposition de contrôle standard*

Cette image suivante montre les modifications apportées à contrôler les tailles pour les fenêtres de mise à jour 10 octobre 2018. Plus précisément, d’alignement sur la grille 40epx.

![Exemple de commande standard](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Dimensionnement Compact Fluent

Dimensionnement compact permet denses, riche en informations des groupes de contrôles et peut aider avec les éléments suivants :

- Navigation du contenu volumineux.
- Optimisation de contenu visible sur une page.
- Naviguer et interagir avec les contrôles et le contenu

**Dimensionnement compact est principalement conçu pour prendre en charge d’entrée de pointeur.**

### <a name="examples"></a>Exemples

Dimensionnement compact est implémentée via un dictionnaire de ressources spéciales qui peut être spécifié dans votre application au niveau de la page ou sur une disposition spécifique. Le dictionnaire de ressources est disponible dans le [WinUI](https://docs.microsoft.com/en-us/uwp/toolkits/winui/) package Nuget.

Les exemples suivants montrent comment le `Compact` style peut être appliqué pour la page et un contrôle de grille individuel.

#### <a name="page-level"></a>Niveau page

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

#### <a name="grid-level"></a>Niveau de la grille

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="related-articles"></a>Articles connexes

- [Instructions pour les cibles de tactile](../input/guidelines-for-targeting.md)
- [Références aux ressources ResourceDictionary et XAML](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [Dictionnaire de ressources](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.resourcedictionary)
- [Styles XAML](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/xaml-styles) 
