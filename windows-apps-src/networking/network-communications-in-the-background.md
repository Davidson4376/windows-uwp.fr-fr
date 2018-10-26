---
author: stevewhims
description: Afin de poursuivre la communication réseau en dehors de l'arrière-plan, une application doit utiliser les tâches d'arrière-plan et le broker de socket ou les déclencheurs de chaîne de contrôle.
title: Communications réseau en arrière-plan
ms.assetid: 537F8E16-9972-435D-85A5-56D5764D3AC2
ms.author: stwhi
ms.date: 06/14/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34fad804bb36ad1b4ce92a56772c33318e10faa8
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5561188"
---
# <a name="network-communications-in-the-background"></a>Communications réseau en arrière-plan
Pour poursuivre la communication réseau alors qu’elle n’est pas au premier plan, votre application peut utiliser des tâches en arrière-plan et l’autre de ces deux options.
- Broker de socket. Si votre application utilise des sockets pour les connexions à long terme, lorsqu’il quitte le premier plan, elle peut déléguer la propriété d’un socket à un broker de socket système. Le broker puis: active votre application lorsque le trafic atteint le socket; transfère la propriété à votre application; et votre application traite alors le trafic entrant.
- Déclencheurs de canal de contrôle. 

## <a name="performing-network-operations-in-background-tasks"></a>Exécution d’opérations réseau dans les tâches en arrière-plan
- Utilisez un [SocketActivityTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.socketactivitytrigger) pour activer la tâche en arrière-plan lorsqu’un paquet est reçu et que vous devez effectuer une tâche de courte durée. Après l’exécution de la tâche, la tâche en arrière-plan doit s’arrêter pour économiser l’énergie.
- Utilisez un [ControlChannelTrigger](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) pour activer la tâche en arrière-plan lorsqu’un paquet est reçu et que vous devez effectuer une tâche de longue durée.

**Conditions liées au réseau et indicateurs**

- Ajoutez la condition **InternetAvailable** à votre tâche en arrière-plan [BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) pour retarder le déclenchement de la tâche en arrière-plan jusqu'à ce que la pile réseau s’exécute. Cette condition économise l’énergie car la tâche en arrière-plan ne s’exécute pas avant que le réseau soit fonctionnel. Cette condition ne fournit pas d’activation en temps réel.

Quel que soit le déclencheur que vous utilisez, définissez [IsNetworkRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder) sur votre tâche en arrière-plan pour vous assurer que le réseau reste opérationnel pendant que cette tâche s’exécute. Cela indique à l’infrastructure de tâches en arrière-plan qu’elle doit maintenir le réseau actif pendant l’exécution de la tâche, même si le périphérique est passé en mode de veille connectée. Si votre tâche en arrière-plan n’utilise pas **IsNetworkRequested**, alors votre tâche en arrière-plan ne sera pas en mesure d’accéder au réseau en mode de veille connectée (par exemple, lorsque l’écran du téléphone est éteint).

## <a name="socket-broker-and-the-socketactivitytrigger"></a>Broker de socket et SocketActivityTrigger
Si votre application utilise des connexions [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882), ou [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906), il est préférable d’utiliser [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) et le broker de socket pour être averti lorsque du trafic atteint votre application alors qu’elle n’est plus au premier plan.

Pour que votre application reçoive et traite les données reçues sur un socket lorsqu’elle est inactive, elle doit effectuer une configuration unique au démarrage, puis transférer la propriété du socket au broker de socket lorsqu’elle bascule vers un état d’inactivité.

Les étapes de l’installation ponctuelle visent à créer un déclencheur, à inscrire une tâche en arrière-plan pour ce déclencheur, et à activer le socket pour le broker de socket:
  - Créez un **SocketActivityTrigger** et inscrivez une tâche en arrière-plan pour le déclencheur en définissant le paramètre TaskEntryPoint sur votre code de traitement d’un paquet reçu.
```csharp
            var socketTaskBuilder = new BackgroundTaskBuilder();
            socketTaskBuilder.Name = _backgroundTaskName;
            socketTaskBuilder.TaskEntryPoint = _backgroundTaskEntryPoint;
            var trigger = new SocketActivityTrigger();
            socketTaskBuilder.SetTrigger(trigger);
            _task = socketTaskBuilder.Register();
```
  - Appelez **EnableTransferOwnership** sur le socket, avant de lier le socket.
