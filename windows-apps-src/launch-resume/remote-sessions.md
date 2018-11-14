---
author: PatrickFarley
title: Connecter des appareils par le biais de sessions à distance
description: Créez des expériences partagées sur plusieurs appareils en les rejoignant dans une session à distance.
ms.assetid: 1c8dba9f-c933-4e85-829e-13ad784dd3e2
ms.author: pafarley
ms.date: 06/28/2017
ms.topic: article
keywords: les appareils Windows 10, uwp, connectés, systèmes distants, rome, le projet «Rome»
ms.localizationpriority: medium
ms.openlocfilehash: 0606c3b1b0b36dadb888166310f8b347d40a480a
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6262334"
---
# <a name="connect-devices-through-remote-sessions"></a>Connecter des appareils par le biais de sessions à distance

La fonctionnalité Sessions à distance permet à une application de se connecter à d’autres appareils via une session, que ce soit pour une messagerie d’application explicite ou pour un échange réparti de données gérées par le système, comme le **[SpatialEntityStore](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialentitystore)** pour le partage holographique entre des appareils Windows Holographic.

Les sessions à distance peuvent être créées par n’importe quel appareil Windows, et n’importe quel appareil Windows peut demander à y participer (même si les sessions peuvent avoir une visibilité sur invitation uniquement), y compris les appareils connectés par d’autres utilisateurs. Ce guide fournit un exemple de code de base pour tous les principaux scénarios qui utilisent des sessions à distance. Ce code peut être intégré à un projet d’application existant et modifié en fonction des besoins. Pour une implémentation de bout en bout, voir l’[Exemple d’application de jeu de quiz](https://github.com/microsoft/Windows-appsample-remote-system-sessions).

## <a name="preliminary-setup"></a>Configuration préliminaire

### <a name="add-the-remotesystem-capability"></a>Ajouter la fonctionnalité remoteSystem

Pour que votre application puisse lancer une application sur un appareil distant, vous devez ajouter la fonctionnalité `remoteSystem` dans le manifeste de votre package d’application. Vous pouvez l’ajouter avec le concepteur de manifeste de package en sélectionnant **Système distant** dans l’onglet **Capacités**, ou manuellement en ajoutant la ligne suivante dans le fichier _Package.appxmanifest_ de votre projet.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-user-discovering-on-the-device"></a>Activer la détection entre utilisateurs sur l’appareil
La fonctionnalité Sessions à distance est destinée à connecter plusieurs utilisateurs différents, c’est pourquoi le partage entre utilisateurs doit être activé sur les appareils concernés. Il s’agit d’un paramètre système qu’il est possible d’interroger avec une méthode statique dans la classe **RemoteSystem**:

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Everyone nearby".
}
```

Pour modifier ce paramètre, l’utilisateur doit ouvrir l’application **paramètres**. Dans le menu **Système** > **Expériences partagées** > **Partager avec d’autres appareils**, une liste déroulante permet à l’utilisateur d’indiquer les appareils avec lesquels leur système peut partager des contenus.

![Page de paramètres Expériences partagées](images/shared-experiences-settings.png)

### <a name="include-the-necessary-namespaces"></a>Inclure les espaces de noms nécessaires
Pour utiliser tous les extraits de code dans ce guide, vous aurez besoin des déclarations `using` suivantes dans votre ou vos fichiers de classe.

```csharp
using System.Runtime.Serialization.Json;
using Windows.Foundation.Collections;
using Windows.System.RemoteSystems;
```

## <a name="create-a-remote-session"></a>Créer une session à distance

Pour créer une instance de session à distance, vous devez commencer avec un objet **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)**. Utilisez la structure suivante pour créer une nouvelle session et gérer les demandes de participation à partir d’autres appareils.

```csharp
public async void CreateSession() {
    
    // create a session controller
    RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob’s Minecraft game");
    
    // register the following code to handle the JoinRequested event
    manager.JoinRequested += async (sender, args) => {
        // Get the deferral
        var deferral = args.GetDeferral();
        
        // display the participant (args.JoinRequest.Participant) on UI, giving the 
        // user an opportunity to respond
        // ...
        
        // If the user chooses "accept", accept this remote system as a participant
        args.JoinRequest.Accept();
    };
    
    // create and start the session
    RemoteSystemSessionCreationResult createResult = await manager.CreateSessionAsync();
    
    // handle the creation result
    if (createResult.Status == RemoteSystemSessionCreationStatus.Success) {
        // creation was successful, get a reference to the session
        RemoteSystemSession currentSession = createResult.Session;
        
        // optionally subscribe to the disconnection event
        currentSession.Disconnected += async (sender, args) => {
            // update the UI, using args.Reason
            //...
        };
    
        // Use session (see later section)
        //...
    
    } else if (createResult.Status == RemoteSystemSessionCreationStatus.SessionLimitsExceeded) {
        // creation failed. Optionally update UI to indicate that there are too many sessions in progress
    } else {
        // creation failed for an unknown reason. Optionally update UI
    }
}
```

### <a name="make-a-remote-session-invite-only"></a>Définir une session à distance sur invitation uniquement

Si vous souhaitez empêcher que votre session à distance soit détectable publiquement, vous pouvez la définir comme étant sur invitation uniquement. Seuls les appareils qui recevront une invitation seront en mesure d’envoyer des demandes de participation. 

La procédure est quasiment identique à celle indiquée ci-dessus, mais lorsque vous créerez l’instance **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)**, vous transmettrez un objet **[RemoteSystemSessionOptions](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.RemoteSystemSessionOptions)** configuré.

```csharp
// define the session options with the invite-only designation
RemoteSystemSessionOptions sessionOptions = new RemoteSystemSessionOptions();
sessionOptions.IsInviteOnly = true;

