---
author: TylerMSFT
title: "Convertir un service d’application pour qu’il s’exécute dans le même processus que son fournisseur"
description: "Convertissez un code de service d’application qui s’exécutait dans un processus distinct en arrière-plan en code qui s’exécute dans le même processus que votre fournisseur de service d’application."
translationtype: Human Translation
ms.sourcegitcommit: 9e959a8ae6bf9496b658ddfae3abccf4716957a3
ms.openlocfilehash: 0990e9938bb9bf1794cf58c5541a64f22853b093

---

# Convertir un service d’application pour qu’il s’exécute dans le même processus que son fournisseur

Une [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) permet à une autre application de réactiver votre application en arrière-plan et de démarrer une ligne directe de communication avec celle-ci.

Avec l’introduction des services d’application à processus unique, deux applications au premier plan en cours d’exécution peuvent disposer d’une ligne directe de communication via une connexion de service d’application. Les services d’application peuvent désormais s’exécuter dans le même processus que l’application au premier plan, ce qui simplifie grandement la communication entre des applications tout en supprimant la nécessité de séparer le code de service dans un projet distinct.

Transformer un service d’application de modèle à plusieurs processus en modèle à processus unique nécessite deux modifications. La première est une modification du manifeste.

> ```xml
>  <uap:Extension Category="windows.appService">
>          <uap:AppService Name="SingleProcessAppService" />
>  </uap:Extension>
> ```

Supprimez l’attribut `EntryPoint`. Désormais, le rappel [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) sera utilisé comme méthode de rappel lorsque le service d’application est invoqué.

La seconde modification consiste à déplacer la logique de service de son projet de tâche distinct en arrière-plan dans les méthodes qui peuvent être appelées à partir de **OnBackgroundActivated()**.

À présent, votre application peut directement exécuter votre service d’application.  Par exemple:

> ``` cs
> private AppServiceConnection appServiceconnection;
> private BackgroundTaskDeferral appServiceDeferral;
> protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
> {
>     base.OnBackgroundActivated(args);
>     IBackgroundTaskInstance taskInstance = args.TaskInstance;
>     AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
>     appServiceDeferral = taskInstance.GetDeferral();
>     taskInstance.Canceled += OnAppServicesCanceled;
>     appServiceConnection = appService.AppServiceConnection;
>     appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
>     appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
> }
>
> private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
> {
>     AppServiceDeferral messageDeferral = args.GetDeferral();
>     ValueSet message = args.Request.Message;
>     string text = message["Request"] as string;
>              
>     if ("Value" == text)
>     {
>         ValueSet returnMessage = new ValueSet();
>         returnMessage.Add("Response", "True");
>         await args.Request.SendResponseAsync(returnMessage);
>     }
>     messageDeferral.Complete();
> }
>
> private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
> {
>     appServiceDeferral.Complete();
> }
>
> private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
> {
>     appServiceDeferral.Complete();
> }
> ```

Dans le code ci-dessus, la méthode `OnBackgroundActivated` gère l’activation du service d’application. Tous les événements nécessaires pour la communication via une [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) sont enregistrés, et l’objet de report de tâche est stocké de façon qu’il puisse être marqué comme terminé une fois la communication entre les applications effectuée.

Lorsque l’application reçoit une demande, elle lit l’élément [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx) fourni pour voir si les chaînes `Key` et `Value` sont présentes. Si elles sont bien présentes, le service d’application renvoie alors une paire de valeurs de chaîne `Response` et `True` à l’application sur l’autre côté de la **AppServiceConnection**.

Pour en savoir plus sur la connexion et la communication avec d’autres applications, consultez [Créer et utiliser un service d’application](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service?f=255&MSPPError=-2147217396).



<!--HONumber=Aug16_HO3-->


