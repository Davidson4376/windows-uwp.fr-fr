---
Description: Découvrez comment créer des notifications efficaces et orienté utilisateur qui rendent vos utilisateurs productif et heureux.
title: Guide de l’expérience utilisateur de toast
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, uwp, notification, collection, groupe, l’expérience utilisateur, obtenir des conseils de l’expérience utilisateur, des conseils, action, toast, centre de maintenance, noninterruptive, notifications efficaces, les notifications non intrusives, exploitables, gérer, d’organiser
ms.localizationpriority: medium
ms.openlocfilehash: 327a2add84343be3b972f7bb1f232298e7ef92ad
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320730"
---
# <a name="toast-notification-ux-guidance"></a>Recommandations relatives à l’expérience utilisateur Notification toast
Les notifications sont une partie nécessaire de vie moderne ; elles aident les utilisateurs à être plus productif et engagés avec les applications et sites Web, mais aussi être informé avec les mises à jour. Toutefois, les notifications peuvent activer rapidement à partir d’utile d’overbearing et intrusive si elles ne sont pas conçues de façon orientée utilisateur. Vos notifications sont un clic droit en dehors de l’arrêt et il est peu probable une fois qu’ils sont désactivés, ils sont activées à nouveau.  Par conséquent, assurez-vous que vos notifications sont respectueux de l’espace d’écran de l’utilisateur et l’heure, afin que ce canal engagement ouvert.

