---
author: mijacobs
Description: "La navigation dans les applications de plateforme Windows universelle (UWP) est basée sur un modèle flexible de structures et d’éléments de navigation, ainsi que de fonctionnalités au niveau du système."
title: "Informations de base relatives à la navigation pour les applications UWP (applications Windows)"
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 72596a2d902dff72af76c84115bc1eb0be8fa743
ms.lasthandoff: 02/07/2017

---

#  <a name="navigation-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception de la navigation pour les applications UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

La navigation dans les applications de plateforme Windows universelle (UWP) est basée sur un modèle flexible de structures et d’éléments de navigation, ainsi que de fonctionnalités au niveau du système. Ensemble, ces composantes permettent d’offrir diverses expériences utilisateur intuitives de déplacement entre les applications, les pages et dans leur contenu.

Dans certains cas, vous pouvez faire tenir la totalité du contenu et des fonctionnalités de votre application sur une seule page, de sorte que l’utilisateur n’ait plus qu’à faire défiler celle-ci pour en parcourir le contenu. Toutefois, la plupart des applications comptent généralement plusieurs pages de contenu et fonctionnalités permettant d’explorer et d’interagir. Si une application possède plusieurs pages, vous devez fournir une expérience de navigation adaptée.

Pour être réussie et sensée pour l’utilisateur, une expérience de navigation multipage dans une application UWP inclut ce qui suit (décrit plus loin en détail) :

-   **Structure de navigation appropriée**

    La création d’une structure de navigation logique pour l’utilisateur est essentielle pour assurer une expérience de navigation intuitive.

-   **Éléments de navigation compatibles** qui prennent en charge la structure choisie.

    Les éléments de navigation aident l’utilisateur à accéder au contenu souhaité et à savoir où il se trouve dans l’application. Toutefois, ils occupent aussi l’espace que l’application pourrait utiliser pour les éléments de contenu ou de commande. Il est donc important d’utiliser les éléments de navigation qui sont adaptés à la structure de votre application.

-   **Réponses appropriées aux fonctionnalités de navigation au niveau système (par exemple, au bouton Précédent)**

    Pour fournir une expérience cohérente qui semble intuitive, répondez aux fonctionnalités de navigation de niveau système de manière à satisfaire les attentes des utilisateurs.

## <a name="build-the-right-navigation-structure"></a>Créer la structure de navigation appropriée


Considérons une application comme une collection de groupes de pages, chaque page contenant un ensemble unique de contenu ou de fonctionnalités. Par exemple, une application de photos peut avoir une page pour la prise des photos, une page pour la modification d’image et une autre pour la gestion de votre bibliothèque d’images. La manière dont vous organisez ces pages en groupes définit la structure de navigation de l’application. Deux méthodes sont couramment utilisées pour organiser un groupe de pages :

<table class="uwpd-noborder uwpd-top-aligned-table">
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Dans une hiérarchie</th>
<th align="left">En tant qu’homologues</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;"><p><img src="images/nav/nav-pages-hiearchy.png" alt="Pages arranged in a hierarchy" /></p></td>
<td style="text-align: center;"><p><img src="images/nav/nav-pages-peer.png" alt="Pages arranged as peers" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align: top">Les pages sont organisées selon une structure arborescente. Chaque page enfant n’a qu’un parent, mais un parent peut avoir une ou plusieurs pages enfants. Pour atteindre une page enfant, vous naviguez via le parent. </td>
<td style="vertical-align: top"> Les pages se trouvent côte à côte. Vous pouvez aller d’une page à une autre dans n’importe quel ordre. </td>
</tr>
</tbody>
</table>

 

Une application standard utilise les deux dispositions, certaines parties étant organisées en tant qu’homologues et d’autres l’étant en hiérarchies.

![Application à structure hybride](images/nav/nav-hybridstructure.png.png)

Quand devez-vous organiser les pages en hiérarchies et quand devez-vous les organiser en tant qu’homologues ? Pour répondre à cette question, nous devons prendre en compte le nombre de pages contenues dans le groupe, si les pages doivent être parcourues dans un ordre particulier et la relation entre les pages. En règle générale, les structures plus plates sont plus faciles à comprendre et la navigation y est plus aisée, mais il convient parfois de disposer d’une hiérarchie à plusieurs niveaux.



