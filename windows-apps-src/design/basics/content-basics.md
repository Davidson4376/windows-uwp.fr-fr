---
author: mijacobs
Description: An overview of common page patterns and UI elements for displaying content in your UWP app.
title: Informations de base relatives à la conception de contenu pour les applications de plateformeWindows universelle (UWP)
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 12/1/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 91187b176d32fcc53b51faa7e8c42a10fd4908b8
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4689479"
---
# <a name="content-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception de contenu pour les applicationsUWP

Une application a principalement pour objet d’offrir un accès à un contenu. Dans la mesure où les applications répondent à des buts différents, leur contenu se présente sous de nombreuses formes: dans une application de retouche photo, la photo est le contenu; dans une application de voyage, les cartes et les informations sur les destinations sont le contenu; et ainsi de suite. 

Cet article fournit une vue d’ensemble de la présentation de contenu dans votre application. Nous y décrivons les éléments d’interface utilisateur et les modèles de page courants que vous pouvez utiliser pour afficher le contenu, quelle qu’en soit la forme.

## <a name="common-page-patterns"></a>Modèles de page courants

De nombreuses applications utilisent tout ou partie de ces modèles de page pour afficher différents types de contenu. De même, n’hésitez pas à combiner ces modèles pour optimiser le contenu de votre application.

### <a name="landing"></a>Accueil

![page d’accueil](images/content-basics/hero-screen.png)

Les pages d’accueil, également connues sous le nom d’écrans bannière, apparaissent souvent au niveau supérieur d’une expérience applicative. La grande surface sert à exposer les applications, et plus particulièrement à mettre en avant le contenu que les utilisateurs peuvent vouloir parcourir et utiliser.

### <a name="collections"></a>Collections

![galerie](images/content-basics/gridview.png)

Les collections permettent aux utilisateurs de parcourir les groupes de contenu ou de données. L’[affichage Grille](../controls-and-patterns/item-templates-gridview.md) est idéal pour les photos ou les contenus centrés sur les médias; le [mode Liste](../controls-and-patterns/item-templates-listview.md) est, quant à lui, plus particulièrement destiné aux données ou aux contenus riches en texte.

### <a name="hub"></a>Hub

![hub](images/content-basics/hub.png)

Les [hubs](../controls-and-patterns/hub.md) sont conçus pour favoriser le «lèche-vitrines». L’important est de montrer la grande diversité de contenus tout en restant bref. Les utilisateurs disposent donc ainsi d’un bon aperçu du contenu offert. Par exemple, la section1 du hub peut contenir un écran bannière, la section2 une collection, la section3 une autre collection, et ainsi de suite.

### <a name="masterdetail"></a>Maître/Détail

![maître et détails](images/content-basics/master-detail.png)

Le modèle [maître/détails](../controls-and-patterns/master-details.md) se compose du mode Liste (maître) et d’une vue de contenu (détail). Ces deux volets sont figés et offrent un défilement vertical. Il existe une relation claire entre l’élément de liste et la vue de contenu: lorsque l’élément de la vue maître est sélectionné, la vue détaillée est mise à jour en conséquence. Outre la navigation dans la vue détaillée, il est possible d’ajouter et de supprimer des éléments de la vue maître.

### <a name="details"></a>Détails

![plusieurs vues](images/multi-view.png)

Pensez à créer une page dédiée à l’affichage du contenu pour que les utilisateurs puissent voir la page exempte de distractions lorsqu’ils trouvent le contenu qu’ils recherchent. Dans la mesure du possible, [créez une option d’affichage plein écran](../layout/show-multiple-views.md) qui ajuste le contenu à l’écran et masque tous les autres éléments de l’interface utilisateur. 

Pour tenir compte des changements de taille de l’écran, pensez également à créer une [conception réactive](design-and-ui-intro.md) qui masque/affiche les éléments de l’interface utilisateur en fonction des besoins.

### <a name="forms"></a>Formulaires
![formulaire](images/content-basics/forms.png)

Un [formulaire](../controls-and-patterns/forms.md) est un groupe de commandes qui collectent et envoient les données des utilisateurs. La plupart des applications, voire toutes, utilisent un formulaire pour les pages de paramètres, les portails de connexion, les hubs de commentaires, la création de compte, etc. 

## <a name="common-content-elements"></a>Éléments de contenu couramment utilisés

Pour créer ces modèles de page, vous devez utiliser une combinaison d’éléments de contenu. Voici quelques éléments d’interface utilisateur couramment utilisés pour afficher du contenu. (Pour en obtenir la liste complète, voir [Contrôles et modèles](../controls-and-patterns/index.md).

<div class="mx-responsive-img">
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
<td align="left">Audio et vidéo<br/><br/>
    <img src="images/content-basics/media-transport.png" alt="media transport control" /></td>
<td align="left"><a href="../controls-and-patterns/media-playback.md">Contrôles de transport et de lecture de média</a></td>
<td align="left">Lit du contenu audio et vidéo.</td>
</tr>
<tr class="even">
<td align="left">Visionneuses d’image<br/><br/>
    <img src="images/content-basics/flipview.jpg" alt="flip view" /></td>
<td align="left"><a href="../controls-and-patterns/flipview.md">Vue symétrique</a>, <a href="../controls-and-patterns/images-imagebrushes.md">image</a></td>
<td align="left">Affiche des images. La vue symétrique permet d’afficher les images d’une collection, par exemple les photos d’un album ou les éléments d’une page de détails sur le produit, image par image.</td>
</tr>
<tr class="odd">
<td align="left">Collections <br/><br/>
    <img src="images/content-basics/listview.png" alt="list view" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">Mode Liste et affichage Grille</a></td>
<td align="left">Présente les éléments dans une liste interactive ou une grille. Utilisez ces éléments pour permettre aux utilisateurs de sélectionner un film parmi une liste de nouveautés ou de gérer un inventaire.</td>
</tr>
<tr class="even">
<td align="left">Texte et saisie de texte <br/><br/>
    <img src="images/content-basics/textbox.png" alt="text box" /></td>
<td align="left"><p><a href="../controls-and-patterns/text-block.md">Bloc de texte</a>, <a href="../controls-and-patterns/text-box.md">zone de texte</a>, <a href="../controls-and-patterns/rich-edit-box.md">zone d’édition enrichie</a></p>
</td>
<td align="left">Affiche le texte. Certains éléments permettent à l’utilisateur de modifier du texte. Pour plus d’informations, voir <a href="../controls-and-patterns/text-controls.md">Contrôles de texte</a>.
<p>Pour savoir comment afficher du texte, voir [Typographie](../style/typography.md).</p>
</td>
</tr>
<tr class="odd">
<td align="left">Cartes<br/><br/>
    <img src="images/content-basics/mapcontrol.png" alt="map control" /></td>
<td align="left"><a href="../../maps-and-location/display-maps.md">MapControl</a></td>
<td align="left">Affiche une carte symbolique ou photoréaliste de la Terre.</td>
</tr>
<tr class="even">
<td align="left">WebView</td>
<td align="left"><a href="../controls-and-patterns/web-view.md">WebView</a></td>
<td align="left">Restitue le contenuHTML.</td>
</tr>
</tbody>
</table>
</div>
