---
author: PatrickFarley
Description: Learn how to use the powerful Visits Tracking feature for more practical location tracking.
title: Instructions pour l'utilisation du suivi des visites
ms.assetid: 0c101684-48a9-4592-9ed5-6c20f3b830f2
ms.author: pafarley
ms.date: 05/18/2017
ms.topic: article
keywords: windows10, uwp, carte, emplacement, geovisit
ms.localizationpriority: medium
ms.openlocfilehash: 3a78b2689a10dff50696e5e65cc44f79a6f1ea7d
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6046726"
---
# <a name="guidelines-for-using-visits-tracking"></a>Instructions pour l'utilisation du suivi des visites

La fonctionnalité de suivi des visites simplifie le processus de suivi d’emplacement afin de le rendre plus efficace dans le cadre pratique de nombreuses applications. Une visite est définie comme une zone géographique importante dans laquelle l’utilisateur entre et sort. Les visites sont similaires aux [limites géographiques](guidelines-for-geofencing.md) dans la mesure où elles permettent à l’application d’être notifiée uniquement lorsque l’utilisateur entre dans certaines zones d’intérêt ou en sort, évitant ainsi un suivi continu de l’emplacement qui peut entraîner le déchargement de la batterie. Cependant, contrairement aux limites géographiques, les zones de visite sont identifiées dynamiquement au niveau de la plateforme et n’ont pas besoin d’être définies de façon explicite par les applications individuelles. En outre, la sélection des visites suivies par une application est gérée par un paramètre de granularité unique, plutôt qu'au moyen d’abonnements à des emplacements individuels.

## <a name="preliminary-setup"></a>Configuration préliminaire

Avant de poursuivre, assurez-vous que votre application est capable d’accéder à l’emplacement de l’appareil. Vous devrez déclarer la fonctionnalité `Location` dans le manifeste et appeler la méthode **[Geolocator.RequestAccessAsync](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator.RequestAccessAsync)** pour vous assurer que les utilisateurs accordent à l’application des autorisations d’emplacement. Voir [Obtenir l’emplacement de l’utilisateur](get-location.md) pour plus d’informations sur la procédure à suivre. 

N’oubliez pas d’ajouter l’espace de noms `Geolocation` à votre classe. Il sera nécessaire au bon fonctionnement de tous les extraits de code dans ce guide.

```csharp
using Windows.Devices.Geolocation;
```

