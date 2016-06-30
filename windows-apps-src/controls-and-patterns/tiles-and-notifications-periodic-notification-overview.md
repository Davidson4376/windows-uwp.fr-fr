---
author: mijacobs
Description: "Les notifications périodiques, également appelées notifications interrogées, mettent à jour les vignettes et les badges à intervalle fixe en téléchargeant du contenu à partir d’un service cloud."
title: "Vue d’ensemble des notifications périodiques"
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
label: TBD
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 55932595e0d5592003456a28d00ffd70c5e05eba

---

# Vue d’ensemble des notifications périodiques





Les notifications périodiques, également appelées notifications interrogées, mettent à jour les vignettes et les badges à intervalle fixe en téléchargeant du contenu à partir d’un service cloud. Pour utiliser des notifications périodiques, le code de votre application cliente doit fournir deux éléments d’informations :

-   l’URI (Uniform Resource Identifier) d’un emplacement Web que Windows interroge pour mettre à jour les vignettes ou les badges de votre application ;
-   la fréquence d’interrogation de cet URI.

Les notifications périodiques permettent à votre application d’obtenir des mises à jour de vignettes dynamiques avec un minimum d’intervention du service cloud et d’investissement client. Les notifications périodiques sont une bonne méthode de distribution du même contenu à un large public.