<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">Nous recommandons une relation hiérarchique lorsque :
<ul>
<li>Vous pensez que l’utilisateur va parcourir les pages dans un ordre spécifique. Organisez la hiérarchie pour appliquer cet ordre.</li>
<li>Il existe une relation parent-enfant clairement définie entre l’une des pages et les autres pages du groupe.</li>
<li>Le groupe contient plus de 7 pages.
<p>S’il y a plus de 7 pages dans le groupe, il peut être difficile pour les utilisateurs de comprendre dans quelle mesure les pages sont uniques ou de connaître leur emplacement actuel au sein du groupe. Si vous ne pensez pas que ce soit un problème pour votre application, lancez-vous et faites des pages des homologues. Sinon, envisagez d’utiliser une structure hiérarchique pour répartir les pages en deux groupes au moins plus petits. (Un contrôle hub peut vous aider à regrouper des pages en catégories.)</p></li>
</ul>
  </div>
  <div class="side-by-side-content-right">Nous recommandons l’utilisation d’une relation d’homologue lorsque :
<ul>
<li>Les pages peuvent être affichées dans n’importe quel ordre.</li>
<li>Les pages sont clairement distinctes les unes des autres et n’ont aucune relation parent/enfant évidente.</li>
<li><p>Le groupe contient moins de 8 pages.</p>
<p>S’il y a plus de 7 pages dans le groupe, il peut être difficile pour les utilisateurs de comprendre dans quelle mesure les pages sont uniques ou de connaître leur emplacement actuel au sein du groupe. Si vous ne pensez pas que ce soit un problème pour votre application, lancez-vous et faites des pages des homologues. Sinon, envisagez d’utiliser une structure hiérarchique pour répartir les pages en deux groupes au moins plus petits. (Un contrôle hub peut vous aider à regrouper des pages en catégories.)</p></li>
</ul>
  </div>
</div>
</div>
 

## <a name="use-the-right-navigation-elements"></a>Utiliser les éléments de navigation appropriés


Les éléments de navigation peuvent fournir deux services : ils aident l’utilisateur à accéder au contenu souhaité, et certains éléments lui permettent également de savoir où il se trouve dans l’application. Cependant, ils occupent aussi l’espace que l’application pourrait utiliser pour les éléments de contenu ou de commande. Il est donc important d’utiliser les éléments de navigation qui sont adaptés à la structure de votre application.

### <a name="peer-to-peer-navigation-elements"></a>Éléments de navigation pair à pair

Les éléments de navigation pair à pair permettent de naviguer entre les pages de même niveau d’une même sous-arborescence.

![Navigation pair à pair](images/nav/nav-lateralmovement.png)

Pour la navigation pair à pair, nous vous recommandons d’utiliser des onglets ou un volet de navigation.

<table>
<thead>
<tr class="header">
<th align="left">Élément de navigation</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">[Onglets et pivot](../controls-and-patterns/tabs-pivot.md)
<p><img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></p></td>
<td style="vertical-align:top;">Affiche une liste permanente de liens vers les pages du même niveau.
<p>Utilisez les onglets/pivots lorsque.</p>
<ul>
<li><p>Il y a 2 à 5 pages.</p>
<p>(Utilisez les onglets/pivots lorsqu’il y a plus de 5 pages, mais qu’il peut être difficile d’ajuster l’ensemble des onglets/pivots à l’écran.)</p></li>
<li>Vous pensez que les utilisateurs passeront fréquemment d’une page à l’autre.</li>
</ul>
<p>Cette conception pour une application de recherche de restaurants utilise des onglets/pivots :</p>
<p><img src="images/food-truck-finder/uap-foodtruck-tabletphone-sbs-sm-400.png" alt="Example of an app using tabs/pivots pattern" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;">[Volet de navigation](../controls-and-patterns/nav-pane.md)
<p><img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></p></td>
<td style="vertical-align:top;">Affiche une liste de liens vers les pages de niveau supérieur.
<p>Utilisez un volet de navigation lorsque.</p>
<ul>
<li>Vous ne pensez pas que les utilisateurs passeront fréquemment d’une page à l’autre.</li>
<li>Vous souhaitez économiser de l’espace, même si cela ralentit les opérations de navigation.</li>
<li>Les pages se trouvent au niveau supérieur.</li>
</ul>
<p>Cette conception pour une application de maison intelligente utilise un volet de navigation :</p>
<p><img src="images/smart-home/uap-smarthome-tabletphone-sbs-sm-400.png" alt="Example of an app that uses a nav pane pattern" /></p>
<p></p></td>
</tr>
</tbody>
</table>

 

Si votre structure de navigation comporte plusieurs niveaux, nous vous recommandons de lier les éléments de navigation pair à pair uniquement aux homologues au sein de leur sous-arborescence actuelle. Prenez en considération l’illustration suivante, qui montre une structure de navigation à trois niveaux :

