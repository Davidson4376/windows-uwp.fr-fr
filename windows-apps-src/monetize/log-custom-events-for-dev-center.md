---
Description: You can log custom events from your UWP app and review those events in the Usage report in Partner Center.
title: Consigner des événements personnalisés pour l’Espace partenaires
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, événements de journal
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: d7b338fd3b34d530ad365b0377d6b6c6c65398b7
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8714109"
---
# <a name="log-custom-events-for-partner-center"></a>Consigner des événements personnalisés pour l’Espace partenaires

Le [rapport d’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) dans l’espace partenaires vous permet d’obtenir des informations sur les événements personnalisés que vous avez définis dans votre application de plateforme Windows universelle (UWP). Un événement personnalisé est une chaîne arbitraire qui représente un événement ou une activité dans votre application. Par exemple, un jeu peut définir des événements personnalisés nommés *firstLevelPassed*, *secondLevelPassed*, etc., qui sont consignés lors de chaque passage au niveau supérieur de l’utilisateur.

Pour consigner un événement à partir de votre application, passez la chaîne d’événement à la méthode [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) fournie par le Microsoft Store Services SDK. Vous pouvez passer en revue le total d’occurrences de vos événements personnalisés dans la section **événements personnalisés** du [rapport d’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) dans l’espace partenaires.

> [!NOTE]
> Les événements personnalisés que vous vous connectez à l’espace partenaires ne sont pas liés aux [événements Windows](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx), et elles n’apparaissent pas dans **L’Observateur d’événements**.

## <a name="prerequisites"></a>Prérequis

Avant que vous pouvez passer en revue les événements de journalisation personnalisés dans le **rapport d’utilisation** pour votre application dans l’espace partenaires, votre application doit être publiée dans le Windows Store.

## <a name="how-to-log-custom-events"></a>Comment consigner des événements personnalisés

1. Si vous ne l’avez pas encore fait, [installez le Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) sur votre ordinateur de développement.

2. Ouvrez votre projet dans Visual Studio.

3. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud **Références**, puis sélectionnez **Ajouter une référence**.

4. Dans le **Gestionnaire de références**, développez **Windows universel**, puis cliquez sur **Extensions**.

5. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK**.

6. Ajoutez l’instruction suivante en haut de chaque fichier de code dans lequel vous souhaitez consigner des événements personnalisés.
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. Dans chaque section de votre code où vous souhaitez consigner un événement personnalisé, récupérez un objet [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log), puis appelez la méthode [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log). Transmettez votre chaîne d’événements personnalisés à la méthode.
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > Le [Rapport d’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) peut prendre un certain temps pour se charger si votre application enregistre un grand nombre d’événements personnalisés avec des noms longs. Nous vous recommandons d’utiliser des noms brefs pour vos événements personnalisés. 

## <a name="related-topics"></a>Rubriquesconnexes

* [Rapport sur l’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Méthode log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
