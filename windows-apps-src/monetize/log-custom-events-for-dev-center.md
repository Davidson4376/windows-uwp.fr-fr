---
author: mcleanbyron
Description: You can log custom events from your UWP app and review those events in the Usage report on the Windows Dev Center dashboard.
title: Consigner des événements personnalisés pour le Centre de développement
ms.author: mcleans
ms.date: 06/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK, événements de journal
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 2b9cd4d7c527001bb382596c9c805be4ad5e7b08
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3390245"
---
# <a name="log-custom-events-for-dev-center"></a>Consigner des événements personnalisés pour le Centre de développement

Le [Rapport sur l’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) du tableau de bord du Centre de développement vous permet de récupérer des informations sur les événements personnalisés définis dans votre applicationUWP. Un événement personnalisé est une chaîne arbitraire qui représente un événement ou une activité dans votre application. Par exemple, un jeu peut définir des événements personnalisés nommés *firstLevelPassed*, *secondLevelPassed*, etc., qui sont consignés lors de chaque passage au niveau supérieur de l’utilisateur.

Pour consigner un événement à partir de votre application, passez la chaîne d’événement à la méthode [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) fournie par le Microsoft Store Services SDK. Vous pouvez passer en revue le total d’occurrences de vos événements personnalisés dans la section **Événements personnalisés** du [Rapport sur l’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) du tableau de bord du Centre de développement.

> [!NOTE]
> Les événements personnalisés que vous enregistrés dans le centre de développement ne sont pas liés aux [événements Windows](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx), et ils n'apparaissent pas dans le **Observateur d’événements**.

## <a name="prerequisites"></a>Prérequis

Pour pouvoir passer en revue les événements de journalisation personnalisés relatifs à votre application dans le **Rapport sur l’utilisation** du tableau de bord, vous devez publier votre application dans le WindowsStore.

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
