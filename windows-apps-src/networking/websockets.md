---
author: DelfCo
description: "Les WebSockets fournissent un mécanisme de communication bidirectionnelle sécurisée et rapide entre un client et un serveur sur le web à l’aide du protocole HTTP(S)."
title: WebSockets
ms.assetid: EAA9CB3E-6A3A-4C13-9636-CCD3DE46E7E2
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ff2429e1e9ea56c414978c126497551b1e1864b8

---

# WebSockets

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**API importantes**

-   [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)
-   [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)

Les WebSockets fournissent un mécanisme de communication bidirectionnelle sécurisée et rapide entre un client et un serveur sur le web à l’aide du protocole HTTP(S).

Avec le [protocole WebSocket](http://tools.ietf.org/html/rfc6455), les données sont transférées immédiatement via une connexion duplex intégral à un seul socket, ce qui permet d’échanger des messages entre les deux points de terminaison en temps réel. Les WebSockets conviennent parfaitement pour les jeux en temps réel, où les notifications instantanées de réseau social et les affichages actualisés d’informations (statistiques de jeu) doivent être sécurisés et utiliser un transfert de données rapide. Les développeurs pour la plateforme Windows universelle (UWP) peuvent utiliser les classes [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) et [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) pour se connecter à des serveurs qui prennent en charge le protocole Websocket.

| MessageWebSocket                                                         | StreamWebSocket                                                                               |
|--------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| Approprié pour les situations classiques où les messages ne sont pas très volumineux.   | Approprié pour les situations dans lesquelles des fichiers volumineux (tels que des photos ou vidéos) sont transférés. |
| Active la notification indiquant qu’un message WebSocket entier a été reçu. | Permet la lecture de sections d’un message avec chaque opération de lecture.                             |
| Prend en charge à la fois UTF-8 et les messages binaires.                                 | Prend en charge uniquement les messages binaires.                                                                |
| Similaire à un socket UDP ou datagramme.                                     | Similaire à un socket TCP ou de flux.                                                            |

Dans la plupart des cas, vous devez utiliser une connexion WebSocket sécurisée pour chiffrer les données envoyées et reçues. Cela augmente les chances que votre connexion aboutisse, car de nombreux proxys rejettent les connexions WebSocket non chiffrées. Le protocole WebSocket définit les deux schémas d’URI suivants.

-   ws:, utilisé pour les connexions non chiffrées.
-   wss:, utilisé pour sécuriser des connexions qui devraient être chiffrées.

Pour chiffrer une connexion WebSocket, utilisez le schéma d’URI wss:, par exemple, `wss://www.contoso.com/mywebservice`.

## Utilisation de MessageWebSocket

Le [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) permet la lecture de sections d’un message à chaque opération de lecture. Un **MessageWebSocket** est généralement utilisé dans des cas où les messages ne sont pas très volumineux. Les fichiers UTF-8 et binaires sont pris en charge.

Le code de cette section crée un objet [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842), se connecte à un serveur WebSocket et envoie des données à celui-ci. Une fois qu’une connexion a été correctement établie, l’application attend le déclenchement de l’événement [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358), qui indique que des données ont été reçues.

Cet exemple utilise le serveur echo WebSocket.org, service qui renvoie simplement à l’expéditeur l’écho de toute chaîne qui lui est envoyée. Cet exemple utilise une connexion sécurisée pour envoyer et recevoir des messages à l’aide du spécificateur de protocole «wss:».

> [!div class="tabbedCodeSnippets"]
> ```cpp
> void Game::InitWebSockets()
> {
>     // Create a new web socket
>     m_messageWebSocket = ref new MessageWebSocket();
> 
>     // Set the message type to UTF-8
>     m_messageWebSocket->Control->MessageType = Windows::Networking::Sockets::SocketMessageType::Utf8;
> 
>     // Register callbacks for notifications of interest
>     m_messageWebSocket->MessageReceived += 
>        ref new TypedEventHandler<MessageWebSocket^, MessageWebSocketMessageReceivedEventArgs^>(this, &Game::WebSocketMessageReceived);
>     m_messageWebSocket->Closed += ref new TypedEventHandler<IWebSocket^, WebSocketClosedEventArgs^>(this, &Game::WebSocketClosed);
> 
>     // This test code uses the websocket.org echo service to illustrate sending a string and receiving the echoed string back
>     // Note that wss: makes this an encrypted connection.
>     m_serverUri = ref new Uri("wss://echo.websocket.org");
> 
>     // Establish the connection, and set m_socketConnected on success
>     create_task(m_messageWebSocket->ConnectAsync(m_serverUri)).then([this] (task<void> previousTask)
>     {
>         try
>         {
>             // Try getting all exceptions from the continuation chain above this point.
>             previousTask.get();
> 
>             // websocket connected. update state variable
>             m_socketConnected = true;
>             OutputDebugString(L"Successfully initialized websockets\n");
>         }
>         catch (Platform::COMException^ exception)
>         {
>             // Add code here to handle any exceptions
>             // HandleException(exception);
> 
>         }
>     });
> }
> ```
> ```cs
> MessageWebSocket webSock = new MessageWebSocket();
> 
> //In this case we will be sending/receiving a string so we need to set the MessageType to Utf8.
> webSock.Control.MessageType = SocketMessageType.Utf8;
> 
> //Add the MessageReceived event handler.
> webSock.MessageReceived += WebSock_MessageReceived;
> 
> //Add the Closed event handler.
> webSock.Closed += WebSock_Closed;
> 
> Uri serverUri = new Uri("wss://echo.websocket.org");
> 
> try
> {
>     //Connect to the server.
>     await webSock.ConnectAsync(serverUri);
> 
>     //Send a message to the server.
>     await WebSock_SendMessage(webSock, "Hello, world!");
> }
> catch (Exception ex)
> {
>     //Add code here to handle any exceptions
> }
> ```

Une fois que vous avez initialisé la connexion WebSocket, votre code doit effectuer les activités suivantes pour envoyer et recevoir des données correctement.

### Implémenter un rappel pour l’événement MessageWebSocket.MessageReceived

Avant d’établir une connexion et d’envoyer des données avec WebSocket, votre application doit inscrire un rappel d’événement pour recevoir une notification lors de la réception de données. Quand l’événement [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358) se produit, le rappel inscrit est appelé et reçoit des données de [**MessageWebSocketMessageReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br226852). Cet exemple est écrit en supposant que les messages envoyés sont au format UTF-8.

L’exemple de fonction suivant reçoit d’un serveur WebSocket connecté une chaîne qu’il imprime dans la fenêtre Sortie du débogueur.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketMessageReceived(MessageWebSocket^ sender, MessageWebSocketMessageReceivedEventArgs^ args)
>{
>    DataReader^ messageReader = args->GetDataReader();
>    messageReader->UnicodeEncoding = Windows::Storage::Streams::UnicodeEncoding::Utf8;
>
>    String^ readString = messageReader->ReadString(messageReader->UnconsumedBufferLength);
>    // Data has been read and is now available from the readString variable.
>    swprintf(m_debugBuffer, 511, L"WebSocket Message received: %s\n", readString->Data());
>    OutputDebugString(m_debugBuffer);
>}
>```
>```csharp
>//The MessageReceived event handler.
>private void WebSock_MessageReceived(MessageWebSocket sender, MessageWebSocketMessageReceivedEventArgs args)
>{
>    DataReader messageReader = args.GetDataReader();
>    messageReader.UnicodeEncoding = UnicodeEncoding.Utf8;
>    string messageString = messageReader.ReadString(messageReader.UnconsumedBufferLength);
>
>    //Add code here to do something with the string that is received.
>}
>```

###  Implémenter un rappel pour l’événement MessageWebSocket.Closed

Avant d’établir une connexion et d’envoyer des données avec un WebSocket, votre application doit inscrire un rappel d’événement pour recevoir une notification lors de la fermeture du WebSocket par le serveur WebSocket. Quand l’événement [**MessageWebSocket.Closed**](https://msdn.microsoft.com/library/windows/apps/hh701364) se produit, le rappel inscrit est appelé pour indiquer que le serveur WebSocket a fermé la connexion.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketClosed(IWebSocket^ sender, WebSocketClosedEventArgs^ args)
>{
>    // The method may be triggered remotely by the server sending unsolicited close frame or locally by Close()/delete operator.
>    // This method assumes we saved the connected WebSocket to a variable called m_messageWebSocket
>    if (m_messageWebSocket != nullptr)
>    {
>        delete m_messageWebSocket;
>        m_messageWebSocket = nullptr;
>        OutputDebugString(L"Socket was closed\n");
>    }
>    m_socketConnected = false;
> }
>```
>```csharp
>//The Closed event handler
>private void WebSock_Closed(IWebSocket sender, WebSocketClosedEventArgs args)
>{
>    //Add code here to do something when the connection is closed locally or by the server
>}
>```

###  Envoyer un message sur un WebSocket

Une fois qu’une connexion est établie, le client WebSocket peut envoyer des données au serveur. La méthode [**DataWriter.StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) retourne un paramètre qui correspond à une valeur int non signée. Cela modifie la manière dont nous définissons la tâche d’envoi du message par rapport à la tâche d’établissement de la connexion.

**Remarque** Lorsque vous créez un objet DataWriter à l’aide de l’OutputStream du MessageWebSocket, le DataWriter prend possession de l’OutputStream et libère celui-ci quand le DataWriter est hors de portée. Ainsi, toute tentative ultérieure d’utilisation de l’OutputStream échoue avec une valeur HRESULT de 0x80000013. Pour éviter la libération de l’OutputStream, ce code appelle la méthode DetachStream du DataWriter, qui retourne la propriété du flux à l’objet WebSocket.

La fonction suivante envoie la chaîne donnée à un WebSocket connecté, et imprime un message de vérification dans la fenêtre Sortie du débogueur.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::SendWebSocketMessage(Windows::Networking::Sockets::MessageWebSocket^ sendingSocket, Platform::String^ message)
>{
>    if (m_socketConnected)
>    {
>        // WebSocket is connected, so send a message
>        m_messageWriter = ref new DataWriter(sendingSocket->OutputStream);
>
>        m_messageWriter->WriteString(message);
>
>        // Send the data as one complete message
>        create_task(m_messageWriter->StoreAsync()).then([this] (unsigned int)
>        {
>            // Send Completed
>            m_messageWriter->DetachStream();    // give the stream back to m_messageWebSocket
>            OutputDebugString(L"Sent websocket message\n");
>        })
>            .then([this] (task<void>> previousTask)
>        {
>            try
>            {
>                // Try getting all exceptions from the continuation chain above this point.
>                previousTask.get();
>            }
>            catch (Platform::COMException ^ex)
>            {
>                // Add code to handle the exception
>                // HandleException(exception);
>            }
>        });
>    }
>}
>```
>```csharp
>//Send a message to the server.
>private async Task WebSock_SendMessage(MessageWebSocket webSock, string message)
>{
>    DataWriter messageWriter = new DataWriter(webSock.OutputStream);
>    messageWriter.WriteString(message);
>    await messageWriter.StoreAsync();
>}
>```

## Utilisation de contrôles avancés avec des WebSockets

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) et [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) suivent le même modèle pour l’utilisation de contrôles avancés. Aux classes principales ci-dessus correspondent des classes associées permettant d’accéder à des contrôles avancés.

[**MessageWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226843) fournit des données de contrôle de socket sur un objet [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842).
[**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) fournit des données de contrôle de socket sur un objet [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).
Le modèle de base pour l’utilisation des contrôles avancés est le même pour les deux types de WebSockets. La discussion suivante utilise un [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) comme exemple, mais le même processus peut être utilisé avec [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842).

1.  Créez l’objet [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).
2.  Utilisez la propriété [**StreamWebSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226934) pour récupérer l’instance [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) associée à cet objet [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).
3.  Obtenez ou définissez des propriétés sur l’instance [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) afin d’obtenir ou de définir des contrôles avancés spécifiques.

Tant [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) que [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) imposent des exigences relatives à la situation où des contrôles avancés peuvent être définis.

-   Pour tous les contrôles avancés sur le [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923), l’application doit toujours définir la propriété avant de lancer une opération de connexion. Pour cette raison, il est préférable de définir toutes les propriétés de contrôle immédiatement après la création de l’objet **StreamWebSocket**. N’essayez pas de définir une propriété de contrôle après l’appel de la méthode [**StreamWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226933).
-   Pour tous les contrôles avancés sur le [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) à l’exception du type de message, vous devez définir la propriété avant de lancer une opération de connexion. Il est recommandé de définir toutes les propriétés de contrôle immédiatement après la création de l’objet **MessageWebSocket**. À l’exception du type de message, n’essayez pas de modifier une propriété de contrôle après l’appel de [**MessageWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226859).

## Classes d’informations WebSocket

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) et [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) ont une classe correspondante qui fournit des informations supplémentaires sur une instance WebSocket.

[**MessageWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226849)Fournit des informations sur un [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842), et vous récupérez une instance de la classe d’informations à l’aide de la propriété [**MessageWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226861).

[**StreamWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226929)Fournit des informations sur un [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923), et vous récupérez une instance de la classe d’informations à l’aide de la propriété [**StreamWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226935).

Notez que toutes les propriétés des deux classes d’informations sont en lecture seule, et que vous pouvez récupérer les informations actuelles à tout moment pendant la durée de vie d’un objet socket web.

## Gestion des exceptions réseau

Une erreur rencontrée dans une opération [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) est renvoyée sous forme de valeur **HRESULT**. La méthode [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) sert à convertir une erreur réseau résultant d’une opération WebSocket en valeur d’énumération [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818). La plupart des valeurs d’énumération **WebErrorStatus** correspondent à une erreur renvoyée par l’opération cliente HTTP native. Votre application peut filtrer des valeurs d’énumération **WebErrorStatus** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Pour les erreurs de validation de paramètre, une application peut également utiliser la valeur **HRESULT** à partir de l’exception pour obtenir des informations plus détaillées sur l’erreur à l’origine de l’exception. Les valeurs **HRESULT** possibles sont répertoriées dans le fichier d’en-tête *Winerror.h*. Pour la plupart des erreurs de validation de paramètre, la valeur **HRESULT** renvoyée est **E\_INVALIDARG**.

## Définition de délais d’attente sur des opérations de WebSocket

Les classes MessageWebSocket et StreamWebSocket utilisent un service système interne pour envoyer des demandes de client WebSocket et recevoir des réponses d’un serveur. La valeur de délai d’expiration par défaut utilisée pour une opération de connexion de WebSocket est de 60 secondes. Si le serveur HTTP qui prend en charge les WebSocket est momentanément arrêté ou bloqué par une panne réseau, et si le serveur ne répond pas ou ne peut pas répondre à la demande de connexion de WebSocket, le service système interne patiente pendant les 60 secondes par défaut avant de retourner une erreur qui entraîne la levée d’une exception sur la méthode ConnectAsync du WebSocket. Si la requête de nom pour un nom de serveur HTTP dans l’URI renvoie plusieurs adresses IP, le service système interne essaie jusqu’à 5 adresses IP pour le site, chacune avec un délai d’expiration de 60 secondes par défaut avant d’échouer. Une application effectuant une demande de connexion de WebSocket peut essayer pendant quelques minutes de se connecter à plusieurs adresses IP avant qu’une erreur soit retournée et une exception levée. Ce comportement pourrait faire croire à l’utilisateur que l’application a cessé de fonctionner. Le délai d’expiration par défaut utilisé pour les opérations d’envoi et de réception après l’établissement d’une connexion de WebSocket est de 30 secondes.

Pour rendre votre application plus réactive et minimiser ces problèmes, il peut définir un délai d’expiration plus court sur les demandes de connexion, afin que l’opération échoue en raison du délai d’attente plus rapidement qu’à cause des paramètres par défaut.

Vous définissez les délais d’expiration de la même façon pour les StreamWebSockets et les MessageWebSockets. L’exemple suivant montre comment définir un délai d’expiration sur StreamWebSocket, mais le processus est similaire pour un MessageWebSocket.

1.  Créez une tâche qui se termine après un délai spécifié à l’aide d’un minuteur.
2.  Créez une tâche pour l’opération de WebSocket avec une cancellation\_token\_source pour prendre en charge l’annulation.
3.  Si la tâche associée au délai d’expiration spécifié se termine avant l’opération de connexion de WebSocket, annulez la tâche pour cette opération.

L’exemple suivant crée une tâche qui se termine après un délai spécifié, et crée une deuxième tâche qui annule après un délai spécifié. Ces classes peuvent être utilisées avec StreamWebSocket et MessageWebSocket lors d’une tentative d’établissement de connexion pour définir un délai d’expiration spécifique. Un exemple d’utilisation serait d’appeler la méthode StreamWebSocket.ConnectAsync dans une tâche, avec une cancellation\_token\_source qui prend en charge l’annulation. Si le délai d’expiration s’achève en premier, la cancellation\_token\_source est utilisée pour annuler la tâche relative à l’opération de connexion de WebSocket.

```cpp
    #include <agents.h>
    #include <ppl.h>
    #include <ppltasks.h>

    using namespace concurrency;
    using namespace std;

    // Creates a task that completes after the specified delay.
    task<void> complete_after(unsigned int timeout)
    {
        // A task completion event that is set when a timer fires.
        task_completion_event<void> tce;

        // Create a non-repeating timer.
        shared_ptr<timer<int>> fire_once(new timer<int>(timeout, 0, nullptr, false));
        
        // Create a call object that sets the completion event after the timer fires.
        shared_ptr<call<int>> callback(new call<int>([tce](int)
        {
            tce.set();
        }));

        // Connect the timer to the callback and start the timer.
        fire_once->link_target(callback.get());
        fire_once->start();

        // Create a task that completes after the completion event is set.
        task<void> event_set(tce);

        // Create a continuation task that cleans up resources and
        // and return that continuation task.
        return event_set.then([callback, fire_once]()
        {
        });
    }

    // Cancels the provided task after the specifed delay, if the task
    // did not complete.
    template<typename T>
    task<T> cancel_after_timeout(task<T> t, cancellation_token_source cts, unsigned int timeout)
    {
        // Create a task that returns true after the specified task completes.
        task<bool> success_task = t.then([](T)
        {
            return true;
        });
        // Create a task that returns false after the specified timeout.
        task<bool> failure_task = complete_after(timeout).then([]
        {
            return false;
        });

        // Create a continuation task that cancels the overall task  
        // if the timeout task finishes first. 
        return (failure_task || success_task).then([t, cts](bool success)
        {
            if (!success)
            {
                // Set the cancellation token. The task that is passed as the 
                // t parameter should respond to the cancellation and stop 
                // as soon as it can.
                cts.cancel();
            }
 
            // Return the original task.
            return t;
        });
    }
```




<!--HONumber=Aug16_HO3-->


