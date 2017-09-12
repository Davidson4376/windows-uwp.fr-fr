---
author: jnHs
Description: "Le rapport sur les commentaires du tableau de bord du Centre de développement Windows vous permet de voir les problèmes, les suggestions et les votes pour soumis par vos clients Windows10 par le biais du Hub de commentaires."
title: Rapport sur les commentaires
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: e78ab0ac31bd41c3fb7b05039b14f422e4c9071b
ms.sourcegitcommit: 5ece992c31870df4c089360ef47501bd4ce14fa9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2017
---
# <a name="feedback-report"></a>Rapport sur les commentaires

Le **rapport sur les commentaires** du tableau de bord du Centre de développement Windows vous permet de voir les problèmes, les suggestions et les votes pour soumis par vos clients Windows10 par le biais du Hub de commentaires. Vous pouvez afficher ces données dans votre tableau de bord ou exporter les données à consulter hors connexion.

> [!NOTE]
> Vous pouvez également [répondre aux commentaires](respond-to-customer-feedback.md) directement à partir de ce rapport pour faire savoir aux clients que vous êtes à leur écoute.

Inciter vos clients à faire des commentaires sur votre application est un excellent moyen d’en savoir plus sur les problèmes et les fonctionnalités qui sont plus importantes pour eux. Lorsque vos clients savent qu’ils peuvent vous envoyer directement leurs commentaires, ils sont moins susceptibles de le faire par le biais d’un avis négatif dans le WindowsStore.

Vous pouvez utiliser l’API de commentaires dans le [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) pour permettre aux clients de [lancer directement le Hub de commentaires à partir de votre application](../monetize/launch-feedback-hub-from-your-app.md). N’oubliez pas que tout client ayant téléchargé votre application sur un appareil Windows10 prenant en charge le Hub de commentaires a la possibilité de laisser des commentaires à son sujet à l’aide de cette application. C’est la raison pour laquelle vous pouvez voir des commentaires de clients dans ce rapport, même si vous n’avez pas spécifiquement demandé de commentaires depuis votre application.

Les commentaires peuvent également se révéler utiles si vous utilisez la [distribution de version d’évaluation de package](package-flights.md), dans la mesure où le rapport sur les commentaires vous indique le package spécifique que chaque client a installé sur son appareil au moment où il a laissé son commentaire.

> [!TIP]
> Pour obtenir un aperçu des avis, des évaluations et des commentaires des utilisateurs formulés pour l’ensemble de vos applications au cours des 30derniers jours, développez **Engager** dans le menu de navigation de gauche, puis sélectionnez **Reviews and feedback**. 


## <a name="apply-filters"></a>Appliquer des filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La sélection par défaut est **Durée de vie**, mais vous pouvez choisir d’afficher les données portant sur 30jours ou sur 3, 6 ou 12mois.

Vous pouvez également développer **Filtres** pour filtrer toutes les données de cette page en fonction des options suivantes.

- **Type de commentaire**: le paramétrage par défaut est **Tous**. Vous pouvez sélectionner **Problème** ou **Suggestions** pour n’afficher que ce type de commentaire.
- **Type d’appareil**: le paramétrage par défaut est **Tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement les commentaires laissés par les clients qui utilisent ce type d’appareil.
- **Version du package**: le paramétrage par défaut est **Tous les packages**. Vous pouvez sélectionner l’un de vos packages pour afficher uniquement les commentaires laissés par les clients ayant utilisé ce package spécifique lorsqu’ils ont laissé leur commentaire.
- **Marché** : le paramètre par défaut est **Tous les marchés**. Vous pouvez choisir un marché spécifique pour n’afficher que les commentaires des clients de ce marché.
- **Groupe** : le paramètre par défaut est **Tous**. Vous pouvez choisir d’afficher uniquement les commentaires soumis par les [Windows Insiders](http://insider.windows.com).

> [!TIP]
> Si cette page ne contient aucun commentaire, assurez-vous que vos filtres n’ont pas exclu la totalité des commentaires concernant votre application. Par exemple, si vous filtrez les commentaires en fonction d’un **type d’appareil** non pris en charge par votre application, aucun commentaire n’apparaîtra sur cette page.


## <a name="viewing-feedback-details"></a>Affichage des détails de vos commentaires

Ce rapport vous présente les commentaires individuels laissés par vos clients. À gauche du texte du commentaire de chaque élément s’affiche le nombre de fois où d’autres clients ont voté pour ce commentaire dans l’application Hub de commentaires. Vous pouvez trier le commentaire de trois façons:

- **Votes pour** (par défaut) : affiche les commentaires pour lesquels les autres clients ont voté, en commençant par le commentaire ayant reçu le plus de votes.
- **Fréquents** : affiche les commentaires pour lesquels les autres clients ont voté au cours des sept derniers jours en commençant par le commentaire ayant fait l’objet de l’activité la plus récente.
- **Les plus récents** : montre tous les commentaires en commençant par le commentaire laissé le plus récemment.

La date à laquelle le commentaire a été laissé et le type de commentaire s’affiche en regard de chaque commentaire. Vous verrez également le marché du client, le package spécifique de votre application installé sur l’appareil utilisé lorsqu’il a laissé le commentaire, le type d’appareil, et **Windows Insider** si le client ayant laissé le commentaire est un membre du programme Windows Insider.

En outre, une option vous permet de [répondre aux commentaires](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Traduction des commentaires

Les commentaires qui n’ont pas été rédigés dans votre langue préférée sont traduits par défaut. Si vous le souhaitez, vous pouvez désactiver la traduction des commentaires en décochant la case **Traduire les commentaires** située dans la zone supérieure droite près des filtres de page.

Notez que les commentaires sont traduits par un système de traduction automatique et que le résultat de la traduction n’est pas toujours précis. Le texte d’origine est fourni si vous souhaitez comparer la traduction ou utiliser un autre moyen de traduction.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Lancement du Hub de commentaires directement depuis votre application

Comme indiqué plus haut, nous vous recommandons d’intégrer un lien direct vers le Hub de commentaires directement dans votre application afin d’inciter les clients à envoyer leurs commentaires. Pour plus d’informations, voir [Lancer le Hub de commentaires à partir de votre application](../monetize/launch-feedback-hub-from-your-app.md)
