---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: Informations de base relatives à la conception des commandes pour les applications de plateforme Windows universelle (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 07b9ce7b5a57f6dc1ba202ed57e8b2d4d93e583f
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654388"
---
#  <a name="command-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception des commandes pour les applications UWP

Dans une application de plateforme Windows universelle(UWP), les *éléments de commande* sont les éléments d’interface utilisateur interactifs qui permettent aux utilisateurs d’effectuer des actions, telles que l’envoi d’un message électronique, la suppression d’un élément ou l’envoi d’un formulaire. 

Cet article décrit les éléments de commande courants, les interactions qu’ils prennent en charge et les surfaces de commande permettant de les héberger.

![éléments de commande dans l’application Cartes](images/maps.png)

Vous trouverez ci-dessus des exemples d’éléments de commande dans l’application Cartes.

## <a name="provide-the-right-type-of-interactions"></a>Fournissez le type approprié d’interactions

Lorsque vous créez une interface de commande, la décision la plus importante consiste à choisir ce que les utilisateurs auront la possibilité de faire. Pour planifier le type d’interactions approprié, concentrez-vous sur votre application: étudiez les expériences utilisateur que vous souhaitez favoriser et les étapes que les utilisateurs devront suivre. Une fois que vous avez défini ce que les utilisateurs doivent faire, vous pouvez leur fournir les outils nécessaires pour y parvenir.

Interactions que vous souhaiterez peut-être proposer pour votre application:

- Envoi d’informations 
- Sélection de paramètres et d’options
- Recherche et filtrage de contenu
- Ouverture, enregistrement et suppression de fichiers
- Modification ou création de contenu

## <a name="use-the-right-command-element-for-the-interaction"></a>Utiliser l’élément de commande approprié pour l’interaction

L'utilisation d'éléments adéquats pour favoriser les interactions de commande peut faire toute la différence entre une application intuitive et facile à utiliser, et une application dont l’utilisation est complexe et déroutante. La plateforme Windows universelle(UWP) fournit un large éventail d’éléments de commande que vous pouvez utiliser dans votre application. Le tableau suivant répertorie quelques-uns des contrôles les plus courants et offre un résumé des interactions qu’ils peuvent permettre.

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">Catégorie</th>
<th align="left">Éléments</th>
<th align="left">Interaction</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>Boutons</b><br/><br/>
    <img src="../controls-and-patterns/images/controls/button.png" alt="button" /></td>
<td align="left"><a href="../controls-and-patterns/buttons.md">Bouton</a></td>
<td align="left">Déclenche une action immédiate. Il peut s’agir de l’envoi d’un message électronique, de la confirmation d’une action dans une boîte de dialogue ou de l’envoi des données d’un formulaire.</td>
</tr>
<tr class="even">
<td align="left">Listes<br/><br/>
    <img src="../controls-and-patterns/images/controls/combo-box-open.png" alt="drop down list" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">Liste déroulante, zone de liste, affichage Liste et affichage Grille</a></td>
<td align="left">Présente les éléments dans une liste ou une grille interactives. Généralement utilisées pour de nombreuses options ou pour afficher les éléments.</td>
</tr>
<tr class="odd">
<td align="left">Contrôles de sélection<br/><br/>
    <img src="../controls-and-patterns/images/controls/radio-button.png" alt="radio button" /></td>
<td align="left"><a href="../controls-and-patterns/checkbox.md">case à cocher</a>, <a href="../controls-and-patterns/radio-button.md">case d’option</a>, <a href="../controls-and-patterns/toggles.md">bouton bascule</a></td>
<td align="left">Permettent aux utilisateurs de choisir entre différentes options, par exemple lors du remplissage d’un questionnaire ou de la configuration des paramètres d’application.</td>
</tr>
<tr class="even">
<td align="left">Sélecteurs de date et d’heure<br/><br/>
    <img src="../controls-and-patterns/images/controls/calendar-date-picker-open.png" alt="date picker" /></td>
<td align="left"><a href="../controls-and-patterns/date-and-time.md">sélecteur de date de calendrier, affichage de calendrier, sélecteur de date, sélecteur d’heure</a></td>
<td align="left">Permettent aux utilisateurs d’afficher et de modifier les informations de date et d’heure, notamment lors de la création d’un événement ou de la configuration d’une alarme.</td>
</tr>
<tr class="odd">
<td align="left">Saisie de texte prédictive<br/><br/>
    <img src="../controls-and-patterns/images/controls/auto-suggest-box.png" alt="autosuggest box" /></td>
<td align="left"><a href="../controls-and-patterns/auto-suggest-box.md">Zone de suggestion automatique</a></td>
<td align="left">Fournit des suggestions au fur et à mesure de la saisie de données ou lors de requêtes.</td>
</tr>
</tbody>
</table>
</div>

Pour obtenir la liste complète de ces éléments, voir [Contrôles et éléments d’interface utilisateur](https://dev.windows.com/design/controls-patterns).

##  <a name="place-commands-on-the-right-surface"></a>Placer les commandes sur la surface appropriée
Vous pouvez placer les éléments de commande sur un certain nombre de surfaces dans votre application, notamment le canevas d’application ou des conteneurs de commande spécifiques, tels que des barres de commandes, des menus, des boîtes de dialogue et des menus volants.

Laissez autant que possible les utilisateurs manipuler directement le contenu au lieu d’ajouter des commandes qui agissent sur ce dernier. Par exemple, laissez les utilisateurs réorganiser les listes en en faisant glisser les éléments au lieu d’utiliser les boutons de commande vers le haut et vers le bas.
  
Si les utilisateurs ne peuvent pas manipuler le contenu directement, placez des éléments de commande sur une surface de commande dans votre application:

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">Surface</th>
<th align="left">Description</th>
<th align="left">Exemple</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top">Canvas (zone de contenu) de l’application
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>

<td align="left" style="vertical-align: top;">Si une commande est constamment nécessaire pour permettre aux utilisateurs d’effectuer des scénarios de base, placez-la sur le canevas. Comme vous pouvez placer les commandes près des objets qu’elles affectent (ou sur ceux-ci), le fait de les placer sur le Canvas rend leur utilisation facile et évidente.
<p>Toutefois, choisissez les commandes que vous placez sur le Canvas avec soin. S’il y a trop de commandes, vous perdrez de l’espace précieux sur l’écran et risquez de submerger l’utilisateur. Si la commande n’est pas vouée à être fréquemment utilisée, pensez à la placer sur une autre surface de commande.</p> 
</td><td>
Une zone de suggestion automatique sur le canevas d’application Cartes.
<br></br>
  <img src="images/maps-canvas.png" alt="autosuggest box on Maps app canvas"/>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/app-bars.md">Barre de commandes</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p></td>
<td align="left" style="vertical-align: top;"> Les barres de commandes simplifient l’organisation des commandes et en facilitent l’accès. Les barres de commandes peuvent être placées en haut de l’écran et/ou au bas de l’écran. 
</td>
<td>
Une barre de commandes en haut de l’application Cartes.
<br></br>
<img src="images/maps-commandbar.png" alt="command bar in Maps app"/>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/menus.md">Menus et menus contextuels</a>
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left" style="vertical-align: top;">Parfois, pour économiser de l’espace, il est plus efficace de regrouper plusieurs commandes dans un menu de commandes. Les menus et les menus contextuels affichent une liste de commandes ou d’options lorsque l’utilisateur les demande.
<p>Les menus contextuels peuvent fournir des raccourcis pour les actions courantes et un accès aux commandes secondaires qui ne sont pertinentes que dans certains contextes, comme le presse-papier ou les commandes personnalisées. Les menus contextuels sont généralement affichés à la suite d’un clic sur le bouton droit.</p>
</td><td>
Un menu contextuel s’affiche lorsque les utilisateurs cliquent sur le bouton droit dans l’application Cartes.
<br></br>
  <img src="images/maps-contextmenu.png" alt="context menu in Maps app"/>
</td>
</tr>
</table>
</div>

## <a name="provide-feedback-for-interactions"></a>Fournir des commentaires pour les interactions

Les commentaires communiquent les résultats des commandes et permettent aux utilisateurs de comprendre ce qu’ils ont fait et ce qu’ils peuvent faire ensuite. Idéalement, les commentaires doivent être intégrés naturellement à l'interface utilisateur pour que les utilisateurs n’aient pas à être interrompus ou à prendre de mesures supplémentaires, à moins que cela ne soit absolument nécessaire. 

Voici quelques solutions pour fournir des commentaires dans votre application.

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">Surface</th>
<th align="left">Description</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"> <a href="../controls-and-patterns/app-bars.md">Barre de commandes</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p>
</td>
<td align="left" style="vertical-align: top;"> La zone de contenu de la barre de commandes est un endroit intuitif pour communiquer le statut aux utilisateurs s’ils veulent voir les commentaires.
<p>
  <img src="images/commandbar_anatomy.png" alt="Command bar content area for feedback"/>
  </p>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">Menu volant</a>
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left" style="vertical-align: top;">
Une fenêtre contextuelle simple peut être fermée en appuyant ou en cliquant quelque part à l’extérieur du menu volant.
<p>
  <img src="../controls-and-patterns/images/controls/flyout.png" alt="Flyout above button"/>
  </p>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">Contrôles de boîte de dialogue</a>
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left" style="vertical-align: top;">Les boîtes de dialogue sont des superpositions d’interface utilisateur modales qui fournissent des informations contextuelles sur l’application. Dans la plupart des cas, les boîtes de dialogue bloquent les interactions avec la fenêtre de l’application jusqu’à ce qu’elles soient masquées explicitement. Elles demandent souvent un type d’action de l’utilisateur.
<p>Les boîtes de dialogue peuvent nuire à la fluidité de l’expérience et ne doivent être utilisées que dans certaines situations. Pour plus d’informations, voir la section [Quand confirmer ou annuler des actions](#when-to-confirm-or-undo-actions).</p>
<p>
  <img src="../controls-and-patterns/images/dialogs/dialog_RS2_delete_file.png" alt="dialog delete file"/></p>
</td>
</tr>

</table>
</div>

> [!TIP]
> Prenez garde à ne pas utiliser un trop grand nombre de boîtes de dialogue de confirmation dans votre application: elles peuvent être très utiles en cas d’erreur de l’utilisateur, mais elles représentent un obstacle chaque fois que l’utilisateur tente d’effectuer une action intentionnellement.

### <a name="when-to-confirm-or-undo-actions"></a>Quand confirmer ou annuler des actions

Même si l’interface utilisateur est parfaitement pensée et que l’utilisateur fait preuve de la plus grande précaution, il arrivera inévitablement à celui-ci d’effectuer une action non souhaitée. Votre application peut remédier à ce genre de situation en demandant à l’utilisateur de confirmer une action ou en lui offrant un moyen d’annuler les actions récentes.

-   Pour les actions qui ne peuvent pas être annulées et dont les conséquences sont importantes, nous vous recommandons d’utiliser une boîte de dialogue de confirmation. Exemples de telles actions :
    -   Remplacement d’un fichier
    -   Non-enregistrement d’un fichier avant la fermeture
    -   Confirmation de la suppression définitive d’un fichier ou de données
    -   Achat (sauf si l’utilisateur refuse de demander une confirmation)
    -   Envoi d’un formulaire, par exemple pour s’inscrire à un programme
-   Pour les actions qui peuvent être annulées, une simple commande d’annulation est généralement suffisante. Exemples de telles actions :
    -   Suppression d’un fichier
    -   Suppression d’un e-mail (pas définitivement)
    -   Modification du contenu ou d’un texte
    -   Changement de nom d’un fichier

##  <a name="optimize-for-specific-input-types"></a>Optimiser les types d’entrée spécifiques

Voir les [Notions fondamentales sur les interactions](../input/index.md) pour plus d’informations sur l’optimisation des expériences utilisateur avec un type ou un périphérique d’entrée spécifique.