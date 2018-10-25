---
author: Xansky
Description: To run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must code the experiment in your app.
title: Coder votre application à des fins d’expérimentation
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, MicrosoftStore Services SDK, tests A/B, expériences
ms.localizationpriority: medium
ms.openlocfilehash: 21c6eab0e021f703c8103462a8459ea190693a10
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5482792"
---
# <a name="code-your-app-for-experimentation"></a>Coder votre application à des fins d’expérimentation

Une fois que vous avez [créé un projet et défini des variables distantes dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), vous êtes prêt à mettre à jour le code dans votre application de plateformeWindows universelle (UWP) dans le but de:
* recevoir des valeurs de variables distances à partir du Centre de développement Windows;
* utiliser des variables distantes pour configurer des expériences d’application pour vos utilisateurs;
* consigner les événements dans le Centre de développement qui indiquent à quels moments les utilisateurs ont visualisé votre expérience et effectué une action désirée (aussi appelée *conversion*).

Pour ajouter ce comportement à votre application, vous allez utiliser les API fournies par le Microsoft Store Services SDK.

Les sections suivantes décrivent le processus général d’obtention de variantes pour votre expérience et de consignation des événements dans le Centre de développement. Après avoir codé votre application à des fins d’expérimentation, vous pouvez [définir une expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md). Pour découvrir une procédure pas à pas illustrant le processus de création et d’exécution d’une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

> [!NOTE]
> Certaines des API d’expérimentation dans le Microsoft Store Services SDK utilisent le [modèle asynchrone](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) pour récupérer les données du centre de développement. Cela signifie qu’une partie de l’exécution de ces méthodes peut avoir lieu après l’appel des méthodes, afin que l’interface utilisateur de votre application puisse rester réactive pendant que les opérations se terminent. Le modèle asynchrone exige que votre application utilise le mot-clé **async** et l’opérateur **await** pour appeler les API, comme illustré par les exemples de code dans cet article. Par convention, les méthodes asynchrones se terminent par **Async**.

## <a name="configure-your-project"></a>Configurer votre projet

Pour commencer, installez le Kit de développement logiciel Microsoft Store Services SDK sur votre ordinateur de développement et ajoutez les références nécessaires à votre projet.

