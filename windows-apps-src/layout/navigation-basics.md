---
author: mijacobs
Description: "La navigation dans les applications de plateformeWindows universelle (UWP) est basée sur un modèle flexible de structures et d’éléments de navigation, ainsi que de fonctionnalités au niveau du système."
title: "Informations de base relatives à la navigation pour les applicationsUWP"
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: a944e02ea116c0e054918397d9d46d366d43622a
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2017
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception de la navigation pour les applicationsUWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Si vous imaginez qu'une application est une collection de pages, le terme *navigation* décrit le fait de se déplacer entre ces pages et au sein d'une page. Il s'agit du point de départ de l’expérience utilisateur. C'est de cette manière que les utilisateurs trouvent le contenu et les fonctionnalités qui les intéressent. C'est un élément très important, qui peut être difficile à réaliser correctement. 

> **API importantes**: [Trame](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame), [Classe Pivot](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Pivot), [Classe NavigationView](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

Cette difficulté s'explique en partie par le fait que, en tant que concepteurs d’applications, nous avons un très grand nombre de choix à faire. Si nous concevions un livre, nos choix seraient simples: il faudrait choisir l'ordre des chapitres. Avec une application, nous pouvons créer une expérience de navigation qui simule un livre, en obligeant l’utilisateur à parcourir une série de pages dans un certain ordre. Ou bien, nous pouvons fournir un menu qui permet à l’utilisateur d’accéder directement à n’importe quelle page de son choix; toutefois si nous avons un trop grand nombre de pages, l'utilisateur peut se sentir dépassé par tous ces choix. Ou bien, nous pourrions tout placer sur une seule page et fournir des mécanismes de filtrage pour afficher le contenu. 

Il n’existe aucune conception de navigation unique qui fonctionne pour toutes les applications, mais il existe un ensemble de principes et de recommandations, que vous pouvez suivre pour mieux déterminer la conception appropriée à votre application. 

## <a name="principles-of-good-design"></a>Principes de conception 
Commençons par les principes de base qui forment la base d'une conception de navigation efficace: 

* Soyez cohérent: répondre aux attentes des utilisateurs.
* Restez simple: ne faites pas plus que nécessaire.
* Restez épuré: ne laissez pas les fonctionnalités de navigation perturber l’utilisateur.

### <a name="be-consistent"></a>Soyez cohérent 
La navigation doit être cohérente avec les attentes des utilisateurs, et s'appuyer sur les conventions standard pour les icônes, l’emplacement et le style. 

Par exemple, dans l’illustration suivante, vous pouvez voir les zones où les utilisateurs espèrent en général trouver des fonctionnalités, telles que le volet de navigation et la barre de commande. Les différentes familles d’appareils détiennent leurs propres conventions pour les éléments de navigation. Par exemple, le volet de navigation apparaît généralement sur le côté gauche de l’écran pour les tablettes, mais sur la partie supérieure pour les périphériques mobiles.

<figure class="wdg-figure">
  ![Emplacement par défaut des éléments de navigation](images/nav/nav-component-layout.png)
  <figcaption>Les utilisateurs s’attendent à trouver certains éléments d’interface utilisateur dans des emplacements standard.</figcaption>
</figure> 

### <a name="keep-it-simple"></a>La simplicité avant tout
La loi Hick-Hyman est un autre facteur important de la conception de la navigation, souvent citée pour les options de navigation. Cette loi nous encourage à ajouter moins d’options dans le menu. Plus nombreuses sont les options, plus les interactions utilisateur seront lentes, surtout lorsque les utilisateurs explorent une nouvelle application. 

<figure class="wdg-figure">
  ![Comparaison entre un menu simple et un menu complexe](images/nav/nav-simple-menus.png)
  <figcaption> Sur la gauche, vous remarquerez que l'utilisateur dispose d'un nombre réduit d'options à sa disposition, alors qu'il y en a davantage sur la droite. La loi Hick-Hyman indique que le menu de gauche sera plus facile à comprendre et à utiliser pour les utilisateurs.
</figcaption>
</figure> 

### <a name="keep-it-clean"></a>Une présentation épurée
Une interaction épurée constitue la caractéristique clé finale de la navigation, laquelle fait référence à la manière physique dont les utilisateurs interagissent avec la navigation dans des contextes différents. Vous devez vous mettre à la place des utilisateurs pour mieux guider votre conception. Essayez de comprendre vos utilisateurs et leurs comportements. Par exemple, si vous créez une application de cuisine, vous pouvez envisager de fournir un accès aisé à une liste d'achat et à un minuteur. 

## <a name="three-general-rules"></a>Trois règles générales
Maintenant nous allons utiliser nos principes de conception (cohérence, simplicité et interaction épurée) pour élaborer des règles générales. Comme pour n’importe quelle règle générale, utilisez ces concepts comme point de départ et adaptez-les à vos besoins. 

1. Évitez les hiérarchies de navigation profondes. Quel nombre de niveaux de navigation est idéal pour vos utilisateurs? Une navigation au niveau supérieur avec un niveau juste en-dessous est en général largement suffisant. Au-delà de trois niveaux de navigation, vous rompez le principe de simplicité. Pire encore, vous risquez d’égarer votre utilisateur dans une hiérarchie profonde qu’il aura des difficultés à quitter.

2. Évitez de fournir un trop grand nombre d’options de navigation. Les présentations les plus courantes comptent trois à six éléments de navigation par niveau. Si votre navigation a besoin de davantage de niveaux, en particulier au niveau supérieur de votre hiérarchie, envisagez alors de diviser votre application en plusieurs applications, car vous essayez peut-être d'intégrer trop d'options dans un seul et même emplacement. Un trop grand nombre d'éléments de navigation dans une application conduit généralement à des objectifs incohérents.

3. Évitez l'effet «bâton sauteur» L'effet du bâton sauteur se produit lorsqu’il existe du contenu associé, mais que la navigation vers celui-ci oblige l'utilisateur à remonter d'un niveau puis à descendre à nouveau. Le phénomène de bâton sauteur entrave le principe d'interaction épurée car il exige des clics ou des interactions inutiles pour atteindre un objectif évident, dans ce cas précis, accéder à des contenus associés dans une série. (L’exception à cette règle sont les opérations de recherche et de navigation, où le phénomène de bâton sauteur peut être la seule façon de fournir la diversité et la profondeur requises).
<figure class="wdg-figure">
  ![Exemple de phénomène de bâton sauteur](images/nav/nav-pogo-sticking-1.png)
  <figcaption> Phénomène de bâton sauteur pour naviguer dans une application: l’utilisateur doit revenir en arrière (flèche verte Précédent) vers la page principale afin d’accéder à l’onglet «Projets».
</figcaption>
</figure> 
<figure class="wdg-figure">
  ![La navigation latérale, via un balayage, résout le problème de bâton sauteur](images/nav/nav-pogo-sticking-2.png)
  <figcaption>Vous pouvez résoudre certains problèmes relatifs au phénomène de bâton sauteur en insérant une icône (notez le geste de balayage en vert).
</figcaption>
</figure> 


## <a name="use-the-right-structure"></a>Utiliser la structure appropriée 
Maintenant que vous êtes familiarisé avec les principes de navigation et les règles générales, il est temps de prendre la plus importante de toutes les décisions de navigation: comment allez-vous structurer votre application? Il existe deux structures générales: plate et hiérarchique. 

### <a name="flatlateral"></a>Plate/latérale
Dans une structure plate/latérale, les pages se trouvent côte-à-côte. Vous pouvez aller d’une page à une autre dans n’importe quel ordre. 
<figure class="wdg-figure">
  <img src="images/nav/nav-pages-peer.png" alt="Pages arranged in a flat structure" />
<figcaption>Pages organisées dans une structure plate</figcaption>
</figure> 

Les structures plates présentent de nombreux avantages: elles sont simples et faciles à comprendre, et elles permettent à l’utilisateur d’accéder directement à une page spécifique sans avoir à passer en revue les pages intermédiaires.  En règle générale, les structures plates sont très efficaces, mais elles ne fonctionnent pas pour toutes les applications. Si votre application comporte un grand nombre de pages, une liste plate peut être imposante. Si les pages doivent être affichées dans un ordre particulier, une structure plate ne sera pas la solution appropriée. 

Nous vous recommandons d’utiliser une structure plate dans les cas suivants: 
<ul>
<li>Les pages peuvent être affichées dans n’importe quel ordre.</li>
<li>Les pages sont clairement distinctes les unes des autres et n’ont aucune relation parent/enfant évidente.</li>
<li>Le groupe contient moins de 8 pages.<br/>
S’il y a plus de 7 pages dans le groupe, il peut être difficile pour les utilisateurs de comprendre dans quelle mesure les pages sont uniques ou de connaître leur emplacement actuel au sein du groupe. Si vous ne pensez pas que ce soit un problème pour votre application, lancez-vous et faites des pages des homologues. Sinon, envisagez d’utiliser une structure hiérarchique pour répartir les pages en deux groupes au moins plus petits. (Un contrôle hub peut vous aider à regrouper des pages en catégories.)</li>
</ul>


### <a name="hierarchical"></a>Hiérarchique

Dans une structure hiérarchique, les pages sont organisées dans une structure arborescente. Chaque page enfant n’a qu’un parent, mais un parent peut avoir une ou plusieurs pages enfants. Pour atteindre une page enfant, vous naviguez via le parent.

<figure class="wdg-figure">
  <img src="images/nav/nav-pages-hiearchy.png" alt="Pages arranged in a hierarchy" />
<figcaption>Pages organisées dans une hiérarchie</figcaption>
</figure>

Les structures hiérarchiques sont parfaites pour l'organisation de contenus complexes qui s’étendent sur de nombreuses pages, ou lorsque les pages doivent être affichées dans un ordre particulier. L’inconvénient de cette structure, est que les pages hiérarchiques introduisent une surcharge de navigation: plus la structure est profonde, plus les utilisateurs doivent cliquer pour naviguer entre les pages. 

Nous recommandons une structure hiérarchique dans les cas suivants: 
<ul>
<li>Vous pensez que l’utilisateur va parcourir les pages dans un ordre spécifique. Organisez la hiérarchie pour appliquer cet ordre.</li>
<li>Il existe une relation parent-enfant clairement définie entre l’une des pages et les autres pages du groupe.</li>
<li>Le groupe contient plus de 7pages.<br/>
S’il y a plus de 7pages dans le groupe, il peut être difficile pour les utilisateurs de comprendre dans quelle mesure les pages sont uniques ou de connaître leur emplacement actuel au sein du groupe. Si vous ne pensez pas que ce soit un problème pour votre application, lancez-vous et faites des pages des homologues. Sinon, envisagez d’utiliser une structure hiérarchique pour répartir les pages en deux groupes au moins plus petits. (Un contrôle hub peut vous aider à regrouper des pages en catégories.)</li>
</ul>

### <a name="combining-structures"></a>Combinaison de structures
Vous n’avez pas besoin de choisir entre une structure ou une autre; de nombreuses applications très bien conçues utilisent à la fois les structures plates et hiérarchiques:

![application à structure hybride](images/nav/nav-hybridstructure.png.png)

Ces applications utilisent des structures plates pour les pages de niveau supérieur qui peuvent être affichées dans n’importe quel ordre, et des structures hiérarchiques pour les pages qui ont des relations plus complexes. 

Si votre structure de navigation comporte plusieurs niveaux, nous vous recommandons de lier les éléments de navigation pair à pair uniquement aux homologues au sein de leur sous-arborescence actuelle. Prenez en considération l’illustration suivante, qui montre une structure de navigation à trois niveaux:

![Application avec deux sous-arborescences](images/nav/nav-subtrees.png)
-   Pour le niveau1, l’élément de navigation pair à pair doit donner accès aux pagesA, B, C etD.
-   Au niveau2, les éléments de navigation pair à pair des pagesA2 doivent uniquement être liés aux autres pagesA2. Ils ne doivent pas renvoyer aux pages de niveau2 de la sous-arborescenceC.

![application avec deux sous-arborescences](images/nav/nav-subtrees2.png)
 

## <a name="use-the-right-controls"></a>Utilisez les contrôles appropriés

Une fois que vous avez choisi votre structure de page, vous devez déterminer comment les utilisateurs navigueront à travers ces pages. UWP fournit une variété de contrôles de navigation pour vous aider. Étant donné que ces contrôles sont disponibles pour chaque application UWP, leur utilisation permet de garantir une expérience de navigation cohérente et fiable. 


<table>
<tr>
    <th>Contrôle</th>
    <th>Description</th>
</tr>
<tr>
    <td>[Trame](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame)</td>
    <td>À quelques exceptions près, toutes les applications dotées de plusieurs pages utilisent une trame. Dans une installation classique, l’application est dotée d'une page principale qui contient la trame et un élément de navigation principal, comme un contrôle d’affichage de la navigation. Lorsque l’utilisateur sélectionne une page, la trame se charge et l’affiche.</td>
</tr>
<tr>
    <td>[Onglets et pivot](../controls-and-patterns/tabs-pivot.md)<br/><br/>
    <img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></td>
    <td>Affiche une liste de liens vers les pages du même niveau.
<p>Utilisez les onglets/pivots dans les cas suivants:</p>
<ul>
<li><p>Il y a 2 à 5 pages.</p>
<p>(Utilisez les onglets/pivots lorsqu’il y a plus de 5 pages, mais qu’il peut être difficile d’ajuster l’ensemble des onglets/pivots à l’écran.)</p></li>
<li>Vous pensez que les utilisateurs passeront fréquemment d’une page à l’autre.</li>
</ul></td>
</tr>
<tr>
    <td>[Mode de navigation](../controls-and-patterns/navigationview.md)<br/><br/>
    <img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></td>
    <td>Affiche une liste de liens vers les pages de niveau supérieur.
<p>Utilisez un volet de navigation lorsque.</p>
<ul>
<li>Vous ne pensez pas que les utilisateurs passeront fréquemment d’une page à l’autre.</li>
<li>Vous souhaitez économiser de l’espace, même si cela ralentit les opérations de navigation.</li>
<li>Les pages se trouvent au niveau supérieur.</li>
</ul></td>
</tr>
<tr>
<td>[Maître/Détails](../controls-and-patterns/master-details.md)<br/><br/>
<img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></td>
<td>Affiche la liste (affichage Maître) des résumés des éléments. La sélection d’un élément affiche sa page d’éléments correspondante dans la section Détails.
<p>Utilisez l’élément maître/détails lorsque.</p>
<ul>
<li>Vous pensez que les utilisateurs passeront fréquemment d’un élément enfant à un autre.</li>
<li>Vous voulez permettre à l’utilisateur d’effectuer des opérations générales, comme la suppression ou le tri, sur des éléments individuels ou des groupes d’éléments. Vous voulez également lui permettre d’afficher ou de mettre à jour les détails de chaque élément.</li>
</ul>
<p>Les éléments maîtres/détails sont particulièrement bien adaptés aux boîtes de réception, aux listes de contacts et à l’entrée de données.</p>
</td>
</tr>
<tr>
<td s>[Précédent](navigation-history-and-backwards-navigation.md)</td>
<td style="vertical-align:top;">Permet à l’utilisateur de parcourir l’historique de navigation dans une application et, en fonction de l’appareil, d’une application à l’autre. Pour en savoir plus, consultez l’article [Historique de navigation et navigation vers l’arrière](navigation-history-and-backwards-navigation.md).</td>
</tr>
<tr class="odd">
<td>Liens hypertexte et boutons</td>
<td>Les éléments de navigation incorporés au contenu apparaissent dans le contenu d’une page. Contrairement aux autres éléments de navigation, qui doivent être cohérents dans un groupe ou une sous-arborescence de page, les éléments de navigation incorporés au contenu sont uniques d’une page à l’autre.</td>
</tr>
</table>

## <a name="next-add-navigation-code-to-your-app"></a>Étape suivante: Ajouter du code de navigation à votre application
L’article suivant [Implémenter la navigation de base](navigate-between-two-pages.md), indique les fichiers XAML et le code nécessaires pour utiliser un contrôle de trame afin d'activer la navigation de base dans votre application. 


<!--
## History and the back button
UWP provides a back button and a system for traversing the user's navigation hsitory within an app. This system does most of the work for you, but there are a few APIs you need to call so that it works properly. For more info and instructions, see [History and backwards navigation](navigation-history-and-backwards-navigation.md). 
-->





