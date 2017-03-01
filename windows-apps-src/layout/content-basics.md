---
author: mijacobs
Description: "Une application a principalement pour objet d’offrir un accès à un contenu. Dans une application de retouche photo, le contenu correspond aux photos ; dans une application de voyage, le contenu comprend les cartes et les informations sur les destinations, etc."
title: "Informations de base relatives à la conception de contenu pour les applications de plateforme Windows universelle (UWP)"
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
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
ms.openlocfilehash: 9bb39b117fe4c4e95616c06921b05199e153cddd
ms.lasthandoff: 02/07/2017

---

#  <a name="content-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception de contenu pour les applications UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Le rôle principal de toute application est d’offrir un accès à un contenu. Dans une application de retouche photo, le contenu correspond aux photos ; dans une application de voyage, le contenu comprend les cartes et les informations sur les destinations, etc. Les éléments de navigation donnent accès au contenu. Les éléments de commande permettent à l’utilisateur d’interagir avec contenu. Les éléments de contenu affichent le contenu réel.

Cet article fournit des recommandations de conception de contenu pour les trois scénarios de contenu.

## <a name="design-for-the-right-content-scenario"></a>Scénario de conception de contenu adapté


Il existe trois scénarios principaux en matière de contenu :

-   **Consommation** : expérience principalement unidirectionnelle où le contenu est utilisé. Les actions de lecture de documents, de morceaux de musique ou de vidéos, ou d’affichage de photos et d’images rentrent dans la catégorie Consommation.
-   **Création** : expérience principalement unidirectionnelle dans laquelle l’objectif est de créer du contenu. Elle inclut les actions de création d’éléments à partir de zéro (par exemple, capture de photos ou de vidéos, création d’images dans une application de dessin ou ouverture d’un nouveau document).
-   **Interaction** : expérience de contenu bidirectionnelle qui inclut la consommation, la création et la révision de contenu.

## <a name="consumption-focused-apps"></a>Applications axées sur la consommation


Les éléments de contenu reçoivent la priorité la plus élevée dans une application axée sur la consommation, suivie des [éléments de navigation](navigation-basics.md) permettant aux utilisateurs de trouver le contenu qu’ils recherchent. Exemples d’applications axées sur la consommation : lecteurs de films, applications de lecture, applications de musique et visionneuses de photos.

![application de lecteur de news](images/news-reader/v2/newsreader-v2-tablet-phone.png)

Recommandations générales pour les applications axées sur la consommation :

-   Pensez à créer des pages de [navigation](navigation-basics.md) dédiées et des pages d’affichage de contenu, afin que lorsque les utilisateurs trouvent le contenu recherché, ils puissent l’afficher sur une page dédiée exempte de distractions.
-   Pensez à créer une option de mode plein écran qui étend le contenu pour remplir tout l’écran et masque tous les autres éléments d’interface utilisateur.

## <a name="creation-focused-apps"></a>Applications axées sur la création


Le contenu et les éléments de [commande](commanding-basics.md) sont les éléments d’interface utilisateur les plus importants dans une application axée sur la création. Les éléments de commande permettent à l’utilisateur de créer du contenu. Exemples : applications de peinture, applications de retouche photo, applications de retouche vidéo et applications de traitement de texte.

Par exemple, voici une conception pour une application de retouche photo qui utilise des barres de commandes pour donner accès aux outils et aux options de manipulation de photos. Comme toutes les commandes se trouvent dans la barre de commandes, la majeure partie de l’écran de l’application peut être utilisée pour le contenu, à savoir la photo en cours de modification.

![exemple d’une conception d’application de retouche photo utilisant un Canvas actif](images/photo-editor/uap-photo-tabletphone-sbs.png)

Recommandations générales pour les applications axées sur la création :

-   Réduisez l’utilisation des éléments de [navigation](navigation-basics.md).
-   Les éléments de [commande](commanding-basics.md) sont particulièrement importants dans les applications axées sur la création. Dans la mesure où les utilisateurs doivent exécuter un grand nombre de commandes, nous vous recommandons de fournir une fonctionnalité d’historique/annulation de commande.

## <a name="apps-with-interactive-content"></a>Applications avec contenu interactif


Dans une application avec contenu interactif, les utilisateurs créent, affichent et modifient du contenu. De nombreuses applications entrent dans cette catégorie. Exemples : applications métier, applications de gestion de l’inventaire, applications permettant à l’utilisateur de créer ou de modifier des recettes de cuisine.

![conception pour un outil de collaboration, application disposant d’un contenu interactif](images/collaboration-tool/uap-collaboration-tabphone-700.png)

Ces applications doivent trouver un équilibre entre les trois éléments de l’interface utilisateur :

-   Les éléments de [navigation](navigation-basics.md) permettent aux utilisateurs de rechercher et d’afficher du contenu. Si l’objectif principal consiste à afficher et à rechercher du contenu, donnez la priorité aux éléments de navigation, au filtrage et au tri, ainsi qu’à la recherche.
-   Les éléments de [commande](commanding-basics.md) permettent à l’utilisateur de créer, modifier et manipuler du contenu.

Recommandations générales concernant les applications avec un contenu interactif :

-   Il peut être difficile d’équilibrer les éléments de navigation, de contenu et de commande, car ils ont chacun leur importance. Si possible, envisagez de créer des écrans distincts pour la navigation, la création et la modification du contenu, ou fournissez des changements de mode.

## <a name="commonly-used-content-elements"></a>Éléments de contenu couramment utilisés


Voici quelques éléments d’interface utilisateur couramment utilisés pour afficher du contenu. (Pour en obtenir la liste complète, voir [Contrôles et éléments d’interface utilisateur](https://msdn.microsoft.com/library/windows/apps/dn611856).)

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Catégorie</th>
<th align="left">Éléments</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Audio et vidéo</td>
<td align="left">[Contrôles de transport et de lecture de média](../controls-and-patterns/media-playback.md)</td>
<td align="left">Lit du contenu audio et vidéo.</td>
</tr>
<tr class="even">
<td align="left">Visionneuses d’image</td>
<td align="left">[Vue symétrique](../controls-and-patterns/flipview.md), [image](../controls-and-patterns/images-imagebrushes.md)</td>
<td align="left">Affiche des images. La vue symétrique permet d’afficher les images d’une collection, par exemple les photos d’un album ou les éléments d’une page de détails sur le produit, image par image.</td>
</tr>
<tr class="odd">
<td align="left">Listes</td>
<td align="left">[liste déroulante, zone de liste, affichage Liste et affichage Grille](../controls-and-patterns/lists.md)</td>
<td align="left">Présente les éléments dans une liste interactive ou une grille. Utilisez ces éléments pour permettre aux utilisateurs de sélectionner un film parmi une liste de nouveautés ou de gérer un inventaire.</td>
</tr>
<tr class="even">
<td align="left">Texte et saisie de texte</td>
<td align="left"><p>[Bloc de texte](../controls-and-patterns/text-block.md), [zone de texte](../controls-and-patterns/text-box.md), [zone d’édition enrichie](../controls-and-patterns/rich-edit-box.md)</p>
</td>
<td align="left">Affiche le texte. Certains éléments permettent à l’utilisateur de modifier du texte. Pour plus d’informations, voir [Contrôles de texte](../controls-and-patterns/text-controls.md).</td>
</tr>
</tbody>
</table>



 

 