1. Installez le [Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Ouvrez votre projet dans VisualStudio.
3. Dans l’Explorateur de solutions, développez votre nœud de projet, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence**.
3. Dans le **Gestionnaire de références**, développez **Windows universel**, puis cliquez sur **Extensions**.
4. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK**.

> [!NOTE]
> Les exemples de code dans cet article supposent que votre fichier de code contient des instructions **using** pour les espaces de noms **System.Threading.Tasks** et **Microsoft.Services.Store.Engagement** .

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>Obtenir des données de variante et consigner l’événement d’affichage pour votre expérience

Dans le projet, recherchez le code de la fonctionnalité que vous souhaitez modifier dans votre expérience. Ajoutez du code qui récupère les données pour une variante, utilisez ces données pour modifier le comportement de la fonctionnalité que vous testez, puis consignez l’événement d’affichage pour votre expérience dans le service de test A/B dans le Centre de développement.

Le code spécifique requis dépendra de votre application, mais l’exemple suivant illustre le processus de base. Pour obtenir un exemple de code complet, consultez [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

[!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

Les étapes suivantes décrivent les éléments importants de ce processus en détail.

1. Déclarez un objet [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) qui représente l’affectation de variante actuelle et un objet [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) que vous utiliserez pour consigner les événements d’affichage et de conversion dans le Centre de développement.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

2. Déclarez une variable de chaîne affectée à l’[ID de projet](run-app-experiments-with-a-b-testing.md#terms) de l’expérience que vous souhaitez récupérer.
    > [!NOTE]
    > Vous obtenez un projet ID lorsque vous [créez un projet dans le tableau de bord du centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). L’ID de projet présenté ci-dessous n’est fourni qu’à titre d’exemple.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

3. Obtenez l’affectation de variante mise en cache actuelle de votre expérience en appelant la méthode statique [GetCachedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync) et transmettez l’ID de projet de votre expérience à la méthode. Cette méthode renvoie un objet [StoreServicesExperimentVariationResult](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult) qui donne accès à l’affectation de variante via la propriété [ExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation).

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. Vérifiez la propriété [IsStale](htthttps://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale) pour déterminer si l’affectation de variante mise en cache doit être actualisée avec une affectation de variante distante à partir du serveur. Si tel n’est pas le cas, appelez la méthode statique [GetRefreshedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync) pour rechercher une affectation de variante mise à jour sur le serveur et actualiser la variante mise en cache locale.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. Utilisez les méthodes [GetBoolean](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean), [GetDouble](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble), [GetInt32](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32) ou [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) de l’objet [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) pour obtenir les valeurs de l’affectation de variante. Dans chacune de ces méthodes, le premier paramètre est le nom de la variante que vous voulez récupérer (nom que vous entrez dans le tableau de bord du Centre de développement). Le deuxième paramètre est la valeur par défaut que la méthode doit retourner si elle ne parvient pas à récupérer la valeur spécifiée dans le Centre de développement (par exemple, en cas d’absence de connectivité réseau) et si aucune version mise en cache de la variante n’est disponible.

    L’exemple suivant utilise [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) pour obtenir une variable nommée *buttonText* et spécifie la valeur par défaut **Grey Button** à cette variable.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

6. Dans votre code, utilisez les valeurs de variable pour modifier le comportement de la fonctionnalité que vous testez. Par exemple, le code suivant affecte la valeur *buttonText* au contenu d’un bouton dans votre application. Cet exemple suppose que vous avez déjà défini ce bouton ailleurs dans votre projet.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

7. Pour finir, consignez l’[événement d’affichage](run-app-experiments-with-a-b-testing.md#terms) de votre expérience dans le service de test A/B dans le Centre de développement. Initialisez le champ ```logger``` sur un objet [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) et appelez la méthode [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation). Transmettez [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) qui représente l’affectation de la variante actuelle (cet objet fournit un contexte pour l’événement au Centre de développement) et le nom de l’événement d’affichage que vous avez défini dans votre expérience. Cela doit correspondre au nom de l’événement d’affichage que vous entrez pour votre expérience dans le tableau de bord du Centre de développement. Votre code doit consigner l’événement d’affichage lorsque l’utilisateur commence à visualiser une variante faisant partie intégrante de votre expérience.

    L’exemple suivant montre comment consigner un événement d’affichage nommé **userViewedButton**. Dans cet exemple, l’objectif est d’inciter l’utilisateur à cliquer sur un bouton dans l’application, afin de consigner l’événement d’affichage une fois que l’application a récupéré les données de variante (en l’occurrence, le texte du bouton) et lui a attribué le contenu du bouton.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-dev-center"></a>Consigner les événements de conversion dans le Centre de développement

Ensuite, ajoutez du code qui consigne les [événements de conversion](run-app-experiments-with-a-b-testing.md#terms) dans le service des testsA/B du Centre de développement. Votre code doit consigner un événement de conversion quand l’utilisateur atteint un objectif pour votre expérience. Le code spécifique dont vous avez besoin dépend de votre application, mais voici les étapes générales. Pour obtenir un exemple de code complet, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Dans le code qui s’exécute quand l’utilisateur atteint un objectif pour l’un des objectifs de l’expérience, appelez de nouveau la méthode [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) et transmettez l’objet [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) et le nom d’un événement de conversion pour votre expérience. Cela doit correspondre aux noms des événements de conversion que vous entrez pour votre expérience dans le tableau de bord du Centre de développement.

    L’exemple suivant consigne un événement de conversion nommé **userClickedButton** à partir du gestionnaire d’événements **Click** pour un bouton. Dans cet exemple, l’objectif de l’expérience est d’obtenir de l’utilisateur qu’il clique sur le bouton.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>Étapes suivantes

Dès lors que vous avez codé l’expérience dans votre application, vous êtes prêt pour les étapes suivantes:
1. [Définissez votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md). Créez une expérience qui définisse les événements d’affichage, les événements de conversion et les variantes uniques pour votre test A/B.
2. [Exécutez et gérez votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md).


## <a name="related-topics"></a>Rubriques connexes

* [Créer un projet et définir des variables distantes dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Définir votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md)
* [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)
