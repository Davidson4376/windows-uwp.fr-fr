---
Description: La navigation dans les applications de plateforme Windows universelle (UWP) est basée sur un modèle flexible de structures et d’éléments de navigation, et de fonctionnalités au niveau du système.
title: Informations de base relatives à la navigation pour les applications UWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6290b142eee4aff7287b9542b645df89164d173b
ms.sourcegitcommit: 34671182c26f5d0825c216a6cededc02b0059a9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67286943"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception de la navigation pour les applications UWP

![En-tête de base de navigation](images/nav/navigation-basics-header.jpg)

Si l’on considère une application comme une collection de pages, le terme *navigation* décrit le fait de se déplacer d’une page à l’autre et au sein d’une même page. La navigation est le point de départ de l’expérience utilisateur, car elle permet aux utilisateurs de rechercher le contenu et les fonctionnalités qui les intéressent. C’est un élément très important, qui peut être difficile à réaliser correctement.

Nous avons un nombre considérable de choix à faire pour la navigation. Par exemple :

:::row:::
    :::column:::
        ![navigation example 1](images/nav/nav-1.svg)

Nous pouvons obliger l’utilisateur à accéder à toutes les pages, dans l’ordre.
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

Nous pouvons également fournir un menu qui permet aux utilisateurs d’accéder directement à n’importe quelle page.
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

Nous pouvons aussi tout placer sur une seule page et fournir des mécanismes de filtrage pour afficher le contenu.
    :::column-end:::
:::row-end:::

Il n’existe aucune conception de navigation unique qui fonctionne pour toutes les applications, mais il existe un ensemble de principes et de recommandations, que vous pouvez suivre pour mieux déterminer la conception appropriée à votre application.

## <a name="principles-of-good-navigation"></a>Principes d’une navigation réussie

Commençons par les principes de base d’une navigation réussie :

- **Cohérence :** Répondre aux attentes des utilisateurs.
- **Simplicité :** Ne pas faire plus que nécessaire.
- **Clarté :** Fournir des chemins d’accès clair et des options.

### <a name="consistency"></a>Consistency

