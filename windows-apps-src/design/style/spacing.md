---
title: Espacement et tailles
description: Les nouveaux styles de contrôle Standard Fluent et Compact garantissent une expérience utilisateur agréable, quels que soient l’appareil et la méthode de saisie.
keywords: UWP, Windows 10, contrôles, taille, densité, standard, compact
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ff88332e0b44749bcd86e06adafeb596ca8678b3
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63820636"
---
# <a name="control-size-and-density"></a>Taille et densité du contrôle

Utilisez une combinaison de taille et de densité du contrôle pour optimiser votre application Universal Windows Platform (UWP) et fournir une expérience utilisateur qui convient le mieux pour les exigences de fonctionnalités et d’interaction de votre application.

Par défaut, les applications UWP sont rendues avec une mise en page à faible densité (ou `Standard`). Toutefois, depuis WinUI 2.1, une option de mise en page à haute densité (ou `Compact`), pour une interface utilisateur riche en informations et des scénarios similaires spécialisés, est également prise en charge. Il est possible de la spécifier via une ressource de style de base (voir les exemples ci-dessous).

Alors que la fonctionnalité et le comportement n’ont pas changé et restent cohérents entre les deux options de taille et de densité, la taille de police de corps par défaut a été mise à jour sur 14px pour tous les contrôles afin de prendre en charge ces deux options de densité. Cette taille de police est compatible entre les divers appareils et régions, et garantit que votre application demeure équilibrée et agréable pour les utilisateurs.

## <a name="fluent-standard-sizing"></a>Taille Fluent Standard

La *taille Fluent Standard* a été créée pour fournir un équilibre entre le confort de l’utilisateur et la densité des informations. Tous les éléments sur l’écran s’alignent sur une cible de pixels effectifs (epx) de 40x40, ce qui permet d’aligner les éléments de l’interface utilisateur sur une grille et de les mettre à l’échelle de manière appropriée en fonction de la mise à l’échelle au niveau du système.

**La taille Standard est conçue pour prendre en charge les entrées tactiles et du pointeur.**

> [!NOTE]
>Pour plus d’informations sur les pixels effectifs et la mise à l’échelle, consultez [Présentation de la conception des applications UWP](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Pour plus d’informations sur la mise à l’échelle au niveau du système, consultez [Alignement, marge, remplissage](../layout/alignment-margin-padding.md).

Pour Windows 10 October 2018 Update (version 1809), la taille standard par défaut de tous les contrôles UWP a été diminuée pour accroître la facilité d’utilisation dans tous les scénarios d’utilisation.

L’illustration suivante montre certains changements de la mise en forme du contrôle qui ont été introduits dans Windows 10 October 2018 Update. Plus précisément, la marge entre un en-tête et le haut d’un contrôle a été réduite de 8epx à 4epx, et la grille 44epx a été remplacée par une grille 40epx.

![Exemple de mise en forme de contrôle standard](images/standarddensity.png)

*Exemple de mise en forme de contrôle standard*

L’image suivante montre les modifications apportées aux tailles de contrôle pour Windows 10 October 2018 Update. Plus précisément, l’alignement sur la grille 40epx.

![Exemple de commande standard](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Taille Fluent Compact

La taille Compact permet d’obtenir des groupes de contrôles denses et riches en informations, et elle peut être utile pour ce qui suit :

- Navigation dans de grandes quantités de contenu.
- Optimisation du contenu visible sur une page.
- Navigation et interaction avec les contrôles et le contenu

**La taille Compact est principalement conçue pour prendre en charge les entrées du pointeur.**

### <a name="examples"></a>Exemples

La taille Compact est implémentée via un dictionnaire de ressources spécial qui peut être spécifié dans votre application au niveau de la page ou dans une mise en forme spécifique. Le dictionnaire de ressources est disponible dans le package Nuget [WinUI](https://docs.microsoft.com/en-us/uwp/toolkits/winui/).

Les exemples suivants montrent comment le style `Compact` peut être appliqué pour la page et un contrôle de grille individuel.

#### <a name="page-level"></a>Niveau page

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

#### <a name="grid-level"></a>Niveau grille

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="related-articles"></a>Articles connexes

- [Instructions pour les cibles tactiles](../input/guidelines-for-targeting.md)
- [Références aux ressources ResourceDictionary et XAML](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [Dictionnaire de ressources](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.resourcedictionary)
- [Styles XAML](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/xaml-styles) 
