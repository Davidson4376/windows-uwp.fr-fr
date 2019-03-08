---
title: Utilisez des Projections de WinRT 2 jeux de conversation
description: Découvrez comment utiliser Xbox Live Game Chat 2 avec les projections WinRT pour ajouter la communication vocale à votre jeu.
ms.date: 04/11/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, chat jeu 2, jeu chat, communication vocale
ms.localizationpriority: medium
ms.openlocfilehash: 1cb0578151d4262d61f5fbc078bebab721fb3bfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639164"
---
# <a name="using-game-chat-2-winrt-projections"></a>Utilisation de la conversation jeu 2 (Projections WinRT)

Il s’agit d’une brève procédure pas à pas sur l’utilisation de jeu Chat 2 C# API. Les développeurs de jeux souhaitant accéder au jeu Chat 2 via C++ devrait [2 de discuter de jeu à l’aide de](using-game-chat-2.md).

## <a name="prerequisites-a-nameprereq"></a>Conditions préalables <a name="prereq">

Afin de consommer un jeu Chat 2, vous devez inclure le [Microsoft.Xbox.Services.GameChat2 nuget package](https://www.nuget.org/packages/Microsoft.Xbox.Game.Chat.2.WinRT.UWP/).

Avant de commencer à coder avec jeu Chat 2, vous devez avoir configuré AppXManifest de votre application pour déclarer la fonctionnalité de l’appareil « microphone ». AppXManifest fonctionnalités sont décrites plus en détail dans leurs sections respectives de la documentation de la plateforme ; l’extrait suivant montre le nœud de capacité de périphérique « microphone » doit exister sous le nœud Package/fonctionnalités ou sinon conversation sera bloquée :

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

## <a name="initialization-a-nameinit"></a>Initialisation <a name="init">

Commencer à interagir avec la bibliothèque en instanciant un `GameChat2ChatManager` objet avec le nombre maximal d’utilisateurs de chat simultanées censé être ajouté à l’instance. Pour modifier cette valeur, le `GameChat2ChatManager` objet doit être supprimé et recréé avec la valeur souhaitée. Vous pouvez avoir uniquement une `GameChat2ChatManager` instancié à la fois.

```cs
GameChat2ChatManager myGameChat2ChatManager = new GameChat2ChatManager(<maxChatUserCount>);
```

Le `GameChat2ChatManager` objet a des propriétés supplémentaires, facultatifs qui peuvent être configurées à tout moment. L’exemple de code suivant part du principe que les variables recevra une valeur avant d’être utilisés pour définir les propriétés sur le `myGameChat2ChatManager` objet.

```cs
GameChat2SpeechToTextConversionMode mySpeechToTextConversionMode;
GameChat2SharedDeviceCommunicationRelationshipResolutionMode mySharedDeviceResolutionMode;
GameChat2CommunicationRelationship myDefaultLocalToRemoteCommunicationRelationship;
float myDefaultAudioRenderVolume;
GameChat2AudioEncodingTypeAndBitrate myAudioEncodingTypeAndBitRate;

...

myGameChat2ChatManager.SpeechToTextConversionMode = mySpeechToTextConversionMode;
myGameChat2ChatManager.SharedDeviceResolutionMode = mySharedDeviceResolutionMode;
myGameChat2ChatManager.DefaultLocalToRemoteCommunicationRelationship = myDefaultLocalToRemoteCommunicationRelationship;
myGameChat2ChatManager.DefaultAudioRenderVolume = myDefaultAudioRenderVolume;
myGameChat2ChatManager.AudioEncodingTypeAndBitRate = myAudioEncodingTypeAndBitRate;
```

## <a name="configuring-users-a-nameconfig"></a>Configurer les utilisateurs <a name="config">

Une fois que l’instance est initialisée, vous devez ajouter les utilisateurs locaux à l’instance GC2. Dans cet exemple, l’utilisateur A représentera un utilisateur local.

```cs
string userAXuid;

...

IGameChat2ChatUser userA = myGameChat2ChatManager.AddLocalUser(userAXuid);
```

Vous devez ensuite ajouter les utilisateurs à distance et les identificateurs qui sera utilisé pour représenter les « points de terminaison distants » qui se trouve sur chaque utilisateur distant. « Points de terminaison » sont représentés par les objets appartenant à l’application qui implémentent la `IGameChat2Endpoint` interface. L’exemple suivant montre un exemple d’implémentation de `MyEndpoint` classe qui implémente le `IGameChat2Endpoint`.

```cs
class MyEndpoint : IGameChat2Endpoint
{
    private uint endpointIdentifier;
    public MyEndpoint(uint identifier)
    {
        endpointIdentifier = identifier;
    }

    // Implementing IsSameEndpointAs, the only method on the IGameChat2Endpoint interface
    public bool IsSameEndpointAs(IGameChat2Endpoint gameChat2Endpoint)
    {
        return endpointIdentifier == ((MyEndpoint)gameChat2Endpoint).endpointIdentifier;
    }
}
```

Dans l’exemple de code suivant, les utilisateurs B, C et D sont des utilisateurs distants en cours d’ajout. L’utilisateur B est sur un périphérique distant et utilisateurs C et D sont sur un autre appareil à distance. Cet exemple suppose que les variables seront définies et que `myGameChat2ChatManager` est une instance de `GameChat2ChatManager` et que « MyEndpoint » est une classe qui implémente `IGameChat2Endpoint`.

```cs
string userBXuid;
string userCXuid;
string userDXuid;
MyEndpoint myRemoteEndpointOne;
MyEndpoint myRemoteEndpointTwo;

...

IGameChat2ChatUser remoteUserB = myGameChat2ChatManager.AddRemoteUser(userBXuid, myRemoteEndpointOne);
IGameChat2ChatUser remoteUserC = myGameChat2ChatManager.AddRemoteUser(userCXuid, myRemoteEndpointTwo);
IGameChat2ChatUser remoteUserD = myGameChat2ChatManager.AddRemoteUser(userDXuid, myRemoteEndpointTwo);
```

Maintenant, vous configurez la relation de communication entre chaque utilisateur distant et l’utilisateur local. Dans cet exemple, supposons que l’utilisateur A et B se trouvent sur la même équipe et la communication bidirectionnelle est autorisée. `GameChat2CommunicationRelationship.SendAndReceiveAll` est défini pour représenter la communication bidirectionnelle. Définissez la relation de l’utilisateur A à l’utilisateur B avec :

```cs
GameChat2ChatUserLocal localUserA = userA as GameChat2ChatUserLocal;
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

Supposons maintenant que les utilisateurs C et D sont « spectateurs » et doivent être autorisé à écouter à l’utilisateur A, mais pas parler. `GameChat2CommunicationRelationship.ReceiveAll` est défini cette communication unidirectionnelle. Définir les relations avec :

```cs
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.ReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.ReceiveAll);
```

Si, à tout moment, il existe des utilisateurs distants qui ont été ajoutés à la `GameChat2ChatManager` , mais elle n’ont pas été configuré pour communiquer avec les utilisateurs locaux - d’OK ! Cela est peut-être le cas dans les scénarios où les utilisateurs de déterminer les équipes ou peuvent modifier les canaux énonciation arbitrairement. Jeu 2 Chat met en cache uniquement les informations (par exemple, les relations de confidentialité et sa réputation) pour les utilisateurs qui ont été ajoutés à l’instance, ce qui est utile informer 2 de discuter du jeu de tous les utilisateurs possibles, même si elles ne peut pas parler à tous les utilisateurs locaux à un moment précis dans le temps.

Enfin, supposons que D de l’utilisateur a quitté le jeu et doivent être supprimé à partir de l’instance locale de jeu 2 Chat. Qui peut être fait avec l’appel suivant :

```cs
remoteUserD.Remove();
```

L’objet utilisateur est invalidé immédiatement lorsque `Remove()` est appelée. Le dernier état de l’utilisateur est mis en cache lorsqu’il est supprimé. Toutes les méthodes d’information appelés une fois que l’objet utilisateur est invalidé reflète l’état de l’utilisateur avant d’être supprimé. Autres méthodes lèveront une erreur lorsqu’elle est appelée.

## <a name="processing-data-frames-a-namedata"></a>Traitement des trames de données <a name="data">

Jeu 2 Chat n’a pas sa propre couche de transport ; Cela doit être fourni par l’application. Ce plug-in est géré par les appels de l’application régulière, à intervalles réguliers à le `GameChat2ChatManager.GetDataFrames()`. Cette méthode est comment jeu Chat 2 fournit des données sortantes à l’application. Il est conçu pour fonctionner rapidement de sorte qu’il peut être interrogé fréquemment sur un thread dédié mise en réseau. Cela fournit un emplacement pratique pour récupérer toutes les données en file d’attente sans vous soucier du caractère imprévisible de minutage de réseau ou de la complexité de rappel multi-thread.

Lorsque `GameChat2ChatManager.GetDataFrames()` est appelée, toutes les données en file d’attente sont signalées dans une liste de `IGameChat2DataFrame` objets. Applications doivent effectuer une itération sur la liste, inspecter la `TargetEndpointIdentifiers`et utiliser la couche d’application mise en réseau pour fournir les données vers les instances d’application distant approprié. Dans cet exemple, `HandleOutgoingDataFrame` est une fonction qui envoie les données dans le `Buffer` à chaque utilisateur sur chaque « Endpoint » spécifié dans le `TargetEndpointIdentifiers` conformément à la `TransportRequirement`.

```cs
IReadOnlyList<IGameChat2DataFrame> frames = myGameChat2ChatManager.GetDataFrames();
foreach (IGameChat2DataFrame dataFrame in frames)
{
    HandleOutgoingDataFrame(
        dataFrame.Buffer,
        dataFrame.TargetEndpointIdentifiers,
        dataFrame.TransportRequirement
        );
}
```

Plus fréquemment les trames de données sont traitées, plus la latence audio apparente à l’utilisateur final. L’audio est fusionné en trames de données de 40 ms ; Il s’agit de la période d’interrogation suggérée.

## <a name="processing-state-changes-a-namestate"></a>Traitement des modifications d’état <a name="state">

Jeu 2 Chat offre des mises à jour à l’application, telles que les messages texte reçu, via des appels de l’application régulière, à intervalles réguliers à le `GameChat2ChatManager.GetStateChanges()` (méthode). Il est conçu pour fonctionner rapidement tel qu’il puisse être appelé chaque bloc de graphique dans la boucle de rendu de l’interface utilisateur. Cela fournit un emplacement pratique pour récupérer toutes les modifications en file d’attente sans vous soucier du caractère imprévisible de minutage de réseau ou de la complexité de rappel multi-thread.

Lorsque `GameChat2ChatManager.GetStateChanges()` est appelée, toutes les mises à jour en file d’attente sont signalés dans une liste de `IGameChat2StateChange` objets. Les applications doivent :
1. Effectuer une itération sur la liste
2. Examiner la structure de base pour son type plus spécifique
3. Effectuer un cast de la structure de base correspondant plus type
4. Gérer cette mise à jour comme il convient. 

```cs
IReadOnlyList<IGameChat2StateChange> stateChanges = myGameChat2ChatManager.GetStateChanges();
foreach (IGameChat2StateChange stateChange in stateChanges)
{
    switch (stateChange.Type)
    {
        case GameChat2StateChangeType.CommunicationRelationshipAdjusterChanged:
        {
            MyHandleCommunicationRelationshipAdjusterChanged(stateChange as GameChat2CommunicationRelationshipAdjusterChangedStateChange);
            break;
        }
        case GameChat2StateChangeType.TextChatReceived:
        {
            MyHandleTextChatReceived(stateChange as GameChat2TextChatReceivedStateChange);
            break;
        }

        ...
    }
}
```

## <a name="text-chat-a-nametext"></a>Conversations textuelles <a name="text">

Pour envoyer les conversations textuelles, utilisez `GameChat2ChatUserLocal.SendChatText()`. Exemple :

```cs
localUserA.SendChatText("Hello");
```

Jeu 2 Chat générera une trame de données contenant le message ; les points de terminaison cible pour la trame de données seront celles associées aux utilisateurs qui ont été configurés pour recevoir le texte à partir de l’utilisateur local. Lorsque les données sont traitées par les points de terminaison distants, le message sera exposé un `GameChat2TextChatReceivedStateChange`. À l’instar de la conversation vocale, les restrictions de privilège et de confidentialité sont respectées pour les conversations textuelles. Si une paire d’utilisateurs ont été configurés pour autoriser les conversations de texte, mais les restrictions de privilège ou de confidentialité interdisent cette communication, le message est abandonné.

Prise en charge d’entrée de conversation de texte et l’affichage est requise pour l’accessibilité (consultez [accessibilité](#access) pour plus d’informations).

## <a name="accessibility-a-nameaccess"></a>Accessibilité <a name="access">

Prise en charge de la conversation de texte d’entrée et l’affichage est obligatoire. Entrée de texte est nécessaire car, même sur des plateformes ou game genres qui historiquement n’ont pas eu généralisée clavier physique utiliser, les utilisateurs peuvent configurer le système pour utiliser les technologies d’assistance synthèse vocale. De même, l’affichage de texte est requis, car les utilisateurs peuvent configurer le système pour utiliser la parole-texte. Ces préférences peuvent être détectées sur les utilisateurs locaux en vérifiant la `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` et `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` propriétés respectivement, et vous pouvez souhaiter activer conditionnellement des mécanismes de texte.

### <a name="text-to-speech"></a>Conversion de texte par synthèse vocale

Lorsqu’un utilisateur a activé, de synthèse vocale `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` sera 'true'. Lorsque cet état est détecté, l’application doit fournir une méthode d’entrée de texte. Une fois que vous avez l’entrée de texte fournie par un clavier réel ou virtuel, transmettez la chaîne à la `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` (méthode). Jeu 2 Chat détectera et synthétiser les données audio en fonction de la chaîne et les préférences de voix accessible de l’utilisateur. Exemple :

```cs
localUserA.SynthesizeTextToSpeech("Hello");
```

L’audio synthétisé dans le cadre de cette opération vont être transporté à tous les utilisateurs qui ont été configurés pour recevoir les données audio à partir de cet utilisateur local. Si `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` est appelée sur un utilisateur qui ne dispose pas de n’entreprendre aucune action de synthèse vocale sera jeu Chat 2 est activée.

### <a name="speech-to-text"></a>Parole-texte

Lorsqu’un utilisateur a activé, parole-texte `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` aura la valeur true. Lorsque cet état est détecté, l’application doit être prêt à fournir que l’interface utilisateur associés aux messages de conversation retranscrits. GC sera automatiquement transcription audio de chaque utilisateur distant et l’exposer via un `GameChat2TranscribedChatReceivedStateChange`.

### <a name="speech-to-text-performance-considerations"></a>Considérations sur les performances de la parole-texte

Lors de la reconnaissance vocale est activée, l’instance 2 de discuter de jeu sur chaque appareil à distance initie une connexion de socket web avec le point de terminaison de services de reconnaissance vocale. Chaque client distant de jeu Chat 2 charge audio sur le point de terminaison de services de reconnaissance vocale via ce websocket ; le point de terminaison de services de reconnaissance vocale retourne parfois un message de la transcription à l’appareil distant. L’appareil distant envoie ensuite le message de transcription (autrement dit, un message texte) sur l’appareil local, où le message retranscrits est donné par jeu Chat 2 à l’application à restituer.

Par conséquent, le coût de performances principal de la parole-texte est l’utilisation du réseau. La plupart du trafic réseau est le téléchargement de l’audio encodé. Le websocket télécharge audio qui a déjà été encodée par jeu Chat 2 dans le chemin d’accès de la conversation vocale « normal » ; l’application a un contrôle sur la vitesse de transmission via `GameChat2ChatManager.AudioEncodingTypeAndBitrate`.

## <a name="ui-a-nameui"></a>INTERFACE UTILISATEUR <a name="UI">

Il est recommandé que n’importe où joueurs sont affichés, en particulier dans une liste d’identités comme un tableau des scores, également afficher Muet/à propos des icônes en tant que commentaires de l’utilisateur. Le `IGameChat2ChatUser.ChatIndicator` propriété représente l’état en cours, instantanée de conversation pour ce lecteur. L’exemple suivant illustre la récupération de la valeur d’indicateur pour un `IGameChat2ChatUser` objet vers lequel pointe la variable « UtilisateurA » pour déterminer une valeur de constante icône particulière pour attribuer à une variable 'iconToShow' :

```cs
switch (userA.ChatIndicator)
{
    case GameChat2UserChatIndicator.Silent:
    {
        iconToShow = Icon_InactiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.Talking:
    {
        iconToShow = Icon_ActiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.LocalMicrophoneMuted:
    {
        iconToShow = Icon_MutedSpeaker;
        break;
    }
    
    ...
}
```

La valeur de `IGameChat2ChatUser.ChatIndicator` est amené à changer fréquemment en tant que lecteurs start et stop parlé, par exemple. Il est conçu pour prendre en charge les applications interrogent chaque frame d’interface utilisateur en conséquence.

## <a name="muting-a-namemute"></a>Désactivation <a name="mute">

Le `GameChat2ChatUserLocal.MicrophoneMuted` propriété peut être utilisée pour activer/désactiver l’état muet du microphone d’un utilisateur local. Lorsque le microphone est coupé, sans contenu audio à partir de ce microphone est capturé. Si l’utilisateur se trouve sur un appareil partagé, telles que Kinect, l’état muet s’applique à tous les utilisateurs. Cette méthode ne reflète pas un contrôle de bouton Muet du matériel (par exemple, un bouton sur casque de l’utilisateur). Il n’existe aucune méthode pour récupérer l’état muet du matériel de périphérique audio d’un utilisateur à travers le jeu Chat 2.

Le `GameChat2ChatUserLocal.SetRemoteUserMuted()` méthode peut être utilisée pour activer/désactiver l’état de désactivation d’un utilisateur distant par rapport à un utilisateur local particulier. Lorsque l’utilisateur distant est coupé, l’utilisateur local ne sont pas entendre des données audio ou recevoir des messages texte à partir de l’utilisateur distant.

## <a name="bad-reputation-auto-mute-a-nameautomute"></a>Mauvaise réputation automatique-muet <a name="automute">

En général, les utilisateurs distants commenceront unmuted. Jeu 2 Chat démarre les utilisateurs dans un état muet lorsque (1) l’utilisateur distant n’est pas amis avec l’utilisateur local, et (2) l’utilisateur distant a un indicateur de mauvaise réputation. Lorsque les utilisateurs sont ignorés en raison de cette opération, `IGameChat2ChatUser.ChatIndicator` retournera `GameChat2UserChatIndicator.ReputationRestricted`. Cet état sera remplacé par le premier appel à `GameChat2ChatUserLocal.SetRemoteUserMuted()` qui inclut l’utilisateur distant en tant que l’utilisateur cible.

## <a name="privilege-and-privacy-a-namepriv"></a>Privilège et confidentialité <a name="priv">

En haut de la relation de communication configurée par le jeu, jeux 2 Chat applique les restrictions de privilège et de confidentialité. Jeu 2 Chat effectue des recherches de restriction de privilège et de confidentialité lors de l’ajout d’un utilisateur ; l’utilisateur `IGameChat2ChatUser.ChatIndicator` retournera toujours `GameChat2UserChatIndicator.Silent` jusqu'à ce que ces opérations terminées. Si la communication avec un utilisateur est affectée par un privilège ou confidentialité restriction, l’utilisateur `IGameChat2ChatUser.ChatIndicator` retournera `GameChat2UserChatIndicator.PlatformRestricted`. Restrictions de communication de plate-forme s’appliquent aux conversations vocales et de texte ; Il ne sera jamais y avoir une instance où conversations textuelles sont bloquée par une restriction de la plateforme, mais la conversation vocale n’est pas, ou vice versa.

`GameChat2ChatUserLocal.GetEffectiveCommunicationRelationship()` peut être utilisé pour aider à distinguer lorsque les utilisateurs ne peuvent pas communiquer en raison de privilèges incomplet et les opérations de confidentialité. Elle retourne la relation de communication appliquée par jeu Chat 2 sous la forme de `GameChat2CommunicationRelationship` et la raison la relation ne peut pas être égale à la relation configurée sous la forme de `GameChat2CommunicationRelationshipAdjuster`. Par exemple, si les opérations de recherche sont toujours en cours, le `GameChat2CommunicationRelationshipAdjuster` sera `GameChat2CommunicationRelationshipAdjuster.Initializing`. Cette méthode est censée être utilisée dans les scénarios de développement et débogage ; Il ne doit pas être utilisé pour influencer l’interface utilisateur (consultez [l’interface utilisateur](#UI)).

## <a name="cleanup-a-namecleanup"></a>Nettoyage <a name="cleanup">

Lorsque l’application n’a plus besoin des communications via jeu Chat 2, vous devez appeler `GameChat2ChatManager.Dispose()`. Cela permet de jeu Chat 2 libérer des ressources qui ont été alloués pour gérer les communications.

## <a name="how-to-configure-popular-scenarios"></a>Comment configurer des scénarios courants

### <a name="push-to-talk"></a>Appuyer pour parler

Appuyer pour parler doit être implémenté avec `GameChat2ChatUserLocal.MicrophoneMuted`. Définissez `MicrophoneMuted` sur false pour autoriser la reconnaissance vocale et `MicrophoneMuted` comme « true » pour le limiter. Cette modification de propriété fournira la réponse de latence la plus basse à partir de jeu Chat 2.

### <a name="teams"></a>Équipes

Supposons que l’utilisateur A et B sont sur l’équipe Blue et utilisateur C et D de l’utilisateur se trouvent sur équipe rouge. Chaque utilisateur est dans une instance unique de l’application.

Sur l’appareil de l’utilisateur A :

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

Sur l’appareil de l’utilisateur B :

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

Sur l’appareil de l’utilisateur C :

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

Sur l’appareil de l’utilisateur D :

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

### <a name="broadcast"></a>Diffusion

Supposons que l’utilisateur A est le leader en donnant des commandes et les utilisateurs B, C et D peuvent écouter uniquement. Chaque lecteur est sur un appareil unique.

Sur l’appareil de l’utilisateur A :

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAll);
```

Sur l’appareil de l’utilisateur B :

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

Sur l’appareil de l’utilisateur C :

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

Sur l’appareil de l’utilisateur D :

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
```
