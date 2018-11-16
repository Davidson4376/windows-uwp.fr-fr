---
author: Jwmsft
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: Informations de base relatives à la navigation pour les applicationsUWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 7/16/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 82623a86548866a78f56385ee0a535bfcb822c46
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6849999"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception de la navigation pour les applications UWP

![En-tête de base de navigation](images/nav/navigation-basics-header.jpg)

Si l’on considère une application comme une collection de pages, le terme *navigation* décrit le fait de se déplacer d’une page à l’autre et au sein d’une même page. La navigation est le point de départ de l’expérience utilisateur, car elle permet aux utilisateurs de rechercher le contenu et les fonctionnalités qui les intéressent. C’est un élément très important, qui peut être difficile à réaliser correctement.

Nous avons un nombre considérable de choix à faire pour la navigation. Par exemple:

:::row:::
    :::column:::
        ![navigation example 1](images/nav/nav-1.svg)

        Require users to go through a series of pages in order.
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

        Provide a menu that allows users to jump directly to any page.
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

        Place everything on a single page and provide filtering mechanisms for viewing content.
    :::column-end:::
:::row-end:::

Il n’existe aucune conception de navigation unique qui fonctionne pour toutes les applications, mais il existe un ensemble de principes et de recommandations, que vous pouvez suivre pour mieux déterminer la conception appropriée à votre application.

## <a name="principles-of-good-navigation"></a>Principes d’une navigation réussie

Commençons par les principes de base d’une navigation réussie:

- **Cohérence:** répondre aux attentes des utilisateurs.
- **Simplicité:** ne pas faire plus que nécessaire.
- **Clarté:** fournir des chemins d’accès clair et des options.

### <a name="consistency"></a>Cohérence

