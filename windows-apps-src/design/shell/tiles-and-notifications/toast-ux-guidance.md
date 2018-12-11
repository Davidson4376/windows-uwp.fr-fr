---
Description: Learn how to create effective and user-focused notifications that make your users productive and happy.
title: Recommandations d’expérience utilisateur de toast
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, uwp, notification, collection, groupe, expérience utilisateur, expérience utilisateur des instructions, recommandations, action, toast, centre de notifications, noninterruptive, notifications efficaces, les notifications non intrusives, exploitables, gérer, organiser
ms.localizationpriority: medium
ms.openlocfilehash: 878df85db9ab0e33db06a86ddb726f07dc28f013
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8891571"
---
# <a name="toast-notification-ux-guidance"></a>Recommandations d’expérience utilisateur toast Notification
Les notifications sont une partie nécessaire de la vie moderne; ils aident les utilisateurs à être plus productifs et commencé avec les applications et sites Web, mais aussi rester en cours avec les mises à jour. Toutefois, les notifications peuvent activer rapidement à partir d’utile à overbearing et contraignant s’ils ne sont pas conçus de manière centrée sur l’utilisateur. Vos notifications sont un clic droit direction opposée en cours est désactivée et il est peu probable une fois qu’ils sont désactivés, ils ne seront activés à nouveau.  Par conséquent, assurez-vous que vos notifications sont de l’espace d’écran de l’utilisateur et l’heure, afin que vous pouvez conserver ce canal d’engagement ouvert.

