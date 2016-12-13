---
author: mcleanbyron
Description: "Vous pouvez consigner des événements associés à votre application&nbsp;UWP et les passer en revue dans le Rapport sur l’utilisation, dans le tableau de bord du Centre de développement Windows."
title: "Consigner des événements personnalisés pour le Centre de développement"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: e0012d586d9b79db77bdeded6f0e1d2ce848bbea

---

# <a name="log-custom-events-for-dev-center"></a>Consigner des événements personnalisés pour le Centre de développement

Le [Rapport sur l’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) du tableau de bord du Centre de développement vous permet de récupérer des informations sur les événements personnalisés définis dans votre application&nbsp;UWP. Un événement personnalisé est une chaîne arbitraire qui représente un événement ou une activité dans votre application. Par exemple, un jeu peut définir des événements personnalisés nommés *firstLevelPassed*, *secondLevelPassed*, etc., qui sont consignés lors de chaque passage au niveau supérieur de l’utilisateur.

Pour consigner un événement à partir de votre application, passez la chaîne d’événement à la méthode [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) fournie par le Microsoft Store Services SDK. Vous pouvez passer en revue le total d’occurrences de vos événements personnalisés dans la section **Événements personnalisés** du [Rapport sur l’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) du tableau de bord du Centre de développement.

## <a name="prerequisites"></a>Prérequis

Pour pouvoir passer en revue les événements de journalisation personnalisés relatifs à votre application dans le **Rapport sur l’utilisation** du tableau de bord, vous devez publier votre application dans le Windows&nbsp;Store.

## <a name="how-to-log-custom-events"></a>Comment consigner des événements personnalisés

1. Si vous ne l’avez pas encore fait, [Installez le Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) sur votre ordinateur de développement. Outre l’API relative à la consignation des événements personnalisés, ce SDK fournit des API pour d’autres fonctionnalités, telles que l’exécution d’expériences dans vos applications avec des tests A/B et l’affichage d’annonces publicitaires.
2. Ouvrez votre projet dans Visual&nbsp;Studio.
3. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud **Références**, puis sélectionnez **Ajouter une référence**.
4. Dans le **Gestionnaire de références**, développez **Windows universel**, puis cliquez sur **Extensions**.
5. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK**.
7. Ajoutez l’instruction suivante en haut de chaque fichier de code dans lequel vous souhaitez consigner des événements personnalisés.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

8. Dans chaque section de votre code où vous souhaitez consigner un événement personnalisé, récupérez un objet [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx), puis appelez la méthode [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx). Transmettez votre chaîne d’événements personnalisés à la méthode.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur l’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Méthode log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)



<!--HONumber=Dec16_HO1-->


