---
author: serenaz
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: Informations de base relatives à la navigation pour les applicationsUWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3edb7dc28561b5d316a908df951e3052eafc995b
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654268"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception de la navigation pour les applicationsUWP

Si l’on considère une application comme une collection de pages, le terme *navigation* décrit le fait de se déplacer d’une page à l’autre et au sein d’une même page. La navigation est le point de départ de l’expérience utilisateur, car elle permet aux utilisateurs de rechercher le contenu et les fonctionnalités qui les intéressent. C’est un élément très important, qui peut être difficile à réaliser correctement.

Cette difficulté s’explique par le fait que, en tant que concepteurs d’applications, nous avons un très grand nombre de choix à faire. Nous pouvons obliger l’utilisateur à accéder à toutes les pages, dans l’ordre. Nous pouvons également fournir un menu qui permet aux utilisateurs d’accéder directement à n’importe quelle page. Nous pouvons aussi tout placer sur une seule page et fournir des mécanismes de filtrage pour afficher le contenu.
 
Il n’existe aucune conception de navigation unique qui fonctionne pour toutes les applications, mais il existe un ensemble de principes et de recommandations, que vous pouvez suivre pour mieux déterminer la conception appropriée à votre application. 

<figure class="wdg-figure">
  <img alt="Navigation diagram for an app" src="images/navigation_diagram.png" />
  <figcaption>Schéma de la navigation dans une application</figcaption>
</figure> 

## <a name="principles-of-good-navigation"></a>Principes d’une navigation réussie
Commençons par les principes de base d’une navigation réussie: 

* Cohérence: répondre aux attentes des utilisateurs.
* Simplicité: ne pas faire plus que nécessaire.
* Clarté: fournir des chemins d’accès clair et des options.