**Remarque** Pour plus d’informations, téléchargez l’[exemple de notifications Push et périodiques](http://go.microsoft.com/fwlink/p/?linkid=231476) pour Windows 8.1, puis réutilisez son code source dans votre application Windows 10.

 

## <span id="How_it_works"></span><span id="how_it_works"></span><span id="HOW_IT_WORKS"></span>Fonctionnement


Les notifications périodiques nécessitent que votre application héberge un service cloud. Le service est interrogé périodiquement par tous les utilisateurs qui disposent de l’application. À chaque intervalle d’interrogation, par exemple une fois par heure, Windows envoie une requête HTTP GET à l’URI, télécharge le contenu demandé pour la vignette ou le badge (contenu XML par exemple), qui est fourni en réponse à la requête, puis affiche ce contenu dans la vignette de l’application.

Notez que les mises à jour périodiques ne peuvent pas être utilisées avec les notifications toast. Pour la remise des toasts, il est préférable de recourir aux notifications [planifiées](https://msdn.microsoft.com/library/windows/apps/hh465417) ou aux notifications [Push](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252).

## <span id="URI_location_and_XML_content"></span><span id="uri_location_and_xml_content"></span><span id="URI_LOCATION_AND_XML_CONTENT"></span>Emplacement d’URI et contenu XML


Toute adresse Web HTTP ou HTTPS valide peut être utilisée comme URI interrogeable.

La réponse du serveur cloud comprend le contenu téléchargé. Le contenu renvoyé à partir de l’URI doit être conforme à la spécification de schéma XML de la [vignette](tiles-and-notifications-adaptive-tiles-schema.md) ou du [badge](https://msdn.microsoft.com/library/windows/apps/br212851). De plus, il doit être au format UTF-8. Vous pouvez utiliser des en-têtes HTTP définis pour spécifier l’[heure d’expiration](#expiry) ou la [balise](#taggo) de la notification.

## <span id="Polling_Behavior"></span><span id="polling_behavior"></span><span id="POLLING_BEHAVIOR"></span>Comportement de l’interrogation


Appelez l’une de ces méthodes pour lancer l’interrogation :

-   [
            **StartPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701684) (vignette)
-   [
            **StartPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701611) (badge)
-   [
            **StartPeriodicUpdateBatch**](https://msdn.microsoft.com/library/windows/apps/hh967945) (vignette)

Quand vous appelez l’une de ces méthodes, l’URI est immédiatement interrogé et la vignette ou le badge est mis à jour à l’aide du contenu reçu. Après cette interrogation initiale, Windows continue de fournir des mises à jour en fonction de l’intervalle demandé. L’interrogation se poursuit jusqu’à ce que vous l’arrêtiez explicitement (avec [**TileUpdater.StopPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701697)), jusqu’à ce que votre application soit désinstallée, ou, dans le cas d’une vignette secondaire, jusqu’à ce que la vignette soit supprimée. Dans le cas contraire, Windows continue à rechercher des mises à jour pour votre vignette ou votre badge, même si votre application n’est jamais relancée.

### <span id="The_recurrence_interval"></span><span id="the_recurrence_interval"></span><span id="THE_RECURRENCE_INTERVAL"></span>Intervalle de récurrence

Vous devez définir l’intervalle de récurrence sous forme de paramètre des méthodes indiquées ci-dessus. Note que même si Windows effectue l’interrogation en respectant au mieux ce qui est indiqué, l’intervalle n’est pas précis. L’intervalle d’interrogation demandé peut être retardé de 15 minutes au maximum en fonction de Windows.

### <span id="The_start_time"></span><span id="the_start_time"></span><span id="THE_START_TIME"></span>Heure de début

Vous pouvez indiquer une heure particulière de la journée pour le lancement de l’interrogation. Prenons l’exemple d’une application qui change son contenu de vignette une seule fois par jour. Dans ce cas précis, nous vous recommandons d’effectuer l’interrogation peu de temps avant de mettre à jour votre service cloud. Par exemple, si un site propose quotidiennement des offres d’achat et s’il publie les offres du jour à 8 h 00, effectuez la recherche de nouvelles vignettes de contenu juste après 8 h 00.

Si vous indiquez une heure de début, le premier appel de la méthode entraîne immédiatement une recherche de contenu. Puis, une interrogation à intervalles réguliers démarre moins de 15 minutes après l’heure de début indiquée.

### <span id="Automatic_retry_behavior"></span><span id="automatic_retry_behavior"></span><span id="AUTOMATIC_RETRY_BEHAVIOR"></span>Comportement des nouvelles tentatives automatiques

L’URI est interrogé seulement si l’appareil est en ligne. Si le réseau est disponible, mais que l’URI ne peut pas être contacté pour une raison quelconque, cette itération de l’intervalle d’interrogation est ignorée et l’URI est réinterrogé lors du prochain intervalle. Si l’appareil est dans un état d’arrêt, de veille ou de veille prolongée quand un intervalle d’interrogation est atteint, l’URI est interrogé quand l’appareil quitte son état d’arrêt ou de veille.

## <span id="expiry"></span><span id="EXPIRY"></span>Expiration des notifications par vignette et par badge


Par défaut, les notifications périodiques par vignette et par badge expirent trois jours après avoir été téléchargées. Quand une notification expire, le contenu est supprimé du badge, de la vignette ou de la file d’attente, et n’est plus présenté à l’utilisateur. Il est conseillé de définir un délai d’expiration explicite pour toutes les notifications périodiques par vignette et par badge en utilisant un délai approprié pour votre application ou notification, afin de vous assurer que le contenu ne persiste pas plus longtemps que nécessaire. Un délai d’expiration explicite est essentiel pour les contenus dont la durée de vie est limitée. Cette approche assure également la suppression du contenu périmé si votre service cloud n’est plus accessible, ou que l’utilisateur se déconnecte du réseau pour une période prolongée.

Votre service cloud définit une date et une heure d’expiration pour une notification en incluant l’en-tête HTTP X-WNS-Expires dans la charge utile de réponse. L’en-tête HTTP X-WNS-Expires est conforme au [format HTTP-date](http://go.microsoft.com/fwlink/p/?linkid=253706). Pour plus d’informations, voir [**StartPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701684) ou [**StartPeriodicUpdateBatch**](https://msdn.microsoft.com/library/windows/apps/hh967945).

Par exemple, au cours d’une journée active d’échanges sur le marché boursier, vous pouvez doubler le délai d’expiration de la mise à jour du cours d’une action par rapport à l’intervalle d’interrogation (par exemple, une heure après la réception du contenu, si vous effectuez l’interrogation chaque demi-heure). Autre exemple, dans une application d’infos, le délai d’expiration approprié pour la mise à jour quotidienne des vignettes d’infos est d’une journée.

## <span id="taggo"></span><span id="TAGGO"></span>Notifications périodiques dans la file d’attente de notifications


Vous pouvez utiliser des mises à jour périodiques de vignettes avec le [cycle des notifications](https://msdn.microsoft.com/library/windows/apps/hh781199). Par défaut, une vignette de l’écran d’accueil affiche le contenu d’une seule notification jusqu’à ce qu’elle soit remplacée par une nouvelle notification. Lorsque vous activez le cycle, une file d’attente peut comporter jusqu’à cinq notifications que la vignette affiche à tour de rôle.

Si la file d’attente atteint le nombre maximal de cinq notifications, la nouvelle notification suivante remplace la notification la plus ancienne dans la file d’attente. Toutefois, vous avez la possibilité de modifier cette stratégie de remplacement dans la file d’attente en définissant des balises sur vos notifications. Une balise est une chaîne spécifique de l’application, d’une longueur maximale de 16 caractères alphanumériques, qui ne respecte pas la casse et qui est spécifiée dans l’en-tête HTTP [X-WNS-Tag](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_tag) de la charge utile de réponse. Windows compare la balise d’une notification entrante aux balises de toutes les notifications déjà présentes en file d’attente. Si une correspondance existe, la nouvelle notification remplace la notification en file d’attente ayant la même balise. S’il n’existe aucune correspondance, la règle de remplacement par défaut est appliquée et la nouvelle notification remplace la notification la plus ancienne dans la file d’attente.

Vous pouvez utiliser la mise en file d’attente des notifications et les balises pour mettre en œuvre toutes sortes de scénarios de notification. Par exemple, une application de cotations boursières peut envoyer cinq notifications, chacune d’elles s’appliquant à une action spécifique et portant une balise du nom de l’action en question. De cette manière, la file d’attente ne peut pas comporter deux notifications pour la même action, avec la plus ancienne des deux qui serait obsolète.

Pour plus d’informations, voir [Utilisation de la file d’attente de notifications](https://msdn.microsoft.com/library/windows/apps/hh781199).

### <span id="Enabling_the_notification_queue"></span><span id="enabling_the_notification_queue"></span><span id="ENABLING_THE_NOTIFICATION_QUEUE"></span>Activation de la file d’attente de notifications

Pour implémenter une file d’attente de notifications, activez d’abord la file d’attente pour votre vignette (voir [Comment utiliser la file d’attente de notifications avec des notifications locales](https://msdn.microsoft.com/library/windows/apps/hh465429)). L’appel qui permet d’activer la file d’attente doit être effectué une et une seule fois. Cependant, cela ne pose aucun problème d’effectuer l’appel chaque fois que votre application est lancée.

### <span id="Polling_for_more_than_one_notification_at_a_time"></span><span id="polling_for_more_than_one_notification_at_a_time"></span><span id="POLLING_FOR_MORE_THAN_ONE_NOTIFICATION_AT_A_TIME"></span>Interrogation de plusieurs notifications à la fois

Vous devez fournir un URI unique pour chaque notification que Windows doit télécharger pour votre vignette. À l’aide de la méthode [**StartPeriodicUpdateBatch**](https://msdn.microsoft.com/library/windows/apps/hh967945), vous pouvez fournir jusqu’à cinq URI à la fois utilisables par la file d’attente de notifications. Chaque URI fait l’objet d’une interrogation pour une seule charge utile de notification, plus ou moins au même moment. Chaque URI interrogé peut renvoyer son propre délai d’expiration et sa propre valeur de balise.

## <span id="related_topics"></span>Rubriques connexes


* [Recommandations en matière de notifications périodiques](https://msdn.microsoft.com/library/windows/apps/hh761461)
* [Comment configurer des notifications périodiques pour les badges](https://msdn.microsoft.com/library/windows/apps/hh761476)
* [Comment configurer des notifications périodiques pour des vignettes](https://msdn.microsoft.com/library/windows/apps/hh761476)
 

 







<!--HONumber=Jun16_HO4-->


