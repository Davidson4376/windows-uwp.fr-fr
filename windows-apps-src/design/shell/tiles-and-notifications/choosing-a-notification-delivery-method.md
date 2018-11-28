---
Description: This article covers the four notification options&\#8212;local, scheduled, periodic, and push&\#8212;that deliver tile and badge updates and toast notification content.
title: Choisir une méthode de remise de notification
ms.assetid: FDB43EDE-C5F2-493F-952C-55401EC5172B
label: Choose a notification delivery method
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 503f7baad0d91f4e7c29010145ecb162f98bc81c
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7826618"
---
# <a name="choose-a-notification-delivery-method"></a>Choisir une méthode de remise de notification

 


Ce article présente les quatre options de notification (locale, planifiée, périodique et Push) disponibles pour remettre des mises à jour de vignette et de badge, ainsi que du contenu de notification toast. Une vignette ou une notification toast peuvent transmettre des informations à votre utilisateur, même si celui-ci n’est pas en train d’utiliser votre application. La nature et le contenu de votre application et des informations que vous voulez remettre peuvent vous aider à déterminer la méthode de notification la mieux appropriée à votre cas.

## <a name="notification-delivery-methods-overview"></a>Vue d’ensemble des méthodes de remise des notifications


Une application peut utiliser quatre méthodes pour remettre une notification:

-   **Locales**
-   **Planifiées**
-   **Périodiques**
-   **Push**

Ce tableau récapitule les types de remise des notifications.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Mode de remise</th>
<th align="left">À utiliser avec</th>
<th align="left">Description</th>
<th align="left">Exemples</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Locales</td>
<td align="left">Vignette, Badge, Toast</td>
<td align="left">Ensemble d’appels d’API qui envoient des notifications pendant que votre application s’exécute, lesquelles mettent directement à jour la vignette ou le badge, ou envoient une notification toast.</td>
<td align="left"><ul>
<li>Une application musicale met à jour sa vignette de sorte à afficher la &quot;Lecture en cours&quot;.</li>
<li>Une application de jeu met à jour sa vignette en indiquant le meilleur score de l’utilisateur lorsqu’il quitte le jeu.</li>
<li>Un badge dont le glyphe indique qu’il y a de nouvelles informations dans l’application est désactivé lors de la désactivation de celle-ci.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Planifiées</td>
<td align="left">Vignette, Toast</td>
<td align="left">Ensemble d’appels d’API qui planifient une notification à l’avance afin d’effectuer une mise à jour à l’heure que vous indiquez.</td>
<td align="left"><ul>
<li>Une application de calendrier définit un rappel sous forme de notification toast pour une prochaine réunion.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Périodiques</td>
<td align="left">Vignette, Badge</td>
<td align="left">Notifications qui mettent à jour les vignettes et les badges régulièrement à intervalles fixes en interrogeant un service cloud afin d’obtenir le nouveau contenu.</td>
<td align="left"><ul>
<li>Une application météo met à jour sa vignette, laquelle indique les prévisions toutes les 30minutes.</li>
<li>Un site &quot;Affaire du jour&quot; met à jour son offre quotidienne tous les matins.</li>
<li>Une vignette qui indique le nombre de jours restant avant un événement met quotidiennement à jour le compte à rebours à minuit.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Push</td>
<td align="left">Vignette, badge, toast, brut</td>
<td align="left">Notifications envoyées depuis un serveur cloud, même si votre application n’est pas en cours d’exécution.</td>
<td align="left"><ul>
<li>Une application de shopping envoie une notification toast pour informer un utilisateur de soldes sur un article qu’il regarde.</li>
<li>Une application d’information met à jour sa vignette au fil des actualités.</li>
<li>Une application de sports met sa vignette à jour pendant un match.</li>
<li>Une application de communication fournit des alertes en cas de messages entrants ou d’appels téléphoniques.</li>
</ul></td>
</tr>
</tbody>
</table>

 

## <a name="local-notifications"></a>Notifications locales


La mise à jour de la vignette ou du badge de l’application, ou le déclenchement d’une notification toast pendant que l’application s’exécute constitue le mécanisme de remise des notifications le plus simple; il requiert uniquement des appels d’API locaux. Chaque application peut donner des informations utiles ou intéressantes sur la vignette, même si ce contenu ne change qu’une fois que l’utilisateur a lancé l’application ou interagi avec elle. Les notifications locales sont également un bon moyen d’assurer l’actualisation de la vignette de l’application, même si vous utilisez également l’un des autres mécanismes de notification. Par exemple, la vignette d’une application de photo peut montrer des photos d’un album récemment ajouté.

Nous recommandons que votre application mette à jour sa vignette en local au premier démarrage, ou au moins immédiatement après toute modification apportée par l’utilisateur qui se refléterait normalement sur la vignette. Cette mise à jour ne se voit pas tant que l’utilisateur n’a pas quitté l’application, mais l’effectuer alors que l’application est en cours d’utilisation permet de veiller à ce que la vignette soit déjà mise à jour au moment où l’utilisateur s’en va.

