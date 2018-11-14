---
author: Jwmsft
Description: The hub control uses a hierarchical navigation pattern to support apps with a relational information architecture.
title: Contrôles hub
ms.assetid: F1319960-63C6-4A8B-8DA1-451D59A01AC2
label: Hub
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: yulikl
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 3d73ff59b03f288227b1435b0b931d11860259ec
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6181442"
---
# <a name="hub-controlpattern"></a>Modèle/contrôle hub

 


Un contrôle hub vous permet d’organiser le contenu de l’application en sections ou catégories, distinctes et pourtant liées. Les sections d’un hub sont censées être parcourues dans un ordre préféré et peuvent servir de point de départ pour des expériences plus détaillées.

> **API importantes**: [classe Hub](https://msdn.microsoft.com/library/windows/apps/dn251843), [classe HubSection](https://msdn.microsoft.com/library/windows/apps/dn251845)

![Exemple de hub](images/hub_example_tablet.png)

Le contenu d’un hub peut être affiché dans une vue panoramique qui offre aux utilisateurs un aperçu des nouveautés, des éléments disponibles et du contenu pertinent. Les hubs ont généralement un en-tête de page et les sections de contenu obtiennent chacune un en-tête de section.


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Le contrôle hub fonctionne bien pour afficher de grandes quantités de contenu organisé dans une hiérarchie. Les hubs hiérarchisent la navigation et la découverte de nouveau contenu, et sont utiles pour afficher des éléments dans un magasin ou une collection multimédia.

Le contrôle hub dispose de plusieurs fonctionnalités qui lui permettent de générer un modèle de navigation de contenu.

-   **Navigation visuelle**

    Un hub permet d’afficher du contenu dans un tableau varié, simple et facile à analyser.

-   **Catégorisation**

    Chaque section du hub permet d’organiser son contenu dans un ordre logique.

-   **Types de contenu mixte**

    Les types de contenu mixte utilisent fréquemment des proportions et des tailles de ressources variables. Un hub permet de disposer chaque type de contenu de façon unique et précise dans chaque section de hub.

-   **Largeurs de page et de contenu variables**

    Grâce à son statut de modèle panoramique, le hub peut prendre en charge des largeurs de section variables. Cela constitue la solution idéale pour les contenus de quantité ou de profondeurs différentes.

-   **Architecture flexible**

    Si vous préférez conserver l’architecture de votre application avec peu de niveaux, vous pouvez ajuster tout le contenu du canal dans un résumé de section du hub.

Un hub fait partie des éléments de navigation que vous pouvez utiliser. Pour en savoir plus sur les modèles de navigation et sur les autres éléments de navigation, voir [Informations de base en matière de conception de la navigation pour les applications de plateforme Windows universelle (UWP)](../basics/navigation-basics.md).

## <a name="hub-architecture"></a>Architecture du hub

Le contrôle hub utilise un modèle de navigation hiérarchique qui prend en charge les applications avec une architecture d’informations relationnelle. Un hub comprend différentes catégories de contenu dont chacune est mappée sur les pages de section de l’application. Les pages de section peuvent s’afficher sous la forme qui représente le mieux le scénario et le contenu de la section.

![Maquette conceptuelle d’une application hiérarchique Food with Friends](images/navigation_diagram_food_with_friends_app_new.png)

## <a name="layouts-and-panningscrolling"></a>Dispositions et panoramique/défilement

Plusieurs méthodes permettent de présenter et de parcourir le contenu d’un hub. Assurez-vous simplement que les listes de contenu d’un hub effectuent toujours un mouvement panoramique dans une direction perpendiculaire à sa direction de défilement.

**Mouvement panoramique horizontal**

![Exemple de hub à mouvement panoramique horizontal](images/controls_hub_horizontal_pan.png)
**Mouvement panoramique vertical**

![Exemple de hub à mouvement panoramique vertical](images/controls_hub_vertical_pan.png)
**Mouvement panoramique horizontal avec une liste/grille à défilement vertical**

![Exemple de hub à mouvement panoramique horizontal avec une liste à défilement vertical](images/controls_hub_horizontal_vertical_scroll.png)
**Mouvement panoramique vertical avec une liste/grille à défilement horizontal**

![Exemple de hub à mouvement panoramique horizontal](images/controls_hub_vertical_horizontal_scroll.png)

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Hub">ouvrir l’application et voir le Hub en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Le Hub permet une grande flexibilité lors de la conception. Il vous permet de concevoir des applications offrant un large éventail d’expériences visuelles riches et attractives. Vous pouvez utiliser une image Hero ou une section de contenu pour le premier groupe ; une grande image pour la zone Hero peut être rognée verticalement et horizontalement sans perte du centre d’intérêt. Voici un exemple d’image Hero simple qui peut être rognée en mode portrait, paysage ou largeur étroite.

![Image Hero rognée pour différentes tailles de fenêtre](images/hub_hero_cropped2.png)

Sur les appareils mobiles, une seule section du hub est visible à la fois.

![Exemple de modèle de hub sur un petit écran](images/phone_hub_example.png)

## <a name="recommendations"></a>Recommandations

-   Pour permettre aux utilisateurs de savoir qu’une section du hub contient d’autres informations, nous vous recommandons de détourer le contenu afin qu’une partie dépasse.
-   En fonction des besoins de votre application, vous pouvez ajouter plusieurs sections du hub au contrôle hub, disposant chacune de son propre objectif fonctionnel. Par exemple, une section peut contenir une série de liens et de contrôles, tandis qu’une autre peut servir de référentiel pour les miniatures. L’utilisateur peut faire un mouvement panoramique entre ces sections en utilisant les mouvements pris en charge dans le contrôle hub.
-   Redisposer dynamiquement le contenu est la meilleure façon de l’adapter à différentes tailles de fenêtre.
-   Si votre application comporte de nombreuses sections de hub, pensez à ajouter un zoom sémantique. Cela facilite la recherche des sections lorsque l’application est redimensionnée avec une largeur étroite.
-   Nous recommandons de ne pas avoir d’élément de section de hub conduisant à un autre hub. Il est préférable d’utiliser des en-têtes interactifs pour naviguer vers une autre section ou page de hub.
-   Conçu comme un point de départ, le hub est destiné à être personnalisé pour répondre aux besoins de votre application. Vous pouvez modifier les aspects suivants d’un hub :
    -   Nombre de sections
    -   Type de contenu de chaque section
    -   Emplacements et ordre des sections
    -   Taille des sections
    -   Espacement entre les sections
    -   Espacement entre une section et le haut ou le bas du Hub
    -   Style du texte et taille des en-têtes et du contenu
    -   Couleur de l’arrière-plan, des sections, des en-têtes de section et du contenu des sections

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles associés

- [Classe Hub](https://msdn.microsoft.com/library/windows/apps/dn251843)
- [Notions de base sur la navigation](../basics/navigation-basics.md)
- [Utilisation d’un hub](https://msdn.microsoft.com/library/windows/apps/xaml/dn308518)
- [Exemple de contrôle hub XAML](http://go.microsoft.com/fwlink/p/?LinkID=310072)
