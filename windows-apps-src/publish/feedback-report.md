---
Description: Le rapport de commentaires dans Partner Center vous permet de voir les problèmes, et les suggestions upvotes vos clients Windows 10 ont soumises via le Hub de commentaires.
title: Rapport sur les commentaires
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2ab7385d4c61c52b71c74fb61797be306bcc9851
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591824"
---
# <a name="feedback-report"></a>Rapport sur les commentaires

Le **rapport de commentaires** dans Partner Center vous permet de voir les problèmes, et les suggestions upvotes vos clients Windows 10 ont soumises via le Hub de commentaires. Vous pouvez afficher ces données dans l’espace partenaires, ou exporter les données à afficher en mode hors connexion.

> [!NOTE]
> Vous pouvez également [répondre aux commentaires](respond-to-customer-feedback.md) directement à partir de ce rapport pour faire savoir aux clients que vous êtes à leur écoute.

Inciter vos clients à faire des commentaires sur votre application est un excellent moyen d’en savoir plus sur les problèmes et les fonctionnalités qui sont plus importantes pour eux. Lorsque vos clients savent qu’ils peuvent vous envoyer directement leurs commentaires, ils sont moins susceptibles de le faire par le biais d’un avis négatif dans le Windows Store.

Vous pouvez utiliser l’API de commentaires dans le [Microsoft Store Services SDK](https://aka.ms/store-em-sdk) pour permettre aux clients de [lancer directement le Hub de commentaires à partir de votre application](../monetize/launch-feedback-hub-from-your-app.md). N’oubliez pas que tout client ayant téléchargé votre application sur un appareil Windows 10 prenant en charge le Hub de commentaires a la possibilité de laisser des commentaires à son sujet à l’aide de cette application. Pour cette raison, vous pouvez voir les commentaires des clients dans ce rapport même si vous n’avez pas spécifiquement demandé de votre part au sein de votre application.

Commentaires peuvent également être utile lorsque vous utilisez [version d’évaluation du package](package-flights.md), dans la mesure où le **commentaires** rapport vous présente le package spécifique que chaque client avait installé sur leur appareil quand ils laissé le commentaire.

> [!TIP]
> Pour examiner rapidement les révisions, les évaluations et les commentaires des utilisateurs dans l’ensemble de vos applications au cours des 30 derniers jours, développez **engager** dans le menu de navigation gauche, puis sélectionnez **révisions et commentaires.** 


## <a name="apply-filters"></a>Appliquer les filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La sélection par défaut est **Durée de vie**, mais vous pouvez choisir d’afficher les données portant sur 30 jours ou sur 3, 6 ou 12 mois.

Vous pouvez également développer **Filtres** pour filtrer toutes les données de cette page en fonction des options suivantes.

- **Type de commentaires**: Le paramètre par défaut est **tous les**. Vous pouvez sélectionner **Problème** ou **Suggestions** pour n’afficher que ce type de commentaire.
- **Type d’appareil**: Le paramètre par défaut est **tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement les commentaires laissés par les clients qui utilisent ce type d’appareil.
- **Version du package**: Le paramètre par défaut est **tous les packages**. Vous pouvez sélectionner l’un de vos packages pour afficher uniquement les commentaires laissés par les clients ayant utilisé ce package spécifique lorsqu’ils ont laissé leur commentaire.
- **Marché**: Le paramètre par défaut est **tous les marchés**. Vous pouvez choisir un marché spécifique pour n’afficher que les commentaires des clients de ce marché.
- **Groupe** : Le paramètre par défaut est **tous les**. Vous pouvez choisir d’afficher uniquement les commentaires soumis par les [Windows Insiders](https://insider.windows.com).

> [!TIP]
> Si cette page ne contient aucun commentaire, assurez-vous que vos filtres n’ont pas exclu la totalité des commentaires concernant votre application. Par exemple, si vous filtrez les commentaires en fonction d’un **type d’appareil** non pris en charge par votre application, aucun commentaire n’apparaîtra sur cette page.


## <a name="viewing-feedback-details"></a>Affichage des détails de vos commentaires

Ce rapport vous présente les commentaires individuels laissés par vos clients. À gauche du texte du commentaire de chaque élément s’affiche le nombre de fois où d’autres clients ont voté pour ce commentaire dans l’application Hub de commentaires. Vous pouvez trier le commentaire de trois façons :

- **Upvoted** (valeur par défaut) : Affiche des commentaires qui a été upvoted par d’autres clients, en commençant par les commentaires qui a reçu l’upvotes la plupart des.
- **Analyse des tendances**: Affiche des commentaires qui a été upvoted par d’autres clients au cours des sept derniers jours, en commençant par le commentaire, qui a été mise à l’activité la plus récente.
- **Dernière**: Affiche tous les commentaires, en commençant par les commentaires plus récemment gauche.

La date à laquelle le commentaire a été laissé et le type de commentaire s’affiche en regard de chaque commentaire. Vous verrez également sur le marché du client, le package spécifique qui a été installé sur l’appareil qu’ils utilisaient lorsqu’elles laissées les commentaires, le type de cet appareil, et **Windows Insider** si le client d’envoyer les commentaires est un membre de le programme Windows Insider.

En outre, une option vous permet de [répondre aux commentaires](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Traduction des commentaires

Par défaut, les commentaires qui a été écrit pas dans votre langage préféré sont traduite pour vous. Si vous le souhaitez, vous pouvez désactiver la traduction des commentaires en décochant la case **Traduire les commentaires** située dans la zone supérieure droite près des filtres de page.

Notez que les commentaires sont traduits par un système de traduction automatique et que le résultat de la traduction n’est pas toujours précis. Le texte d’origine est fourni si vous souhaitez comparer la traduction ou utiliser un autre moyen de traduction.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Lancement du Hub de commentaires directement depuis votre application

Comme indiqué plus haut, nous vous recommandons d’intégrer un lien direct vers le Hub de commentaires directement dans votre application afin d’inciter les clients à envoyer leurs commentaires. Pour plus d’informations, voir [Lancer le Hub de commentaires à partir de votre application](../monetize/launch-feedback-hub-from-your-app.md)
