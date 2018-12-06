---
Description: Lists display and enable interaction with collection-based content.
title: Listes
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Lists
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d8ea08cd02314fb566680d8f5b249eaf735b977a
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8746083"
---
# <a name="lists"></a>Listes

Les listes affichent et activent l’interaction avec du contenu basé sur des collections. Les quatre modèles de liste traités dans cet article sont les suivants:

-   Affichages Liste, principalement utilisés pour afficher des collections de contenus riches en texte
-   Affichages Grille, principalement utilisés pour afficher des collections de contenus riches en images
-   Listes déroulantes, permettant aux utilisateurs de choisir un élément dans une liste développée
-   Zones de liste, permettant aux utilisateurs de choisir un ou plusieurs éléments dans une zone pouvant défiler

Des recommandations en matière de conception, des fonctionnalités et des exemples sont fournis pour chaque modèle de liste.

> **API importantes**: [classe ListView](https://msdn.microsoft.com/library/windows/apps/br242878), [classe GridView](https://msdn.microsoft.com/library/windows/apps/br242705), [classe ComboBox](https://msdn.microsoft.com/library/windows/apps/br209348)


> <div id="main">
> <strong>Windows10 Fall Creators Update - Changement de comportement</strong>
> </div>
> Par défaut, au lieu d’effectuer la sélection, un stylet actif fait défiler/parcourt une liste dans les applications UWP (comme l’interaction tactile, le pavé tactile et le stylet passif).
> Si votre application repose sur le comportement précédent, vous pouvez remplacer le défilement du stylet et rétablir le comportement précédent. Consultez la rubrique de référence de [Classe ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer) API pour plus d’informations.

## <a name="list-views"></a>Affichages Liste

Les affichages Liste permettent de classer des éléments et d’affecter des en-têtes de groupe, de glisser-déplacer des éléments, de traiter du contenu et de réorganiser des éléments.

### <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un affichage Liste pour :

-   Afficher une collection de contenus principalement composée de texte.
-   Naviguer dans une collection de contenu unique ou catégorisée.
-   Créer le volet principal dans le [modèle Maître/Détails](master-details.md). Un modèle Maître/Détails est souvent utilisé dans les applications de messagerie, dans lesquelles un volet (maître) contient une liste d’éléments sélectionnables, tandis que l’autre (détails) affiche une vue détaillée de l’élément sélectionné.

### <a name="examples"></a>Exemples

Voici un affichage Liste simple montrant des données regroupées sur un téléphone.

![Un affichage Liste avec des données regroupées](images/simple-list-view-phone.png)

### <a name="recommendations"></a>Recommandations

-   Les éléments d’une liste doivent avoir le même comportement.
-   Si votre liste est répartie en groupes, vous pouvez utiliser un [zoom sémantique](semantic-zoom.md) pour permettre aux utilisateurs de naviguer plus facilement dans un contenu regroupé.

### <a name="list-view-articles"></a>Articles sur l’affichage Liste
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">Affichage Liste et affichage Grille</a></p></td>
<td align="left"><p>Découvrez les principaux éléments de l’utilisation d’un affichage Liste ou Grille dans votre application.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">Modèles et conteneurs d’éléments</a></p></td>
<td align="left"><p>Les éléments que vous affichez dans une liste ou une grille peuvent jouer un rôle majeur dans l’apparence générale de votre application. Modifiez les modèles de contrôle et les modèles de données pour définir l’apparence des éléments et donner un aspect satisfaisant à votre application.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">Modèles d’éléments d'affichage Liste</a></p></td>
<td align="left"><p>Utilisez ces exemples de modèles d'éléments de ListView pour obtenir l’apparence des types d’applications courants.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">Listes inversées</a></p></td>
<td align="left"><p>Les listes inversées placent les nouveaux éléments ajoutés en bas, comme dans une application de chat. Suivez ces conseils pour utiliser une liste inversée dans votre application.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">Tirer pour actualiser</a></p></td>
<td align="left"><p>Le modèle Tirer pour actualiser permet à l’utilisateur de dérouler une liste de données à l’aide de la fonction tactile pour récupérer plus de données. Utilisez ces instructions pour implémenter le modèle tirer pour actualiser dans votre affichage Liste.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Interface utilisateur imbriquée</a></p></td>
<td align="left"><p>L’interface utilisateur imbriquée est une interface utilisateur qui expose des contrôles exploitables inclus dans un conteneur, lequel peut également être actionné par l’utilisateur. Par exemple, si un élément de l’affichage Liste contient un bouton, l’utilisateur peut sélectionner l’élément de liste ou appuyer sur le bouton imbriqué à l’intérieur. Suivez ces bonnes pratiques pour fournir la meilleure expérience d’interface utilisateur imbriquée à vos utilisateurs.</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>Affichages Grille

Les affichages Grille conviennent pour l’organisation et l’exploration des collections de contenus à base d’images. Une disposition de liste Grille défile verticalement et s’étend horizontalement. Les éléments sont disposés dans un ordre de lecture de gauche à droite, puis de haut en bas.

### <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un affichage Liste pour :

-   Afficher une collection de contenus composée essentiellement d’images.
-   Afficher des bibliothèques de contenu.
-   Mettre en forme les deux affichages de contenu associés à un [zoom sémantique](semantic-zoom.md).

### <a name="examples"></a>Exemples

Cet exemple illustre une disposition d’affichage Grille standard, dans ce cas pour des applications de navigation. Les métadonnées pour les éléments d’un affichage Grille sont généralement limitées à quelques lignes de texte et à une évaluation.

![Exemple de disposition d’affichage Grille](images/controls_gridview_example02.png)

Un affichage Grille est une solution idéale pour une bibliothèque de contenu, souvent utilisée pour présenter du contenu multimédia tel que des images et vidéos. Dans une bibliothèque de contenu, l’utilisateur s’attend à pouvoir appuyer sur un élément pour appeler une action.

![Exemple de bibliothèque de contenu](images/controls_list_contentlibrary.png)

### <a name="recommendations"></a>Recommandations

-   Les éléments d’une liste doivent avoir le même comportement.
-   Si votre liste est répartie en groupes, vous pouvez utiliser un [zoom sémantique](semantic-zoom.md) pour permettre aux utilisateurs de naviguer plus facilement dans un contenu regroupé.

### <a name="grid-view-articles"></a>Articles sur l’affichage Grille
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">Affichage Liste et affichage Grille</a></p></td>
<td align="left"><p>Découvrez les principaux éléments de l’utilisation d’un affichage Liste ou Grille dans votre application.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">Modèles et conteneurs d’éléments</a></p></td>
<td align="left"><p>Les éléments que vous affichez dans une liste ou une grille peuvent jouer un rôle majeur dans l’apparence générale de votre application. Modifiez les modèles de contrôle et les modèles de données pour définir l’apparence des éléments et donner un aspect satisfaisant à votre application.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">Modèles d’éléments d'affichage Grille</a></p></td>
<td align="left"><p>Utilisez ces exemples de modèles d'éléments de GridView pour obtenir l’apparence des types d’applications courants.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Interface utilisateur imbriquée</a></p></td>
<td align="left"><p>L’interface utilisateur imbriquée est une interface utilisateur qui expose des contrôles exploitables inclus dans un conteneur, lequel peut également être actionné par l’utilisateur. Par exemple, si un élément de l’affichage Liste contient un bouton, l’utilisateur peut sélectionner l’élément de liste ou appuyer sur le bouton imbriqué à l’intérieur. Suivez ces bonnes pratiques pour fournir la meilleure expérience d’interface utilisateur imbriquée à vos utilisateurs.</p></td>
</tr>
</tbody>
</table>

## <a name="drop-down-lists"></a>Listes déroulantes

Les listes déroulantes, également appelées zones de liste déroulante, démarrent dans un état compact et se développent pour afficher une liste d’éléments sélectionnables. L’élément sélectionné est toujours visible, et les éléments non visibles peuvent s’afficher lorsque l’utilisateur appuie sur la zone de liste déroulante pour la développer.

### <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

-   Utilisez une liste déroulante pour permettre aux utilisateurs de sélectionner une valeur unique parmi un ensemble d’éléments qui peuvent être représentés correctement à l’aide de simples lignes de texte.
-   Utilisez un affichage Liste ou Grille au lieu d’une zone de liste déroulante pour afficher des éléments contenant plusieurs lignes de texte ou images.
-   En présence de moins de cinq éléments, utilisez des [cases d’option](radio-button.md) (si un seul élément peut être sélectionné) ou des [cases à cocher](checkbox.md) (si plusieurs éléments peuvent être sélectionnés).
-   Utilisez cette zone de liste déroulante pour les éléments de sélection d’importance secondaire au sein de votre application. Si l’option par défaut est recommandée pour la plupart des utilisateurs dans la majorité des situations, l’affichage de tous les éléments à l’aide d’un affichage Liste risque d’attirer l’attention sur les options plus qu’il n’est nécessaire. Pour économiser de l’espace et éviter de distraire l’utilisateur, utilisez une zone de liste déroulante.

### <a name="examples"></a>Exemples

Une zone de liste déroulante en état compact peut afficher un en-tête.

![Exemple de liste déroulante à l’état compact](images/combo_box_collapsed.png)

Bien que les zones de listes déroulantes se développent pour prendre en charge des chaînes plus longues, évitez les chaînes excessivement longues qui rendent la lecture difficile.

![Exemple de liste déroulante avec une longue chaîne de texte](images/combo_box_listitemstate.png)

Si la collection figurant dans une zone de liste déroulante est suffisamment longue, une barre de défilement s’affiche. Regroupez les éléments logiquement dans la liste.

![Exemple de barre de défilement dans une liste déroulante](images/combo_box_scroll.png)

### <a name="recommendations"></a>Recommandations

-   Limitez le contenu texte des éléments de zone de liste déroulante à une seule ligne.
-   Triez les éléments d’une zone de liste déroulante dans l’ordre le plus logique. Regroupez les options associées et placez les options les plus courantes en haut. Triez les noms par ordre alphabétique, les nombres par ordre numérique et les dates par ordre chronologique.
-   Pour créer une liste déroulante avec mise à jour dynamique pendant que l’utilisateur utilise les touches de direction (comme une liste déroulante de sélection de police), définissez SelectionChangedTrigger sur «Toujours».  

### <a name="text-search"></a>Recherche en texte

Les zones de liste déroulante prennent automatiquement en charge la recherche au sein de leurs collections. Lorsque les utilisateurs tapent des caractères sur un clavier physique dans une zone de liste déroulante ouverte ou fermée, des candidats correspondant à la chaîne entrée apparaissent. Cette fonctionnalité est particulièrement utile lors de la navigation dans une liste longue. Par exemple, lorsqu’un utilisateur interagit avec une liste déroulante contenant une liste des États américains, le fait d’appuyer sur la touche «W» affiche «Washington» pour une sélection rapide.


## <a name="list-boxes"></a>Zones de liste

Une zone de liste permet à l’utilisateur de choisir un ou plusieurs éléments d’une collection. Les zones de liste sont similaires aux listes déroulantes, sauf qu’elles sont toujours ouvertes, c’est-à-dire qu’elles n’ont pas d’état compact (non développé). Les éléments de la liste peuvent défiler si l’espace est insuffisant pour les afficher tous.

### <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

-   Une zone de liste peut être utile quand des éléments de la liste sont suffisamment importants pour être mis en avant, et quand l’écran offre suffisamment d’espace pour afficher la liste complète.
-   Une zone de liste doit attirer l’attention de l’utilisateur sur toutes les possibilités d’un choix important. En revanche, une liste déroulante attire initialement l’attention de l’utilisateur sur l’élément sélectionné.
-   Évitez d’utiliser une zone de liste dans les cas suivants :
    -   Il existe un très petit nombre d’éléments pour la liste. Si une zone de liste à sélection unique comporte toujours les deux mêmes options, mieux vaut utiliser des [cases d’option](radio-button.md). Pensez également à utiliser des cases d’option quand 3 ou 4 éléments statiques figurent dans la liste.
    -   La zone de liste est à sélection unique, et propose toujours les deux mêmes options, l’une étant l’inverse de l’autre (par exemple, « activé » et « désactivé »). Utilisez une case à cocher ou un bouton bascule.
    -   Le nombre d’éléments est très élevé. Pour les longues listes, mieux vaut utiliser un affichage Grille ou Liste. Pour les très longues listes de données groupées, utilisez de préférence un zoom sémantique.
    -   Les éléments sont des valeurs numériques contiguës. Si tel est le cas, pensez à utiliser un [curseur](slider.md).
    -   Les éléments de sélection ont une importance secondaire dans le flux de votre application, ou l’option par défaut est recommandée pour la plupart des utilisateurs dans la majorité des situations. Dans ce cas, utilisez plutôt une liste déroulante.

### <a name="recommendations"></a>Recommandations

-   La plage idéale d’éléments dans une zone de liste est de 3 à 9.
-   Une zone de liste est efficace quand ses éléments peuvent varier de manière dynamique.
-   Dans la mesure du possible, la taille de la zone de liste doit être suffisante pour que vous n’ayez pas à faire défiler la liste des éléments.
-   Vérifiez qu’il n’y a aucune ambiguïté quand à la fonction de la zone de liste et aux éléments sélectionnés actuellement.
-   Réservez les effets visuels et les animations pour le retour tactile et pour l’état sélectionné des éléments.
-   Limitez le contenu textuel de l’élément de zone de liste à une seule ligne. Si les éléments sont visuels, vous pouvez personnaliser la taille. Si un élément contient plusieurs lignes de texte ou images, utilisez plutôt un affichage de Grille ou Liste.
-   Utilisez la police par défaut à moins que vos instructions de personnalisation imposent d’en utiliser une autre.
-   N’utilisez pas une zone de liste pour exécuter des commandes ou pour afficher ou masquer de manière dynamique d’autres contrôles.

## <a name="selection-mode"></a>Mode de sélection

Le mode de sélection permet aux utilisateurs de sélectionner et d’exécuter une action sur un ou plusieurs éléments. Il peut être appelé par le biais d’un menu contextuel, à l’aide de la combinaison CTRL+clic ou MAJ+clic sur un élément, ou par la substitution d’une cible sur un élément dans un affichage Galerie. Quand le mode de sélection est actif, des cases à cocher s’affichent à côté de chaque élément de la liste, et des actions peuvent apparaître en haut ou en bas de l’écran.

Il existe trois modes de sélection :

-   Unique : l’utilisateur ne peut sélectionner qu’un élément à la fois.
-   Multiple : l’utilisateur peut sélectionner plusieurs éléments sans utiliser de modificateur.
-   Étendu : l’utilisateur peut sélectionner plusieurs éléments avec un modificateur, par exemple, en maintenant la touche MAJ enfoncée.

L’appui sur un emplacement quelconque d’un élément entraîne la sélection de celui-ci. L’appui sur une action de la barre de commandes affecte tous les éléments sélectionnés. Si aucun élément n’est sélectionné, toutes les actions de la barre de commandes doivent être inactives, à l’exception de l’action Sélectionner tout.

Le mode Sélection ne comporte pas de modèle d’abandon interactif ; l’appui sur une zone à l’extérieur du cadre dans lequel le mode Sélection est actif ne permet pas d’annuler ce mode. Ceci empêche toute désactivation accidentelle du mode. En cliquant sur le bouton Précédent, vous fermez le mode de sélection multiple.

Affichez une confirmation visuelle lors de la sélection d’une action. Envisagez d’afficher une boîte de dialogue de confirmation pour certaines actions, notamment pour les opérations destructrices, comme la suppression.

Le mode Sélection est limité à la page dans laquelle il est actif et ne peut pas affecter les éléments situés à l’extérieur de cette page.

Le point d’entrée du mode Sélection doit être juxtaposé au contenu concerné.

Pour des recommandations relatives à la barre de commandes, voir [Recommandations en matière de barres de commandes](app-bars.md).

## <a name="globalization-and-localization-checklist"></a>Liste de contrôle de globalisation et de localisation

<table>
<tr>
<th>Renvoi à la ligne</th><td>Autorisez deux lignes pour l’étiquette de liste.</td>
</tr>
<tr>
<th>Extension horizontale</th><td>Assurez-vous que les champs prennent en charge l’extension de texte et sont défilants.</td>
</tr>
<tr>
<th>Espacement vertical</th><td>Utilisez les caractères non latins d’espacement vertical pour assurer un affichage correct des scripts non latins.</td>
</tr>
</table>


## <a name="related-articles"></a>Articles connexes

- [Hub](hub.md)
- [Maître/détails](master-details.md)
- [Volet de navigation](navigationview.md)
- [Zoom sémantique](semantic-zoom.md)
- [Glisser-déplacer](https://msdn.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [Images miniatures](../../files/thumbnails.md)

**Pour les développeurs**
- [Classe ListView](https://msdn.microsoft.com/library/windows/apps/br242878)
- [Classe GridView](https://msdn.microsoft.com/library/windows/apps/br242705)
- [Classe ComboBox](https://msdn.microsoft.com/library/windows/apps/br209348)
- [Classe ListBox](https://msdn.microsoft.com/library/windows/apps/br242868)