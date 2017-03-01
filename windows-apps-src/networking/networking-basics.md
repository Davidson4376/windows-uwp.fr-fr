---
author: DelfCo
description: "Ce que vous devez faire pour toute application réseau."
title: "Notions de base en matière de réseau"
ms.assetid: 1F47D33B-6F00-4F74-A52D-538851FD38BE
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d24d1ff9408585246aae4c4849d3af5e8f4846be
ms.lasthandoff: 02/07/2017

---

# <a name="networking-basics"></a>Notions de base en matière de réseau

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Ce que vous devez faire pour toute application réseau.

## <a name="capabilities"></a>Fonctionnalités

Pour pouvoir utiliser le réseau, vous devez ajouter des éléments de fonctionnalité appropriés au manifeste de votre application. Si aucune fonctionnalité réseau n’est spécifiée dans le manifeste de votre application, celle-ci n’a aucune fonctionnalité réseau, et toute tentative de connexion au réseau échoue.

Les fonctionnalités réseau les plus utilisées sont les suivantes.

| Fonctionnalité | Description |
|------------|-------------|
| **internetClient** | Fournit un accès sortant à Internet et aux réseaux dans les lieux publics, comme les aéroports et les cafés. La plupart des applications qui nécessitent un accès Internet doivent utiliser cette fonctionnalité. |
| **internetClientServer** | Fournit à l’application un accès réseau entrant et sortant depuis Internet et les réseaux mis à disposition dans les lieux publics, comme les aéroports et les cafés. |
| **privateNetworkClientServer** | Fournit à l’application un accès réseau entrant et sortant dans les lieux approuvés par l’utilisateur, comme le domicile ou l’entreprise. |

D’autres fonctionnalités peuvent être nécessaires pour votre application dans certaines circonstances.

| Fonctionnalité | Description |
|------------|-------------|
| **enterpriseAuthentication** | Permet à une application de se connecter aux ressources réseau qui nécessitent des informations d’identification de domaine. Un administrateur de domaine devra activer cette fonctionnalité pour toutes les applications. Il peut s’agir, par exemple, d’une application qui récupère des données à partir de serveurs Sharepoint sur un intranet privé. <br/> Avec cette fonctionnalité, vos informations d’identification peuvent être utilisées pour accéder aux ressources réseau sur un réseau nécessitant des informations d’identification. Une application disposant de cette fonctionnalité peut emprunter votre identité sur le réseau. <br/> Cette fonctionnalité n’est pas obligatoire pour autoriser une application à accéder à Internet par le biais d’un proxy d’authentification. |
| **proximity** | Requis pour la communication de proximité en champ proche avec des appareils à proximité immédiate de l’ordinateur. La proximité en champ proche peut être utilisée pour envoyer des fichiers ou communiquer avec une application sur l’appareil proche. <br/> Cette fonctionnalité permet à une application d’accéder au réseau pour se connecter à un appareil à proximité immédiate. L’utilisateur doit confirmer l’envoi d’une invitation ou l’acceptation d’une invitation. |
| **sharedUserCertificates** | Cette fonctionnalité permet à une application d’accéder aux certificats logiciels et matériels, tels que les certificats de carte à puce. Lorsque cette fonctionnalité est appelée au moment de l’exécution, l’utilisateur doit entreprendre une action, comme insérer une carte ou sélectionner un certificat. <br/> Avec cette fonctionnalité, vos certificats matériels et logiciels ou une carte à puce sont utilisés pour l’identification dans l’application. Cette fonctionnalité peut être utilisée par votre employeur, votre banque ou les services publics pour l’identification. |

## <a name="communicating-when-your-app-is-not-in-the-foreground"></a>Communication lorsque votre application n’est pas au premier plan

