---
Description: Vous pouvez enregistrer des événements personnalisés à partir de votre application UWP et passez en revue ces événements dans le rapport d’utilisation de partenaires.
title: Consigner des événements personnalisés pour l’Espace partenaires
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, événements de journal
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: e45b14daf6951142cb0d0ed8714e981eb6a55628
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371038"
---
# <a name="log-custom-events-for-partner-center"></a>Consigner des événements personnalisés pour l’Espace partenaires

Le [rapport d’utilisation](https://docs.microsoft.com/windows/uwp/publish/usage-report) dans permet de partenaires vous obtenez des informations sur les événements personnalisés que vous avez définies dans votre application de plateforme universelle Windows (UWP). Un événement personnalisé est une chaîne arbitraire qui représente un événement ou une activité dans votre application. Par exemple, un jeu peut définir des événements personnalisés nommés *firstLevelPassed*, *secondLevelPassed*, etc., qui sont consignés lors de chaque passage au niveau supérieur de l’utilisateur.

Pour consigner un événement à partir de votre application, passez la chaîne d’événement à la méthode [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) fournie par le Microsoft Store Services SDK. Vous pouvez consulter le nombre total d’occurrences pour vos événements personnalisés dans le **événements personnalisés** section de la [rapport d’utilisation](https://docs.microsoft.com/windows/uwp/publish/usage-report) dans Partner Center.

> [!NOTE]
> Événements personnalisés que vous vous connectez à des partenaires ne sont pas liées à [les événements Windows](https://docs.microsoft.com/windows/desktop/Events/windows-events), et ils n’apparaissent pas dans **Observateur d’événements**.

## <a name="prerequisites"></a>Prérequis

Avant de pouvoir examiner les événements de journalisation personnalisée dans le **rapport d’utilisation** pour votre application dans le centre de partenaires, votre application doit être publiée dans le Store.

## <a name="how-to-log-custom-events"></a>Comment consigner des événements personnalisés

1. Si vous ne l’avez pas encore fait, [Installez le Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) sur votre ordinateur de développement.

2. Ouvrez votre projet dans Visual Studio.

3. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud **Références**, puis sélectionnez **Ajouter une référence**.

4. Dans le **Gestionnaire de références**, développez **Windows universel**, puis cliquez sur **Extensions**.

5. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK**.

6. Ajoutez l’instruction suivante en haut de chaque fichier de code dans lequel vous souhaitez consigner des événements personnalisés.
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. Dans chaque section de votre code où vous souhaitez consigner un événement personnalisé, récupérez un objet [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log), puis appelez la méthode [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log). Transmettez votre chaîne d’événements personnalisés à la méthode.
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > Le [Rapport d’utilisation](https://docs.microsoft.com/windows/uwp/publish/usage-report) peut prendre un certain temps pour se charger si votre application enregistre un grand nombre d’événements personnalisés avec des noms longs. Nous vous recommandons d’utiliser des noms brefs pour vos événements personnalisés. 

## <a name="related-topics"></a>Rubriques connexes

* [Rapport d’utilisation](https://docs.microsoft.com/windows/uwp/publish/usage-report)
* [Méthode de journalisation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](https://docs.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
