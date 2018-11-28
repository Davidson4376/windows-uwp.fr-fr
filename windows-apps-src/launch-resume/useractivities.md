---
title: Poursuivre l’activité utilisateur, même sur différents appareils
description: Cette rubrique décrit comment permettre aux utilisateurs de reprendre ce qu’ils étaient en train de faire dans votre application, même sur plusieurs appareils.
keywords: activité utilisateur, activités utilisateur, chronologie, cortana reprend là où vous en étiez, cortana reprend là où j’en étais, projet rome
ms.date: 04/27/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 98a7697715b456498db3ee5642e2743ae228b3cb
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7964222"
---
# <a name="continue-user-activity-even-across-devices"></a>Poursuivre l’activité utilisateur, même sur différents appareils

Cette rubrique décrit comment permettre aux utilisateurs de reprendre ce qu’ils étaient en train de faire dans votre application sur leur PC, et sur plusieurs appareils.

## <a name="user-activities-and-timeline"></a>Activités et Chronologie utilisateur

Chaque jour, nous répartissons notre temps sur plusieurs appareils. Nous pouvons utiliser notre téléphone dans le bus, un PC pendant la journée, puis un téléphone ou une tablette le soir. À compter de Windows10, build1803 ou d'une version supérieure, la création d’une [activité utilisateur](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) fait apparaître cette activité dans la Chronologie Windows et dans la fonctionnalité Pick Up Where I Left Off de Cortana. La Chronologie est un affichage des tâches enrichi qui tire parti des activités utilisateur pour afficher une présentation chronologique des éléments sur lesquels vous avez travaillé. Elle peut également inclure les éléments sur lesquels vous avez travaillé sur plusieurs appareils.

![Image de Chronologie Windows](images/timeline.png)

De même, associer votre téléphone à votre PC Windows vous permet de continuer ce que vous faisiez précédemment sur votre appareil iOS ou Android.

Considérez un objet **UserActivity** comme un élément spécifique sur lequel travaillait l’utilisateur dans votre application. Par exemple, si vous utilisez un lecteur RSS, un objet **UserActivity** pourrait être le flux que vous lisez. Si vous jouez à un jeu, l'objet **UserActivity** pourrait être le niveau auquel vous jouez. Si vous écoutez une application de musique, l'objet **UserActivity** pourrait être la playlist que vous écoutez. Si vous travaillez sur un document, l'objet **UserActivity** pourrait être là où vous avez arrêté votre travail, et ainsi de suite.  En résumé, un objet **UserActivity** représente une destination dans votre application qui permet à l’utilisateur de reprendre ce qu’il était en train de faire.

Lorsque vous recourez à un objet **UserActivity** en appelant [UserActivity.CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), le système crée un enregistrement d'historique indiquant l’heure de début et de fin de cet objet **UserActivity**. Chaque fois que vous faites appel à cet objet **UserActivity**, un nouvel enregistrement d'historique est enregistré.

## <a name="add-user-activities-to-your-app"></a>Ajouter des activités utilisateur à votre application

Un objet [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) est l’unité de l’engagement utilisateur dans Windows. Il se compose de trois parties: un URI utilisé pour activer l’application à laquelle appartient l’activité, des visuels et des métadonnées qui décrivent l’activité.