[Prise en charge de votre application avec des tâches en arrière-plan](https://msdn.microsoft.com/library/windows/apps/mt299103) contient des informations générales sur l’utilisation des tâches en arrière-plan pour effectuer des tâches lorsque votre application n’est pas au premier plan. Plus précisément, votre code doit prévoir des mesures spéciales pour que votre application soit notifiée quand elle n’est pas au premier plan et que des données qui lui sont destinées arrivent sur le réseau. À cette fin, dans Windows 8, vous utilisiez des déclencheurs de canal de contrôle qui sont toujours pris en charge dans Windows 10. Pour des informations complètes sur l’utilisation de déclencheurs de canal de contrôle, voir [**here**](https://msdn.microsoft.com/library/windows/apps/hh701032). Une nouvelle technologie dans Windows 10 assure une meilleure fonctionnalité à moindre coût dans certaines situations. C’est, par exemple, le cas des sockets de flux à extension push : le broker de socket et les déclencheurs d’activité de socket.

Si votre application utilise [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) ou[**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906), elle peut transférer la propriété d’un socket ouvert à un broker de socket fourni par le système, puis quitter le premier plan, ou même s’arrêter. Quand une connexion est établie sur le socket transféré, ou quand du trafic arrive sur ce socket, votre application ou sa tâche en arrière-plan désignée sont activées. Si votre application n’est pas en cours d’exécution, elle est démarrée. Le broker de socket avertit alors votre application à l’aide d’un [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) qu’un nouveau trafic est arrivé. Votre application récupère le socket à partir du broker de socket et traite le trafic sur le socket. Cela signifie que votre application consomme beaucoup moins de ressources système quand elle ne traite pas activement le trafic réseau.

Le broker de socket est destiné à remplacer les déclencheurs de canal de contrôle quand c’est possible, car il offre les mêmes fonctionnalités, mais avec moins de restrictions et un moindre encombrement mémoire. Un broker de socket peut être utilisé par des applications autres que d’écran de verrouillage, et de la même manière sur des téléphones que sur d’autres appareils. Pour être activées par le broker de socket, les applications ne doivent pas nécessairement être en cours d’exécution quand le trafic arrive. Par ailleurs, le broker de socket prend en charge l’écoute sur les sockets TCP, contrairement aux déclencheurs de canal de contrôle.


### <a name="choosing-a-network-trigger"></a>Choix d’un déclencheur réseau

Il existe des cas où les deux types de déclencheurs seraient appropriés. Lorsque vous choisissez le type de déclencheur à utiliser dans votre application, tenez compte des conseils suivants.

-   Si vous utilisez [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151), [**System.Net.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) ou [System.Net.Http.HttpClientHandler](http://go.microsoft.com/fwlink/p/?linkid=241638), vous devez opter pour [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).
-   Si vous utilisez des **StreamSockets** compatibles avec les notifications push, vous pouvez utiliser des déclencheurs de canal de contrôle. Toutefois, il est préférable d’utiliser [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009). Cela permet au système de libérer de la mémoire et de réduire la consommation d’énergie quand la connexion n’est pas activement utilisée.
-   Si vous souhaitez réduire l’encombrement mémoire de votre application quand celle-ci ne traite pas activement de demandes réseau, préférez [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) chaque fois que possible.
-   Si vous souhaitez que votre application puisse recevoir des données quand le système est en mode de veille connectée, utilisez [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009).

Pour plus d’informations et des exemples concernant la manière d’utiliser le broker de socket, voir [Communications réseau en arrière-plan](network-communications-in-the-background.md).

## <a name="secured-connections"></a>Connexions sécurisées

Le protocole SSL (Secure Sockets Layer) et le protocole TLS (Transport Layer Security), plus récent, sont des protocoles de chiffrement conçus pour fournir l’authentification et le chiffrement pour la communication réseau. Ces protocoles sont conçus pour éviter les écoutes clandestines et la falsification lors de l’envoi et la réception de données réseau. Ces protocoles utilisent un modèle client-serveur pour les échanges de protocole. Ces protocoles utilisent des certificats numériques et des autorités de certification pour vérifier que le serveur est bien celui qu’il prétend être.

### <a name="creating-secure-socket-connections"></a>Création de connexions de sockets sécurisées

Un objet [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) peut être configuré de manière à utiliser SSL/TLS pour les communications entre le client et le serveur. Cette prise en charge de SSL/TLS est limitée à l’utilisation de l’objet **StreamSocket** en tant que client dans la négociation SSL/TLS. Vous ne pouvez pas utiliser SSL/TLS avec le **StreamSocket** créé par un [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) lorsque les communications entrantes sont reçues, car la négociation SSL/TLS en tant que serveur n’est pas implémentée par la classe **StreamSocket**.

Il existe deux manières de sécuriser une connexion [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) avec SSL/TLS :

-   [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) - Établir la connexion initiale à un service réseau et négocier immédiatement l’utilisation de SSL/TLS pour toutes les communications.
-   [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) - Se connecter initialement à un service réseau sans chiffrement. L’application peut envoyer ou recevoir des données. Elle peut ensuite mettre à niveau la connexion afin d’utiliser les protocoles SSL/TLS pour toutes les communications à venir.

La valeur SocketProtectionLevel que vous fournissez définit le niveau de protection minimal que vous êtes disposé à autoriser. Toutefois, le niveau de protection éventuelle de la connexion établie est déterminé dans un processus de négociation entre deux points de terminaison de la connexion. Le résultat peut être un niveau de protection plus sécurisé que celui que vous avez spécifié si l’autre point de terminaison requiert un niveau supérieur. La force SSL réellement négociée à l’aide de [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) ou [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) peut être déterminée en obtenant la propriété [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) après le déroulement correct de l’opération asynchrone.

> **Remarque**  Votre code ne doit jamais dépendre implicitement d’un niveau de protection spécifique ou d’une hypothèse selon laquelle un niveau de sécurité donné est utilisé par défaut. Le paysage de sécurité change constamment, et les protocoles et les niveaux de protection par défaut seront modifiés au fil du temps pour éviter l’utilisation de protocoles présentant des faiblesses connues. Les valeurs par défaut peuvent varier en fonction de la configuration de chaque ordinateur ou en fonction des logiciels installés et des correctifs appliqués. Si votre application repose sur l’utilisation d’un niveau de sécurité particulier, vous devez explicitement spécifier ce niveau, puis effectuez une vérification pour vous assurer qu’il est réellement utilisé sur la connexion établie.

### <a name="use-connectasync"></a>Utiliser ConnectAsync

[**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) peut être utilisé pour établir la connexion de départ avec un service réseau, puis négocier immédiatement l’utilisation de SSL/TLS pour toutes les communications. Deux méthodes **ConnectAsync** prennent en charge un paramètre *protectionLevel* :

-   [**ConnectAsync(EndpointPair, SocketProtectionLevel)**](https://msdn.microsoft.com/library/windows/apps/hh701511) - Démarre une opération asynchrone sur un objet [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) pour se connecter à une destination réseau distante spécifiée en tant qu’objet [**EndpointPair**](https://msdn.microsoft.com/library/windows/apps/hh700953) et un [**SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880).
-   [**ConnectAsync(HostName, String, SocketProtectionLevel)**](https://msdn.microsoft.com/library/windows/apps/br226916) - Démarre une opération asynchrone sur un objet [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) pour se connecter à une destination distante spécifiée par un nom d’hôte distant, un nom de service distant et un [**SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880).

Si le paramètre *protectionLevel* est défini sur **Windows.Networking.Sockets.SocketProtectionLevel.Ssl** lors de l’appel de l’une ou l’autre des méthodes [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) citées ci-dessus, [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) doit utiliser SSL/TLS pour le chiffrement. Cette valeur doit être chiffrée et ne prend pas en charge les éléments de chiffrement NULL.

La séquence normale à utiliser avec l’une de ces méthodes [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) est la même.

-   Créez une méthode [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882).
-   Si vous devez ajouter une option avancée pour le socket, utilisez la propriété [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917) pour obtenir l’instance [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893) qui est associée à un objet [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). Définissez une propriété pour **StreamSocketControl**.
-   Appelez l’une des méthodes [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) ci-dessus pour démarrer une opération de connexion à une destination distante et négociez immédiatement l’utilisation de SSL/TLS.
-   La force SSL réellement négociée à l’aide de [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) peut être déterminée en obtenant la propriété [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) après le déroulement correct de l’opération asynchrone.

Dans l’exemple suivant, un objet [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) est créé, puis essaie d’établir une connexion au service réseau et de négocier immédiatement l’utilisation de SSL/TLS. Si la négociation réussit, toutes les communications réseau utilisant l’objet **StreamSocket** entre le client et le serveur réseau seront chiffrées.

> [!div class="tabbedCodeSnippets"]
```csharp
using Windows.Networking;
using Windows.Networking.Sockets;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
     
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "https";
    
    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 
    
    // Try to connect to contoso using HTTPS (port 443)
    try {

        // Call ConnectAsync method with SSL
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.Ssl);

        NotifyUser("Connected");
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }
        
        NotifyUser("Connect failed with error: " + exception.Message);
        // Could retry the connection, but for this simple example
        // just close the socket.
        
        clientSocket.Dispose();
        clientSocket = null; 
    }
           
    // Add code to send and receive data using the clientSocket
    // and then close the clientSocket
```
```cpp
using Windows::Networking;
using Windows::Networking::Sockets;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    HostName^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "https";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to the server using HTTPS and SSL (port 443)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::SSL)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
            
            clientSocket.Close();
            clientSocket = null;
        }
    });
    // Add code to send and receive data using the clientSocket
    // Then close the clientSocket when done
```

### <a name="use-upgradetosslasync"></a>Utiliser UpgradeToSslAsync

Lorsque vote code utilise [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922), il commence par établir une connexion au service réseau sans chiffrement. L’application peut envoyer ou recevoir des données, puis mettre à jour la connexion de manière à utiliser SSL/TLS pour toutes les prochaines communications.

La méthode [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) prend deux paramètres. Le paramètre *protectionLevel* indique le niveau de protection souhaité. Le paramètre *validationHostName* est le nom d’hôte de la destination réseau distante qui est utilisée pour la validation lors de la mise à niveau vers SSL. *validationHostName* est en principe le même nom d’hôte que celui utilisé initialement par l’application pour établir la connexion. Si le paramètre *protectionLevel* est défini sur **Windows.System.Socket.SocketProtectionLevel.Ssl** lors de l’appel de **UpgradeToSslAsync**, le [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) doit utiliser SSL/TLS pour le chiffrement des prochaines communications sur le socket. Cette valeur doit être chiffrée et ne prend pas en charge les éléments de chiffrement NULL.

La séquence normale à utiliser avec la méthode [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) est la suivante :

-   Créez une méthode [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882).
-   Si vous devez ajouter une option avancée pour le socket, utilisez la propriété [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917) pour obtenir l’instance [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893) qui est associée à un objet [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). Définissez une propriété pour **StreamSocketControl**.
-   Si des données doivent être envoyées et reçues non chiffrées, envoyez-les maintenant.
-   Appelez la méthode [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) pour démarrer une opération de mise à niveau de la connexion vers SSL/TLS.
-   La force SSL réellement négociée à l’aide de [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) peut être déterminée en obtenant la propriété [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) après le déroulement correct de l’opération asynchrone.

Dans l’exemple suivant, un objet [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) est créé, essaie d’établir une connexion au service réseau, envoie des données initiales, puis négocie l’utilisation de SSL/TLS. Si la négociation réussit, toutes les communications réseau utilisant le **StreamSocket** entre le client et le serveur réseau seront chiffrées.

> [!div class="tabbedCodeSnippets"]
```csharp
using Windows.Networking;
using Windows.Networking.Sockets;
using Windows.Storage.Streams;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
 
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    try {
        // Call ConnectAsync method with a plain socket
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.PlainSocket);

        NotifyUser("Connected");

    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Connect failed with error: " + exception.Message, NotifyType.ErrorMessage);
        // Could retry the connection, but for this simple example
        // just close the socket.

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }

    // Now try to sent some data
    DataWriter writer = new DataWriter(clientSocket.OutputStream);
    string hello = "Hello World ☺ ";
    Int32 len = (int) writer.MeasureString(hello); // Gets the UTF-8 string length.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser("Client: sending hello");

    try {
        // Call StoreAsync method to store the hello message
        await writer.StoreAsync();

        NotifyUser("Client: sent data");

        writer.DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
    }
    catch (Exception exception) {
        NotifyUser("Store failed with error: " + exception.Message);
        // Could retry the store, but for this simple example
            // just close the socket.

            clientSocket.Dispose();
            clientSocket = null; 
            return;
    }

    // Now upgrade the client to use SSL
    try {
        // Try to upgrade to SSL
        await clientSocket.UpgradeToSslAsync(SocketProtectionLevel.Ssl, serverHost);

        NotifyUser("Client: upgrade to SSL completed");
           
        // Add code to send and receive data 
        // The close clientSocket when done
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }
```
```cpp
using Windows::Networking;
using Windows::Networking::Sockets;
using Windows::Storage::Streams;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    Hostname^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");

        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
 
            clientSocket->Close();
            clientSocket = null;
        }
    });
       
    // Now try to sent some data
    DataWriter^ writer = new ref DataWriter(clientSocket.OutputStream);
    String hello = "Hello World ☺ ";
    Int32 len = (int) writer->MeasureString(hello); // Gets the UTF-8 string length.
    writer->writeInt32(len);
    writer->writeString(hello);
    NotifyUser("Client: sending hello");

    task<void>(writer->StoreAsync()).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

            NotifyUser("Client: sent hello");

            writer->DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
       }
       catch (Exception^ exception) {
               NotifyUser("Store failed with error: " + exception->Message);
               // Could retry the store, but for this simple example
               // just close the socket.
 
               clientSocket->Close();
               clientSocket = null;
               return
       }
    });

    // Now upgrade the client to use SSL
    task<void>(clientSocket->UpgradeToSslAsync(clientSocket.SocketProtectionLevel.Ssl, serverHost)).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

           NotifyUser("Client: upgrade to SSL completed");
           
           // Add code to send and receive data 
           // Then close clientSocket when done
        }
        catch (Exception^ exception) {
            // If this is an unknown status it means that the error is fatal and retry will likely fail.
            if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
                throw;
            }

            NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

            clientSocket->Close();
            clientSocket = null; 
            return;
        }
    });
```

### <a name="creating-secure-websocket-connections"></a>Création de connexions WebSocket sécurisées

Tout comme les connexions de sockets classiques, les connexions WebSocket peuvent également être chiffrées à l’aide des protocoles TLS (Transport Layer Security)/SSL (Secure Sockets Layer) lors de l’utilisation des fonctionnalités [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) et [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) de Windows 8 pour une application du Windows Store. En règle générale, vous utilisez une connexion WebSocket sécurisée. Cela augmente les chances que votre connexion s’établisse, car de nombreux serveurs proxy rejettent les connexions WebSocket non chiffrées.

Pour obtenir des exemples de création ou de mise à niveau d’une connexion de sockets sécurisée vers un service réseau, voir [Comment sécuriser les connexions WebSocket avec TLS/SSL](https://msdn.microsoft.com/library/windows/apps/xaml/hh994399).

Outre le chiffrement TLS/SSL, un serveur peut avoir besoin d’une valeur d’en-tête **Sec-WebSocket-Protocol** pour pouvoir effectuer l’établissement d’une liaison initiale. Cette valeur, représentée par les propriétés [**StreamWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701514) et [**MessageWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701358), indique la version de protocole de la connexion. Par ailleurs, elle permet au serveur d’interpréter correctement l’établissement d’une liaison de départ, ainsi que les données échangées par la suite. Grâce à ces informations relatives au protocole, si le serveur ne parvient pas à interpréter les données entrantes en toute sécurité, la connexion peut être fermée.

Si la demande initiale du client ne contient pas cette valeur ou si elle fournit une valeur qui ne correspond pas aux informations attendues par le serveur, la valeur attendue est envoyée du serveur au client lors de l’erreur d’établissement de liaison WebSocket.

## <a name="authentication"></a>Authentification

Comment fournir des informations d’authentification lors d’une connexion via le réseau.

### <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>Fourniture d’un certificat client avec la classe StreamSocket

La classe [**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) prend en charge l’utilisation des protocoles SSL/TLS pour authentifier le serveur avec lequel l’application communique. Dans certains cas, l’application doit également s’authentifier auprès du serveur à l’aide d’un certificat client TLS. Dans Windows 10, vous pouvez prévoir un certificat client sur l’objet [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226893) (cela doit être défini avant le début de la négociation TLS). Si le serveur demande le certificat client, Windows répond avec le certificat fourni.

Voici un extrait de code montrant comment implémenter cela :

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

### <a name="providing-authentication-credentials-to-a-web-service"></a>Transmission d’informations d’identification d’authentification à un service Web

Les API réseau qui permettent aux applications d’interagir avec les services web sécurisés fournissent chacune leurs propres méthodes pour initialiser un client ou pour définir un en-tête de demande avec les informations d’identification d’authentification du serveur et du proxy. Chaque méthode est définie par un objet [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) qui indique un nom d’utilisateur, un mot de passe et la ressource pour laquelle ces informations d’identification sont utilisées. Le tableau suivant fournit un mappage de ces API :

| **WebSockets** | [**MessageWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226848) |
|-------------------------|----------------------------------------------------------------------------------------------------------|
|  | [**MessageWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226847) |
|  | [**StreamWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226928) |
|  | [**StreamWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226927) |
|  |  |
| **Transfert en arrière-plan** | [**BackgroundDownloader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701076) |
|  | [**BackgroundDownloader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701068) |
|  | [**BackgroundUploader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701184) |
|  | [**BackgroundUploader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701178) |
|  |  |
| **Syndication** | [**SyndicationClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702355) |
|  | [**SyndicationClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461) |
|  | [**SyndicationClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459) |
|  |  |
| **AtomPub** | [**AtomPubClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702262) |
|  | [**AtomPubClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243428) |
|  | [**AtomPubClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243423) |
 
## <a name="handling-network-exceptions"></a>Gestion des exceptions réseau

Dans la plupart des domaines de programmation, une exception indique un problème ou un échec importants dus à un défaut du programme. Dans la programmation réseau, il existe une source supplémentaire d’exceptions : le réseau lui-même et la nature des communications réseau. Les communications réseau sont intrinsèquement peu fiables et sujettes à des échecs inattendus. Quelle que soit la manière dont votre application utilise le réseau, vous devez conserver des informations d’état. Par ailleurs, le code de votre application doit gérer les exceptions réseau en mettant à jour ces informations d’état et en initiant une logique appropriée afin que votre application rétablisse ou réessaie d’établir toute communication défaillante.

Quand des applications Windows universelles lèvent un exception, votre gestionnaire d’exceptions peut récupérer des informations plus détaillées sur la cause de l’exception dans le but d’analyser l’échec et de prendre des décisions appropriées.

Chaque projection de langage prend en charge une méthode permettant d’accéder à ces informations plus détaillées. Une exception est projetée en tant que valeur **HRESULT** dans les applications Windows universelles. Le fichier include *Winerror.h* contient une très grande liste de valeurs **HRESULT** possibles qui comprend des erreurs réseau.

Les API de réseau prennent en charge différentes méthodes permettant de récupérer ces informations détaillées sur la cause d’une exception :

-   Certaines API fournissent une méthode d’assistance qui convertit la valeur **HRESULT** de l’exception en valeur d’énumération.
-   D’autres API fournissent une méthode pour récupérer la valeur **HRESULT** réelle.

## <a name="related-topics"></a>Rubriques connexes

* [Améliorations de l’API de réseau dans Windows 10](http://blogs.windows.com/buildingapps/2015/07/02/networking-api-improvements-in-windows-10/)
 


