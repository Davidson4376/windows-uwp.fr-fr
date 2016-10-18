---
author: mcleanbyron
Description: "Pour exécuter une expérience dans votre app. de plateformeWindows universelle (UWP) avec des testsA/B, vous devez code l’expérience dans votre application."
title: "Coder votre application à des fins d’expérimentation"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: 29a94fd14d11256ade28463c04abfec81287cf39
ms.openlocfilehash: e5de32dcc7b0694e72d9686b3b9a64de17a02277

---

# Coder votre application à des fins d’expérimentation

Une fois que vous avez [créé un projet et défini des variables distantes dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), vous êtes prêt à mettre à jour le code dans votre application de plateformeWindows universelle (UWP) dans le but de:
* recevoir des valeurs de variables distances à partir du Centre de développement Windows;
* utiliser des variables distantes pour configurer des expériences d’application pour vos utilisateurs;
* consigner les événements dans le Centre de développement qui indiquent à quels moments les utilisateurs ont visualisé votre expérience et effectué une action désirée (aussi appelée *conversion*).

Les sections suivantes décrivent le processus général d’obtention de variantes pour votre expérience et de consignation des événements dans le Centre de développement. Après avoir codé votre application à des fins d’expérimentation, vous pouvez [définir une expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md). Pour découvrir une procédure pas à pas illustrant le processus de création et d’exécution d’une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Configurer votre projet

Pour commencer, installez le Kit de développement logiciel (SDK) Microsoft Store Services sur votre ordinateur de développement et ajoutez les références nécessaires à votre projet.

1. Installez [Microsoft Store Services SDK](http://aka.ms/store-em-sdk).
2. Ouvrez votre projet dans VisualStudio.
3. Dans l’Explorateur de solutions, développez votre nœud de projet, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence**.
3. Dans le **Gestionnaire de références**, développez **Windows universel**, puis cliquez sur **Extensions**.
4. Dans la liste des Kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK**.

## Ajouter du code pour obtenir des données de variante

Dans le projet, recherchez le code de la fonctionnalité que vous souhaitez modifier dans votre expérience. Ajoutez du code qui récupère les données d’une variante, puis utilisez ces données pour modifier le comportement de la fonctionnalité que vous testez. Le code spécifique dont vous avez besoin dépend de votre application, mais voici les étapes générales. Pour obtenir un exemple de code complet, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Déclarez un objet [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) qui représente l’affectation de variante actuelle et un objet [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) que vous utiliserez pour consigner les événements d’affichage et de conversion dans le Centre de développement.
```CSharp
private StoreServicesExperimentVariation variation;
private StoreServicesCustomEventLogger logger;
```

2. Obtenez l’affectation de variante mise en cache actuelle pour votre expérience en appelant la méthode statique [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) et passez l’[ID de projet](run-app-experiments-with-a-b-testing.md#terms) de votre expérience à la méthode. Cette méthode retourne un objet [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx)qui donne accès à l’affectation de variante via la propriété [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx).
  >**Remarque**&nbsp;&nbsp;Vous obtenez un ID de projet au moment de [créer un projet dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). L’ID de projet présenté ci-dessous l’est uniquement à titre d’exemple.

  ```CSharp
var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(
      "F48AC670-4472-4387-AB7D-D65B095153FB");
variation = result.ExperimentVariation;
```

4. Vérifiez la propriété [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) pour déterminer si l’affectation de variante mise en cache doit être actualisée avec une affectation de variante distante à partir du serveur. Si ce n’est pas le cas, appelez la méthode statique [GetRefreshedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) pour rechercher une affectation de variante mise à jour sur le serveur et actualiser la variante mise en cache locale.
```CSharp
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
{
      result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

      // If the call succeeds, use the new result. Otherwise, use the cached value.
      if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
      {
          variation = result.ExperimentVariation;
      }
}
```

5. Utilisez les méthodes [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean.aspx), [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble.aspx), [GetInt32](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32.aspx) ou [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) de l’objet [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) pour obtenir les valeurs de l’affectation de variante. Dans chacune de ces méthodes, le premier paramètre est le nom de la variante que vous voulez récupérer (nom que vous entrez dans le tableau de bord du Centre de développement). Le deuxième paramètre est la valeur par défaut que la méthode doit retourner si elle ne parvient pas à récupérer la valeur spécifiée dans le Centre de développement (par exemple, en cas d’absence de connectivité réseau) et si aucune version mise en cache de la variante n’est disponible.

  L’exemple suivant utilise [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) pour obtenir une variable nommée *buttonText* et spécifie la valeur de variable par défaut **Grey Button**.
```CSharp
var buttonText = variation.GetString("buttonText", "Grey Button");
```
4. Dans votre code, utilisez les valeurs de variable pour modifier le comportement de la fonctionnalité que vous testez. Par exemple, le code suivant affecte la valeur *buttonText* au contenu d’un bouton.
```CSharp
button.Content = buttonText;
```

## Ajouter du code pour consigner les événements d’affichage et de conversion dans le Centre de développement

Ensuite, ajoutez du code qui consigne les [événements d’affichage](run-app-experiments-with-a-b-testing.md#terms) et les [événements de conversion](run-app-experiments-with-a-b-testing.md#terms) dans le service de testsA/B du Centre de développement. Votre code doit consigner l’événement d’affichage dès que l’utilisateur commence à visualiser une variante faisant partie de votre expérience. De même, il doit consigner l’événement de conversion dès que l’utilisateur atteint un objectif pour votre expérience.

Le code spécifique dont vous avez besoin dépend de votre application, mais voici les étapes générales. Pour obtenir un exemple de code complet, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Dans le code qui s’exécute au moment où l’utilisateur commence à visualiser une variante, initialisez le champ ```logger``` sur un objet [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) et appelez la méthode [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx). Passez l’objet [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) qui représente l’affectation de variante actuelle (cet objet fournit au Centre de développement du contexte sur l’événement), ainsi que le nom de l’événement d’affichage (nom d’un événement d’affichage que vous entrez dans le tableau de bord du Centre de développement). L’exemple suivant consigne un événement d’affichage nommé **userViewedButton**.

  ```CSharp
  if (logger == null)
  {
      logger = StoreServicesCustomEventLogger.GetDefault();
  }

  logger.LogForVariation(variation, "userViewedButton");
  ```

2. Dans le code qui s’exécute quand l’utilisateur atteint l’un des objectifs de l’expérience, appelez à nouveau la méthode [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) et passez l’objet [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) et le nom d’un événement de conversion (nom d’un événement de conversion que vous entrez dans le tableau de bord du Centre de développement). L’exemple suivant consigne un événement de conversion nommé **userClickedButton**.
```CSharp
logger.LogForVariation(variation, "userClickedButton");
```

## Étapes suivantes

Dès lors que vous avez codé l’expérience dans votre application, vous êtes prêt pour les étapes suivantes:
1. [Définissez votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md). Créez une expérience qui définisse les événements d’affichage, les événements de conversion et les variantes uniques pour votre test A/B.
2. [Exécutez et gérez votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md).


## Rubriques connexes

* [Créer un projet et définir des variables distantes dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Définir votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md)
* [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Sep16_HO1-->