1. L'URI [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri#Windows_ApplicationModel_UserActivities_UserActivity_ActivationUri) est utilisé pour la reprise de l’application avec un contexte spécifique. En général, ce lien se présente sous forme de gestionnaire de protocole pour un schéma (par exemple, «my-app://page2?action=edit») ou d’AppUriHandler (par exemple, http://constoso.com/page2?action=edit).
2. [VisualElements](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.visualelements) expose une classe qui permet à l’utilisateur d’identifier visuellement une activité avec un titre, une description ou des éléments de carte adaptative.
3. Enfin, [Contenu](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.content#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_Content) est l'emplacement où vous pouvez stocker les métadonnées de l’activité qui peuvent être utilisées pour regrouper et récupérer des activités dans un contexte spécifique. Cela se présente souvent sous forme de données [http://schema.org](http://schema.org).

Pour ajouter un objet **UserActivity** à votre application:

1. Générez des objets **UserActivity** lorsque le contexte de votre utilisateur évolue dans l’application (par exemple, navigation entre les pages, nouveau niveau de jeu, etc.)
2. Remplissez des objets **UserActivity** avec l’ensemble minimal de champs obligatoires: [ActivityId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activityid#Windows_ApplicationModel_UserActivities_UserActivity_ActivityId), [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri) et [UserActivity.VisualElements.DisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.displaytext#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_DisplayText).
3. Ajoutez un gestionnaire de schéma personnalisé à votre application afin qu’elle puisse être réactivée par un objet **UserActivity**.

Un objet **UserActivity** peut être intégré à une application avec seulement quelques lignes de code. Par exemple, imaginez ce code dans MainPage.xaml.cs, à l’intérieur de la classe MainPage (remarque: supposons `using Windows.ApplicationModel.UserActivities;`):

```csharp
UserActivitySession _currentActivity;
private async Task GenerateActivityAsync()
{
    // Get the default UserActivityChannel and query it for our UserActivity. If the activity doesn't exist, one is created.
    UserActivityChannel channel = UserActivityChannel.GetDefault();
    UserActivity userActivity = await channel.GetOrCreateUserActivityAsync("MainPage");
 
    // Populate required properties
    userActivity.VisualElements.DisplayText = "Hello Activities";
    userActivity.ActivationUri = new Uri("my-app://page2?action=edit");
     
    //Save
    await userActivity.SaveAsync(); //save the new metadata
 
    // Dispose of any current UserActivitySession, and create a new one.
    _currentActivity?.Dispose();
    _currentActivity = userActivity.CreateSession();
}
```

La première ligne de la méthode `GenerateActivityAsync()` ci-dessus correspond à l'objet [UserActivityChannel](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel) d'un utilisateur. Il s’agit du flux sur lequel seront publiées les activités de cette application. La ligne suivante interroge le canal d’une activité appelée `MainPage`.

* Votre application doit nommer des activités de sorte qu'un même identifiant est généré chaque fois que l’utilisateur se trouve dans un emplacement spécifique au sein de l’application. Par exemple, si votre application est fondée sur une page, utilisez un identifiant pour cette page; si elle est fondée sur un document, utilisez le nom de ce document (ou un hachage du nom).
* Si une activité existante se trouve dans le flux avec le même identifiant, cette activité est renvoyée par le canal avec la valeur [Publiée](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitystate)) attribuée à `UserActivity.State`. Si aucune activité ne porte ce nom, la nouvelle activité est renvoyée avec la valeur **Nouvelle** attribuée à `UserActivity.State`.
* Les activités sont étendues à votre application. Vous n’avez pas besoin de vous soucier d'un conflit entre l'identifiant de votre activité et des identifiants présents dans d’autres applications.

Après avoir obtenu ou créé l'objet **UserActivity**, remplissez les deux autres champs obligatoires: `UserActivity.VisualElements.DisplayText` et `UserActivity.ActivationUri`.

Ensuite, enregistrez les métadonnées **UserActivity** en appelant [SaveAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.saveasync) et enfin [CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), qui renvoie un objet [UserActivitySession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitysession). L'objet **UserActivitySession** nous permet de gérer les situations où l’utilisateur fait vraiment appel à l'objet **UserActivity**. Par exemple, nous devons appeler `Dispose()` sur l'objet **UserActivitySession** lorsque l’utilisateur quitte la page. Dans l’exemple ci-dessus, nous appelons également `Dispose()` sur `_currentActivity` avant d’appeler `CreateSession()`. Nous procédons à cette opération, car nous avons défini `_currentActivity` comme un champ membre de notre page et nous souhaitons arrêter toute activité existante avant de commencer la nouvelle (remarque: le `?` est l'[opérateur conditionnel Null](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-conditional-operators) qui effectue des tests sur la valeur Null avant d’exécuter l’accès membre).

Étant donné que, dans ce cas, l'objet `ActivationUri` est un schéma personnalisé, nous devons également enregistrer le protocole dans le manifeste de l’application. Cette opération est effectuée dans le fichier XML Package.appmanifest ou à l'aide du concepteur.

Pour réaliser le changement avec le concepteur, double-cliquez sur le fichier Package.appmanifest dans votre projet pour lancer le concepteur, sélectionnez l'onglet **Déclarations**, puis ajoutez une définition **Protocole**. À ce stade, **Nom** est la seule propriété qui doit être remplie. Elle doit correspondre à l’URI que nous avons spécifié ci-dessus, `my-app`.