Alors que les appels d’API sont locaux, les notifications peuvent référencer des images Web. Si l’image Web n’est pas disponible au téléchargement, est endommagée ou ne satisfait pas les spécifications correspondantes, les vignettes et le toast répondent différemment :

-   Vignettes: la mise à jour ne s’affiche pas
-   Toast: la notification s’affiche, mais sans votre image

Par défaut, les notifications toast locales ont une durée de vie de troisjours, mais les notifications par vignette locales n’expirent jamais. Nous vous recommandons de remplacer ces valeurs par défaut par une durée d’expiration explicite adaptée à vos notifications (durée de vie maximum des notifications toast: troisjours). 

Pour plus d’informations, consultez les rubriques suivantes:

-   [Envoyer une notification par vignette locale](sending-a-local-tile-notification.md)
-   [Envoyer une notification toast locale](send-local-toast.md)
-   [Exemples de code de notifications de plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="scheduled-notifications"></a>Notifications planifiées


Les notifications planifiées sont le sous-ensemble des notifications locales pouvant indiquer l’heure précise à laquelle une vignette doit être mise à jour ou une notification toast envoyée. Les notifications planifiées sont idéales pour les situations dans lesquelles le contenu à mettre à jour est connu d’avance, comme dans une invitation à une réunion. Si vous ne connaissez pas le contenu de la notification à l’avance, il est préférable d’utiliser une notification Push ou périodique.

Notez que les notifications planifiées ne peuvent pas être utilisées pour les notifications de badge; celles-ci sont mieux servies par des notifications locales, périodiques ou Push.

Par défaut, les notifications planifiées expirent trois jours après leur émission. Vous pouvez modifier ce délai d’expiration par défaut dans les notifications par vignette planifiées, mais pas dans les notifications toast planifiées.

Pour plus d’informations, consultez les rubriques suivantes:

-   [Exemples de code de notifications de plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="periodic-notifications"></a>Notifications périodiques


Les notifications périodiques vous donnent des mises à jour de vignette en direct avec un minimum de service cloud et d’investissement client. Elles sont également une excellente méthode de distribution du même contenu à un large public. Votre code client spécifie l’URL d’un emplacement cloud que Windows interroge afin d’obtenir les mises à jour de vignette ou de badge, ainsi que la fréquence d’interrogation de cet emplacement. À chaque interrogation, Windows contacte l’URL afin de télécharger le contenu XML spécifié pour l’afficher sur la vignette.

Les notifications périodiques requièrent que l’application héberge un service cloud, et ce service est interrogé en fonction de l’intervalle spécifié par tous les utilisateurs qui ont installé l’application. Notez que les mises à jour périodiques ne peuvent pas être utilisées pour les notifications toast; celles-ci sont mieux servies par des notifications planifiées ou Push.

Par défaut, les notifications périodiques expirent trois jours après l’interrogation. Si nécessaire, vous pouvez remplacer cette valeur par défaut par un délai d’expiration explicite.

Pour plus d’informations, voir les rubriques suivantes :

-   [Vue d’ensemble des notifications périodiques](periodic-notification-overview.md)
-   [Exemples de code de notification de plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <a name="push-notifications"></a>Notifications Push


Les notifications Push sont idéales pour communiquer des données en temps réel ou des données personnalisées pour l’utilisateur. Les notifications Push servent au contenu généré à des moments imprévus, comme les actualités, les mises à jour de réseaux sociaux ou les messages instantanés. Les notifications Push s’avèrent également utiles dans les situations dans lesquelles les données sont ponctuelles d’une manière qui ne convient pas aux notifications périodiques, notamment comme l’évolution des scores pendant un événement sportif.

Les notifications Push requièrent un service cloud qui gère des canaux de notification Push et détermine quand et à qui envoyer les notifications.

Par défaut, les notifications Push expirent troisjours après leur réception par l’appareil. Si nécessaire, vous pouvez remplacer cette valeur par défaut par un délai d’expiration explicite (durée de vie maximale des notifications Toast: 3jours).

Pour plus d’informations, consultez:

-   [Vue d’ensemble des services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md)
-   [Recommandations en matière de notifications Push](https://msdn.microsoft.com/library/windows/apps/hh761462)
-   [Exemples de code de notification de plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)


## <a name="related-topics"></a>Rubriques connexes


* [Envoyer une notification par vignette locale](sending-a-local-tile-notification.md)
* [Envoyer une notification toast locale](send-local-toast.md)
* [Recommandations en matière de notifications Push](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [Recommandations en matière de notifications toast](https://msdn.microsoft.com/library/windows/apps/hh465391)
* [Vue d’ensemble des notifications périodiques](periodic-notification-overview.md)
* [Vue d’ensemble des services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md)
* [Exemples de code de notification de plateforme Windows universelle (UWP) sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)
 

 