## <a name="check-the-latest-visit"></a>Vérifier la dernière visite
Le moyen le plus simple d’utiliser la fonctionnalité de suivi des visites consiste à récupérer le dernier changement d’état connu lié à la visite. Un changement d’état est un événement consigné par la plateforme, dans lequel l’utilisateur entre dans un emplacement important ou en sort, un mouvement significatif est détecté depuis le dernier rapport ou l’emplacement de l’utilisateur est perdu (voir l'énumération **[VisitStateChange](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.visitstatechange)**). Les changements d’état sont représentés par les instances **[Geovisit](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisit)**. Pour récupérer l’instance **Geovisit** pour le dernier changement d’état enregistré, il vous suffit d’utiliser la méthode désignée dans la classe **[GeovisitMonitor](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisitmonitor)**.

> [!NOTE]
> Vérifier la dernière visite consignée ne garantit pas que les visites sont actuellement suivies par le système. Afin de suivre les visites quand elles se produisent, vous devez les surveiller au premier plan ou inscrire le suivi en arrière-plan (voir les sections ci-dessous).

```csharp
private async void GetLatestStateChange() {
    // retrieve the Geovisit instance
    Geovisit latestVisit = await GeovisitMonitor.GetLastReportAsync();

    // Using the properties of "latestVisit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```

### <a name="parse-a-geovisit-instance-optional"></a>Analyser une instance Geovisit (facultatif)
La méthode suivante convertit toutes les informations stockées dans une instance **Geovisit** en une chaîne facilement lisible. Elle peut être utilisée dans n'importe quel scénario de ce guide afin d’aider à fournir des commentaires pour les visites consignées.

```csharp
private string ParseGeovisit(Geovisit visit){
    string visitString = null;

    // Use a DateTimeFormatter object to process the timestamp. The following
    // format configuration will give an intuitive representation of the date/time
    Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatterLongTime;
    
    formatterLongTime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(
        "{hour.integer}:{minute.integer(2)}:{second.integer(2)}", new[] { "en-US" }, "US", 
        Windows.Globalization.CalendarIdentifiers.Gregorian, 
        Windows.Globalization.ClockIdentifiers.TwentyFourHour);
    
    // use this formatter to convert the timestamp to a string, and add to "visitString"
    visitString = formatterLongTime.Format(visit.Timestamp);

    // Next, add on the state change type value
    visitString += " " + visit.StateChange.ToString();

    // Next, add the position information (if any is provided; this will be null if 
    // the reported event was "TrackingLost")
    if (visit.Position != null) {
        visitString += " (" +
        visit.Position.Coordinate.Point.Position.Latitude.ToString() + "," +
        visit.Position.Coordinate.Point.Position.Longitude.ToString() + 
        ")";
    }

    return visitString;
}
```

## <a name="monitor-visits-in-the-foreground"></a>Surveiller les visites au premier plan

La classe **GeovisitMonitor** utilisée dans la section précédente gère également le scénario d’écoute des changements d’état sur une période de temps. Pour ce faire, vous pouvez instancier cette classe, inscrire une méthode de gestionnaire pour cet événement et appeler la méthode `Start`.

```csharp
// this GeovisitMonitor instance will belong to the class scope
GeovisitMonitor monitor;

public void RegisterForVisits() {

    // Create and initialize a new monitor instance.
    monitor = new GeovisitMonitor();
    
    // Attach a handler to receive state change notifications.
    monitor.VisitStateChanged += OnVisitStateChanged;
    
    // Calling the start method will start Visits tracking for a specified scope:
    // For higher granularity such as venue/building level changes, choose Venue.
    // For lower granularity in the range of zipcode level changes, choose City.
    monitor.Start(VisitMonitoringScope.Venue);
}
```

Dans cet exemple, la méthode `OnVisitStateChanged` gérera les rapports de visites entrantes. L'instance **Geovisit** correspondante est transférée via le paramètre d’événement.

```csharp
private void OnVisitStateChanged(GeoVisitWatcher sender, GeoVisitStateChangedEventArgs args) {
    Geovisit visit = args.Visit;
    
    // Using the properties of "visit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```
Lorsque l’application a terminé de surveiller les changements d’état liés à la visite, il doit arrêter la surveillance et annuler l’inscription du ou des gestionnaires d’événements. Cela doit être fait également chaque fois que l’application est suspendue ou fermée.

```csharp
public void UnregisterFromVisits() {
    
    // Stop the monitor to stop tracking Visits. Otherwise, tracking will
    // continue until the monitor instance is destroyed.
    monitor.Stop();
    
    // Remove the handler to stop receiving state change events.
    monitor.VisitStateChanged -= OnVisitStateChanged;
}
```

## <a name="monitor-visits-in-the-background"></a>Surveiller les visites en arrière-plan

Vous pouvez également implémenter la surveillance des visites dans une tâche en arrière-plan, afin que l’activité liée à la visite puisse être gérée sur l’appareil, même lorsque votre application n’est pas ouverte. Il s’agit de la méthode recommandée, car elle est la plus polyvalente et la plus économe en énergie. 

Ce guide utilise le modèle fourni dans [Créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task), dans lequel les fichiers de l’application principale se trouvent dans un projet et le fichier de la tâche en arrière-plan se trouve dans un projet distinct de la même solution. Si vous ne connaissez pas l’implémentation des tâches en arrière-plan, il est recommandé de suivre ce conseil principal, en faisant les substitutions ci-dessous nécessaires pour créer une tâche en arrière-plan de gestion des visites.

> [!NOTE]
> Dans les extraits de code suivants, certaines fonctionnalités importantes, telles que la gestion des erreurs et le stockage local, sont absentes par souci de simplicité. Pour une implémentation fiable de la gestion des visites en arrière-plan, voir l’[exemple d’application](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation).


Tout d’abord, assurez-vous que votre application a déclaré des autorisations de tâches en arrière-plan. Dans l’élément `Application/Extensions` de votre fichier *Package.appxmanifest*, ajoutez l’extension suivante (ajoutez un élément `Extensions` s’il n’existe pas déjà).

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.VisitBackgroundTask">
    <BackgroundTasks>
        <Task Type="location" />
    </BackgroundTasks>
</Extension>
```

Ensuite, dans la définition de la classe de tâche en arrière-plan, collez le code suivant. La méthode `Run` de cette tâche en arrière-plan transmettra simplement les détails du déclencheur (qui contiennent les informations de visites) dans une méthode distincte.

```csharp
using Windows.ApplicationModel.Background;

namespace Tasks {
    
    public sealed class VisitBackgroundTask : IBackgroundTask {
        
        public void Run(IBackgroundTaskInstance taskInstance) {
            
            // get a deferral
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
            
            // this task's trigger will be a Geovisit trigger
            GeovisitTriggerDetails triggerDetails = taskInstance.TriggerDetails as GeovisitTriggerDetails;

            // Handle Visit reports
            GetVisitReports(triggerDetails);         

            finally {
                deferral.Complete();
            }
        }        
    }
}
```

Définissez la méthode `GetVisitReports` quelque part dans cette même classe.

```csharp
private void GetVisitReports(GeovisitTriggerDetails triggerDetails) {

    // Read reports from the triggerDetails. This populates the "reports" variable 
    // with all of the Geovisit instances that have been logged since the previous
    // report reading.
    IReadOnlyList<Geovisit> reports = triggerDetails.ReadReports();

    foreach (Geovisit report in reports) {
        // Using the properties of "visit", parse out the time that the state 
        // change was recorded, the device's location when the change was recorded,
        // and the type of state change.
    }

    // Note: depending on the intent of the app, you many wish to store the
    // reports in the app's local storage so they can be retrieved the next time 
    // the app is opened in the foreground.
}
```

Ensuite, dans le projet principal de votre application, vous devez effectuer l’inscription de cette tâche en arrière-plan. Créez une méthode d’inscription qui peut être appelée par une action de l’utilisateur ou qui est appelée chaque fois que la classe est activée.

```csharp
// a reference to this registration should be declared at the class level
private IBackgroundTaskRegistration visitTask = null;

// The app must call this method at some point to allow future use of 
// the background task. 
private async void RegisterBackgroundTask(object sender, RoutedEventArgs e) {
    
    string taskName = "MyVisitTask";
    string taskEntryPoint = "Tasks.VisitBackgroundTask";

    // First check whether the task in question is already registered
    foreach (var task in BackgroundTaskRegistration.AllTasks) {
        if (task.Value.Name == taskName) {
            // if a task is found with the name of this app's background task, then
            // return and do not attempt to register this task
            return;
        }
    }
    
    // Attempt to register the background task.
    try {
        // Get permission for a background task from the user. If the user has 
        // already responded once, this does nothing and the user must manually 
        // update their preference via Settings.
        BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

        switch (backgroundAccessStatus) {
            case BackgroundAccessStatus.AlwaysAllowed:
            case BackgroundAccessStatus.AllowedSubjectToSystemPolicy:
                // BackgroundTask is allowed
                break;

            default:
                // notify user that background tasks are disabled for this app
                //...
                break;
        }

        // Create a new background task builder
        BackgroundTaskBuilder visitTaskBuilder = new BackgroundTaskBuilder();

        visitTaskBuilder.Name = exampleTaskName;
        visitTaskBuilder.TaskEntryPoint = taskEntryPoint;

        // Create a new Visit trigger
        var trigger = new GeovisitTrigger();

        // Set the desired monitoring scope.
        // For higher granularity such as venue/building level changes, choose Venue.
        // For lower granularity in the range of zipcode level changes, choose City. 
        trigger.MonitoringScope = VisitMonitoringScope.Venue; 

        // Associate the trigger with the background task builder
        visitTaskBuilder.SetTrigger(trigger);

        // Register the background task
        visitTask = visitTaskBuilder.Register();      
    }
    catch (Exception ex) {
        // notify user that the task failed to register, using ex.ToString()
    }
}
```

Cela établit qu’une classe de tâche en arrière-plan appelée `VisitBackgroundTask` dans l’espace de noms `Tasks` fera quelque chose avec le type de déclencheur `location`. 

Votre application doit maintenant être capable d’inscrire la tâche en arrière-plan de gestion des visites, et cette tâche doit être activée chaque fois que l’appareil consigne un changement d’état lié à la visite. Vous devrez renseigner la logique dans votre classe de tâche en arrière-plan afin de déterminer ce qu'il faut faire avec ces informations de changement d’état.

## <a name="related-topics"></a>Rubriques associées
* [Créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
* [Obtenir l’emplacement de l’utilisateur](get-location.md)
* [Espace de noms Windows.Devices.Geolocation](https://docs.microsoft.com/uwp/api/windows.devices.geolocation)