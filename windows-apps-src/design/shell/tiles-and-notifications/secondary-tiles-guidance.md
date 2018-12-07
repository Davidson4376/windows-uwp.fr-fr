---
Description: Learn about when and where you should use secondary tiles in your UWP app.
title: Vignettes secondaires
label: Secondary tiles
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows10, uwp, vignettes secondaires, instructions, recommandations, meilleures pratiques
ms.localizationpriority: medium
ms.openlocfilehash: de3bfa94de1152b3945d42169143a5ae36328c75
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8795907"
---
# <a name="secondary-tile-guidance"></a>Instructions relatives aux vignettes secondaires


Une vignette secondaire offre aux utilisateurs un moyen cohérent et efficace d’accéder directement à des zones spécifiques au sein d’une application à partir du menu Démarrer. Que l’utilisateur choisisse ou non d’épingler une vignette secondaire dans le menu Démarrer, les zones de regroupement d’une application sont déterminées par le développeur. Pour obtenir un résumé plus détaillé, voir [Vue d’ensemble des vignettes secondaires](secondary-tiles.md). Tenez compte de ces recommandations lorsque vous activez des vignettes secondaires et concevez l’interface utilisateur associée dans votre application.

> [!NOTE]
> Seuls les utilisateurs peuvent épingler une vignette secondaire au menu Démarrer; les applications ne peuvent pas épingler de vignettes secondaires par programme. Les utilisateurs contrôlent également la suppression des vignettes et peuvent supprimer une vignette secondaire à partir du menu Démarrer ou de l’application parente.


## <a name="recommendations"></a>Recommandations

Tenez compte des recommandations suivantes lors de l’activation des vignettes secondaires dans votre application:

* Lorsque le contenu en question peut être épinglé, la barre de l’application doit contenir un bouton «Épingler au menu Démarrer» pour créer une vignette secondaire pour l’utilisateur.
* Lorsque l’utilisateur clique sur «Épingler au menu Démarrer», vous devez immédiatement appeler l’API à partir du thread d’interface utilisateur pour [épingler la vignette secondaire](secondary-tiles-pinning.md).
* Si le contenu en question est déjà épinglé, remplacez le bouton «Épingler au menu Démarrer» de la barre de l’application par un bouton «Détacher du menu Démarrer». Ce bouton doit supprimer la vignette secondaire existante.
* Lorsque le contenu en question ne peut pas être épinglé, n’affichez pas de bouton «Épingler au menu Démarrer» (ou affichez un bouton «Épingler au menu Démarrer» désactivé).
* Utilisez les glyphes fournis par le système pour les boutons «Épingler au menu Démarrer» et «Détacher du menu Démarrer» (voir les membres d’épingler ou de désépingler dans Windows.UI.Xaml.Controls.Symbol ou WinJS.UI.AppBarIcon).
* Utilisez le texte du bouton standard: «Épingler au menu Démarrer» et «Détacher du menu Démarrer». Vous devrez remplacer le texte par défaut lorsque vous utilisez les glyphes d’épinglage et de désépinglage fournis par le système.
* N’utilisez pas une vignette secondaire comme un bouton de commande virtuel pour interagir avec l’application parente, comme une vignette permettant de passer à la piste suivante.


## <a name="additional-usage-guidance-for-devs"></a>Instructions d’utilisation supplémentaires pour les développeurs

* Lorsqu’une application est lancée, elle doit toujours énumérer ses vignettes secondaires, en cas d’ajout ou de suppression dont il n’a pas connaissance. Lorsqu’une vignette secondaire est supprimée par le biais de la barre d’application de l’écran Démarrer, Windows supprime simplement la vignette. L’application proprement dite est chargée de libérer les ressources qui ont été utilisées par la vignette secondaire. Lorsque des vignettes secondaires sont copiées via le cloud, les notifications par badge ou vignette actuelles de la vignette secondaire, les notifications planifiées, les canaux de notifications Push et les URI utilisés avec les notifications périodiques ne sont pas copiés avec la vignette secondaire et doivent être de nouveau configurés.
* Une application doit utiliser des ID explicites, uniques et pouvant être recréés pour les vignettes secondaires. L’utilisation d’ID de vignette secondaire prévisibles qui ont un sens pour une application permet à l’application de comprendre quoi faire avec ces vignettes lorsqu’elles apparaissent au cours d’une nouvelle installation sur un nouvel ordinateur.
  * Lors de l’exécution, l’application peut demander si une vignette spécifique existe.
  * La plateforme de vignette secondaire peut demander à renvoyer l’ensemble des vignettes secondaires appartenant à une application spécifique. L’utilisation d’ID explicites et uniques pour ces vignettes peut aider l’application à examiner l’ensemble des vignettes secondaires et à effectuer des actions appropriées. Par exemple, pour une application de médias sociaux, les ID peuvent identifier des contacts pour lesquels les vignettes ont été créées.
* Les vignettes secondaires, comme toutes les vignettes figurant sur l’écran de démarrage, sont des éléments dynamiques qui peuvent être mis à jour fréquemment avec du nouveau contenu. Les vignettes secondaires peuvent faire apparaître les notifications et mises à jour en utilisant les mêmes mécanismes que n’importe quelle autre vignette. Voir [Choisir une méthode de remise de notification](choosing-a-notification-delivery-method.md) pour en savoir plus.


## <a name="related"></a>Similaire

* [Vue d’ensemble des vignettes secondaires](secondary-tiles.md)
* [Épingler les vignettes secondaires](secondary-tiles-pinning.md)
* [Ressources de vignette](app-assets.md)
* [Documentation sur le contenu des vignettes](create-adaptive-tiles.md)
* [Envoyer une notification par vignette locale](sending-a-local-tile-notification.md)
