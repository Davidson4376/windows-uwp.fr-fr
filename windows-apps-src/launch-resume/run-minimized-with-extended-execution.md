---
author: TylerMSFT
description: "Découvrir comment utiliser l’exécution étendue pour que votre application continue de s’exécuter lorsqu’elle est en mode réduit"
title: "Lancer une application en mode réduit avec exécution étendue"
translationtype: Human Translation
ms.sourcegitcommit: e9fcb1f0d1248de25d576029d50070792ad72182
ms.openlocfilehash: 40b2a15379129142a84c4a5caf4317dc50041e06

---

# <a name="run-while-minimized-with-extended-execution"></a>Lancer une application en mode réduit avec exécution étendue

Cet article vous explique comment utiliser l’exécution étendue pour retarder l’interruption de votre application, afin qu’elle continue de s’exécuter lorsqu’elle est en mode réduit.

Lorsque l’utilisateur réduit ou ferme l’application, cette dernière adopte l’état interrompu.  Sa mémoire est conservée, mais son code ne s’exécute pas. Cela s’avère tout particulièrement sur l’ensemble des éditions de système d’exploitation dotées d’une interface utilisateur visuelle. Pour en savoir plus sur les situations d’interruption de votre application, voir [Cycle de vie des applications](app-lifecycle.md).

Il peut arriver que l’application doive continuer de s’exécuter lorsqu’elle est réduite, plutôt que d’être interrompue. Si tel est le cas, le système d’exploitation peut s’assurer lui-même que l’application continue de s’exécuter, ou peut lui demander de continuer à s’exécuter. Ainsi, lorsqu’un morceau s’exécute en arrière-plan, le système d’exploitation peut assurer l’exécution de l’application pendant une plus longue période si vous suivez la procédure de la section [Lecture multimédia en arrière-plan](../audio-video-camera/background-audio.md). Dans le cas contraire, vous devez demander manuellement l’allongement de cette période.

Créez une session [ExtendedExecutionSession](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.extendedexecution.extendedexecutionsession.aspx) afin de demander davantage de temps pour exécuter une opération en arrière-plan. Le type de session **ExtendedExecutionSession** que vous créez est déterminé par la valeur [ExtendedExecutionReason](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.extendedexecution.extendedexecutionreason.aspx) que vous indiquez lorsque vous la générez. Il existe trois valeurs d’énumération **ExtendedExecutionReason** : **Unspecified, LocationTracking** et **SavingData**.

## <a name="run-while-minimized"></a>Exécution en mode réduit

Indiquez le paramètre **ExtendedExecutionReason.Unspecified** lorsque vous créez une session **ExtendedExecutionSession** si vous voulez demander du temps supplémentaire avant de faire passer l’application en arrière-plan pour les situations de type traitement multimédia, compilation de projet, ou maintien de la connexion réseau. Sur les appareils de bureau exécutant Windows 10 pour les éditions de bureau (Famille, Entreprise et Éducation), utilisez cette approche si vous voulez empêcher l’interruption d’une application en mode réduit.

Demandez une extension d’exécution lorsque vous lancez une opération sur le long cours, afin de reporter la transition de l’application vers l’état **Interruption en cours** lorsque cette dernière passe en arrière-plan. Sur les appareils de bureau, les sessions d’exécution étendue créées via le paramètre **ExtendedExecutionReason.Unspecified** sont soumises à une limite de temps dépendant de la batterie. Si l’appareil est branché à une prise murale, la durée de la période d’extension de l’exécution n’est soumise à aucune limite. Si l’appareil est relié à une batterie, la durée de la période d’extension de l’exécution en arrière-plan est limitée à dix minutes au maximum. Un utilisateur de tablette ou d’ordinateur portable peut rencontrer le même comportement d’exécution sur le long terme au détriment de l’autonomie de la batterie lorsque l’option **Toujours autoriser** est sélectionnée dans la zone **Paramètres d’utilisation de la batterie**.

