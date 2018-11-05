---
author: mijacobs
Description: Periodic notifications, which are also called polled notifications, update tiles and badges at a fixed interval by downloading content from a cloud service.
title: Vue d’ensemble des notifications périodiques
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d02bfb8b8bd112a969895d4f2bd5d324fce9d6b8
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6027935"
---
# <a name="periodic-notification-overview"></a>Vue d’ensemble des notifications périodiques
 


Les notifications périodiques, également appelées notifications interrogées, mettent à jour les vignettes et les badges à intervalle fixe en téléchargeant du contenu à partir d’un service cloud. Pour utiliser des notifications périodiques, le code de votre application cliente doit fournir deux éléments d’informations :

-   l’URI (Uniform Resource Identifier) d’un emplacement Web que Windows interroge pour mettre à jour les vignettes ou les badges de votre application ;
-   la fréquence d’interrogation de cet URI.

Les notifications périodiques permettent à votre application d’obtenir des mises à jour de vignettes dynamiques avec un minimum d’intervention du service cloud et d’investissement client. Les notifications périodiques sont une bonne méthode de distribution du même contenu à un large public.

**Remarque**  plus en téléchargement de l' [exemple Push et les notifications périodiques](http://go.microsoft.com/fwlink/p/?linkid=231476) pour Windows8.1 et réutilisez son code source dans votre application Windows 10.

 

## <a name="how-it-works"></a>Fonctionnement


Les notifications périodiques nécessitent que votre application héberge un service cloud. Le service est interrogé périodiquement par tous les utilisateurs qui disposent de l’application. À chaque intervalle d’interrogation, par exemple une fois par heure, Windows envoie une requête HTTP GET à l’URI, télécharge le contenu demandé pour la vignette ou le badge (contenu XML par exemple), qui est fourni en réponse à la requête, puis affiche ce contenu dans la vignette de l’application.