La navigation doit être cohérente avec les attentes des utilisateurs. Utilisez des [contrôles standard](#use-the-right-controls) auxquels les utilisateurs sont familiers et respectez les conventions standard pour les icônes, les emplacements et le style pour rendre la navigation prévisible et intuitive pour les utilisateurs.

![image des composants d'une page](images/nav/page-components.svg)

> Les utilisateurs s’attendent à trouver certains éléments d’interface utilisateur dans des emplacements standard.

### <a name="simplicity"></a>Simplicité

Un recours moins large aux éléments de navigation simplifie la prise de décisions pour les utilisateurs. Le fait de fournir un accès simple à des destinations importantes et de masquer les éléments moins importants permet aux utilisateurs d’accéder plus facilement et rapidement à la page de leur choix.

:::row:::
    :::column:::
        ![do example](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

Présentez les éléments de navigation dans un menu de navigation familier.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

Noyez les utilisateurs dans de nombreuses options de navigation.
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>Clarté

Des chemins d’accès clairs favorisent une navigation logique pour les utilisateurs. Le fait de rendre les options de navigation évidentes et de clarifier les relations entre les pages de navigation doit empêcher les utilisateurs de se perdre.

![exemple faire](images/nav/clarity-image.svg)

> Les destinations sont clairement nommées pour que les utilisateurs sachent où ils sont.

## <a name="general-recommendations"></a>Recommandations générales

Maintenant nous allons utiliser nos principes de conception (cohérence, simplicité et clarté) pour élaborer des règles générales.

1. Réfléchissez à vos utilisateurs. Définissez le parcours qu’ils sont susceptibles de suivre dans votre application et cherchez à savoir pourquoi les utilisateurs accèdent à une page et où ils souhaitent aller ensuite.

2. Évitez les hiérarchies de navigation profondes. Au-delà de trois niveaux de navigation, vous risquez d’égarer votre utilisateur dans une hiérarchie profonde qu’il aura des difficultés à quitter.

3. Évitez l’effet « bâton sauteur ». L’effet du bâton sauteur se produit lorsqu’il existe du contenu associé, mais que la navigation vers celui-ci oblige l’utilisateur à remonter d’un niveau puis à descendre à nouveau.

## <a name="use-the-right-structure"></a>Utiliser la structure appropriée

Maintenant que vous êtes familiarisé avec les principes de navigation générale, comment devez-vous structurer votre application ? Il existe deux structures générales : plate et hiérarchique.

:::row:::
    :::column:::
        ![Pages arranged in a flat structure](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Flat/lateral

Dans une structure plate/latérale, les pages se trouvent côte-à-côte. Vous pouvez aller d’une page à une autre dans n’importe quel ordre.

Nous vous recommandons d’utiliser une structure plate dans les cas suivants :

- Les pages peuvent être affichées dans n’importe quel ordre.
- Les pages sont clairement distinctes les unes des autres et n’ont aucune relation parent/enfant évidente.
- Le groupe contient moins de 8 pages. <br>
(S’il y a plus de pages, il peut être difficile pour les utilisateurs de comprendre dans quelle mesure les pages sont uniques ou de connaître leur emplacement actuel au sein du groupe. Si vous ne pensez pas que ce soit un problème pour votre application, lancez-vous et faites des pages des homologues. Sinon, envisagez d’utiliser une structure hiérarchique pour répartir les pages en deux groupes au moins plus petits.)

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Pages arranged in a hierarchy](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Hierarchical

Dans une structure hiérarchique, les pages sont organisées dans une structure arborescente. Chaque page enfant n’a qu’un parent, mais un parent peut avoir une ou plusieurs pages enfants. Pour atteindre une page enfant, vous naviguez via le parent.

Les structures hiérarchiques sont parfaites pour organiser du contenu complexe qui s’étend sur un grand nombre de pages. L’inconvénient de cette structure est qu’elle surcharge la navigation : plus la structure est profonde, plus les utilisateurs doivent cliquer pour naviguer entre les pages.

Nous recommandons une structure hiérarchique dans les cas suivants :
        
- Les pages doivent être parcourues dans un ordre spécifique.
- Il existe une relation parent-enfant claire entre les pages.
- Le groupe contient plus de 7 pages.
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![an app with a hybrid structure](images/nav/combining-structures.svg)
    :::column-end:::
    :::column span="2":::
        ### Combining structures

Vous n’avez pas besoin de choisir entre une structure ou une autre ; de nombreuses applications très bien conçues utilisent à la fois les structures plates et hiérarchiques. Une application utilise des structures plates pour les pages de niveau supérieur qui peuvent être affichées dans n’importe quel ordre, et des structures hiérarchiques pour les pages qui ont des relations plus complexes.

Si votre structure de navigation comporte plusieurs niveaux, nous vous recommandons de lier les éléments de navigation pair à pair uniquement aux homologues au sein de leur sous-arborescence actuelle. Prenez en considération l’illustration adjacente, qui montre une structure de navigation à deux niveaux :

- Pour le niveau 1, l’élément de navigation pair à pair doit donner accès aux pages A, B, C et D.
- Au niveau 2, les éléments de navigation pair à pair des pages A2 doivent uniquement être liés aux autres pages A2. Ils ne doivent pas renvoyer aux pages de niveau 2 de la sous-arborescence C.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>Utilisez les contrôles appropriés

Une fois que vous avez choisi votre structure de page, vous devez déterminer comment les utilisateurs navigueront à travers ces pages. La plateforme UWP fournit une variété de contrôles de navigation pour garantir une expérience de navigation cohérente et fiable dans votre application.

:::row:::
    :::column:::
        ![Frame image](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

À quelques exceptions près, toutes les applications dotées de plusieurs pages utilisent une trame. Généralement, l’application est dotée d’une page principale qui contient la trame et un élément de navigation principal, comme un contrôle d’affichage de la navigation. Lorsque l’utilisateur sélectionne une page, la trame se charge et l’affiche.
:::row-end:::

:::row:::
    :::column:::
        ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Top navigation and tabs**](../controls-and-patterns/navigationview.md)

Affiche une liste horizontale de liens vers les pages de même niveau. Le contrôle [NavigationView](../controls-and-patterns/navigationview.md) implémente la navigation supérieure et les modèles d’onglets.
        
Utilisez la navigation supérieure lorsque :

- Vous souhaitez afficher toutes les options de navigation à l’écran.
- Vous souhaitez davantage d’espace pour le contenu de votre application.
- Les icônes ne peuvent pas décrire clairement vos catégories de navigation.
        
Utilisez les onglets lorsque :

- Vous souhaitez conserver l’état de page et l’historique de navigation.
- Vous pensez que les utilisateurs passeront fréquemment d’un onglet à l’autre.

:::row-end:::

:::row:::
    :::column:::
         ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
        :::column span="2":::
    [**Pivot**](../controls-and-patterns/pivot.md)
    
Identique à [Navigation View](../controls-and-patterns/navigationview.md), mais avec une prise en charge supplémentaire de l’interaction tactile et un comportement de navigation un peu différent.
    
Utilisez un tableau croisé dynamique lorsque :
- Vous souhaitez que votre application autorise le balayage tactile entre les catégories
- Vous souhaitez que les options de navigation défilent à l’infini
- Vous n’avez pas besoin d’un contrôle étendu sur le comportement de navigation entre les catégories

:::row-end:::

:::row:::
    :::column:::
        ![navview image](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Left navigation**](../controls-and-patterns/navigationview.md)

Affiche une liste vertical de liens vers les pages de niveau supérieur. Contexte d'utilisation :
        
- Les pages se trouvent au niveau supérieur.
- Il existe de nombreux éléments de navigation (plus de 5)
- Vous ne pensez pas que les utilisateurs passeront fréquemment d’une page à l’autre.

:::row-end:::
        
:::row:::
    :::column:::
        ![Master details image](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**Master/details**](../controls-and-patterns/master-details.md)

Affiche la liste (affichage Maître) des éléments. La sélection d’un élément affiche sa page dans la section Détails. Contexte d'utilisation :
        
- Vous pensez que les utilisateurs passeront fréquemment d’un élément enfant à un autre.
- Vous voulez permettre à l’utilisateur d’effectuer des opérations générales, comme la suppression ou le tri, sur des éléments individuels ou des groupes d’éléments. Vous voulez également lui permettre d’afficher ou de mettre à jour les détails de chaque élément.

Les éléments maîtres/détails sont particulièrement bien adaptés aux boîtes de réception, aux listes de contacts et à l’entrée de données.
:::row-end:::

:::row:::
    :::column:::
        ![Hyperlinks and buttons image](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    :::column span="2":::
        [**Hyperlinks**](../controls-and-patterns/hyperlinks.md)

Les éléments de navigation incorporés apparaissent dans le contenu d’une page. Contrairement aux autres éléments de navigation, qui doivent être cohérents dans toutes les pages, les éléments de navigation incorporés au contenu sont uniques d’une page à l’autre.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>Étape suivante : Ajouter du code de navigation à votre application

L’article suivant [Implémenter la navigation de base](navigate-between-two-pages.md), indique le code nécessaire pour utiliser un contrôle de cadre afin d’activer la navigation de base entre deux pages de votre application.
