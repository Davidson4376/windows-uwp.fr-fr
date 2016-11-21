---
author: mcleanbyron
Description: "Pour exécuter une expérience dans votre app. de plateformeWindows universelle (UWP) avec des testsA/B, vous devez code l’expérience dans votre application."
title: "Coder votre application à des fins d’expérimentation"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: ae0ddedf09913d42a036f48d2f60d7a62769b4bb

---

# Coder votre application à des fins d’expérimentation

Une fois que vous avez [créé un projet et défini des variables distantes dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), vous êtes prêt à mettre à jour le code dans votre application de plateformeWindows universelle (UWP) dans le but de:
* recevoir des valeurs de variables distances à partir du Centre de développement Windows;
* utiliser des variables distantes pour configurer des expériences d’application pour vos utilisateurs;
* consigner les événements dans le Centre de développement qui indiquent à quels moments les utilisateurs ont visualisé votre expérience et effectué une action désirée (aussi appelée *conversion*).

Pour ajouter ce comportement à votre application, vous allez utiliser les API fournies par le Microsoft Store Services SDK.

Les sections suivantes décrivent le processus général d’obtention de variantes pour votre expérience et de consignation des événements dans le Centre de développement. Après avoir codé votre application à des fins d’expérimentation, vous pouvez [définir une expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md). Pour découvrir une procédure pas à pas illustrant le processus de création et d’exécution d’une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

>**Remarque**&nbsp;&nbsp;Certaines des API d’expérimentation dans le Kit de développement logiciel (SDK) Windows Store Services utilisent le [modèle asynchrone](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) pour récupérer les données à partir du Centre de développement. Cela signifie qu’une partie de l’exécution de ces méthodes peut avoir lieu après l’appel des méthodes, afin que l’interface utilisateur de votre application puisse rester réactive pendant que les opérations se terminent. Le modèle asynchrone exige que votre application utilise le mot-clé **async** et l’opérateur **await** pour appeler les API, comme illustré par les exemples de code dans cet article. Par convention, les méthodes asynchrones se terminent par **Async**.

## Configurer votre projet

Pour commencer, installez le Kit de développement logiciel Microsoft Store Services SDK sur votre ordinateur de développement et ajoutez les références nécessaires à votre projet.