```csharp
           _tcpListener = new StreamSocketListener();

           // Note that EnableTransferOwnership() should be called before bind,
           // so that tcpip keeps required state for the socket to enable connected
           // standby action. Background task Id is taken as a parameter to tie wake pattern
           // to a specific background task.  
           _tcpListener. EnableTransferOwnership(_task.TaskId,SocketActivityConnectedStandbyAction.Wake);
           _tcpListener.ConnectionReceived += OnConnectionReceived;
           await _tcpListener.BindServiceNameAsync("my-service-name");
```

Une fois votre socket correctement configuré et lorsque votre application est sur le point d’être suspendue, appelez la méthode **TransferOwnership** sur le socket pour le transférer au broker de socket. Le broker surveille le socket et active votre tâche en arrière-plan lors de la réception de données. L’exemple suivant inclut une fonction **TransferOwnership** utilitaire pour effectuer le transfert des sockets **StreamSocketListener**. (Notez que les différents types de sockets ont tous leur propre méthode **TransferOwnership**. Vous devez donc appeler la méthode appropriée pour le socket dont vous transférez la propriété. Votre code contient probablement une fonction d’assistance **TransferOwnership** surchargée avec une implémentation pour chaque type de socket utilisé, afin que le code **OnSuspending** reste facile à lire.)

Une application transfère la propriété d’un socket à un broker de socket et transmet l’ID de la tâche en arrière-plan à l’aide de l’une des méthodes suivantes, selon celle qui est la plus appropriée:
-   L’une des méthodes [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn804256) sur un [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319).
-   L’une des méthodes [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn781433) sur un [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882).
-   L’une des méthodes [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn804407) sur un [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906).

```csharp

// declare int _transferOwnershipCount as a field.

private void TransferOwnership(StreamSocketListener tcpListener)
{
    await tcpListener.CancelIOAsync();

    var dataWriter = new DataWriter();
    ++_transferOwnershipCount;
    dataWriter.WriteInt32(transferOwnershipCount);
    var context = new SocketActivityContext(dataWriter.DetachBuffer());
    tcpListener.TransferOwnership(_socketId, context);
}

private void OnSuspending(object sender, SuspendingEventArgs e)
{
    var deferral = e.SuspendingOperation.GetDeferral();

    TransferOwnership(_tcpListener);
    deferral.Complete();
}
```
Dans le gestionnaire d’événements de votre tâche en arrière-plan :
   -  Vous devez d’abord obtenir un report de tâche en arrière-plan afin de pouvoir gérer l’événement à l’aide de méthodes asynchrones.
```csharp
var deferral = taskInstance.GetDeferral();
```
   -  Ensuite, extrayez l’élément SocketActivityTriggerDetails des arguments de l’événement et trouvez le motif de déclenchement de l’événement:
```csharp
var details = taskInstance.TriggerDetails as SocketActivityTriggerDetails;
    var socketInformation = details.SocketInformation;
    switch (details.Reason)
```
   -   Si l’événement a été déclenché en raison de l’activité du socket, créez un DataReader sur le socket, chargez ce lecteur de manière asynchrone, puis utilisez les données en fonction de la conception de votre application. Notez que vous devez renvoyer la propriété du socket vers le broker de socket de manière à être averti de toute activité supplémentaire du socket.

   Dans l’exemple suivant, le texte reçu sur le socket est affiché dans une notification toast.

```csharp
case SocketActivityTriggerReason.SocketActivity:
            var socket = socketInformation.StreamSocket;
            DataReader reader = new DataReader(socket.InputStream);
            reader.InputStreamOptions = InputStreamOptions.Partial;
            await reader.LoadAsync(250);
            var dataString = reader.ReadString(reader.UnconsumedBufferLength);
            ShowToast(dataString);
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   Si l’événement a été déclenché parce qu’un minuteur de connexion active a expiré, votre code doit envoyer des données sur le socket pour le garder actif et redémarrer le minuteur de connexion active. Là encore, il est important de retransférer la propriété du socket au broker de socket pour pouvoir recevoir des notifications d’événement supplémentaires :

```csharp
case SocketActivityTriggerReason.KeepAliveTimerExpired:
            socket = socketInformation.StreamSocket;
            DataWriter writer = new DataWriter(socket.OutputStream);
            writer.WriteBytes(Encoding.UTF8.GetBytes("Keep alive"));
            await writer.StoreAsync();
            writer.DetachStream();
            writer.Dispose();
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   Si l’événement a été déclenché en raison de la fermeture du socket, rétablissez le socket et assurez-vous de transférer la propriété du nouveau socket créé au broker de socket. Dans cet exemple, le nom d’hôte et le port sont stockés dans les paramètres locaux afin de pouvoir être utilisés pour établir une nouvelle connexion de socket :