// create the session controller
RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob's Minecraft game", sessionOptions);

//...
```

Pour envoyer une invitation, vous devez avoir une référence au système distant de réception (acquise via la détection normale du système distant). Transmettez simplement cette référence dans la méthode **[SendInvitationAsync](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession.sendinvitationasync)** de l’objet de session. Tous les participants à une session ont une référence à la session à distance (voir section suivante), de sorte que tout participant peut envoyer une invitation.

```csharp
// "currentSession" is a reference to a RemoteSystemSession.
// "guestSystem" is a previously discovered RemoteSystem instance
currentSession.SendInvitationAsync(guestSystem); 
```

## <a name="discover-and-join-a-remote-session"></a>Détecter et rejoindre une session à distance

Le processus de détection des sessions à distance est géré par la classe **[RemoteSystemSessionWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionwatcher)** et est similaire à la détection de systèmes distants individuels.

```csharp
public void DiscoverSessions() {
    
    // create a watcher for remote system sessions
    RemoteSystemSessionWatcher sessionWatcher = RemoteSystemSession.CreateWatcher();
    
    // register a handler for the "added" event
    sessionWatcher.Added += async (sender, args) => {
        
        // get a reference to the info about the discovered session
        RemoteSystemSessionInfo sessionInfo = args.SessionInfo;
        
        // Optionally update the UI with the sessionInfo.DisplayName and 
        // sessionInfo.ControllerDisplayName strings. 
        // Save a reference to this RemoteSystemSessionInfo to use when the
        // user selects this session from the UI
        //...
    };
    
    // Begin watching
    sessionWatcher.Start();
}
```

Lorsqu’une instance **[RemoteSystemSessionInfo](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioninfo)** est obtenue, elle peut être utilisée pour émettre une demande de participation auprès de l’appareil qui contrôle la session correspondante. Une demande de participation renverra de façon asynchrone un objet **[RemoteSystemSessionJoinResult](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionjoinresult)** contenant une référence à la session rejointe.

```csharp
public async void JoinSession(RemoteSystemSessionInfo sessionInfo) {

    // issue a join request and wait for result.
    RemoteSystemSessionJoinResult joinResult = await sessionInfo.JoinAsync();
    if (joinResult.Status == RemoteSystemSessionJoinStatus.Success) {
        // Join request was approved

        // RemoteSystemSession instance "currentSession" was declared at class level.
        // Assign the value obtained from the join result.
        currentSession = joinResult.Session;
        
        // note connection and register to handle disconnection event
        bool isConnected = true;
        currentSession.Disconnected += async (sender, args) => {
            isConnected = false;

            // update the UI with args.Reason value
        };
        
        if (isConnected) {
            // optionally use the session here (see next section)
            //...
        }
    } else {
        // Join was unsuccessful.
        // Update the UI, using joinResult.Status value to show cause of failure.
    }
}
```

Un appareil peut être joint à plusieurs sessions en même temps. Pour cette raison, il peut être souhaitable de séparer la fonctionnalité de jonction de l’interaction réelle avec chaque session. Tant qu’une référence à l’instance **[RemoteSystemSession](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession)** est conservée dans l’application, une communication peut être tentée sur cette session.

## <a name="share-messages-and-data-through-a-remote-session"></a>Partager des messages et des données par le biais d’une session à distance

### <a name="receive-messages"></a>Recevoir des messages

Vous pouvez échanger des messages et des données avec d’autres appareils participant à la session en utilisant une instance **[RemoteSystemSessionMessageChannel](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionmessagechannel)**, qui représente un canal de communication unique au niveau de la session. Dès qu’il est initialisé, il commence à écouter les messages entrants.

>[!NOTE]
>Les messages doivent être sérialisés et désérialisés à partir de tableaux d’octets lors de l’envoi et de la réception. Cette fonctionnalité est incluse dans les exemples suivants, mais elle peut être implémentée séparément pour une modularité de code optimale. Voir l'[Example d'application](https://github.com/microsoft/Windows-appsample-remote-system-sessions)) pour obtenir un exemple sur ce point.

```csharp
public async void StartReceivingMessages() {
    
    // Initialize. The channel name must be known by all participant devices 
    // that will communicate over it.
    RemoteSystemSessionMessageChannel messageChannel = new RemoteSystemSessionMessageChannel(currentSession, 
        "Everyone in Bob's Minecraft game", 
        RemoteSystemSessionMessageChannelReliability.Reliable);
    
    // write the handler for incoming messages on this channel
    messageChannel.ValueSetReceived += async (sender, args) => {
        
        // Update UI: a message was received from the participant args.Sender
        
        // Deserialize the message 
        // (this app must know what key to use and what object type the value is expected to be)
        ValueSet receivedMessage = args.Message;
        object rawData = receivedMessage["appKey"]);
        object value = new ExpectedType(); // this must be whatever type is expected

        using (var stream = new MemoryStream((byte[])rawData)) {
            value = new DataContractJsonSerializer(value.GetType()).ReadObject(stream);
        }
        
        // do something with the "value" object
        //...
    };
}
```

### <a name="send-messages"></a>Envoyer des messages

Lorsque le canal est établi, l’envoi d’un message à tous les participants de la session est simple.

```csharp
public async void SendMessageToAllParticipantsAsync(RemoteSystemSessionMessageChannel messageChannel, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }
    
    // Send message to all participants. Ordering is not guaranteed.
    await messageChannel.BroadcastValueSetAsync(message);
}
```

Pour envoyer un message à certains participants uniquement, vous devez tout d’abord lancer un processus de détection afin d’acquérir les références aux systèmes distants participant à la session. Cela est similaire au processus de détection des systèmes distants en dehors d’une session. Utilisez une instance **[RemoteSystemSessionParticipantWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionparticipantwatcher)** pour rechercher les appareils participant à une session.

```csharp
public void WatchForParticipants() {
    // "currentSession" is a reference to a RemoteSystemSession.
    RemoteSystemSessionParticipantWatcher watcher = currentSession.CreateParticipantWatcher();

    watcher.Added += (sender, participant) => {
        // save a reference to "participant"
        // optionally update UI
    };   

    watcher.Removed += (sender, participant) => {
        // remove reference to "participant"
        // optionally update UI
    };

    watcher.EnumerationCompleted += (sender, args) => {
        // Apps can delay data model render up until this point if they wish.
    };

    // Begin watching for session participants
    watcher.Start();
}
```

Lorsque vous obtenez une liste de références aux participants de la session, vous pouvez envoyer un message à n’importe quel groupe de ces participants.

Pour envoyer un message à un seul participant (dans l’idéal, sélectionné à l’écran par l’utilisateur), transmettez simplement la référence dans une méthode comme celle qui suit.

```csharp
public async void SendMessageToParticipantAsync(RemoteSystemSessionMessageChannel messageChannel, RemoteSystemSessionParticipant participant, object value) {
    
    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to the participant
    await messageChannel.SendValueSetAsync(message,participant);
}
```

Pour envoyer un message à plusieurs participants (dans l’idéal, sélectionnés à l’écran par l’utilisateur), ajoutez-les à un objet de liste et transmettez la liste dans une méthode comme celle qui suit.

```csharp
public async void SendMessageToListAsync(RemoteSystemSessionMessageChannel messageChannel, IReadOnlyList<RemoteSystemSessionParticipant> myTeam, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to specific participants. Ordering is not guaranteed.
    await messageChannel.SendValueSetToParticipantsAsync(message, myTeam);   
}
```

## <a name="related-topics"></a>Rubriques associées
* [Applications et appareils connectés (projet «Rome»)](connected-apps-and-devices.md)
* [Référence sur l’API Systèmes distants](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)