Nous devons maintenant écrire du code pour indiquer à l'application comment réagir quand elle est activée par un protocole. Nous allons remplacer la méthode `OnActivated` dans App.xaml.cs pour transférer l’URI sur la page principale, comme suit:

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        var uriArgs = e as ProtocolActivatedEventArgs;
        if (uriArgs != null)
        {
            if (uriArgs.Uri.Host == "page2")
            {
                // Navigate to the 2nd page of the  app
            }
        }
    }
    Window.Current.Activate();
}
```

Ce code permet de détecter si l’application a été activée via un protocole. Si tel est le cas, il recherche ce que doit faire l'application pour reprendre la tâche pour laquelle elle est activée. Comme il s'agit d'une application simple, la seule activité que cette application permet de reprendre consiste à vous mettre sur la page secondaire lors de l’ouverture de l'application.

## <a name="use-adaptive-cards-to-improve-the-timeline-experience"></a>Utiliser des cartes adaptatives pour améliorer l’expérience relative à la Chronologie

Les activités utilisateur s’affichent dans Cortana et la Chronologie. Lorsque les activités apparaissent dans la Chronologie, nous les affichons à l’aide de l'infrastructure [Carte adaptative](http://adaptivecards.io/). Si vous ne spécifiez pas une carte adaptative pour chaque activité, la Chronologie crée automatiquement une carte d’activité simple sur la base du nom et de l'icône de votre application, du champ titre et du champ de description facultatif. Ci-dessous se trouve un exemple de charge utile de carte adaptative et la carte générée.

![Une carte adaptative](images/adaptivecard.png)]

Exemple de charge utile de carte adaptative sous forme de chaîne JSON:

```json
{ 
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
  "type": "AdaptiveCard", 
  "version": "1.0",
  "backgroundImage": "https://winblogs.azureedge.net/win/2017/11/eb5d872c743f8f54b957ff3f5ef3066b.jpg", 
  "body": [ 
    { 
      "type": "Container", 
      "items": [ 
        { 
          "type": "TextBlock", 
          "text": "Windows Blog", 
          "weight": "bolder", 
          "size": "large", 
          "wrap": true, 
          "maxLines": 3 
        }, 
        { 
          "type": "TextBlock", 
          "text": "Training Haiti’s radiologists: St. Louis doctor takes her teaching global", 
          "size": "default", 
          "wrap": true, 
          "maxLines": 3 
        } 
      ] 
    } 
  ]
}
```

Ajoutez la charge utile de cartes adaptatives sous forme de chaîne JSON à l'objet **UserActivity** comme suit:

```csharp
activity.VisualElements.Content = 
Windows.UI.Shell.AdaptiveCardBuilder.CreateAdaptiveCardFromJson(jsonCardText); // where jsonCardText is a JSON string that represents the card
```

## <a name="cross-platform-and-service-to-service-integration"></a>Intégration multiplateforme et de service à service

Si votre application s’exécute sur plusieurs plateformes (par exemple sur Android et iOS) ou conserve l’état utilisateur dans le cloud, vous pouvez publier des objets UserActivity via [MicrosoftGraph](https://developer.microsoft.com/graph/).
Une fois que votre application ou service est authentifié(e) avec un compte Microsoft, il suffit de deux simples appels REST pour générer les objets [Activité](https://developer.microsoft.com/graph/docs/api-reference/beta/api/projectrome_put_activity) et [Historique](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/projectrome_historyitem) à l’aide des mêmes données que celles décrites ci-dessus.

## <a name="summary"></a>Résumé

Vous pouvez utiliser l'API [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities) pour que votre application s’affiche dans la Chronologie et dans Cortana.
* En savoir plus sur les [ **UserActivity** API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)
* Consultez l'[exemple de code](https://github.com/Microsoft/project-rome).
* Voir des [cartes adaptatives plus sophistiquées](http://adaptivecards.io/).
* Publiez un objet **UserActivity** à partir d’iOS, d'Android ou de votre service web via [MicrosoftGraph](https://developer.microsoft.com/graph/).
* En savoir plus sur le [projet Rome sur GitHub](https://github.com/Microsoft/project-rome)

## <a name="key-apis"></a>Principales API

* [Espace de noms UserActivities](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Rubriques associées

* [Activités de l’utilisateur (documentation du projet «Rome»)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Cartes adaptatives](https://docs.microsoft.com/adaptive-cards/)
* [Visualiseur de cartes adaptatives, exemples](http://adaptivecards.io/)
* [Gérer l’activation des URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Interagir avec vos clients sur toute plateforme à l’aide de MicrosoftGraph, du flux d’activité et des cartes adaptatives](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)