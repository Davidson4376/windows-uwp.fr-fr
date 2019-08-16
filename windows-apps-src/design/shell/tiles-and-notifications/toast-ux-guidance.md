---
Description: Découvrez comment créer des notifications efficaces et orientées utilisateur qui rendent vos utilisateurs productifs et heureux.
title: Conseils sur Toast UX
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, UWP, notification, collection, groupe, expérience utilisateur, conseils, conseils, action, toast, centre de maintenance, notifications non interinterrompues, notifications efficaces, notifications non intrusives, action possible, gérer, organiser
ms.localizationpriority: medium
ms.openlocfilehash: 111ac9a216b87e120e42c9db7761bd8588548029
ms.sourcegitcommit: 720d8778d94ef44d2f86d2f1f0ebb05d6420805d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68415084"
---
# <a name="toast-notification-ux-guidance"></a>Guide d’expérience utilisateur des notifications Toast
Les notifications sont un élément essentiel de la vie moderne; ils aident les utilisateurs à être plus productifs et à être engagés avec des applications et des sites Web, et à rester à jour avec toutes les mises à jour. Toutefois, les notifications peuvent rapidement devenir utiles pour se décharger et intrusives si elles ne sont pas conçues de façon centrée sur l’utilisateur. Vos notifications sont accessibles par un clic droit à l’extérieur et il est peu probable qu’elles soient désactivées. elles sont réactivées.  Par conséquent, assurez-vous que vos notifications respectent l’espace et l’heure de l’écran de l’utilisateur, ce qui vous permet de garder ce canal d’engagement ouvert.