La navigation doit être cohérente avec les attentes des utilisateurs. À l’aide de [contrôles standard](#use-the-right-controls) que les utilisateurs sont habitués à et suivant les conventions standard pour les icônes, emplacement et le style seront rendre la navigation prévisible et intuitive pour les utilisateurs.

![image des composants d'une page](images/nav/page-components.svg)

> Les utilisateurs s’attendent à trouver certains éléments d’interface utilisateur dans des emplacements standard.

### <a name="simplicity"></a>Simplicité

Un recours moins large aux éléments de navigation simplifie la prise de décisions pour les utilisateurs. Le fait de fournir un accès simple à des destinations importantes et de masquer les éléments moins importants permet aux utilisateurs d’accéder plus facilement et rapidement à la page de leur choix.

:::row:::
    :::column:::
        ![do example](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

        Present navigation items in a familiar navigation menu.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

        Overwhelm users with many navigation options.
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>Clarté

Des chemins d’accès clairs favorisent une navigation logique pour les utilisateurs. Le fait de rendre les options de navigation évidentes et de clarifier les relations entre les pages de navigation doit empêcher les utilisateurs de se perdre.

![exemple faire](images/nav/clarity-image.svg)

> Les destinations sont clairement nommées pour que les utilisateurs sachent où ils sont.

## <a name="general-recommendations"></a>Recommandations générales

Maintenant nous allons utiliser nos principes de conception (cohérence, simplicité et clarté) pour élaborer des règles générales.

1. Réfléchissez à vos utilisateurs. Définissez le parcours qu’ils sont susceptibles de suivre dans votre application et cherchez à savoir pourquoi les utilisateurs accèdent à une page et où ils souhaitent aller ensuite.

2. Évitez les hiérarchies de navigation ciblé. Au-delà de trois niveaux de navigation, vous risquez d’égarer votre utilisateur dans une hiérarchie profonde qu’il aura des difficultés à quitter.

3. Évitez l’effet «bâton sauteur». L’effet du bâton sauteur se produit lorsqu’il existe du contenu associé, mais que la navigation vers celui-ci oblige l’utilisateur à remonter d’un niveau puis à descendre à nouveau.

## <a name="use-the-right-structure"></a>Utiliser la structure appropriée

Maintenant que vous êtes familiarisé avec les principes de navigation générale, comment devez-vous structurer votre application? Il existe deux structures générales: plate et hiérarchique.

:::row:::
    :::column:::
        ![Pages arranged in a flat structure](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Flat/lateral

        In a flat/lateral structure, pages exist side-by-side. You can go from one page to another in any order.

        We recommend using a flat structure when:

        - Les pages peuvent être affichées dans n’importe quel ordre.
        - Les pages sont clairement distinctes les unes des autres et n’ont aucune relation parent/enfant évidente.
        - Il existe moins de 8 pages dans le groupe. <br>
        (S’il y a plus de pages, il peut être difficile pour les utilisateurs de comprendre dans quelle mesure les pages sont uniques ou de connaître leur emplacement actuel au sein du groupe. Si vous ne pensez pas que ce soit un problème pour votre application, lancez-vous et faites des pages des homologues. Sinon, envisagez d’utiliser une structure hiérarchique pour répartir les pages en deux groupes au moins plus petits.)

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Pages arranged in a hierarchy](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Hierarchical

        In a hierarchical structure, pages are organized into a tree-like structure. Each child page has one parent, but a parent can have one or more child pages. To reach a child page, you travel through the parent.

        Hierarchical structures are good for organizing complex content that spans lots of pages. The downside is some navigation overhead: the deeper the structure, the more clicks it takes to get from page to page.

        We recommend a hierarchical structure when:
        
        - Les pages doivent être parcourues dans un ordre spécifique.
        - Il existe une relation parent-enfant claire entre les pages.
        - Le groupe contient plus de 7pages.
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![an app with a hybrid structure](images/nav/combining-structures.svg)
    :::column-end:::
    :::column span="2":::
        ### Combining structures

        You don't have choose to one structure or the other; many well-design apps use both. An app can use flat structures for top-level pages that can be viewed in any order, and hierarchical structures for pages that have more complex relationships.

        If your navigation structure has multiple levels, we recommend that peer-to-peer navigation elements only link to the peers within their current subtree. Consider the adjacent illustration, which shows a navigation structure that has two levels:

        - Au niveau1, l’élément de navigation pair à pair doit donner accès aux pagesA, B, C etD.
        - Au niveau2, les éléments de navigation pair à pair des pagesA2 doivent uniquement être liés aux autres pagesA2. Ils ne doivent pas renvoyer aux pages de niveau2 de la sous-arborescenceC.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>Utilisez les contrôles appropriés

Une fois que vous avez choisi votre structure de page, vous devez déterminer comment les utilisateurs navigueront à travers ces pages. La plateformeUWP fournit une variété de contrôles de navigation pour garantir une expérience de navigation cohérente et fiable dans votre application.

:::row:::
    :::column:::
        ![Frame image](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

        With few exceptions, any app that has multiple pages uses a frame. Typically, an app has a main page that contains the frame and a primary navigation element, such as a navigation view control. When the user selects a page, the frame loads and displays it.
:::row-end:::

:::row:::
    :::column:::
        ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Top navigation and tabs**](../controls-and-patterns/navigationview.md)

        Displays a horizontal list of links to pages at the same level. The [NavigationView](../controls-and-patterns/navigationview.md) control implements the top navigation and tabs patterns.
        
        Use top navigation when:

        - Vous souhaitez afficher toutes les options de navigation à l’écran.
        - Vous souhaitez davantage d’espace pour le contenu de votre application.
        - Les icônes ne peut pas décrivent clairement vos catégories de navigation.
        
        Utilisez les onglets lorsque:

        - Vous souhaitez conserver l’état de la page et l’historique de navigation.
        - Vous pensez que les utilisateurs passeront fréquemment des onglets.

:::row-end:::

:::row:::
    :::column:::
        ![navview image](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Left navigation**](../controls-and-patterns/navigationview.md)

        Displays a vertical list of links to top-level pages. Use when:
        
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

        Displays a list (master view) of items. Selecting an item displays its corresponding page in the details section. Use when:
        
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

        Embedded navigation elements can appear in a page's content. Unlike other navigation elements, which should be consistent across the pages, content-embedded navigation elements are unique from page to page.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>Étape suivante: Ajouter du code de navigation à votre application

L’article suivant [Implémenter la navigation de base](navigate-between-two-pages.md), indique le code nécessaire pour utiliser un contrôle de cadre afin d’activer la navigation de base entre deux pages de votre application.