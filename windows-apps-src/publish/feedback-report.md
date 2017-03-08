---
author: jnHs
Description: "Le rapport sur les commentaires disponible dans le tableau de bord du Centre de développement Windows vous permet de voir les problèmes, les suggestions et les votes pour soumis par vos clients Windows 10 par le biais du Hub de commentaires."
title: Rapport sur les commentaires
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 91ca9a29609cd52db24ddddecf60307e808cc064
ms.lasthandoff: 02/07/2017

---

# <a name="feedback-report"></a>Rapport sur les commentaires

Le **rapport sur les commentaires** disponible dans le tableau de bord du Centre de développement Windows vous permet de voir les problèmes, les suggestions et les votes pour soumis par vos clients Windows 10 par le biais du Hub de commentaires. Vous pouvez afficher ces données dans votre tableau de bord ou exporter les données à consulter hors connexion.

Inciter vos clients à faire des commentaires sur votre application est un excellent moyen d’en savoir plus sur les problèmes et les fonctionnalités qui sont plus importantes pour eux. Lorsque vos clients savent qu’ils peuvent vous envoyer directement leurs commentaires, ils sont moins susceptibles de le faire par le biais d’un avis négatif.

> **Remarque** Vous pouvez également [répondre aux commentaires](respond-to-customer-feedback.md) directement à partir de ce rapport pour faire savoir aux clients que vous êtes à leur écoute.

Vous pouvez utiliser l’API de commentaires dans le [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) pour permettre aux clients de [lancer directement le Hub de commentaires à partir de votre application](../monetize/launch-feedback-hub-from-your-app.md). N’oubliez pas que tout client ayant téléchargé votre application sur un appareil Windows 10 prenant en charge le Hub de commentaires a la possibilité de laisser des commentaires à son sujet à l’aide de cette application. C’est la raison pour laquelle vous pouvez voir des commentaires de clients dans ce rapport, même si vous n’avez pas spécifiquement demandé de commentaires depuis votre application.

> **Conseil** Les commentaires deviennent particulièrement utiles si vous utilisez la [version d’évaluation de package](package-flights.md) dans la mesure où le rapport sur les commentaires indique le package spécifique installé par chaque client sur son appareil au moment où il a laissé son commentaire.

## <a name="viewing-feedback-details"></a>Affichage des détails de vos commentaires

Dans la section **Détails** de ce rapport, vous trouverez les commentaires individuels laissés par vos clients. À gauche du texte du commentaire s’affiche le nombre de fois que les autres clients ont voté pour ce commentaire dans le Hub de commentaires. Vous pouvez trier le commentaire de trois façons :

- **Votes pour** (par défaut) : affiche les commentaires pour lesquels les autres clients ont voté, en commençant par le commentaire ayant reçu le plus de votes.
- **Fréquents** : affiche les commentaires pour lesquels les autres clients ont voté au cours des sept derniers jours en commençant par le commentaire ayant fait l’objet de l’activité la plus récente.
- **Les plus récents** : montre tous les commentaires en commençant par le commentaire laissé le plus récemment.

La date à laquelle le commentaire a été laissé et le type de commentaire s’affiche en regard de chaque commentaire. Vous verrez également le marché du client, le package spécifique de votre application installé sur l’appareil utilisé lorsqu’il a laissé le commentaire, le type d’appareil, et **Windows Insider** si le client ayant laissé le commentaire est un membre du programme Windows Insider.


## <a name="apply-filters"></a>Appliquer les filtres

Dans la zone supérieure de la page, vous pouvez développer l’option **Appliquer les filtres** pour filtrer toutes les données de cette page.

> **Conseil** Si cette page ne contient aucun commentaire, assurez-vous que vos filtres n’ont pas exclu la totalité des commentaires concernant votre application. Par exemple, si vous filtrez les commentaires en fonction d’un **type d’appareil** non pris en charge par votre application, aucun commentaire n’apparaîtra sur cette page.

- **Date** : le filtre par défaut est **Toutes les heures**. Vous pouvez sélectionner des périodes plus courtes allant des **30 derniers jours** aux **12 derniers mois**.
- **Type d’appareil** : le paramètre par défaut est **Tous**. Vous pouvez sélectionner **Problème** ou **Suggestions** pour n’afficher que ce type de commentaire.
- **Type d’appareil** : le paramètre par défaut est **Tous les appareils**. Vous pouvez sélectionner **Téléphone**, **PC**, ou **Tablette** pour afficher uniquement les commentaires laissés à partir de ce type d’appareil.
- **Version du package** : le paramètre par défaut est **Tous les packages**. Vous pouvez sélectionner l’un de vos packages pour afficher uniquement les commentaires laissés par les clients ayant utilisé ce package spécifique lorsqu’ils ont laissé leur commentaire.
- **Marché** : le paramètre par défaut est **Tous les marchés**. Vous pouvez choisir un marché spécifique pour n’afficher que les commentaires des clients de ce marché.
- **Groupe** : le paramètre par défaut est **Tous**. Vous pouvez choisir d’afficher uniquement les commentaires soumis par les [Windows Insiders](http://insider.windows.com).

## <a name="translating-feedback"></a>Traduction des commentaires

Les commentaires qui n’ont pas été rédigés dans votre langue sont traduits par défaut. Si vous le souhaitez, vous pouvez désactiver la traduction des commentaires en décochant la case **Traduire les commentaires** située en haut à droite au-dessus de la liste des commentaires.

Notez que les commentaires sont traduits par un système de traduction automatique et que le résultat de la traduction n’est pas toujours précis. Le texte d’origine est fourni si vous souhaitez comparer la traduction ou utiliser un autre moyen de traduction.

## <a name="launching-feedback-hub-directly-from-your-app"></a>Lancement du Hub de commentaires directement depuis votre application

Comme indiqué plus haut, nous vous recommandons d’intégrer un lien direct vers le Hub de commentaires directement dans votre application afin d’inciter les clients à envoyer leurs commentaires. Pour plus d’informations, voir [Lancer le Hub de commentaires à partir de votre application](../monetize/launch-feedback-hub-from-your-app.md)