Sur l’ensemble des éditions de système d’exploitation, ce type de session d’exécution étendue s’arrête lorsque l’appareil adopte l’état de veille connectée. Sur les appareils mobiles exécutant Windows 10 Mobile, ce type de session d’exécution étendue s’exécute tant que l’écran est sous tension. Lorsque l’écran est hors tension, l’appareil tente immédiatement d’accéder au mode Veille connectée à faible consommation d’énergie. Sur les appareils mobiles, la session continue de s’exécuter, même lorsque l’écran de verrouillage s’affiche. Une fois l’écran hors tension, l’appareil n’accède pas à l’état de veille connectée avant un certain temps. Sur l’édition du système d’exploitation de la console Xbox, l’appareil accède à l’état de veille connectée au bout d’une heure, sauf si l’utilisateur modifie la valeur par défaut.

## <a name="track-the-users-location"></a>Effectuer un suivi de l’emplacement de l’utilisateur

Lorsque vous créez une session **ExtendedExecutionSession**, indiquez le paramètre **ExtendedExecutionReason.LocationTracking** si votre application doit régulièrement consigner l’emplacement de l’utilisateur depuis [GeoLocator](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.geolocation.geolocator.aspx). Les applications effectuant la navigation et le suivi d’activité physique, afin de surveiller régulièrement l’emplacement de l’utilisateur, doivent utiliser ce motif.

Une session d’exécution étendue associée au suivi de l’emplacement peut s’exécuter aussi longtemps que nécessaire. Cependant, une seule session de ce type peut s’exécuter pour chaque appareil. Vous pouvez uniquement demander l’exécution de cette catégorie de session au premier plan ; l’application doit présenter l’état **Exécution en cours**. Cela permet de s’assurer que l’utilisateur est conscient de l’initiation d’une session de suivi d’emplacement étendue par l’application. Vous pouvez toujours utiliser la fonction Geolocator lorsque l’application est exécutée à l’arrière-plan. Pour cela, utilisez une tâche en arrière-plan ou un service d’application sans demander de session d’exécution étendue de suivi de l’emplacement.

## <a name="save-critical-data-locally"></a>Enregistrement en local des données critiques

Il arrive que le fait de ne pas enregistrer des données avant l’arrêt de l’application entraîne une perte de données ou une expérience utilisateur peu satisfaisante. Dans ce cas, spécifiez le paramètre **ExtendedExecutionReason.SavingData** lorsque vous créez une session **ExtendedExecutionSession** afin d’enregistrer les données utilisateur.