1. Installez le [Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Ouvrez votre projet dans VisualStudio.
3. Dans l’Explorateur de solutions, développez votre nœud de projet, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence**.
3. Dans le **Gestionnaire de références**, développez **Windows universel**, puis cliquez sur **Extensions**.
4. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK**.

>**Remarque**&nbsp;&nbsp;Les exemples de code dans cet article supposent que votre fichier de code disposent d’instructions **using** pour les espaces de noms **System.Threading.Tasks** et **Microsoft.Services.Store.Engagement**.

## Obtenir des données de variante et consigner l’événement d’affichage pour votre expérience

Dans le projet, recherchez le code de la fonctionnalité que vous souhaitez modifier dans votre expérience. Ajoutez du code qui récupère les données pour une variante, utilisez ces données pour modifier le comportement de la fonctionnalité que vous testez, puis consignez l’événement d’affichage pour votre expérience dans le service de test A/B dans le Centre de développement.

Le code spécifique requis dépendra de votre application, mais l’exemple suivant illustre le processus de base. Pour obtenir un exemple de code complet, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

```CSharp
private StoreServicesExperimentVariation variation;
private StoreServicesCustomEventLogger logger;

// Assign this variable to the project ID for your experiment from Dev Center.
// The project ID shown below is for example purposes only.
private string projectId = "F48AC670-4472-4387-AB7D-D65B095153FB";

private async Task InitializeExperiment()
{
    // Get the current cached variation assignment for the experiment.
    var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(projectId);
    variation = result.ExperimentVariation;

    // Refresh the cached variation assignment if necessary.
    if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
    {
        result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

        if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
        {
            variation = result.ExperimentVariation;
        }
    }

    // Get the remote variable named "buttonText" and assign the value
    // to the button.
    var buttonText = variation.GetString("buttonText", "Grey Button");
    await button.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            button.Content = buttonText;
        });

    // Log the view event named "userViewedButton" to Dev Center.
    if (logger == null)
    {
        logger = StoreServicesCustomEventLogger.GetDefault();
    }

    logger.LogForVariation(variation, "userViewedButton");
}
```

Les étapes suivantes décrivent les éléments importants de ce processus en détail.

1. Déclarez un objet [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) qui représente l’affectation de variante actuelle et un objet [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) que vous utiliserez pour consigner les événements d’affichage et de conversion dans le Centre de développement.
  ```CSharp
  private StoreServicesExperimentVariation variation;
  private StoreServicesCustomEventLogger logger;
  ```

1. Déclarez une variable de chaîne qui est affectée à [l’ID de projet](run-app-experiments-with-a-b-testing.md#terms) pour l’expérience que vous souhaitez récupérer.
  >**Remarque**&nbsp;&nbsp;Vous obtenez un ID de projet au moment de [créer un projet dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). L’ID de projet présenté ci-dessous l’est uniquement à titre d’exemple.

  ```CSharp
  private string projectId = "F48AC670-4472-4387-AB7D-D65B095153FB";
  ```

2. Obtenez l’affectation de variante mise en cache actuelle pour votre expérience en appelant la méthode statique [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) et passez l’ID de projet de votre expérience à la méthode. Cette méthode retourne un objet [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx) qui donne accès à l’affectation de variante via la propriété [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx).

   ```CSharp
   var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(projectId);
   variation = result.ExperimentVariation;
   ```

4. Vérifiez la propriété [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) pour déterminer si l’affectation de variante mise en cache doit être actualisée avec une affectation de variante distante à partir du serveur. Si ce n’est pas le cas, appelez la méthode statique [GetRefreshedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) pour rechercher une affectation de variante mise à jour sur le serveur et actualiser la variante mise en cache locale.

  ```CSharp
  if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
  {
        result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

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

4. Dans votre code, utilisez les valeurs de variable pour modifier le comportement de la fonctionnalité que vous testez. Par exemple, le code suivant affecte la valeur *buttonText* au contenu d’un bouton dans votre application. Cet exemple suppose que vous avez déjà défini ce bouton ailleurs dans votre projet.

  ```CSharp
  await button.Dispatcher.RunAsync(
      CoreDispatcherPriority.Normal,
      () =>
      {
          button.Content = buttonText;
      });
  ```

5. Pour finir, consignez [l’événement d’affichage](run-app-experiments-with-a-b-testing.md#terms) pour votre expérience dans le service de test A/B dans le Centre de développement. Initialisez le champ ```logger``` sur un objet [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) et appelez la méthode [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx). Transmettez [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) qui représente l’affectation de la variante actuelle (cet objet fournit un contexte pour l’événement au Centre de développement) et le nom de l’événement d’affichage que vous avez défini dans votre expérience. Cela doit correspondre au nom de l’événement d’affichage que vous entrez pour votre expérience dans le tableau de bord du Centre de développement. Votre code doit consigner l’événement d’affichage lorsque l’utilisateur commence à visualiser une variante faisant partie intégrante de votre expérience.

  L’exemple suivant montre comment consigner un événement d’affichage nommé **userViewedButton**. Dans cet exemple, l’objectif de l’expérience est d’obtenir de l’utilisateur qu’il clique sur un bouton dans l’application, afin que l’événement d’affichage soit consigné une fois que l’application a récupéré les données de variante (dans ce cas, le texte du bouton) et leur a attribué le contenu du bouton.

  ```CSharp
  if (logger == null)
  {
      logger = StoreServicesCustomEventLogger.GetDefault();
  }

  logger.LogForVariation(variation, "userViewedButton");
  ```

## Consigner les événements de conversion dans le Centre de développement

Ensuite, ajoutez du code qui consigne les [événements de conversion](run-app-experiments-with-a-b-testing.md#terms) dans le service des testsA/B du Centre de développement. Votre code doit consigner un événement de conversion quand l’utilisateur atteint un objectif pour votre expérience. Le code spécifique dont vous avez besoin dépend de votre application, mais voici les étapes générales. Pour obtenir un exemple de code complet, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Dans le code qui s’exécute quand l’utilisateur atteint un objectif pour l’un des objectifs de l’expérience, appelez de nouveau la méthode [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) et transmettez l’objet [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) et le nom d’un événement de conversion pour votre expérience. Cela doit correspondre aux noms des événements de conversion que vous entrez pour votre expérience dans le tableau de bord du Centre de développement.

  L’exemple suivant consigne un événement de conversion nommé **userClickedButton** à partir du gestionnaire d’événements **Click** pour un bouton. Dans cet exemple, l’objectif de l’expérience est d’obtenir de l’utilisateur qu’il clique sur le bouton.

  ```CSharp
  private void button_Click(object sender, RoutedEventArgs e)
  {
      if (logger == null)
      {
          logger = StoreServicesCustomEventLogger.GetDefault();
      }

      logger.LogForVariation(variation, "userClickedButton");
  }
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



<!--HONumber=Nov16_HO1-->


