---
author: mcleanbyron
Description: "Vous pouvez consigner des événements associés à votre applicationUWP et les passer en revue dans le Rapport sur l’utilisation, dans le tableau de bord du Centre de développement Windows."
title: "Consigner des événements personnalisés pour le Centre de développement"
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: 61874c700ecd31c7246effef5b05ffbf1153dfd5

---

# Consigner des événements personnalisés pour le Centre de développement

Le [Rapport sur l’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) du tableau de bord du Centre de développement vous permet de récupérer des informations sur les événements personnalisés définis dans votre applicationUWP. Un événement personnalisé est une chaîne arbitraire qui représente un événement ou une activité dans votre application. Par exemple, un jeu peut définir des événements personnalisés nommés *firstLevelPassed*, *secondLevelPassed*, etc., qui sont consignés lors de chaque passage au niveau supérieur de l’utilisateur.

Pour consigner un événement à partir de votre application, passez la chaîne d’événement à la méthode [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) fournie par le Microsoft Store Services SDK. Vous pouvez passer en revue le total d’occurrences de vos événements personnalisés dans la section **Événements personnalisés** du [Rapport sur l’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) du tableau de bord du Centre de développement.

## Prérequis

Pour pouvoir passer en revue les événements de journalisation personnalisés relatifs à votre application dans le **Rapport sur l’utilisation** du tableau de bord, vous devez publier votre application dans le WindowsStore.

## Comment consigner des événements personnalisés

1. Si vous ne l’avez pas encore fait, [Installez le Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) sur votre ordinateur de développement. Outre l’API relative à la consignation des événements personnalisés, ce SDK fournit des API pour d’autres fonctionnalités, telles que l’exécution d’expériences dans vos applications avec des tests A/B et l’affichage d’annonces publicitaires. 
2. Ouvrez votre projet dans VisualStudio.
3. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud **Références**, puis sélectionnez **Ajouter une référence**.
4. Dans le **Gestionnaire de références**, développez **Windows universel**, puis cliquez sur **Extensions**.
5. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK**.
7. Ajoutez l’instruction suivante dans la partie supérieure de chaque fichier de code dans lequel vous souhaitez consigner les événements personnalisés.

  ```csharp
  using Microsoft.Services.Store.Engagement;
  ```
8. Dans chacune des sections de votre code dans lesquelles vous souhaitez consigner un événement personnalisé, récupérez un objet [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx), puis appelez la méthode [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx). Passez la chaîne d’événements personnalisés à la méthode.

  ```csharp
  StoreServicesCustomEventLogger logger = StoreServicesCustomEventLogger.GetDefault();
  logger.Log("myCustomEvent");
  ```

## Rubriques connexes

* [Rapport sur l’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Méthode log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)



<!--HONumber=Nov16_HO1-->


