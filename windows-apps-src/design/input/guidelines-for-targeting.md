---
Description: Cette rubrique décrit l’utilisation de la géométrie de contact pour le ciblage tactile et indique les meilleures pratiques de ciblage dans les applications Windows Runtime.
title: Ciblage
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34f8d15b971cc9ed286471010a21d1b44b84af13
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363469"
---
# <a name="guidelines-for-touch-targets"></a>Instructions pour les cibles de tactile

Tous les éléments d’interface utilisateur interactives dans votre application de plateforme universelle Windows (UWP) doivent être suffisamment grands pour les utilisateurs à accéder et l’utiliser, quel que soit le périphérique type ou méthode d’entrée avec précision.

Prise en charge d’entrée tactile (ainsi que la nature relativement imprécise de la zone de contact tactile) nécessite une optimisation supplémentaire par rapport à la disposition de taille et le contrôle cible comme le jeu plus vastes et plus complexe de données d’entrée signalés par le digitaliseur tactile est utilisé pour déterminer le cible prévue (ou le plus probable) de l’utilisateur.

Tous les contrôles UWP ont été conçus avec des tailles de cible par défaut tactiles et les dispositions qui vous permettent de créer des applications visuellement à charge équilibrée et attrayantes qui sont à l’aise, facile à utiliser et inspirent confiance.

Dans cette rubrique, nous décrivons ces comportements par défaut afin de vous pouvez concevoir votre application de facilité d’utilisation maximale à l’aide de contrôles de la plateforme et des contrôles personnalisés (doit votre application ont besoin).

> **API importantes** : [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Fluent dimensionnement Standard

*Dimensionnement Standard Fluent* a été créé pour fournir un équilibre entre le confort de densité et l’utilisateur des informations. En effet, tous les éléments sur l’écran alignent à une cible de pixels effective de 40 x 40 (epx), ce qui permet d’éléments d’interface utilisateur alignent sur une grille et l’échelle de manière appropriée en fonction de la mise à l’échelle au niveau du système.

> [!NOTE]
>Pour plus d’informations sur les pixels efficaces et la mise à l’échelle, consultez [Introduction à la conception d’application UWP](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Pour plus d’informations sur la mise à l’échelle au niveau du système, consultez [alignement, marge, remplissage](../layout/alignment-margin-padding.md).

## <a name="fluent-compact-sizing"></a>Dimensionnement Compact Fluent

Les applications peuvent afficher un haut niveau de densité d’informations avec *dimensionnement Fluent Compact*. Dimensionnement compact aligne les éléments d’interface utilisateur à une cible epx de 32 x 32, ce qui permet des éléments d’interface utilisateur pour l’aligner sur une grille plus étroite et d’une échelle de façon appropriée en fonction de la mise à l’échelle au niveau du système.

### <a name="examples"></a>Exemples

Dimensionnement Compact peut être appliqué au niveau de la page ou de la grille.

### <a name="page-level"></a>Niveau page

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

### <a name="grid-level"></a>Niveau de la grille

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="target-size"></a>Taille de la cible

En général, définissez votre taille de cible tactile à plage carré de 7,5 mm (40 x 40 pixels sur un écran de PPP 135 à un 1.0 x mise à l’échelle de plateau). En règle générale, les contrôles UWP alignent cible tactile à 7,5 mm (Cela peut varier en fonction du contrôle spécifique et les modes d’utilisation courants). Consultez [contrôler la taille et la densité](../style/spacing.md) pour plus de détails.

Vous pouvez modifier ces recommandations en matière de tailles de cibles en fonction du scénario particulier de votre application. Voici quelques éléments à prendre en compte :

- Fréquence de touche - effectuez également les cibles à plusieurs reprises ou fréquemment enfoncées supérieure à la taille minimale.
- Conséquence de l’erreur - cibles qui ont des conséquences graves si couvertes dans l’erreur doit avoir une plus grande marge intérieure et être placée plus loin du bord de la zone de contenu. Ceci concerne tout particulièrement les cibles fréquemment utilisées.
- Position dans la zone de contenu.
- Taille de facteur et écran de formulaire.
- Position du doigt.
- Touch visualisations.

## <a name="related-articles"></a>Articles connexes

- [Présentation de la conception des applications UWP](../basics/design-and-ui-intro.md)
- [Taille du contrôle et la densité](../style/spacing.md)
- [Alignement, marge, remplissage](../layout/alignment-margin-padding.md)

### <a name="samples"></a>Exemples

- [Exemple d’entrée de base](https://go.microsoft.com/fwlink/p/?LinkID=620302)
- [Exemple d’entrée à faible latence](https://go.microsoft.com/fwlink/p/?LinkID=620304)
- [Exemple de mode d’interaction utilisateur](https://go.microsoft.com/fwlink/p/?LinkID=619894)
- [Exemple d’éléments visuels de focus](https://go.microsoft.com/fwlink/p/?LinkID=619895)

### <a name="archive-samples"></a>Exemples d’archive

- [Entrée : Exemple d’événements d’entrée utilisateur XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
- [Entrée : Exemples de fonctionnalités d’appareil](https://go.microsoft.com/fwlink/p/?linkid=231530)
- [Entrée : Exemple de test d’atteinte tactile](https://go.microsoft.com/fwlink/p/?linkid=231590)
- [XAML de défilement, panoramique et zoom d’exemple](https://go.microsoft.com/fwlink/p/?linkid=251717)
- [Entrée : Exemple d’entrée manuscrite simplifiée](https://go.microsoft.com/fwlink/p/?linkid=246570)
- [Entrée : Exemple de mouvements de Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
- [Entrée : Manipulations et exemple de mouvements (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
- [Exemple d’entrée tactile de DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
