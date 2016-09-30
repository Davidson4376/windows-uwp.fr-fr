---
author: mijacobs
Description: "Découvrez comment utiliser les vignettes, badges, toasts et notifications pour fournir des points d’entrée dans votre application et maintenir les utilisateurs informés."
title: Vignettes, badges et notifications
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 32b1c3ec674a84ca4ed08d98119fe21f77e15554

---

# Vignettes, badges et notifications pour les applications UWP




Découvrez comment utiliser les vignettes, badges, toasts et notifications pour fournir des points d’entrée dans votre application et maintenir les utilisateurs informés.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><img src="images/tile-and-live-tile.png" alt="Breakdown of tile elements" /></td>
<td align="left"><p>Chaque application dispose d’une vignette. Une <em>vignette</em> est la représentation d’une application dans le menu Démarrer. Vous pouvez activer différentes tailles de vignettes (petite, moyenne, large et grande). Vous pouvez utiliser une <em>notification par vignette</em> pour mettre à jour la vignette afin de communiquer de nouvelles informations à l’utilisateur, telles que des titres d’actualités ou l’objet du dernier message non lu. Vous pouvez utiliser un <em>badge</em> ou un <em>badge de notification</em> pour fournir des informations d’état ou de résumé sous la forme d’un glyphe fourni par le système ou d’un nombre compris entre 1 et 99.</p>
<p>Une <em>notification toast</em> est une notification que votre application envoie à l’utilisateur par le biais d’un élément de l’interface utilisateur contextuelle appelé <em>toast</em> (ou <em>bannière</em>). La notification est visible, que l’utilisateur se trouve dans votre application ou non.</p>
<p>Une <em>notification Push</em>, ou <em>notification brute</em>, est une notification envoyée à votre application à partir du service de notifications Push Windows (WNS) ou d’une tâche en arrière-plan. Votre application peut répondre à ces notifications soit en informant l’utilisateur qu’un événement l’intéressant s’est produit (par le biais d’une mise à jour de badge, d’une mise à jour de vignette ou d’un toast), soit de la manière de votre choix.</p></td>
</tr>
</tbody>
</table>

 
## Vignettes 
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
<td align="left"><p>[Créer des vignettes](tiles-and-notifications-creating-tiles.md)</p></td>
<td align="left"><p>Personnalisez la vignette par défaut de votre application et fournissez des ressources pour différentes tailles d’écran.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Créer des vignettes adaptatives](tiles-and-notifications-create-adaptive-tiles.md)</p></td>
<td align="left"><p>Les modèles de vignette adaptative sont une nouvelle fonctionnalité de Windows10, qui vous permet de concevoir votre propre contenu de notification par vignette à l’aide d’un langage de balisage simple et flexible adapté à différentes densités d’écran. Cet article vous indique comment créer des vignettes dynamiques adaptatives pour votre application de plateforme Windows universelle (UWP).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Schéma des vignettes adaptatives](tiles-and-notifications-adaptive-tiles-schema.md)</p></td>
<td align="left"><p>Voici les éléments et attributs permettant de créer des vignettes adaptatives.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Modèles de vignette spéciaux](tiles-and-notifications-special-tile-templates-catalog.md)</p></td>
<td align="left"><p>Les modèles de vignette spéciaux sont des modèles uniques qui sont animés, ou qui vous permettent simplement d’effectuer des opérations qui ne sont pas possibles avec des vignettes adaptatives.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Ressources d’icônes de l’application](tiles-and-notifications-app-assets.md)</p></td>
<td align="left"><p>Les ressources d’icône d’application, qui s’affichent sous différentes formes dans le système d’exploitation Windows10, sont les cartes de visite de votre application de plateforme Windows universelle (UWP). Ces recommandations précisent où apparaissent les ressources d’icône d’application dans le système et fournissent des conseils de conception détaillés pour vous aider à créer les plus belles icônes.</p></td>
</tr>
</tbody>
</table>

## Notifications


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
<td align="left"><p>[Notifications toast adaptatives et interactives](tiles-and-notifications-adaptive-interactive-toasts.md)</p></td>
<td align="left"><p>Les notifications toast adaptatives et interactives vous permettent de créer des notifications contextuelles flexibles présentant davantage de contenu, ainsi que des images incluses et une interaction utilisateur facultatives.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Notifications Visualizer](tiles-and-notifications-notifications-visualizer.md)</p></td>
<td align="left"><p>Notifications Visualizer est une nouvelle application de plateforme Windows universelle (UWP) dans [le Windows Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) qui permet aux développeurs de concevoir des vignettes dynamiques adaptatives pour Windows10.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Choisir une méthode de remise de notification](tiles-and-notifications-choosing-a-notification-delivery-method.md)</p></td>
<td align="left"><p>Ce article présente les quatre options de notification(locale, planifiée, périodique et Push) disponibles pour remettre des mises à jour de vignette et de badge, ainsi que du contenu de notification toast.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Envoyer une notification par vignette locale](tiles-and-notifications-sending-a-local-tile-notification.md)</p></td>
<td align="left"><p>Cet article décrit comment envoyer une notification par vignette locale à une vignette principale et une vignette secondaire à l’aide de modèles de vignette adaptative.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Vue d’ensemble des notifications périodiques](tiles-and-notifications-periodic-notification-overview.md)</p></td>
<td align="left"><p>Les notifications périodiques, également appelées notifications interrogées, mettent à jour les vignettes et les badges à intervalle fixe en téléchargeant du contenu à partir d’un service cloud.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Vue d’ensemble des services de notifications Push Windows (WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md)</p></td>
<td align="left"><p>Les services de notification Push Windows (WNS) permettent aux développeurs tiers d’envoyer des mises à jour de toast, de vignette et de badge, ainsi que des mises à jour brutes à partir de leur propre service cloud. Il en résulte un mécanisme fiable et optimal de remise des nouvelles mises à jour aux utilisateurs.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Code généré par l’Assistant Notification Push](tiles-and-notifications-the-code-generated-by-the-push-notification-wizard.md)</p></td>
<td align="left"><p>Grâce à un Assistant Visual Studio, vous pouvez générer des notifications Push à partir d’un service mobile créé via Microsoft Azure Mobile Services. L’Assistant Visual Studio génère du code qui devrait vous aider à démarrer. Cette rubrique explique comment l’Assistant modifie votre projet, ce que le code généré fait, comment utiliser ce code et ce que vous pouvez faire ensuite pour tirer le meilleur parti des notifications Push. Voir [Vue d’ensemble des services de notifications Push Windows (WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Vue d’ensemble des notifications brutes](tiles-and-notifications-raw-notification-overview.md)</p></td>
<td align="left"><p>Les notifications brutes sont des notificationsPush courtes à usage général. Elles ont une finalité exclusivement didactique et n’incluent aucun composant d’interface utilisateur. Comme dans le cas d’autres notifications Push, la fonctionnalité WNS transmet les notifications brutes de votre service cloud à votre application.</p></td>
</tr>
</tbody>
</table>

 

 

 







<!--HONumber=Jun16_HO4-->