Notez que les mises à jour périodiques ne peuvent pas être utilisées avec les notifications toast. Pour la remise des toasts, il est préférable de recourir aux notifications [planifiées](https://msdn.microsoft.com/library/windows/apps/hh465417) ou aux notifications [Push](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252).

## <a name="uri-location-and-xml-content"></a>Emplacement d’URI et contenu XML


Toute adresse Web HTTP ou HTTPS valide peut être utilisée comme URI interrogeable.

La réponse du serveur cloud comprend le contenu téléchargé. Le contenu renvoyé à partir de l’URI doit être conforme à la spécification de schéma XML de la [vignette](adaptive-tiles-schema.md) ou du [badge](https://msdn.microsoft.com/library/windows/apps/br212851). De plus, il doit être au format UTF-8. Vous pouvez utiliser des en-têtes HTTP définis pour spécifier l’[heure d’expiration](#expiration-of-tile-and-badge-notifications) ou la balise de la notification.

## <a name="polling-behavior"></a>Comportement de l’interrogation


Appelez l’une de ces méthodes pour lancer l’interrogation :

-   [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (vignette)
-   [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.BadgeUpdater#Windows_UI_Notifications_BadgeUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (badge)
-   [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (vignette)

Quand vous appelez l’une de ces méthodes, l’URI est immédiatement interrogé et la vignette ou le badge est mis à jour à l’aide du contenu reçu. Après cette interrogation initiale, Windows continue de fournir des mises à jour en fonction de l’intervalle demandé. L’interrogation se poursuit jusqu’à ce que vous l’arrêtiez explicitement (avec [**TileUpdater.StopPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater.StopPeriodicUpdate)), jusqu’à ce que votre application soit désinstallée, ou, dans le cas d’une vignette secondaire, jusqu’à ce que la vignette soit supprimée. Dans le cas contraire, Windows continue à rechercher des mises à jour pour votre vignette ou votre badge, même si votre application n’est jamais relancée.

### <a name="the-recurrence-interval"></a>Intervalle de récurrence

Vous devez définir l’intervalle de récurrence sous forme de paramètre des méthodes indiquées ci-dessus. Note que même si Windows effectue l’interrogation en respectant au mieux ce qui est indiqué, l’intervalle n’est pas précis. L’intervalle d’interrogation demandé peut être retardé de 15minutes au maximum en fonction de Windows.

### <a name="the-start-time"></a>Heure de début

Vous pouvez indiquer une heure particulière de la journée pour le lancement de l’interrogation. Prenons l’exemple d’une application qui change son contenu de vignette une seule fois par jour. Dans ce cas précis, nous vous recommandons d’effectuer l’interrogation peu de temps avant de mettre à jour votre service cloud. Par exemple, si un site propose quotidiennement des offres d’achat et s’il publie les offres du jour à 8 h 00, effectuez la recherche de nouvelles vignettes de contenu juste après 8 h 00.

Si vous indiquez une heure de début, le premier appel de la méthode entraîne immédiatement une recherche de contenu. Puis, une interrogation à intervalles réguliers démarre moins de 15 minutes après l’heure de début indiquée.

### <a name="automatic-retry-behavior"></a>Comportement des nouvelles tentatives automatiques

L’URI est interrogé seulement si l’appareil est en ligne. Si le réseau est disponible, mais que l’URI ne peut pas être contacté pour une raison quelconque, cette itération de l’intervalle d’interrogation est ignorée et l’URI est réinterrogé lors du prochain intervalle. Si l’appareil est dans un état d’arrêt, de veille ou de veille prolongée quand un intervalle d’interrogation est atteint, l’URI est interrogé quand l’appareil quitte son état d’arrêt ou de veille.

### <a name="handling-app-updates"></a>Gestion des mises à jour des applications

Si vous publiez une mise à jour d’application qui modifie votre URI d’interrogation, vous devez ajouter une [tâche en arrière-plan TimeTrigger](../../../launch-resume/run-a-background-task-on-a-timer-.md) quotidienne qui appelle StartPeriodicUpdate avec le nouveau URI pour vérifier que vos vignettes utilisent le nouvel URI. Sinon, si les utilisateurs reçoivent la mise à jour de votre application, mais ne lancent pas votre application, les vignettes utiliseront encore l’ancien URI, ce qui risque d’entraîner un échec de l’affichage si l’URI n’est plus valide ou si la charge utile renvoyée fait référence à des images locales qui n’existent plus.

## <a name="expiration-of-tile-and-badge-notifications"></a>Expiration des notifications par vignette et par badge


Par défaut, les notifications périodiques par vignette et par badge expirent trois jours après avoir été téléchargées. Quand une notification expire, le contenu est supprimé du badge, de la vignette ou de la file d’attente, et n’est plus présenté à l’utilisateur. Il est conseillé de définir un délai d’expiration explicite pour toutes les notifications périodiques par vignette et par badge en utilisant un délai approprié pour votre application ou notification, afin de vous assurer que le contenu ne persiste pas plus longtemps que nécessaire. Un délai d’expiration explicite est essentiel pour les contenus dont la durée de vie est limitée. Cette approche assure également la suppression du contenu périmé si votre service cloud n’est plus accessible, ou que l’utilisateur se déconnecte du réseau pour une période prolongée.

Votre service cloud définit une date et une heure d’expiration pour une notification en incluant l’en-tête HTTP X-WNS-Expires dans la charge utile de réponse. L’en-tête HTTP X-WNS-Expires est conforme au [format HTTP-date](http://go.microsoft.com/fwlink/p/?linkid=253706). Pour plus d’informations, voir [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) ou [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_).

Par exemple, au cours d’une journée active d’échanges sur le marché boursier, vous pouvez doubler le délai d’expiration de la mise à jour du cours d’une action par rapport à l’intervalle d’interrogation (par exemple, une heure après la réception du contenu, si vous effectuez l’interrogation chaque demi-heure). Autre exemple, dans une application d’infos, le délai d’expiration approprié pour la mise à jour quotidienne des vignettes d’infos est d’une journée.

## <a name="periodic-notifications-in-the-notification-queue"></a>Notifications périodiques dans la file d’attente de notifications


Vous pouvez utiliser des mises à jour périodiques de vignettes avec le [cycle des notifications](https://msdn.microsoft.com/library/windows/apps/hh781199). Par défaut, une vignette de l’écran d’accueil affiche le contenu d’une seule notification jusqu’à ce qu’elle soit remplacée par une nouvelle notification. Lorsque vous activez le cycle, une file d’attente peut comporter jusqu’à cinq notifications que la vignette affiche à tour de rôle.

Si la file d’attente atteint le nombre maximal de cinq notifications, la nouvelle notification suivante remplace la notification la plus ancienne dans la file d’attente. Toutefois, vous avez la possibilité de modifier cette stratégie de remplacement dans la file d’attente en définissant des balises sur vos notifications. Une balise est une chaîne spécifique de l’application, d’une longueur maximale de 16 caractères alphanumériques, qui ne respecte pas la casse et qui est spécifiée dans l’en-tête HTTP [X-WNS-Tag](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_tag) de la charge utile de réponse. Windows compare la balise d’une notification entrante aux balises de toutes les notifications déjà présentes en file d’attente. Si une correspondance existe, la nouvelle notification remplace la notification en file d’attente ayant la même balise. S’il n’existe aucune correspondance, la règle de remplacement par défaut est appliquée et la nouvelle notification remplace la notification la plus ancienne dans la file d’attente.

Vous pouvez utiliser la mise en file d’attente des notifications et les balises pour mettre en œuvre toutes sortes de scénarios de notification. Par exemple, une application de cotations boursières peut envoyer cinq notifications, chacune d’elles s’appliquant à une action spécifique et portant une balise du nom de l’action en question. De cette manière, la file d’attente ne peut pas comporter deux notifications pour la même action, avec la plus ancienne des deux qui serait obsolète.

Pour plus d’informations, voir [Utilisation de la file d’attente de notifications](https://msdn.microsoft.com/library/windows/apps/hh781199).

### <a name="enabling-the-notification-queue"></a>Activation de la file d’attente de notifications

Pour implémenter une file d’attente de notifications, activez d’abord la file d’attente pour votre vignette (voir [Comment utiliser la file d’attente de notifications avec des notifications locales](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/01/05/quickstart-how-to-use-the-tile-notification-queue-with-local-notifications/)). L’appel qui permet d’activer la file d’attente doit être effectué une et une seule fois. Cependant, cela ne pose aucun problème d’effectuer l’appel chaque fois que votre application est lancée.

### <a name="polling-for-more-than-one-notification-at-a-time"></a>Interrogation de plusieurs notifications à la fois

Vous devez fournir un URI unique pour chaque notification que Windows doit télécharger pour votre vignette. À l’aide de la méthode [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_), vous pouvez fournir jusqu’à cinq URI à la fois utilisables par la file d’attente de notifications. Chaque URI fait l’objet d’une interrogation pour une seule charge utile de notification, plus ou moins au même moment. Chaque URI interrogé peut renvoyer son propre délai d’expiration et sa propre valeur de balise.

## <a name="related-topics"></a>Rubriques connexes


* [Recommandations en matière de notifications périodiques](https://msdn.microsoft.com/library/windows/apps/hh761461)
* [Comment configurer des notifications périodiques pour les badges](https://msdn.microsoft.com/library/windows/apps/hh761476)
* [Comment configurer des notifications périodiques pour des vignettes](https://msdn.microsoft.com/library/windows/apps/hh761476)
 