```csharp
case SocketActivityTriggerReason.SocketClosed:
            socket = new StreamSocket();
            socket.EnableTransferOwnership(taskInstance.Task.TaskId, SocketActivityConnectedStandbyAction.Wake);
            if (ApplicationData.Current.LocalSettings.Values["hostname"] == null)
            {
                break;
            }
            var hostname = (String)ApplicationData.Current.LocalSettings.Values["hostname"];
            var port = (String)ApplicationData.Current.LocalSettings.Values["port"];
            await socket.ConnectAsync(new HostName(hostname), port);
            socket.TransferOwnership(socketId);
            break;
```

   -   N’oubliez pas de terminer votre report, une fois que vous aurez fini de traiter la notification d’événement :

```csharp
  deferral.Complete();
```

Pour voir un exemple complet de l’utilisation du [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) et du broker de socket, consultez [l’exemple SocketActivityStreamSocket](http://go.microsoft.com/fwlink/p/?LinkId=620606). L’initialisation du socket est effectuée dans Scenario1\_Connect.xaml.cs, et l’implémentation de la tâche en arrière-plan est effectuée dans SocketActivityTask.cs.

Vous remarquerez probablement que l’exemple appelle **TransferOwnership** dès la création d’un nouveau socket ou l’acquisition d’un socket existant, au lieu d’utiliser pour cela le gestionnaire d’événements **OnSuspending**, comme décrit dans cette rubrique. L’exemple se concentre en effet sur l’illustration de l’utilisation du [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) et n’utilise donc le socket pour aucune autre activité pendant son exécution. Votre application sera probablement plus complexe et devra utiliser **OnSuspending** pour déterminer à quel moment appeler **TransferOwnership**.

## <a name="control-channel-triggers"></a>Déclencheurs de canal de contrôle
Assurez-vous tout d’abord que vous utilisez les déclencheurs de canal de contrôle (CCT) de manière appropriée. Si vous utilisez des connexions [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) , [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)ou [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), puis nous vous recommandons d’utiliser [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009). Vous pouvez utiliser les CCT pour **StreamSocket**, mais ils consomment plus de ressources et peuvent ne pas fonctionner en mode Veille connectée.

Si vous utilisez des WebSockets, [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151), [**System.Net.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)ou [**Windows.Web.Http.HttpClient**](/uwp/api/windows.web.http.httpclient), vous devez utiliser [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).

## <a name="controlchanneltrigger-with-websockets"></a>ControlChannelTrigger avec WebSockets
Certaines considérations spéciales s’appliquent lors de l’utilisation de [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Certaines meilleures pratiques et certains modèles d’utilisation spécifiques au transport doivent être respectés lors de l’utilisation d’un **MessageWebSocket** ou **StreamWebSocket** avec **ControlChannelTrigger**. De plus, ces considérations affectent la manière dont les demandes de réception de paquets sur le **StreamWebSocket** sont gérées. Les demandes de réception de paquets sur le **MessageWebSocket** ne sont pas affectées.

Les meilleures pratiques et modèles d’utilisation suivants doivent être respectés lors de l’utilisation de [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).

-   Une réception de socket en attente doit rester publiée en permanence. Cela est obligatoire pour permettre aux tâches de notifications Push de se produire.
-   Le protocole WebSocket définit un modèle standard pour les messages de persistance de connexion. La classe [**WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531) peut envoyer au serveur des messages de persistance de connexion de protocole WebSocket à l’initiative du client. La classe **WebSocketKeepAlive** doit être inscrite en tant que TaskEntryPoint pour un KeepAliveTrigger par l’application.

Certaines considérations spéciales affectent la manière dont les demandes de réception de paquets sur le [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) sont gérées. En particulier, lors de l’utilisation d’un **StreamWebSocket** avec le [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), votre application doit utiliser un modèle asynchrone brut pour la gestion des lectures plutôt que le modèle **await** en C# et VB.NET ou Tâches en C++. Le modèle asynchrone brut est illustré par un exemple de code plus loin dans cette section.

L’utilisation du modèle asynchrone brut permet à Windows de synchroniser la méthode [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) sur la tâche en arrière-plan pour le [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) avec le retour du rappel d’achèvement de réception. La méthode **Run** est appelée après le retour du rappel d’achèvement. Cela permet de s’assurer que l’application a reçu les données/erreurs avant l’appel de la méthode **Run**.

Il est important de noter que l’application doit publier une autre lecture avant de redonner le contrôler suite au rappel d’achèvement. Notez également que le [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) ne peut pas être utilisé directement avec le transport [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) car cela entraîne une rupture de la synchronisation décrite ci-dessus. L’utilisation de la méthode [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/br208135) directement par-dessus le transport n’est pas prise en charge. Au lieu de cela, le [**IBuffer**](https://msdn.microsoft.com/library/windows/apps/br241656) renvoyé par la méthode [**IInputStream.ReadAsync**](https://msdn.microsoft.com/library/windows/apps/br241719) sur la propriété [**StreamWebSocket.InputStream**](https://msdn.microsoft.com/library/windows/apps/br226936) peut être passé ultérieurement à la méthode [**DataReader.FromBuffer**](https://msdn.microsoft.com/library/windows/apps/br208133) pour un traitement plus approfondi.

L’exemple suivant montre comment utiliser un modèle asynchrone brut pour la gestion des lectures sur le [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).

```csharp
void PostSocketRead(int length)
{
    try
    {
        var readBuf = new Windows.Storage.Streams.Buffer((uint)length);
        var readOp = socket.InputStream.ReadAsync(readBuf, (uint)length, InputStreamOptions.Partial);
        readOp.Completed = (IAsyncOperationWithProgress<IBuffer, uint>
            asyncAction, AsyncStatus asyncStatus) =>
        {
            switch (asyncStatus)
            {
                case AsyncStatus.Completed:
                case AsyncStatus.Error:
                    try
                    {
                        // GetResults in AsyncStatus::Error is called as it throws a user friendly error string.
                        IBuffer localBuf = asyncAction.GetResults();
                        uint bytesRead = localBuf.Length;
                        readPacket = DataReader.FromBuffer(localBuf);
                        OnDataReadCompletion(bytesRead, readPacket);
                    }
                    catch (Exception exp)
                    {
                        Diag.DebugPrint("Read operation failed:  " + exp.Message);
                    }
                    break;
                case AsyncStatus.Canceled:

                    // Read is not cancelled in this sample.
                    break;
           }
       };
   }
   catch (Exception exp)
   {
       Diag.DebugPrint("failed to post a read failed with error:  " + exp.Message);
   }
}
```

Il est sûr que le gestionnaire d’achèvement de lecture se déclenchera avant que la méthode [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) sur la tâche en arrière-plan pour le [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) ne soit appelée. Windows dispose d’une synchronisation interne qui l’incite à attendre le retour d’une application du rappel d’achèvement de lecture. En général, l’application traite rapidement les données ou l’erreur du [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) dans le rappel d’achèvement de lecture. Le message proprement dit est traité dans le contexte de la méthode **IBackgroundTask.Run**. Dans l’exemple ci-dessous, cet aspect est illustré par l’utilisation d’une file d’attente de messages dans laquelle le gestionnaire d’achèvement de lecture insère le message, qui est ensuite traitée par la tâche en arrière-plan.

L’exemple suivant montre le gestionnaire d’achèvement de lecture à utiliser avec un modèle asynchrone brut pour la gestion des lectures sur le [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).

```csharp
public void OnDataReadCompletion(uint bytesRead, DataReader readPacket)
{
    if (readPacket == null)
    {
        Diag.DebugPrint("DataReader is null");

        // Ideally when read completion returns error,
        // apps should be resilient and try to
        // recover if there is an error by posting another recv
        // after creating a new transport, if required.
        return;
    }
    uint buffLen = readPacket.UnconsumedBufferLength;
    Diag.DebugPrint("bytesRead: " + bytesRead + ", unconsumedbufflength: " + buffLen);

    // check if buffLen is 0 and treat that as fatal error.
    if (buffLen == 0)
    {
        Diag.DebugPrint("Received zero bytes from the socket. Server must have closed the connection.");
        Diag.DebugPrint("Try disconnecting and reconnecting to the server");
        return;
    }

    // Perform minimal processing in the completion
    string message = readPacket.ReadString(buffLen);
    Diag.DebugPrint("Received Buffer : " + message);

    // Enqueue the message received to a queue that the push notify
    // task will pick up.
    AppContext.messageQueue.Enqueue(message);

    // Post another receive to ensure future push notifications.
    PostSocketRead(MAX_BUFFER_LENGTH);
}
```

Le gestionnaire de persistance de connexion constitue un autre détail des Websockets. Le protocole WebSocket définit un modèle standard pour les messages de persistance de connexion.

Lors de l’utilisation de [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou de [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923), inscrivez une instance de classe [**WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531) en tant que [**TaskEntryPoint**](https://msdn.microsoft.com/library/windows/apps/br224774) pour un KeepAliveTrigger afin de pouvoir effectuer la reprise de l’application et d’envoyer régulièrement des messages de persistance de connexion au serveur (point de terminaison distant). Cette opération doit être effectuée dans le cadre du code d’application d’inscription en arrière-plan, ainsi que dans le manifeste de package.

Ce point d’entrée de tâche de [**Windows.Sockets.WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531) doit être spécifié à deux emplacements :

-   lors de la création du déclencheur KeepAliveTrigger dans le code source (voir l’exemple ci-dessous) ;
-   dans le manifeste de package d’application pour la déclaration de tâche en arrière-plan de persistance de connexion.

L’exemple qui suit ajoute une notification de déclencheur réseau et un déclencheur de persistance de connexion sous l’élément &lt;Application&gt; d’un manifeste d’application.

```xml
  <Extensions>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Background.PushNotifyTask">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Windows.Networking.Sockets.WebSocketKeepAlive">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
  </Extensions>
```

Faites preuve d’extrême prudence lorsque vous utilisez une instruction **await** dans le contexte d’un [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) et une opération asynchrone sur [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923), [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). Vous pouvez utiliser un objet **Task&lt;bool&gt;** pour inscrire un pour la notification Push et les persistances de connexion WebSocket sur **StreamWebSocket** et connecter le transport. Dans le cadre de l’inscription, le transport **StreamWebSocket** est défini comme le transport pour **ControlChannelTrigger** et une lecture est publiée. **Task.Result** bloque le thread actuel jusqu’à ce que toutes les étapes de la tâche s’exécutent et retournent des instructions dans le corps du message. La tâche n’est pas résolue tant que la méthode n’a pas retourné la valeur true ou false. De cette façon, la méthode en entier est exécutée. **Task** peut contenir plusieurs instructions **await** qui sont protégées par **Task**. Ce modèle doit être utilisé avec l’objet **ControlChannelTrigger** quand **StreamWebSocket** ou **MessageWebSocket** est utilisé comme transport. Pour les opérations dont l’exécution peut prendre du temps (par exemple une opération de lecture asynchrone type), l’application doit utiliser le modèle asynchrone brut évoqué précédemment.

L’exemple suivant inscrit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) pour la notification Push et les persistances de connexion WebSocket sur [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).

```csharp
private bool RegisterWithControlChannelTrigger(string serverUri)
{
    // Make sure the objects are created in a system thread
    // Demonstrate the core registration path
    // Wait for the entire operation to complete before returning from this method.
    // The transport setup routine can be triggered by user control, by network state change
    // or by keepalive task
    Task<bool> registerTask = RegisterWithCCTHelper(serverUri);
    return registerTask.Result;
}

async Task<bool> RegisterWithCCTHelper(string serverUri)
{
    bool result = false;
    socket = new StreamWebSocket();

    // Specify the keepalive interval expected by the server for this app
    // in order of minutes.
    const int serverKeepAliveInterval = 30;

    // Specify the channelId string to differentiate this
    // channel instance from any other channel instance.
    // When background task fires, the channel object is provided
    // as context and the channel id can be used to adapt the behavior
    // of the app as required.
    const string channelId = "channelOne";

    // For websockets, the system does the keepalive on behalf of the app
    // But the app still needs to specify this well known keepalive task.
    // This should be done here in the background registration as well
    // as in the package manifest.
    const string WebSocketKeepAliveTask = "Windows.Networking.Sockets.WebSocketKeepAlive";

    // Try creating the controlchanneltrigger if this has not been already
    // created and stored in the property bag.
    ControlChannelTriggerStatus status;

    // Create the ControlChannelTrigger object and request a hardware slot for this app.
    // If the app is not on LockScreen, then the ControlChannelTrigger constructor will
    // fail right away.
    try
    {
        channel = new ControlChannelTrigger(channelId, serverKeepAliveInterval,
                                   ControlChannelTriggerResourceType.RequestHardwareSlot);
    }
    catch (UnauthorizedAccessException exp)
    {
        Diag.DebugPrint("Is the app on lockscreen? " + exp.Message);
        return result;
    }

    Uri serverUriInstance;
    try
    {
        serverUriInstance = new Uri(serverUri);
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Error creating URI: " + exp.Message);
        return result;
    }

    // Register the apps background task with the trigger for keepalive.
    var keepAliveBuilder = new BackgroundTaskBuilder();
    keepAliveBuilder.Name = "KeepaliveTaskForChannelOne";
    keepAliveBuilder.TaskEntryPoint = WebSocketKeepAliveTask;
    keepAliveBuilder.SetTrigger(channel.KeepAliveTrigger);
    keepAliveBuilder.Register();

    // Register the apps background task with the trigger for push notification task.
    var pushNotifyBuilder = new BackgroundTaskBuilder();
    pushNotifyBuilder.Name = "PushNotificationTaskForChannelOne";
    pushNotifyBuilder.TaskEntryPoint = "Background.PushNotifyTask";
    pushNotifyBuilder.SetTrigger(channel.PushNotificationTrigger);
    pushNotifyBuilder.Register();

    // Tie the transport method to the ControlChannelTrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the ControlChannelTrigger object can be reused to plug in a new transport by
    // calling UsingTransport API again.
    try
    {
        channel.UsingTransport(socket);

        // Connect the socket
        //
        // If connect fails or times out it will throw exception.
        // ConnectAsync can also fail if hardware slot was requested
        // but none are available
        await socket.ConnectAsync(serverUriInstance);

        // Call WaitForPushEnabled API to make sure the TCP connection has
        // been established, which will mean that the OS will have allocated
        // any hardware slot for this TCP connection.
        //
        // In this sample, the ControlChannelTrigger object was created by
        // explicitly requesting a hardware slot.
        //
        // On systems that without connected standby, if app requests hardware slot as above,
        // the system will fallback to a software slot automatically.
        //
        // On systems that support connected standby,, if no hardware slot is available, then app
        // can request a software slot by re-creating the ControlChannelTrigger object.
        status = channel.WaitForPushEnabled();
        if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
            && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
        {
            throw new Exception(string.Format("Neither hardware nor software slot could be allocated. ChannelStatus is {0}", status.ToString()));
        }

        // Store the objects created in the property bag for later use.
        CoreApplication.Properties.Remove(channel.ControlChannelTriggerId);

        var appContext = new AppContext(this, socket, channel, channel.ControlChannelTriggerId);
        ((IDictionary<string, object>)CoreApplication.Properties).Add(channel.ControlChannelTriggerId, appContext);
        result = true;

        // Almost done. Post a read since we are using streamwebsocket
        // to allow push notifications to be received.
        PostSocketRead(MAX_BUFFER_LENGTH);
    }
    catch (Exception exp)
    {
         Diag.DebugPrint("RegisterWithCCTHelper Task failed with: " + exp.Message);

         // Exceptions may be thrown for example if the application has not
         // registered the background task class id for using real time communications
         // broker in the package manifest.
    }
    return result
}
```

Pour plus d’informations sur l’utilisation de [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), voir [l’exemple StreamWebSocket ControlChannelTrigger](http://go.microsoft.com/fwlink/p/?linkid=251232).

## <a name="controlchanneltrigger-with-httpclient"></a>ControlChannelTrigger avec HttpClient
Certaines considérations spéciales s’appliquent lors de l’utilisation de [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Certaines meilleures pratiques et certains modèles d’utilisation spécifiques au transport doivent être respectés lors de l’utilisation d’un [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) avec **ControlChannelTrigger**. De plus, ces considérations affectent la manière dont les demandes de réception de paquets sur le [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) sont gérées.

**Remarque** [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) utilisant SSL n'est pas actuellement pris en charge à l’aide de la fonctionnalité de déclencheur réseau et [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).
 
Les meilleures pratiques et modèles d’utilisation suivants doivent être respectés lors de l’utilisation de [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) :

-   Il est possible que l’application doive définir différents en-têtes et propriétés sur l’objet [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) ou [HttpClientHandler](http://go.microsoft.com/fwlink/p/?linkid=241638) dans l’espace de noms [System.Net.Http](http://go.microsoft.com/fwlink/p/?linkid=227894) avant d’envoyer la demande à l’URI spécifique.
-   Il est possible qu’une application doive effectuer une demande initiale pour tester et configurer le transport correctement avant de créer le transport [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) à utiliser avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Une fois que l’application détermine que le transport peut être correctement configuré, un objet [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) peut être configuré comme objet de transport utilisé avec l’objet **ControlChannelTrigger**. Ce processus est conçu pour empêcher certains scénarios d’interrompre la connexion établie sur le transport. En utilisant SSL avec un certificat, une application peut requérir l’affichage d’un dialogue pour la saisie du code confidentiel ou s’il est nécessaire de choisir parmi plusieurs certificats. L’authentification proxy et l’authentification du serveur peuvent être requises. Si l’authentification proxy ou l’authentification du serveur expire, la connexion peut être fermée. Une façon pour l’application de traiter ces problèmes d’expiration d’authentification consiste à définir un minuteur. Quand une redirection HTTP est requise, il n’est pas certain que la seconde connexion puisse être établie de manière fiable. Une demande de test initiale garantit que l’application peut utiliser l’URL redirigée la plus à jour avant d’utiliser l’objet [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) comme transport avec l’objet **ControlChannelTrigger**.

Contrairement aux autres transports réseau, l’objet [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) ne peut pas être passé directement dans la méthode [**UsingTransport**](https://msdn.microsoft.com/library/windows/apps/hh701175) de l’objet [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Au lieu de cela, un objet [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) doit être construit spécialement pour une utilisation avec l’objet [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) et le **ControlChannelTrigger**. L’objet [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) est créé à l’aide de la méthode [RtcRequestFactory.Create](http://go.microsoft.com/fwlink/p/?linkid=259154). L’objet [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) créé est ensuite passé à la méthode **UsingTransport**.

L’exemple suivant montre comment construire un objet [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) utilisable avec l’objet [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) et le [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

public HttpRequestMessage httpRequest;
public HttpClient httpClient;
public HttpRequestMessage httpRequest;
public ControlChannelTrigger channel;
public Uri serverUri;

private void SetupHttpRequestAndSendToHttpServer()
{
    try
    {
        // For HTTP based transports that use the RTC broker, whenever we send next request, we will abort the earlier
        // outstanding http request and start new one.
        // For example in case when http server is taking longer to reply, and keep alive trigger is fired in-between
        // then keep alive task will abort outstanding http request and start a new request which should be finished
        // before next keep alive task is triggered.
        if (httpRequest != null)
        {
            httpRequest.Dispose();
        }

        httpRequest = RtcRequestFactory.Create(HttpMethod.Get, serverUri);

        SendHttpRequest();
    }
        catch (Exception e)
    {
        Diag.DebugPrint("Connect failed with: " + e.ToString());
        throw;
    }
}
```

Certaines considérations spéciales affectent la manière dont sont gérées les demandes d’envoi de demandes HTTP sur le [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) pour initier la réception d’une réponse. En particulier, lors de l’utilisation d’un [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) avec le [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), votre application doit utiliser un objet Task pour la gestion des envois plutôt que le modèle **await**.

En cas d’utilisation de [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637), il n’y a pas de synchronisation avec la méthode [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) sur la tâche en arrière-plan pour le [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) avec le retour du rappel d’achèvement de réception. Pour cette raison, l’application peut uniquement appliquer la technique HttpResponseMessage bloquante dans la méthode**Run** et attendre que la réponse entière ait été reçue.

L’utilisation de [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) diffère sensiblement des transports [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882), [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923). Le rappel de réception [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) est délivré à l’application via un Task à partir du code [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637). Cela signifie que la tâche de notification Push **ControlChannelTrigger** est déclenchée dès que les données ou l’erreur sont distribuées à l’application. Dans l’exemple ci-dessous, le code stocke le responseTask renvoyé par la méthode [HttpClient.SendAsync](http://go.microsoft.com/fwlink/p/?linkid=241637) dans le stockage global qui sera sélectionné et traité inline par la tâche de notification Push.

L’exemple suivant montre comment gérer les demandes d’envoi sur le [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) en cas d’utilisation avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

private void SendHttpRequest()
{
    if (httpRequest == null)
    {
        throw new Exception("HttpRequest object is null");
    }

    // Tie the transport method to the controlchanneltrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the controlchanneltrigger object can be reused to plugin a new transport by
    // calling UsingTransport API again.
    channel.UsingTransport(httpRequest);

    // Call the SendAsync function to kick start the TCP connection establishment
    // process for this http request.
    Task<HttpResponseMessage> httpResponseTask = httpClient.SendAsync(httpRequest);

    // Call WaitForPushEnabled API to make sure the TCP connection has been established,
    // which will mean that the OS will have allocated any hardware slot for this TCP connection.
    ControlChannelTriggerStatus status = channel.WaitForPushEnabled();
    Diag.DebugPrint("WaitForPushEnabled() completed with status: " + status);
    if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
        && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
    {
        throw new Exception("Hardware/Software slot not allocated");
    }

    // The HttpClient receive callback is delivered via a Task to the app.
    // The notification task will fire as soon as the data or error is dispatched
    // Enqueue the responseTask returned by httpClient.sendAsync
    // into a queue that the push notify task will pick up and process inline.
    AppContext.messageQueue.Enqueue(httpResponseTask);
}
```

L’exemple suivant montre comment lire les réponses reçues sur le [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) en cas d’utilisation avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).

```csharp
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

public string ReadResponse(Task<HttpResponseMessage> httpResponseTask)
{
    string message = null;
    try
    {
        if (httpResponseTask.IsCanceled || httpResponseTask.IsFaulted)
        {
            Diag.DebugPrint("Task is cancelled or has failed");
            return message;
        }
        // We' ll wait until we got the whole response.
        // This is the only supported scenario for HttpClient for ControlChannelTrigger.
        HttpResponseMessage httpResponse = httpResponseTask.Result;
        if (httpResponse == null || httpResponse.Content == null)
        {
            Diag.DebugPrint("Cannot read from httpresponse, as either httpResponse or its content is null. try to reset connection.");
        }
        else
        {
            // This is likely being processed in the context of a background task and so
            // synchronously read the Content' s results inline so that the Toast can be shown.
            // before we exit the Run method.
            message = httpResponse.Content.ReadAsStringAsync().Result;
        }
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Failed to read from httpresponse with error:  " + exp.ToString());
    }
    return message;
}
```

Pour plus d’informations sur l’utilisation de [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), voir l’[exemple HttpClient ControlChannelTrigger](http://go.microsoft.com/fwlink/p/?linkid=258323).

## <a name="controlchanneltrigger-with-ixmlhttprequest2"></a>ControlChannelTrigger avec IXMLHttpRequest2
Certaines considérations spéciales s’appliquent lors de l’utilisation de [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Certaines meilleures pratiques et certains modèles d’utilisation spécifiques au transport doivent être respectés lors de l’utilisation d’un **IXMLHTTPRequest2** avec **ControlChannelTrigger**. L’utilisation de **ControlChannelTrigger** n’affecte pas la manière dont sont gérées les demandes d’envoi ou de réception de demandes HTTP sur le **IXMLHTTPRequest2**.

Meilleures pratiques et modèles d’utilisation à appliquer lors de l’utilisation de [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)

-   Un objet [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151), quand il est utilisé comme transport, a une durée de vie limitée à une demande/réponse. Quand il est utilisé avec l’objet [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), il est pratique de créer et de configurer l’objet **ControlChannelTrigger** une fois, puis d’appeler la méthode [**UsingTransport**](https://msdn.microsoft.com/library/windows/apps/hh701175) à plusieurs reprises, en associant chaque fois un nouvel objet **IXMLHTTPRequest2**. Une application doit supprimer l’objet **IXMLHTTPRequest2** précédent avant de fournir un nouvel objet **IXMLHTTPRequest2**, afin de s’assurer que l’application ne dépasse pas les limites d’allocation de ressources.
-   Il est possible que l’application doive appeler les méthodes [**SetProperty**](https://msdn.microsoft.com/library/windows/desktop/hh831167) et [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/desktop/hh831168) pour configurer le transport HTTP avant d’appeler la méthode [**Send**](https://msdn.microsoft.com/library/windows/desktop/hh831164).
-   Il est possible qu’une application doive effectuer une demande [**Send**](https://msdn.microsoft.com/library/windows/desktop/hh831164) initiale pour tester et configurer le transport correctement avant de créer le transport à utiliser avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Une fois que l’application a déterminé que le transport était correctement configuré, l’objet [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) peut être configuré comme objet de transport utilisé avec **ControlChannelTrigger**. Ce processus est conçu pour empêcher certains scénarios d’interrompre la connexion établie sur le transport. En utilisant SSL avec un certificat, une application peut requérir l’affichage d’un dialogue pour la saisie du code confidentiel ou s’il est nécessaire de choisir parmi plusieurs certificats. L’authentification proxy et l’authentification du serveur peuvent être requises. Si l’authentification proxy ou l’authentification du serveur expire, la connexion peut être fermée. Une façon pour l’application de traiter ces problèmes d’expiration d’authentification consiste à définir un minuteur. Quand une redirection HTTP est requise, il n’est pas certain que la seconde connexion puisse être établie de manière fiable. Une demande de test initiale garantit que l’application peut utiliser l’URL redirigée la plus à jour avant d’utiliser l’objet **IXMLHTTPRequest2** comme transport avec l’objet **ControlChannelTrigger**.

Pour plus d’informations sur l’utilisation de [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) avec [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), consultez l’[Exemple ControlChannelTrigger avec IXMLHTTPRequest2](http://go.microsoft.com/fwlink/p/?linkid=258538).

## <a name="important-apis"></a>API importantes
* [SocketActivityTrigger](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)
* [ControlChannelTrigger](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)