> **API importantes** : [Package NuGet de notifications de la communauté Windows](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Nous avons analysé nos données de télémétrie Windows, ainsi que d’autres études de cas de première et tierces, afin de mettre en place quatre règles concernant ce qui constitue un excellent scénario de notification.  Nous sommes convaincus que ces règles sont universellement applicables, quelle que soit la plate-forme, et qui aideront vos notifications à avoir un impact positif sur vos utilisateurs.

## <a name="1-actionable-notifications"></a>1. Notifications actionnables
Les notifications exploitables permettent à vos utilisateurs d’être productifs sans ouvrir votre application.  Bien qu’il soit intéressant de lancer l’application, il ne s’agit pas de la seule mesure de réussite, et permettre aux utilisateurs d’effectuer de petites tâches sans avoir à accéder à votre application peut être un outil très puissant pour mettre vos utilisateurs en lumière.

![Notification actionnable avec zone de texte d’entrée et boutons pour définir des rappels et répondre à la notification](images/actionable-notification-example01.png)

L’exemple ci-dessus illustre une notification qui s’appuie sur des actions. Le sentiment de la finition des tâches est un sentiment universellement positif. vous pouvez apporter cette sensation à votre application ou site Web en envoyant des notifications qui contiennent du contenu actionnable. Les notifications exploitables peuvent également contribuer à augmenter la productivité, dans les scénarios d’entreprise et de consommation, en diminuant le temps nécessaire aux utilisateurs pour accomplir ces tâches plus petites. Nous vous recommandons d’inclure les actions que vos utilisateurs prennent régulièrement, ou les éléments que vous essayez d’apprendre à former vos utilisateurs.  Voici quelques exemples :
* Souhaits, placement, marquage ou contenu de acteurs
* Approuver ou refuser des notes de frais, des congés, des autorisations, etc.
* Réponse en ligne à des messages, e-mails, conversations de groupe, commentaires, etc.
* Exécution des commandes en [attente de mise à jour](toast-pending-update.md)
* Définition d’alertes ou de rappels à un autre moment, ainsi que la réservation de temps dans un calendrier

Les notifications exploitables sont un outil très puissant pour aider vos utilisateurs à être productifs, à accomplir des tâches et à bénéficier d’une expérience efficace et efficace avec votre application ou site Web.  Il y a beaucoup d’opportunités ici. Si vous souhaitez aider à faire des idées de brainstorming, n’hésitez pas à contacter l’équipe Windows notifications.

## <a name="2-timing-and-urgency"></a>2. Minutage et urgence
Contrairement à la façon dont nous pensons souvent aux notifications, le temps réel n’est pas forcément le meilleur! Nous recommandons vivement aux développeurs de réfléchir à l’utilisateur et, si la notification qu’il envoie est des informations urgentes ou non. Les utilisateurs peuvent facilement être surchargés avec trop d’informations et être frustrés s’ils sont interrompus alors qu’ils essaient de se concentrer. Windows fournit quelques options pour tenir compte de l’intrusion des notifications envoyées:

**Notifications brutes:** L'utilisation de [notifications brutes](raw-notification-overview.md) peut être bénéfique pour de nombreuses raisons, en particulier lorsqu’il s’agit de réduire les interruptions à l’utilisateur.  L’envoi de notifications brutes met en éveil votre application en arrière-plan, ce qui vous permet d’évaluer si la notification est logique de remettre immédiatement dans le contexte de votre application. Si vous pensez que l’utilisateur doit s’afficher immédiatement, vous pouvez dépiler un [Toast local](send-local-toast.md) à partir de là.  S’il s’agit d’un nom que l’utilisateur n’a pas besoin de voir pour l’instant, vous pouvez créer un [Toast planifié](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) qui s’activera ultérieurement.


**Ghost Toast:** vous pouvez également déclencher une notification qui s’affiche dans le coin inférieur droit de l’écran et envoie la notification directement au centre de maintenance. Pour ce faire, affectez la valeur true à la [propriété SupressPopup](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) . Bien qu’il puisse y avoir un certain risque de ne pas détourner les notifications en dehors du centre de maintenance, nous voyons un engagement 2-3 fois plus élevé pour les toasts qui vivent dans le centre de maintenance sur le Toast dépilé.  Les utilisateurs sont plus réactifs lorsqu’ils sont prêts à recevoir des notifications et peuvent contrôler le moment où ils sont interrompus, ce qui explique pourquoi le contenu du centre de maintenance peut être tellement plus efficace pour notifier les utilisateurs de manière non invasive.

## <a name="3-clear-out-the-clutter"></a>3. Éliminez le désordre
Les notifications peuvent être conservées dans le centre de notifications pendant une durée relativement longue (trois jours par défaut).  Il est impératif de vérifier que le contenu qui se trouve ici est à jour et pertinent chaque fois que l’utilisateur ouvre le centre de maintenance. Vous gaspillez l’espace d’écran de l’utilisateur et vous occupez des emplacements qui peuvent être utilisés pour un plus grand nombre de mises à jour.  Supposons que l’utilisateur installe votre application de gestion de la messagerie et reçoit dix courriers électroniques et dix notifications avec ces messages.  En fonction de l’expérience souhaitée, vous pouvez envisager de désactiver ces notifications si l’utilisateur a lu l’e-mail correspondant, ou d’ouvrir l’application pour supprimer les anciens courriers électroniques du centre de maintenance.

Nous disposons d’une série d’API [ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) qui vous permettent de voir le contenu qui se trouve dans le centre de maintenance, ainsi que de gérer ces notifications. Soyez respectueux de l’espace d’écran de l’utilisateur et veillez à ne montrer que le contenu pertinent et actuel aux utilisateurs.

## <a name="4-keeping-organized"></a>4. Maintien de l’Organisation
Comme mentionné précédemment, le contenu dans le centre de maintenance des actions est conservé pendant trois jours.  Pour aider vos utilisateurs à choisir les informations qu’ils recherchent rapidement, organisez les notifications dans le centre de maintenance à l’aide d'[en-têtes](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) ou de [regroupements](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection). Vous pouvez voir un exemple d’en-tête ci-dessous.

![Exemples toast avec en-têtes étiquetés «Camping!!»](images/toast-headers-action-center.png)

Regroupez ces notifications de manière à ce que le contenu approprié reste en ensemble (par exemple, pensez à séparer les différents sports Leagues dans une application sportive ou à trier les messages par conversation de groupe). Les collections sont un moyen plus évident de regrouper les notifications, tandis que les en-têtes sont plus subtiles, mais elles permettent aux utilisateurs de trier et de récupérer des notifications plus rapidement.



Ces quatre points ci-dessus sont des conseils que nous avons constatés par notre propre analyse de la télémétrie et par le biais d’expériences de premier et de tiers. Gardez toutefois à l’esprit que ces instructions sont les suivantes: instructions.  Nous sommes convaincus que ces règles vous aideront à augmenter l’engagement et à accroître la productivité de vos notifications, mais rien ne peut remplacer l’approche centrée sur l’utilisateur et l’apprentissage à partir de vos propres données.  

## <a name="related-topics"></a>Rubriques connexes

* [Contenu Toast](adaptive-interactive-toasts.md)
* [Notifications brutes](raw-notification-overview.md)
* [Mise à jour en attente](toast-pending-update.md)
* [Bibliothèque de notifications sur GitHub (qui fait partie de la boîte à outils de la communauté Windows)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