### <a name="consistency"></a>Cohérence
La navigation doit être cohérente avec les attentes des utilisateurs. Utilisez des [contrôles standard](#use-the-right-controls) auxquels les utilisateurs sont familiers et respectez les conventions standard pour les icônes, les emplacements et le style pour rendre la navigation prévisible et intuitive pour les utilisateurs.

<figure class="wdg-figure">
<img src="images/nav/nav-component-layout.png" alt="Preferred location for navigation elements"/>
  <figcaption>Les utilisateurs s’attendent à trouver certains éléments d’interface utilisateur dans des emplacements standard.</figcaption>
</figure> 

### <a name="simplicity"></a>Simplicité
Un recours moins large aux éléments de navigation simplifie la prise de décisions pour les utilisateurs. Le fait de fournir un accès simple à des destinations importantes et de masquer les éléments moins importants permet aux utilisateurs d’accéder plus facilement et rapidement à la page de leur choix.

<figure class="wdg-figure">
<img src="images/nav/nav-simple-menus.png" alt="A simple versus a complex menu"/>
  <figcaption> Le menu de gauche sera plus facile à comprendre et à utiliser pour les utilisateurs, car il contient moins d’éléments.
</figcaption>
</figure> 

### <a name="clarity"></a>Clarté
Des chemins d’accès clairs favorisent une navigation logique pour les utilisateurs. Le fait de rendre les options de navigation évidentes et de clarifier les relations entre les pages de navigation doit empêcher les utilisateurs de se perdre.

<figure class="wdg-figure">
<img src="images/nav/nav-pages.png" alt="Clear paths and destinations"/>
  <figcaption> Les destinations sont clairement nommées pour que les utilisateurs sachent où ils sont.
</figcaption>
</figure> 

## <a name="general-recommendations"></a>Recommandations générales
Maintenant nous allons utiliser nos principes de conception (cohérence, simplicité et clarté) pour élaborer des règles générales.

1. Réfléchissez à vos utilisateurs. Définissez le parcours qu’ils sont susceptibles de suivre dans votre application et cherchez à savoir pourquoi les utilisateurs accèdent à une page et où ils souhaitent aller ensuite. 

2. Évitez les hiérarchies de navigation profondes. Au-delà de trois niveaux de navigation, vous risquez d’égarer votre utilisateur dans une hiérarchie profonde qu’il aura des difficultés à quitter.

3. Évitez l’effet «bâton sauteur». L’effet du bâton sauteur se produit lorsqu’il existe du contenu associé, mais que la navigation vers celui-ci oblige l’utilisateur à remonter d’un niveau puis à descendre à nouveau.

## <a name="use-the-right-structure"></a>Utiliser la structure appropriée 
Maintenant que vous êtes familiarisé avec les principes de navigation générale, comment devez-vous structurer votre application? Il existe deux structures générales: plate et hiérarchique. 

### <a name="flatlateral"></a>Plate/latérale
![Pages organisées dans une structure plate](images/nav/nav-pages-peer.png)

Dans une structure plate/latérale, les pages se trouvent côte-à-côte. Vous pouvez aller d’une page à une autre dans n’importe quel ordre. 

Nous vous recommandons d’utiliser une structure plate dans les cas suivants: 
<ul>
<li>Les pages peuvent être affichées dans n’importe quel ordre.</li>
<li>Les pages sont clairement distinctes les unes des autres et n’ont aucune relation parent/enfant évidente.</li>
<li>Le groupe contient moins de 8pages.<br/>
(S’il y a plus de pages, il peut être difficile pour les utilisateurs de comprendre dans quelle mesure les pages sont uniques ou de connaître leur emplacement actuel au sein du groupe. Si vous ne pensez pas que ce soit un problème pour votre application, lancez-vous et faites des pages des homologues. Sinon, envisagez d’utiliser une structure hiérarchique pour répartir les pages en deux groupes au moins plus petits.)</li>
</ul>

### <a name="hierarchical"></a>Hiérarchique
![Pages organisées dans une hiérarchie](images/nav/nav-pages-hiearchy.png)

Dans une structure hiérarchique, les pages sont organisées dans une structure arborescente. Chaque page enfant n’a qu’un parent, mais un parent peut avoir une ou plusieurs pages enfants. Pour atteindre une page enfant, vous naviguez via le parent.

Les structures hiérarchiques sont parfaites pour organiser du contenu complexe qui s’étend sur un grand nombre de pages. L’inconvénient de cette structure est qu’elle surcharge la navigation: plus la structure est profonde, plus les utilisateurs doivent cliquer pour naviguer entre les pages. 

Nous recommandons une structure hiérarchique dans les cas suivants: 
<ul>
<li>Les pages doivent être parcourues dans un ordre spécifique.</li>
<li>Il existe une relation parent-enfant claire entre les pages.</li>
<li>Le groupe contient plus de 7pages.</li>
</ul>

### <a name="combining-structures"></a>Combinaison de structures
![application à structure hybride](images/nav/nav-hybridstructure.png.png)

Vous n’avez pas besoin de choisir entre une structure ou une autre; de nombreuses applications très bien conçues utilisent à la fois les structures plates et hiérarchiques. Une application utilise des structures plates pour les pages de niveau supérieur qui peuvent être affichées dans n’importe quel ordre, et des structures hiérarchiques pour les pages qui ont des relations plus complexes. 

Si votre structure de navigation comporte plusieurs niveaux, nous vous recommandons de lier les éléments de navigation pair à pair uniquement aux homologues au sein de leur sous-arborescence actuelle. Prenez en considération l’illustration suivante, qui montre une structure de navigation à trois niveaux:

![application avec deux sous-arborescences](images/nav/nav-subtrees.png)
- Au niveau1, l’élément de navigation pair à pair doit donner accès aux pagesA, B, C etD.
- Au niveau2, les éléments de navigation pair à pair des pagesA2 doivent uniquement être liés aux autres pagesA2. Ils ne doivent pas renvoyer aux pages de niveau2 de la sous-arborescenceC.

![application avec deux sous-arborescences](images/nav/nav-subtrees2.png)

## <a name="use-the-right-controls"></a>Utilisez les contrôles appropriés
Une fois que vous avez choisi votre structure de page, vous devez déterminer comment les utilisateurs navigueront à travers ces pages. La plateformeUWP fournit une variété de contrôles de navigation pour garantir une expérience de navigation cohérente et fiable dans votre application. 

Nous vous recommandons de sélectionner un contrôle de navigation en fonction du nombre d’éléments de navigation dans votre application. Si vous disposez de cinq éléments de navigation ou moins, utilisez la navigation de niveau supérieur, comme les [onglets et les sélecteurs de vue](../controls-and-patterns/tabs-pivot.md). Si vous avez plus de cinqéléments de navigation, utilisez la navigation de gauche, notamment l’[affichage de navigation](../controls-and-patterns/navigationview.md) ou [la vue maître/détails](../controls-and-patterns/master-details.md).

<div class="mx-responsive-img">

<table>
<tr>
    <th>Contrôle</th>
    <th>Description</th>
</tr>
<tr>
    <td><a href="https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame">Trame</a><br/><br/>
    <img src="images/frame.png" alt="Frame" /></td>
    <td>Affiche des pages. <p>À quelques exceptions près, toutes les applications dotées de plusieurs pages utilisent une trame. Généralement, l’application est dotée d’une page principale qui contient la trame et un élément de navigation principal, comme un contrôle d’affichage de la navigation. Lorsque l’utilisateur sélectionne une page, la trame se charge et l’affiche.</p></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/tabs-pivot.md">Onglets et sélecteur de vue</a><br/><br/>
    <img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></td>
    <td>Affiche une liste horizontale de liens vers les pages de même niveau.
<p>Contexte d’utilisation:</p>
<ul>
<li>il y a entre 2 et 5pages. (Utilisez les onglets/pivots lorsqu’il y a plus de 5 pages, mais qu’il peut être difficile d’ajuster l’ensemble des onglets/pivots à l’écran.)</li>
<li>Vous pensez que les utilisateurs passeront fréquemment d’une page à l’autre.</li>
</ul></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/navigationview.md">Affichage de navigation</a><br/><br/>
    <img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></td>
    <td>Affiche une liste vertical de liens vers les pages de niveau supérieur.
<p>Contexte d’utilisation:</p>
<ul>
<li>Les pages se trouvent au niveau supérieur.</li>
<li>Il existe de nombreux éléments de navigation (plus de 5).</li>
<li>Vous ne pensez pas que les utilisateurs passeront fréquemment d’une page à l’autre.</li>

</ul></td>
</tr>
<tr>
<td><a href="../controls-and-patterns/master-details.md">Maître/Détails</a><br/><br/>
<img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></td>
<td>Affiche la liste (affichage Maître) des éléments. La sélection d’un élément affiche sa page dans la section Détails.
<p>Contexte d’utilisation:</p>
<ul>
<li>Vous pensez que les utilisateurs passeront fréquemment d’un élément enfant à un autre.</li>
<li>Vous voulez permettre à l’utilisateur d’effectuer des opérations générales, comme la suppression ou le tri, sur des éléments individuels ou des groupes d’éléments. Vous voulez également lui permettre d’afficher ou de mettre à jour les détails de chaque élément.</li>
</ul>
<p>Les éléments maîtres/détails sont particulièrement bien adaptés aux boîtes de réception, aux listes de contacts et à l’entrée de données.</p>
</td>
<tr>
<td><a href="../controls-and-patterns/hub.md">Hub</a><br/><br/>
<img src="images/hub.png" alt="Hub" /></td>
<td> Affiche les sections d’articles commandés dans les grilles. 
<p>Contexte d’utilisation:</p>
<ul>
<li>Vous voulez créer une navigation visuelle avec des images de bannière.</li>
</ul>
<p>Les hubs sont bien adaptés aux expériences de navigation, de consultation ou d’achat.</p>
</td>
</tr>
<tr>
<td><a href="../controls-and-patterns/hyperlinks.md">Liens hypertexte</a> et <a href="../controls-and-patterns/buttons.md">boutons</a></td>
<td> Les éléments de navigation incorporés apparaissent dans le contenu d’une page. Contrairement aux autres éléments de navigation, qui doivent être cohérents dans toutes les pages, les éléments de navigation incorporés au contenu sont uniques d’une page à l’autre.</td>
</tr>
</table>
</div>

## <a name="next-add-navigation-code-to-your-app"></a>Étape suivante: Ajouter du code de navigation à votre application
L’article suivant [Implémenter la navigation de base](navigate-between-two-pages.md), indique le code nécessaire pour utiliser un contrôle de trame afin d’activer la navigation de base entre deux pages de votre application. 