![Application avec deux sous-arborescences](images/nav/nav-subtrees.png)
-   Pour le niveau 1, l’élément de navigation pair à pair doit donner accès aux pages A, B, C et D.
-   Au niveau 2, les éléments de navigation pair à pair des pages A2 doivent uniquement être liés aux autres pages A2. Ils ne doivent pas renvoyer aux pages de niveau 2 de la sous-arborescence C.

![Application avec deux sous-arborescences](images/nav/nav-subtrees2.png)

### <a name="hierarchical-navigation-elements"></a>Éléments de navigation hiérarchique

Les éléments de navigation hiérarchique permettent de naviguer entre une page parente et ses pages enfants.

![Navigation hiérarchique](images/nav/nav-verticalmovement.png)

<table>
<thead>
<tr class="header">
<th align="left">Élément de navigation</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">[Hub](../controls-and-patterns/hub.md)
<p><img src="images/higsecone-hub-thumb.png" alt="Hub" /></p></td>
<td align="left">Un hub est un type spécial de contrôle de navigation qui fournit des aperçus/synthèses de ses pages enfants. Contrairement aux onglets ou au volet de navigation, il permet de naviguer vers ces pages enfants via des liens et des en-têtes de section, incorporés dans la page proprement dite.
<p>Utilisez un hub lorsque.</p>
<ul>
<li>Vous pensez que l’utilisateur souhaitera afficher une partie du contenu des pages enfants sans devoir naviguer vers chacune d’elles.</li>
</ul>
<p>Les hubs favorisent la découverte et l’exploration, ce qui les rend particulièrement bien adaptés aux applications multimédias, d’informations et d’achat.</p>
<p></p></td>
</tr>

<tr class="even">
<td style="vertical-align:top;">[Maître/Détails](../controls-and-patterns/master-details.md)
<p><img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></p></td>
<td align="left">Affiche la liste (affichage Maître) des résumés des éléments. La sélection d’un élément affiche sa page d’éléments correspondante dans la section Détails.
<p>Utilisez l’élément maître/détails lorsque.</p>
<ul>
<li>Vous pensez que les utilisateurs passeront fréquemment d’un élément enfant à un autre.</li>
<li>Vous voulez permettre à l’utilisateur d’effectuer des opérations générales, comme la suppression ou le tri, sur des éléments individuels ou des groupes d’éléments. Vous voulez également lui permettre d’afficher ou de mettre à jour les détails de chaque élément.</li>
</ul>
<p>Les éléments maîtres/détails sont particulièrement bien adaptés aux boîtes de réception, aux listes de contacts et à l’entrée de données.</p>
<p>Cette conception pour une application de suivi d’inventaire utilise un modèle Maître/Détails :</p>
<p><img src="images/stock-tracker/uap-finance-tabletphone-sbs-sm.png" alt="Example of a stock trading app that has a master/details pattern" /></p></td>
</tr>
</tbody>
</table>

 

### <a name="historical-navigation-elements"></a>Éléments de navigation historique

<table>
<thead>
<tr class="header">
<th align="left">Élément de navigation</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">[Précédent](navigation-history-and-backwards-navigation.md)</td>
<td style="vertical-align:top;">Permet à l’utilisateur de parcourir l’historique de navigation dans une application et, en fonction de l’appareil, d’une application à l’autre. Pour en savoir plus, consultez l’article [Historique de navigation et navigation vers l’arrière](navigation-history-and-backwards-navigation.md).</td>
</tr>
</tbody>
</table>

 

### <a name="content-level-navigation-elements"></a>Éléments de navigation au niveau du contenu

<table>
<thead>
<tr class="header">
<th align="left">Élément de navigation</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">Liens hypertexte et boutons</td>
<td style="vertical-align:top;">Les éléments de navigation incorporés au contenu apparaissent dans le contenu d’une page. Contrairement aux autres éléments de navigation, qui doivent être cohérents dans un groupe ou une sous-arborescence de page, les éléments de navigation incorporés au contenu sont uniques d’une page à l’autre.</td>
</tr>
</tbody>
</table>

 

### <a name="combining-navigation-elements"></a>Combinaison d’éléments de navigation

Vous pouvez combiner des éléments de navigation pour créer une expérience de navigation appropriée pour votre application. Par exemple, votre application peut utiliser un volet de navigation pour accéder aux pages de niveau supérieur et des onglets pour accéder aux pages de deuxième niveau.






 