> **API importantes** : [Package nuget de Notifications de kit de ressources de communauté de Windows](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Nous avons analysé nos données de télémétrie Windows, ainsi qu’autres premier tiers études de cas et, pour arriver à quatre règles autour de ce qui rend un récit notification excellent.  Nous sommes convaincus que ces règles sont universellement applicables, quelle que soit la plateforme et aideront vos notifications à avoir un impact positif sur vos utilisateurs.

## <a name="1-actionable-notifications"></a>1. Notifications actionnables
Notifications actionnables autoriser vos utilisateurs à être productifs sans ouvrir votre application.  Il est idéal pour que l’application lance, ce n’est pas la seule mesure de réussite, et en permettant aux utilisateurs d’accomplir petites tâches sans avoir à passer à votre application peuvent être un outil très puissant dans impatients de vos utilisateurs.

![Notification exploitable avec la zone de texte d’entrée et des boutons pour définir des rappels et répondre à la notification](images/actionable-notification-example01.png)

Ci-dessus est un exemple d’une notification qui s’appuie sur les actions. Le sentiment de finalisation de tâches est un sentiment positif universellement et faire passer cette sensation à votre application ou un site Web en envoyant des notifications ayant un contenu pertinent dans. Notifications actionnables peuvent également aider à augmenter la productivité, à la fois dans les scénarios d’entreprise et grand public, en réduisant le temps aux utilisateurs de l’action Appliquer pour accomplir ces tâches plus petites. Nous vous recommandons, y compris les actions que vos utilisateurs effectuer régulièrement ou autres éléments que vous cherchez à former vos utilisateurs à faire.  Voici quelques exemples :
* Convenance, placement, marquage ou starring contenu
* Approuver ou refuser les notes de frais, les congés, autorisations, etc.
* Réponse à des messages, des messages électroniques, groupe de conversations, commentaires, etc. de inline.
* À la fin des commandes à l’aide de [en attente de mise à jour](toast-pending-update.md)
* Configuration des alertes ou des rappels pour une autre fois, mais aussi potentiellement réservation temps sur un calendrier

Notifications actionnables sont un outil très puissant pour aider vos utilisateurs s’apparentent productif, d’accomplir des tâches et de bénéficier d’une excellente et efficace avec votre application ou un site Web.  Il existe un grand nombre d’opportunités ici ! Si vous souhaitez une aide idées de brainstorming, n’hésitez pas à contacter l’équipe de notifications de windows.

## <a name="2-timing-and-urgency"></a>2. Minutage et urgence
Contrairement à la façon dont nous pensons souvent sur les notifications, en temps réel n’est pas forcément les meilleurs ! Nous conseillons vivement aux développeurs de réflexion sur l’utilisateur et si la notification qu’il envoie des informations urgentes ou non. Les utilisateurs peuvent facilement être surchargés avec trop d’informations et obtenir frustrés si elles sont en cours interrompues pendant qu’ils tentent de focus. Windows offre quelques options pour savoir comment prendre en compte le niveau d’intrusion des notifications que vous envoyez :

**Notifications brutes :** À l’aide de [notifications brutes](raw-notification-overview.md) peut être utile pour de nombreuses raisons, surtout lorsqu’il s’agit ot en réduisant les interruptions de l’utilisateur.  Envoi de notifications brutes relancent votre application en arrière-plan, afin d’évaluer si la notification est judicieux de fournir immédiatement dans le contexte de votre application. S’il s’agit de quelque chose que vous vous sentez doit être affiché à l’utilisateur immédiatement, vous êtes en mesure d’afficher un [toast local](send-local-toast.md) à partir de là.  Si c’est quelque chose l’utilisateur n’a pas besoin voir à ce stade, vous êtes en mesure de créer un [planifiée toast](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) qui se déclenche alors à une date ultérieure.


**Ghost toast :** vous pouvez également déclencher une notification qui ignore apparaître dans l’angle inférieur droit de l’écran et à la place envoyer la notification directement au centre de maintenance. Cela s’effectue en définissant le [SupressPopup propriété](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) sur True. Bien qu’il peut y avoir certains scepticisme autour dépilant ne pas les notifications en dehors du centre de maintenance, nous voyons un 2-3 fois plus élevés l’engagement pour les toasts qui se trouvent dans le centre de maintenance sur dépilés toast.  Les utilisateurs sont plus réactives lorsqu’ils sont prêts à recevoir des notifications et que vous peuvent contrôler quand ils sont interrompus, c’est pourquoi le contenu dans le centre de maintenance peut être beaucoup plus efficace de notification non invasive des utilisateurs.

## <a name="3-clear-out-the-clutter"></a>3. Désactivez out l’encombrement
Notifications peuvent rendre persistante dans le centre de maintenance pour assez longtemps (par défaut, trois jours).  Il est impératif que vous vérifiez que le contenu qui se trouve ici est à jour et pertinentes, chaque fois que l’utilisateur ouvre le centre de maintenance. Vous êtes l’espace d’écran de l’utilisateur inutilement et occupant des emplacements qui peut être utilisés pour quelque chose de plus à jour.  Supposons que l’utilisateur installe votre application de gestion de messagerie et reçoit dix messages et dix notifications, ainsi que ces messages électroniques.  Selon votre expérience souhaitée, vous pouvez envisager d’effacer ces notifications si l’utilisateur a lire l’e-mail correspondant, ou ouvrir l’application qui permet de supprimer les éléments superflus ancien à partir du centre de maintenance.

Nous avons une série de [ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) API qui vous permettent de voir quel est le contenu dans le centre de maintenance, mais aussient gérer ces notifications. Respectez les espace d’écran de l’utilisateur et prendre en charge que vous affichez uniquement pertinents et en cours pour les utilisateurs.

## <a name="4-keeping-organized"></a>4. Organisation
Comme mentionné précédemment, le contenu dans le centre de maintenance est conservé pendant trois jours.  Pour aider vos utilisateurs à choisir les informations qu’ils recherchent rapidement, organiser les notifications dans l’action à l’aide de center [en-têtes](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) ou [collections](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection). Vous pouvez voir un exemple d’un en-tête ci-dessous.

![Exemples de toast avec en-têtes étiqueté « Camping !! »](images/toast-headers-action-center.png)

Ces notifications d’une manière de groupe afin que le contenu approprié reste ensemble (par exemple, pensez en les répartissant par championnats sports différents dans une application de sports ou de tri des messages par conversation de groupe). Les collections sont un moyen plus évident de notifications de groupe, tandis que les en-têtes sont plus subtiles, mais les deux permettent aux utilisateurs de trier et sélectionner les notifications plus rapidement.

## <a name="other-resources"></a>Autres ressources
Ces quatre points ci-dessus sont des conseils que nous avons trouvé efficaces via notre propre analyse des données de télémétrie et premier et expériences de tiers. N’oubliez pas, toutefois, que ces instructions sont exactement cela : les instructions.  Nous sommes convaincus que ces règles aidera à augmenter l’engagement et la productivité de vos notifications, mais rien ne remplace pensée orientée utilisateur et à partir de vos propres données d’apprentissage.  

Si vous envoyez dès aujourd'hui des notifications à votre application UWP, vous pouvez afficher l’analytique sur qu’est-il arrivé à vos notifications dans [partenaires](https://partner.microsoft.com/dashboard)! Ces données est fourni gratuites lorsque vous utilisez le [Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) ou [WNS API](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview). Ces mesures vous donnera plus d’informations sur ce qui se passe pour vos notifications sur la plate-forme windows, ainsi que la manière dont les utilisateurs interagissent avec les notifications. Accéder à ces données en accédant au menu sur le côté gauche engager > Notifications, puis en cliquant sur l’onglet « Analyser » dans la page de notification.  Il se trouve au même endroit que vous alliez pour envoyer des notifications à partir du centre de partenaires.

## <a name="related-topics"></a>Rubriques connexes

* [Contenu de toast](adaptive-interactive-toasts.md)
* [Notifications brutes](raw-notification-overview.md)
* [Mise à jour en attente](toast-pending-update.md)
* [Bibliothèque de notifications sur GitHub (partie de la boîte à outils de la Communauté Windows)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