> **API importantes**: [package nuget Windows Community Toolkit Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Nous avons analysés nos données de télémétrie Windows, ainsi qu’autres premier et études de cas de tiers, pour élaborer des quatre règles autour de ce qui rend un article excellent notification.  Nous sommes sûr ces règles sont universellement applicables, quel que soit la plateforme et aideront vos notifications d’avoir un impact positif sur vos utilisateurs.

## <a name="1-actionable-notifications"></a>1. exploitables notifications
Notifications interactives permettent à vos utilisateurs d’être productifs sans ouvrir votre application.  Alors qu’elle est excellente pour que l’application est lancée, cela n’est pas la seule mesure de réussite et qui permet aux utilisateurs de prendre accomplir des petites tâches sans avoir à accéder à votre application peuvent être un outil puissant dans delighting vos utilisateurs.

![Notification exploitable avec une zone de saisie de texte et les boutons de définir des rappels et de répondre à la notification](images/actionable-notification-example01.png)

Ci-dessus est un exemple d’une notification qui s’appuie sur les actions. La sensation des tâches de fin est un effet universellement positif, et vous pouvez apporter cette sensation à votre application ou site Web en envoyant des notifications qui disposent de contenu exploitable. Notifications interactives permettent également d’accroître la productivité, tant dans les scénarios d’entreprise et grand public, en réduisant le temps aux utilisateurs de l’action parcourent pour accomplir ces tâches plus petites. Nous vous recommandons d’inclure les actions que vos utilisateurs effectuer régulièrement ou autres éléments que vous essayez de former vos utilisateurs à faire.  Voici quelques exemples:
* Convenance, favoriting, marquer ou starring contenu
* Approuver ou refuser les notes de frais, temps arrêt, autorisations, etc..
* Inline répondre à des messages, e-mails, historiques de conversation de groupe, commentaires, etc..
* Fin de commandes à l’aide de la [mise à jour en attente](toast-pending-update.md)
* Définition des alertes ou des rappels pour un autre moment, mais aussi potentiellement réservation temps dans un calendrier

Notifications interactives sont un outil puissant pour aider les utilisateurs ressentez productif, d’accomplir des tâches et d’une expérience réussie et efficace avec votre application ou site Web.  Il existe de nombreuses opportunités ici! Si vous souhaitez aide idées de brainstorming, n’hésitez pas à contactez l’équipe de notifications windows.

## <a name="2-timing-and-urgency"></a>2. minutage et d’urgence
Contrairement aux que la manière dont nous pensent souvent sur les notifications, en temps réel n’est pas forcément les meilleurs! Nous conseillons vivement aux développeurs de réflexion relatives à l’utilisateur et si la notification qu’il envoie est informations ou non. Les utilisateurs peuvent facilement être surchargées avec trop d’informations et sont sentir si elles sont en cours interrompus pendant qu’ils tentent de focus. Windows fournit plusieurs options pour savoir comment prendre en compte le niveau d’intrusion de vous envoyez des notifications:

**Des notifications brutes:** À l’aide de [notifications brutes](raw-notification-overview.md) permettre s’avérer utile pour de nombreuses raisons, en particulier lorsqu’il s’agit pas réduction des interruptions de l’utilisateur.  Envoi de notifications brutes est réveiller votre application en arrière-plan, afin d’évaluer si la notification est judicieux de fournir immédiatement dans le contexte de votre application. Si elle est quelque chose que vous ressentez doit être affichée à l’utilisateur immédiatement, vous êtes en mesure d’afficher un [toast locale](send-local-toast.md) à partir de là.  S’il s’agit d’un élément l’utilisateur n’a pas besoin de voir pour l’instant, vous êtes en mesure de créer un [toast planifiée](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) qui se déclenchera à un moment ultérieur.


**Ghost toast:** vous pouvez également se déclencher une notification ignorer train dans le coin inférieur droit de l’écran, et l’envoyer à la place de la notification directement au centre de notifications. Cette opération est effectuée en définissant la [propriété SupressPopup](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) sur True. Bien qu’il puisse être certains scepticisme autour Testez-le ne pas les notifications en dehors du centre de notifications, nous voyons un 2-3 fois supérieure engagement pour les notifications toast qui résident dans le centre de notifications sur les extraits toast.  Les utilisateurs sont plus réactives lorsqu’ils sont prêts à recevoir des notifications et que vous peuvent contrôler quand ils sont interrompus, c’est pourquoi le contenu dans le centre de notifications peut être beaucoup plus efficace de non invasive informer les utilisateurs.

## <a name="3-clear-out-the-clutter"></a>3. clair déconnecter l’encombrement
Les notifications peuvent conservé dans le centre de notifications pour un certain temps (trois jours par défaut).  Il est impératif que vous vérifiez que le contenu qui se trouve ici est à jour et pertinentes chaque fois que l’utilisateur ouvre le centre de notifications. Vous êtes gaspiller de l’espace d’écran de l’utilisateur et occupent des emplacements susceptibles d’être utilisées pour quelque chose de plus à jour.  Supposons que l’utilisateur installe votre application de gestion de courrier électronique et reçoit les dix e-mails et des dix notifications ainsi que de ces e-mails.  En fonction de votre expérience de votre choix, vous pouvez envisager d’effacement de ces notifications si l’utilisateur a lire le message électronique correspondant, ou ouvrir l’application comme un moyen de supprimer l’encombrement de l’ancien centre de notifications.

Nous avons une série de [ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) API qui vous permettent de voir quel est le contenu dans le centre de notifications, ainsi que gérer ces notifications. Respectez les d’espace d’écran de l’utilisateur et prendre en charge que vous affichez uniquement le contenu actuel et adaptées au contexte pour les utilisateurs.

## <a name="4-keeping-organized"></a>4. organisées de conservation
Comme mentionné précédemment, le contenu dans le centre de notifications persiste pendant trois jours.  Pour aider les utilisateurs de sélectionner les informations qu’ils recherchent rapidement, organiser les notifications dans le centre de notifications à l’aide des [en-têtes](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) ou des [collections](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection). Vous pouvez voir un exemple d’en-tête ci-dessous.

![Exemples de toast avec des en-têtes intitulée «Camping!!»](images/toast-headers-action-center.png)

Ces notifications d’une manière de groupe afin que le contenu pertinent reste ensemble (autrement dit, pensez à séparer ligues sports différentes dans une application sportive, ou le tri des messages par conversation de groupe). Les collections sont un moyen plus évident de notifications de groupe, tandis que les en-têtes sont plus subtiles, mais les deux permettent aux utilisateurs de trier et de sélectionner les notifications plus rapidement.

## <a name="other-resources"></a>Autres ressources
Ces quatre points ci-dessus sont des conseils que nous avons découvert efficaces par le biais de notre propre analyse des données de télémétrie et par le biais de la première et les expériences de tiers. N’oubliez pas, toutefois, que ces recommandations sont atteindre cet: recommandations.  Nous sommes sûr ces règles aideront à augmenter l’engagement et la productivité de vos notifications, mais rien ne remplace centrée sur l’utilisateur et à apprendre à partir de vos propres données.  

Si vous envoyez des notifications à votre application UWP aujourd'hui, vous pouvez afficher analytique sur ce qui se passe à vos notifications dans [L’espace partenaires](https://partner.microsoft.com/dashboard)! Ces données provient gratuites lorsque vous utilisez le [Windows Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) ou les [API de WNS](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview). Ces métriques vous donne en plus des informations sur ce qui se produit à vos notifications sur la plateforme windows, ainsi que la manière dont les utilisateurs interagissent avec les notifications. Accéder à ces données en accédant au menu sur le côté gauche engager > Notifications, puis en cliquant sur l’onglet «Analyse» dans la page de Notifications.  Elle se trouve dans le même emplacement que vous est placée pour envoyer des notifications à partir de l’espace partenaires.

## <a name="related-topics"></a>Rubriquesconnexes

* [Contenu des toasts](adaptive-interactive-toasts.md)
* [Notifications brutes](raw-notification-overview.md)
* [Mise à jour en attente](toast-pending-update.md)
* [Bibliothèque Notifications sur GitHub (faisant partie du Kit de ressources de la Communauté Windows)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
