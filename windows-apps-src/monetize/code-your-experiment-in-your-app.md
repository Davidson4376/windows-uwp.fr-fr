---
author: mcleanbyron
Description: Après avoir défini votre expérience dans le tableau de bord du Centre de développement, vous êtes prêt pour coder l’expérience dans votre application.
title: Coder votre application à des fins d’expérimentation
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
---

# Coder votre application à des fins d’expérimentation

Après avoir [défini votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md), vous êtes prêt à mettre à jour le code dans votre application de plateforme Windows universelle (UWP) pour obtenir les données de variante pour l’expérimentation, à utiliser ces données pour modifier le comportement de la fonctionnalité que vous testez et à consigner les événements d’affichage et de conversion dans le Centre de développement.

Les sections suivantes décrivent le processus général d’obtention de variantes pour votre expérience et de journalisation des événements dans le Centre de développement. Pour une procédure pas à pas illustrant le processus de création et d’exécution d’une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Configurer votre projet

Pour commencer, installez le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft sur votre ordinateur de développement et ajoutez les références nécessaires à votre projet.

1. Installez le [Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](http://aka.ms/store-em-sdk).
2. Ouvrez votre projet dans Visual Studio.
3. Dans l’Explorateur de solutions, développez votre nœud de projet, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence**.
3. Dans le **Gestionnaire de références**, développez **Windows universel** et cliquez sur **Extensions**.
4. Dans la liste des Kits de développement logiciel (SDK), cochez la case en regard de **Kit de développement logiciel (SDK) d’engagement de la Boutique Microsoft** et cliquez sur **OK**.

## Ajouter du code pour obtenir des paramètres de variante

Dans le projet, recherchez le code de la fonctionnalité que vous souhaitez modifier dans votre expérience. Ajoutez du code qui récupère des paramètres pour une variante, puis utilisez ces données pour modifier le comportement de la fonctionnalité que vous testez. Le code spécifique dont vous avez besoin dépend de votre application, mais voici les étapes générales. Pour un exemple de code complet, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Déclarez un objet [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) que vous utiliserez pour récupérer les variantes de votre expérience ainsi qu’un objet [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) qui représente l’affectation actuelle de la variante.
```CSharp
private readonly ExperimentClient experiment;
private ExperimentVariation variation;
```

2. Initialisez l’objet [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx), puis transmettez au constructeur la clé API que vous avez récupérée à partir de la page **Expériences** du tableau de bord. Pour en savoir plus sur la clé API, voir [Définir votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md#generate-an-api-key). La clé API illustrée ci-dessous est uniquement fournie à titre d’exemple.
```CSharp
experiment = new ExperimentClient("F48AC670-4472-4387-AB7D-D65B095153FB");
```

3. Obtenez l’affectation actuelle de la variante mise en cache pour votre expérience en appelant la méthode [GetVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.getvariationasync.aspx). Cette méthode retourne un objet [ExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.aspx) qui fournit l’accès à l’affectation de la variante via la propriété [Variation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.variation.aspx).
```CSharp
ExperimentVariationResult result = await experiment.GetVariationAsync();
variation = result.Variation;
```

4. Vérifiez la propriété [NeedsRefresh](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.needsrefresh.aspx) pour déterminer si l’affectation de la variante mise en cache doit être actualisée. Si c’est le cas, appelez la méthode [RefreshVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.refreshvariationasync.aspx) pour rechercher une affectation de variante mise à jour à partir du serveur et actualisez la variante mise en cache locale.
```CSharp
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
{
      result = await experiment.RefreshVariationAsync();

      // If the call succeeds, use the new result. Otherwise, use the
      // cached value we retrieved earlier.
      if (result.ErrorCode == EngagementErrorCode.Success)
      {
          variation = result.Variation;
      }
}
```

5. Utilisez les méthodes [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getboolean.aspx), [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getdouble.aspx), [GetInteger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getinteger.aspx), ou [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx) de l’objet [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) pour obtenir les paramètres de l’affectation de la variante. Dans chacune de ces méthodes, le premier paramètre est le nom du paramètre que vous voulez récupérer (que vous avez entré dans le tableau de bord du Centre de développement). Le second paramètre est la valeur par défaut que la méthode doit retourner si elle n’est pas en mesure d’extraire la valeur spécifiée à partir du Centre de développement (par exemple, en cas d’absence de connectivité réseau) et si une version de la variante mise en cache n’est pas disponible.

  L’exemple suivant utilise [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx) pour obtenir un paramètre nommé *buttonText* et spécifie une valeur de paramètre par défaut pour **Grey Button**.
```CSharp
var buttonText = currentVariation.GetString("buttonText", "Grey Button");
```
4. Dans votre code, utilisez les valeurs de paramètre pour modifier le comportement de la fonctionnalité que vous testez. Par exemple, le code suivant affecte la valeur *buttonText* au contenu d’un bouton.
```CSharp
button.Content = buttonText;
```

## Ajouter du code pour consigner des événements d’affichage et de conversion dans le Centre de développement

Ensuite, ajoutez du code qui consigne les événements d’affichage et de conversion dans le service des tests A/B du Centre de développement. Votre code doit consigner l’événement d’affichage lorsque l’utilisateur affiche une variante faisant partie de votre expérience. Il doit également consigner l’événement de conversion lorsque l’utilisateur atteint un objectif de votre expérience.

Le code spécifique dont vous avez besoin dépend de votre application, mais voici les étapes générales. Pour un exemple de code complet, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Dans le code qui s’exécute lorsque l’utilisateur affiche une variante, appelez la méthode statique [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) de l’objet [StoreServicesCustomEvents](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.aspx). Transmettez le nom de l’événement d’affichage que vous avez défini dans votre expérience sur votre tableau de bord du Centre de développement ainsi que l’objet [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) qui représente l’affectation de la variante actuelle (cet objet fournit un contexte pour l’événement au Centre de développement). L’exemple suivant consigne un événement d’affichage nommé **userViewedButton**.
```CSharp
StoreServicesCustomEvents.Log("userViewedButton", variation);
```
2. Dans le code qui s’exécute quand l’utilisateur atteint un objectif pour l’un des objectifs de l’expérience, appelez de nouveau la méthode [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) et transmettez le nom de l’événement de conversion que vous avez défini dans votre expérience ainsi que l’objet [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx). L’exemple suivant consigne un événement de conversion nommé **userClickedButton**.
```CSharp
StoreServicesCustomEvents.Log("userClickedButton", variation);
```

## Étapes suivantes

Après avoir défini votre expérience dans le tableau de bord du Centre de développement et codé l’expérience dans votre application, vous êtes prêt à [exécuter et à gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md).

## Rubriques connexes

  * [Définir votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md)
  * [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md)
  * [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)


<!--HONumber=May16_HO2-->