N’utilisez pas ce type de session pour étendre la durée de vie d’une application afin de charger ou de télécharger des données. Pour charger des données, demandez un [transfert en arrière-plan](https://msdn.microsoft.com/en-us/windows/uwp/networking/background-transfers) ou enregistrez un élément **MaintenanceTrigger** afin de gérer le transfert lorsque l’alimentation secteur est disponible. Vous pouvez demander une session d’exécution étendue **ExtendedExecutionReason.SavingData** lorsque l’application est au premier plan et présente l’état **Exécution en cours**, ou à l’arrière-plan, à l’état **Interruption en cours**.

L’état **Interruption en cours** correspond à la dernière occasion dont dispose l’application pour effectuer des tâches avant son arrêt. Le fait de demander une session d’exécution étendue **ExtendedExecutionReason.SavingData** alors que l’application présente l’état **Interruption en cours** crée un problème éventuel dont vous devez être informé. Si une session de ce type est demandée alors que l’état est **Interruption en cours**, et si l’utilisateur demande un nouveau lancement de l’application, cette dernière peut prendre un certain temps à démarrer. En effet, la session d’exécution étendue doit être terminée pour qu’il soit possible de fermer l’ancienne instance de l’application et d’en démarrer une nouvelle. Le délai de performances du lancement est sacrifié de manière à garantir la conservation de l’état utilisateur.

## <a name="request-disposal-and-revocation"></a>Demande, cession et révocation

Il existe trois interactions fondamentales avec une session d’exécution étendue : demande, suppression et révocation.  La création de la demande est indiquée dans l’extrait de code suivant.

### <a name="request"></a>Demande

```csharp
var newSession = new ExtendedExecutionSession();
newSession.Reason = ExtendedExecutionReason.Unspecified;
newSession.Description = "Raising periodic toasts";
newSession.Revoked += SessionRevoked;
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

switch (result)
{
    case ExtendedExecutionResult.Allowed:
        DoLongRunningWork();
        break;

    default:
    case ExtendedExecutionResult.Denied:
        DoShortRunningWork();
        break;
}
```
[Consulter l’exemple de code](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L81-L110)  

L’appel de l’élément **RequestExtensionAsync** permet de vérifier auprès du système d’exploitation si l’utilisateur est associé à une activité en arrière-plan approuvée pour l’application, et si le système inclut des ressources disponibles pour permettre l’exécution en arrière-plan.

Vous pouvez vérifier le paramètre [BackgroundExecutionManager](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) au préalable afin de déterminer la valeur [BackgroundAccessStatus](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.backgroundaccessstatus.aspx?f=255&MSPPError=-2147217396), qui correspond au paramètre utilisateur indiquant si votre application peut s’exécuter en arrière-plan ou non. Pour en savoir plus sur ces paramètres utilisateur, consultez la section relative à [l’activité en arrière-plan et à la prise en charge de l’énergie](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#XWK8mEgWD7JHvC10.97).

L’élément **ExtendedExecutionSession** indique l’opération effectuée par votre application en arrière-plan. La chaîne **Description**, lisible par l’utilisateur, explique pourquoi votre application doit effectuer l’opération. Le gestionnaire d’événements **Revoked** est requis pour permettre un arrêt normal de la session d’exécution étendue si l’utilisateur ou le système décide que l’application ne peut plus s’exécuter en arrière-plan.

### <a name="revoked"></a>Révocation

Si une session d’exécution étendue active est associée à une application, et si le système requiert l’arrêt de l’activité en arrière-plan, la session est révoquée. Le système n’arrête jamais l’exécution d’une session d’exécution étendue sans avoir lancé le gestionnaire d’événements **Revoked** au préalable.

Si l’événement **Revoked** est déclenché dans le cas d’une session d’exécution étendue **ExtendedExecutionReason.SavingData**, l’application dispose d’une seconde pour terminer l’opération en cours et passer à l’état **Interruption en cours**.

Une révocation peut survenir pour diverses raisons (un quota énergétique en arrière-plan ou l’échéance d’exécution d’une application ont été atteints, ou la mémoire doit être récupérée pour permettre à l’utilisateur d’ouvrir une nouvelle application au premier plan, par exemple).

Voici un exemple de gestionnaire d’événements Revoked :

```cs
private async void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        switch (args.Reason)
        {
            case ExtendedExecutionRevokedReason.Resumed:
                rootPage.NotifyUser("Extended execution revoked due to returning to foreground.", NotifyType.StatusMessage);
                break;

            case ExtendedExecutionRevokedReason.SystemPolicy:
                rootPage.NotifyUser("Extended execution revoked due to system policy.", NotifyType.StatusMessage);
                break;
        }

        EndExtendedExecution();
    });
}
```
[Consulter l’exemple de code](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L124-L141)

### <a name="dispose"></a>Suppression

La suppression de la session d’exécution étendue constitue l’étape finale. En effet, il est souhaitable de supprimer la session et toutes les autres ressources utilisant une grande quantité de mémoire, car l’énergie utilisée par l’application pendant qu’elle attend la fermeture de la session est comptabilisée dans la valeur de quota énergétique de cette application. Pour préserver autant que possible le quota énergétique de l’application, vous devez supprimer la session lorsque vous avez terminé votre travail au sein de cette dernière, afin que l’application puisse plus rapidement atteindre l’état **Interrompu**.

En supprimant vous-même la session, plutôt que d’attendre l’événement de révocation, vous réduisez le pourcentage du quota énergétique utilisé par l’application. Cela signifie que votre application sera autorisée à s’exécuter en arrière-plan pendant plus longtemps lors des sessions à venir, car le quota énergétique disponible pour permettre cette exécution sera plus important. Vous devez conserver une référence à l’objet **ExtendedExecutionSession** jusqu’à la fin de l’opération, afin de pouvoir appeler la méthode **Dispose** correspondante.

Voici un extrait de code représentant la suppression d’une session d’exécution étendue :

```cs
void ClearExtendedExecution(ExtendedExecutionSession session)
{
    if (session != null)
    {
        session.Revoked -= SessionRevoked;
        session.Dispose();
        session = null;
    }
}
```
[Consulter l’exemple de code](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L49-L63)

Une application ne peut avoir qu’une seule session **ExtendedExecutionSession** active à la fois. De nombreuses applications utilisent des tâches asynchrones pour effectuer des opérations complexes, qui nécessitent un accès aux ressources de type stockage, réseau ou services réseau. Si l’exécution d’une opération nécessite plusieurs tâches asynchrones, l’état de chaque de ces tâches doit être pris en compte avant la suppression de la session **ExtendedExecutionSession** et l’interruption de l’application. Cela nécessite un décompte de références concernant le nombre de tâches encore en cours ; la session ne peut être supprimée que lorsque cette valeur atteint zéro.

Voici un exemple de code permettant de gérer plusieurs tâches pendant une session d’exécution étendue :

```cs
static class ExtendedExecutionHelper
{
    private static ExtendedExecutionSession session = null;
    private static int taskCount = 0;

    public static bool IsRunning
    {
        get
        {
            if (session != null)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

    public static async Task<ExtendedExecutionResult> RequestSessionAsync(ExtendedExecutionReason reason, TypedEventHandler<object, ExtendedExecutionRevokedEventArgs> revoked)
    {
        // The previous Extended Execution must be closed before a new one can be requested.       
        ClearSession();

        var newSession = new ExtendedExecutionSession();
        newSession.Reason = ExtendedExecutionReason.Unspecified;
        newSession.Description = "Running multiple tasks";
        newSession.Revoked += SessionRevoked;

        if(revoked != null)
        {
            newSession.Revoked += revoked;
        }

        ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

        switch (result)
        {
            case ExtendedExecutionResult.Allowed:
                session = newSession;
                break;
            default:
            case ExtendedExecutionResult.Denied:
                newSession.Dispose();
                break;
        }
        return result;
    }

    public static void ClearSession()
    {
        if (session != null)
        {
            session.Dispose();
            session = null;
        }

        taskCount = 0;
    }

    public static Deferral GetExecutionDeferral()
    {
        if (session == null)
        {
            throw new InvalidOperationException("No extended execution session is active");
        }

        taskCount++;
        return new Deferral(OnTaskCompleted);
    }

    private static void OnTaskCompleted()
    {
        if (taskCount > 0)
        {
            taskCount--;
        }
        else if (taskCount == 0 && session != null)
        {
            ClearSession();
        }
    }

    private static void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
    {
        if (session != null)
        {
            session.Dispose();
            session = null;
        }
    }
}
```
[Consulter l’exemple de code](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario4_MultipleTasks.xaml.cs)

## <a name="ensure-that-your-app-uses-resources-well"></a>Vérifier l’utilisation adéquate des ressources par l’application

Il est crucial d’ajuster le taux d’utilisation de l’énergie et de la mémoire par votre application, afin de vérifier que le système d’exploitation va autoriser votre application à poursuivre son exécution lorsqu’elle n’est plus au premier plan. Utilisez les [API de gestion de la mémoire](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.memorymanager.aspx) pour déterminer la quantité de mémoire utilisée par votre application. Plus votre application utilise de mémoire, plus le système d’exploitation a des difficultés à assurer l’exécution de cette application lorsqu’une autre application se trouve au premier plan. L’utilisateur dispose d’un contrôle étroit sur l’ensemble des activités en arrière-plan que votre application peut exécuter, et bénéficie d’une visibilité étendue sur l’impact de cette dernière sur le taux d’utilisation de la batterie.

Utilisez la méthode [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) pour déterminer si l’utilisateur a opté pour une activité limitée de votre application en arrière-plan. Tenez compte du taux d’utilisation de la batterie ; exécutez l’application en arrière-plan uniquement lorsqu’elle est nécessaire dans le cadre d’une action souhaitée par l’utilisateur.

## <a name="see-also"></a>Voir aussi

[Exemple d’exécution étendue](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ExtendedExecution)  
[Cycle de vie des applications](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/app-lifecycle)  
[Gestion de la mémoire en arrière-plan](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/reduce-memory-usage)  
[Transferts en arrière-plan](https://msdn.microsoft.com/en-us/windows/uwp/networking/background-transfers)  [Activité en arrière-plan et prise en charge de la batterie](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#I2bkQ6861TRpbRjr.97)  
[Classe MemoryManager](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.memorymanager.aspx)  
[Lire du contenu multimédia en arrière-plan](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio)  



<!--HONumber=Dec16_HO3-->


