---
author: mijacobs
Description: Learn how to use tiles, badges, toasts, and notifications to provide entry points into your app and keep users up-to-date.
title: Vignettes, badges et notifications
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d1282bb0de6f21b19e43771020d0f1b47d4ea93c
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
ms.locfileid: "1393698"
---
# <a name="tiles-badges-and-notifications-for-uwp-apps"></a>Vignettes, badges et notifications pour les applications UWP
 

Découvrez comment utiliser les vignettes, badges, toasts et notifications pour fournir des points d’entrée dans votre application et maintenir les utilisateurs informés.

> **API importantes**: [package NuGet UWP Community Toolkit Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
Une vignette est la représentation d’une application dans le menu Démarrer. Chaque application UWP dispose d’une vignette. Vous pouvez activer différentes tailles de vignettes (petite, moyenne, large et grande).</p>

<p>Vous pouvez utiliser une <em>notification par vignette</em> pour mettre à jour la vignette afin de communiquer de nouvelles informations à l’utilisateur, telles que des titres d’actualités ou l’objet du dernier message non lu.</p>

<p>Vous pouvez utiliser un <em>badge</em> pour fournir des informations d’état ou de résumé sous la forme d’un glyphe fourni par le système ou d’un nombre compris entre1 et99. Les badges apparaissent également sur l’icône de barre des tâches d’une application. </p>

<p>Une <em>notification toast</em> est une notification que votre application envoie à l’utilisateur via un élément d’interface utilisateur contextuel appelé <em>toast</em> (ou <em>bannière</em>). La notification est visible, que l’utilisateur se trouve dans votre application ou non.</p>
<p>Une <em>notification Push</em>, ou <em>notification brute</em>, est une notification envoyée à votre application à partir du service de notifications Push Windows (WNS) ou d’une tâche en arrière-plan. Votre application peut répondre à ces notifications soit en informant l’utilisateur qu’un événement l’intéressant s’est produit (par le biais d’une mise à jour de badge, d’une mise à jour de vignette ou d’un toast), soit de la manière de votre choix.</p>

 
## <a name="tiles"></a>Vignettes
| Article | Description |
| --- | --- |
| [Créer des vignettes](creating-tiles.md) | Personnalisez la vignette par défaut de votre application et fournissez des ressources pour différentes tailles d’écran. |
| [Ressources d’icônes de l’application](app-assets.md) | Les ressources d’icône d’application, qui s’affichent sous différentes formes dans le système d’exploitation Windows10, sont les cartes de visite de votre application de plateforme Windows universelle (UWP). Ces recommandations précisent où apparaissent les ressources d’icône d’application dans le système et fournissent des conseils de conception détaillés pour vous aider à créer les plus belles icônes. |
| [API de vignette principale](primary-tile-apis.md) | Demandez à épingler la vignette principale de votre application, et vérifiez que la vignette principale est actuellement épinglée. |
| [Contenu de vignette](create-adaptive-tiles.md) | Le contenu des notifications par vignette utilise les modèles de vignette adaptative, une nouvelle fonctionnalité de Windows10 qui vous permet de concevoir votre propre contenu de notification par vignette à l’aide d’un langage de balisage simple et flexible adapté à différentes densités d’écran. Cet article vous explique comment créer des vignettes dynamiques adaptatives pour votre application de plateforme Windows universelle (UWP). |
| [Schéma de contenu de vignette](../tiles-and-notifications/tile-schema.md) | Voici les éléments et attributs permettant de créer des vignettes adaptatives. |
| [Modèles de vignette spéciaux](special-tile-templates-catalog.md) | Les modèles de vignette spéciaux sont des modèles uniques qui sont animés, ou qui vous permettent simplement d’effectuer des opérations qui ne sont pas possibles avec des vignettes adaptatives. |
| [Envoyer une notification par vignette locale](sending-a-local-tile-notification.md) | Découvrez comment envoyer une notification par vignette locale, en ajoutant du contenu dynamique riche à votre vignette dynamique. |


## <a name="notifications"></a>Notifications

| Article | Description |
| --- | --- |
| [Notifications toast](adaptive-interactive-toasts.md) | Les notifications toast sont adaptatives et interactives. Elles vous permettent de créer des notifications contextuelles flexibles avec plus de contenu, d’incorporer des images et de proposer des interactions avec l’utilisateur. |
| [Envoyer une notification toast locale](send-local-toast.md) | Découvrez comment envoyer une notification toast interactive. |
| [Notifications Visualizer](notifications-visualizer.md) | Notifications Visualizer est une nouvelle application de plateforme Windows universelle (UWP) dans [le Windows Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) qui permet aux développeurs de concevoir des vignettes dynamiques adaptatives pour Windows10. |
| [Choisir une méthode de remise de notification](choosing-a-notification-delivery-method.md) | Ce article présente les quatre options de notification (locale, planifiée, périodique et Push) disponibles pour remettre des mises à jour de vignette et de badge, ainsi que du contenu de notification toast. |
| [Vue d’ensemble des notifications périodiques](periodic-notification-overview.md) | Les notifications périodiques, également appelées notifications interrogées, mettent à jour les vignettes et les badges à intervalle fixe en téléchargeant du contenu à partir d’un service cloud. |
| [Vue d’ensemble des services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md) | Les services de notification Push Windows (WNS) permettent aux développeurs tiers d’envoyer des mises à jour de toast, de vignette et de badge, ainsi que des mises à jour brutes à partir de leur propre service cloud. Il en résulte un mécanisme fiable et optimal de remise des nouvelles mises à jour aux utilisateurs. |
| [Code généré par l’Assistant Notification Push](the-code-generated-by-the-push-notification-wizard.md) | Grâce à un Assistant Visual Studio, vous pouvez générer des notifications Push à partir d’un service mobile créé via Microsoft Azure Mobile Services. L’Assistant Visual Studio génère du code qui devrait vous aider à démarrer. Cette rubrique explique comment l’Assistant modifie votre projet, ce que le code généré fait, comment utiliser ce code et ce que vous pouvez faire ensuite pour tirer le meilleur parti des notifications Push. Voir [Vue d’ensemble des services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md). |
| [Vue d’ensemble des notifications brutes](raw-notification-overview.md) | Les notifications brutes sont des notificationsPush courtes à usage général. Elles ont une finalité exclusivement didactique et n’incluent aucun composant d’interface utilisateur. Comme dans le cas d’autres notifications Push, la fonctionnalité WNS transmet les notifications brutes de votre service cloud à votre application. |
