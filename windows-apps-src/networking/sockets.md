---
description: En tant que développeur d’applications de plateforme Windows universelles (UWP), vous pouvez utiliser tant Windows.Networking.Sockets que Winsock pour communiquer avec d’autres appareils.
title: Sockets
ms.assetid: 23B10A3C-E33F-4CD6-92CB-0FFB491472D6
---

# Sockets

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**API importantes**

-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)

En tant que développeur d’applications de plateforme Windows universelles (UWP), vous pouvez utiliser tant [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) que [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523) pour communiquer avec d’autres appareils. Cette rubrique fournit des instructions détaillées sur l’utilisation de l’espace de noms **Windows.Networking.Sockets** pour les opérations réseau.

## Opérations de base d’un socket TCP

Un socket TCP fournit des transferts de données réseau de bas niveau dans chaque direction pour des connexions à durée de vie longue. Les sockets TCP sont la fonctionnalité sous-jacente utilisée par la plupart des protocoles réseau utilisés sur Internet. Cette section montre comment activer une application UWP pour envoyer et recevoir des données avec un socket de flux TCP à l’aide des classes [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) et [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) dans le cadre de l’espace de noms [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960). Dans cette section, nous allons créer une application très simple fonctionnant en tant que client et serveur echo afin d’illustrer les opérations TCP de base.

**Création d’un serveur echo TCP**

L’exemple de code suivant montre comment créer un objet [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) et démarrer l’écoute des connexions TCP entrantes.

```csharp
try
{
    //Create a StreamSocketListener to start listening for TCP connections.
    Windows.Networking.Sockets.StreamSocketListener socketListener = new Windows.Networking.Sockets.StreamSocketListener();

    //Hook up an event handler to call when connections are received.
    socketListener.ConnectionReceived += SocketListener_ConnectionReceived;

    //Start listening for incoming TCP connections on the specified port. You can specify any port that' s not currently in use.
    await socketListener.BindServiceNameAsync("1337");
}
catch (Exception e)
{
    //Handle exception.
}
```

L’exemple de code suivant implémente le gestionnaire d’événements SocketListener\_ConnectionReceived qui était joint à l’événement [**StreamSocketListener.ConnectionReceived**](https://msdn.microsoft.com/library/windows/apps/hh701494) dans l’exemple ci-dessus. Ce gestionnaire d’événements est appelé par la classe [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) chaque fois qu’un client distant établit une connexion avec le serveur echo.

```csharp
private async void SocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, 
    Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    //Read line from the remote client.
    Stream inStream = args.Socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(inStream);
    string request = await reader.ReadLineAsync();
    
    //Send the line back to the remote client.
    Stream outStream = args.Socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(outStream);
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();
}
```

**Création d’un client echo TCP**

L’exemple de code suivant montre comment créer un objet [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882), établir une connexion au serveur distant, envoyer une requête et recevoir une réponse.

```csharp
try
{
    //Create the StreamSocket and establish a connection to the echo server.
    Windows.Networking.Sockets.StreamSocket socket = new Windows.Networking.Sockets.StreamSocket();
    
    //The server hostname that we will be establishing a connection to. We will be running the server and client locally,
    //so we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Every protocol typically has a standard port number. For example HTTP is typically 80, FTP is 20 and 21, etc.
    //For the echo server/client application we will use a random port 1337.
    string serverPort = "1337";
    await socket.ConnectAsync(serverHost, serverPort);

    //Write data to the echo server.
    Stream streamOut = socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string request = "test";
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();

    //Read data from the echo server.
    Stream streamIn = socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string response = await reader.ReadLineAsync();
}
catch (Exception e)
{
    //Handle exception here.            
}
```

## Opérations de base d’un socket UDP

Un socket UDP assure les transferts de données réseau de bas niveau dans chaque direction pour les communications réseau qui ne nécessitent pas une connexion établie. Dans la mesure où les sockets UDP ne conservent pas de connexion sur les deux points de terminaison, ils offrent une solution simple et rapide de mise en réseau entre des ordinateurs distants. Toutefois, les sockets UDP ne garantissent pas l’intégrité des paquets réseau et ne s’assurent pas qu’ils atteignent effectivement la destination distante. Les sockets UDP sont par exemple utilisés par les clients de conversation locale et les clients de découverte de réseau local. Cette section montre comment utiliser la classe [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) pour envoyer et recevoir des messages UDP en créant un client et un serveur echo simples.

**Création d’un serveur echo UDP**

L’exemple de code suivant montre comment créer un objet [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) et le lier à un port spécifique pour que vous puissiez écouter les messages UDP entrants.

```csharp
Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();

socket.MessageReceived += Socket_MessageReceived;

//You can use any port that is not currently in use already on the machine.
string serverPort = "1337";

//Bind the socket to the serverPort so that we can start listening for UDP messages from the UDP echo client.
await socket.BindServiceNameAsync(serverPort);
```

L’exemple de code suivant implémente le gestionnaire d’événements **Socket\_MessageReceived** pour lire un message reçu d’un client et renvoyer le même message.

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo client.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();

    //Create a new socket to send the same message back to the UDP echo client.
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    //Use a separate port number for the UDP echo client because both will be unning on the same machine.
    string clientPort = "1338"
    Stream streamOut = (await socket.GetOutputStreamAsync(args.RemoteAddress, clientPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

**Création d’un client echo UDP**

L’exemple de code suivant montre comment créer un objet [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) et le lier à un port spécifique pour que vous puissiez écouter les messages UDP entrants et envoyer un message UDP au serveur echo UDP.

```csharp
private async void testUdpSocketServer()
{
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    socket.MessageReceived += Socket_MessageReceived;
    
    //You can use any port that is not currently in use already on the machine. We will be using two separate and random 
    //ports for the client and server because both the will be running on the same machine.
    string serverPort = "1337";
    string clientPort = "1338";
    
    //Because we will be running the client and server on the same machine, we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Bind the socket to the clientPort so that we can start listening for UDP messages from the UDP echo server.
    await socket.BindServiceNameAsync(clientPort);
                
    //Write a message to the UDP echo server.
    Stream streamOut = (await socket.GetOutputStreamAsync(serverHost, serverPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string message = "Hello, world!";
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

L’exemple de code suivant implémente le gestionnaire d’événements **Socket\_MessageReceived** pour lire un message reçu du serveur echo UDP.

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, 
    Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo server.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();
}
```

## Opérations en arrière-plan et broker de socket

Si votre application reçoit des connexions ou des données sur des sockets, vous devez être prêt à effectuer ces opérations correctement quand votre application n’est pas au premier plan. Pour ce faire, vous utilisez le broker de socket. Pour plus d’informations concernant la manière d’utiliser le broker de socket, voir [Communications réseau en arrière-plan](network-communications-in-the-background.md).

## Envois par lot

À partir de Windows 10, Windows.Networking.Sockets prend en charge les envois par lot, qui vous permettent d’envoyer ensemble plusieurs tampons de données moyennant une surcharge de basculement entre contextes nettement moindre que celle qu’occasionnerait l’envoi de chaque tampon séparément. Cela est particulièrement utile si votre application effectue des tâches VoIP, VPN ou autres impliquant le déplacement d’un grand nombre de données aussi efficacement que possible.

Chaque appel à WriteAsync sur un socket déclenche une transition de noyau pour atteindre la pile réseau. Quand une application écrit un grand nombre de tampons de données à la fois, chaque écriture entraîne une transition de noyau distincte, qui occasionne une surcharge substantielle. Le nouveau modèle d’envois par lot optimise la fréquence des transitions de noyau. Cette fonctionnalité est actuellement limitée à [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) et aux instances [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) connectées.

Voici par exemple comment une application envoie un grand nombre de tampons de données de manière non optimale.

```csharp
// Send a set of packets inefficiently, one packet at a time.
// This is not recommended if you have many packets to send
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

foreach (IBuffer packet in packetsToSend)
{
    // Incurs kernel transition overhead for each packet
    await outputStream.WriteAsync(packet);
}
```

Cet exemple montre un moyen plus efficace d’envoyer un grand nombre de tampons. Étant donné que cette technique utilise des fonctionnalités propres au langage C#, elle est accessible uniquement aux programmeurs c#. En envoyant plusieurs paquets à la fois, cet exemple permet au système d’effectuer des envois par lot, optimisant ainsi les transitions de noyau pour améliorer les performances.

```csharp
// More efficient way to send packets.
// This way enables the system to do batched sends
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

int i = 0;
Task[] pendingTasks = new Tast[packetsToSend.Count];
foreach (IBuffer packet in packetsToSend)
{
    // track all pending writes as tasks, but do not wait on one before peforming the next
    pendingTasks[i++] = outputStream.WriteAsync(packet).AsTask();
    // Do not modify any buffer' s contents until the pending writes are complete.
}
// Now, wait for all of the pending writes to complete
await Task.WaitAll(pendingTasks);
```

Cet exemple montre une autre façon d’envoyer un grand nombre de tampons d’une manière compatible avec les envois par lot. Par ailleurs, dans la mesure où il n’utilise pas de fonctionnalités spécifiques de C#, il est applicable pour tous les langages (même s’il est illustré ici en C#). Au lieu de cela, il utilise un comportement modifié dans le membre **OutputStream** des classes [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) et [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), qui constitue une nouveauté dans Windows 10.

```csharp
// More efficient way to send packets in Windows 10, using the new behavior of OutputStream.FlushAsync().
int i = 0;
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = socket.OutputStream;

var pendingWrites = new IAsyncOperationWithProgress<uint,uint> [packetsToSend.Count];

foreach (IBuffer packet in packetsToSend)
{
    pendingWrites[i++] = outputStream.WriteAsync(packet);
    // Do not modify any buffer' s contents until the pending writes are complete.
}

// Wait for all pending writes to complete. This step enables batched sends on the output stream.
await outputStream.FlushAsync();
```

Dans les versions antérieures de Windows, **FlushAsync** retournait immédiatement et ne garantissait pas que toutes les opérations sur le flux étaient terminées. Dans Windows 10, le comportement a changé. Il est désormais garanti que **FlushAsync** retourne une fois toutes les opérations sur le flux de sortie terminées.

Certaines limitations importantes découlent de l’utilisation d’écritures par lot dans votre code.

-   Vous ne pouvez pas modifier le contenu des instances **IBuffer** en cours d’écriture tant que l’écriture asynchrone n’est pas terminée.
-   Le modèle **FlushAsync** fonctionne uniquement sur **StreamSocket.OutputStream** et **DatagramSocket.OutputStream**.
-   Le modèle **FlushAsync** fonctionne uniquement à partir de Windows 10.
-   Dans les autres cas, utilisez **Task.WaitAll** au lieu du modèle **FlushAsync**.

## Partage de port pour DatagramSocket

Windows 10 introduit une nouvelle propriété [**DatagramSocketControl**](https://msdn.microsoft.com/library/windows/apps/hh701190), [**MulticastOnly**](https://msdn.microsoft.com/library/windows/apps/dn895368), qui permet de spécifier que le **DatagramSocket** en question est en mesure de coexister avec d’autres sockets multidiffusion Win32 ou WinRT liés à la même adresse/au même port.

## Fourniture d’un certificat client avec la classe StreamSocket

La classe [**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) prend en charge l’utilisation des protocoles SSL/TLS pour authentifier le serveur avec lequel l’application communique. Dans certains cas, l’application doit également s’authentifier auprès du serveur à l’aide d’un certificat client TLS. Dans Windows 10, vous pouvez prévoir un certificat client sur l’objet [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226893) (cela doit être défini avant le début de la négociation TLS). Si le serveur demande le certificat client, Windows répond avec le certificat fourni.

Voici un extrait de code montrant comment implémenter cela :

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

## Exceptions dans Windows.Networking.Sockets

Le constructeur pour la classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) utilisée avec des sockets peut lever une exception si la chaîne passée n’est pas un nom d’hôte valide (c’est-à-dire si elle contient des caractères non autorisés dans un nom d’hôte). Si une application obtient une entrée de l’utilisateur pour la classe **HostName**, le constructeur doit se trouver dans un bloc try/catch. Si une exception est levée, l’application peut notifier l’utilisateur et demander un nouveau nom d’hôte.

L’espace de noms [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) propose des méthodes et des énumérations d’assistance pratiques qui permettent de gérer les erreurs lorsque des sockets et des WebSockets sont utilisés. Ceci peut s’avérer utile pour gérer différemment certaines exceptions réseau dans votre application.

Une erreur rencontrée dans une opération [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) ou [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) est renvoyée sous forme de valeur **HRESULT**. La méthode [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462) sert à convertir une erreur réseau résultant d’une opération de socket en valeur d’énumération [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457). La plupart des valeurs d’énumération **SocketErrorStatus** correspondent à une erreur renvoyée par l’opération de socket Windows native. Une application peut filtrer des valeurs d’énumération **SocketErrorStatus** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Une erreur rencontrée dans une opération [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) est renvoyée sous forme de valeur **HRESULT**. La méthode [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) sert à convertir une erreur réseau résultant d’une opération WebSocket en valeur d’énumération [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818). La plupart des valeurs d’énumération **WebErrorStatus** correspondent à une erreur renvoyée par l’opération cliente HTTP native. Une application peut filtrer sur des valeurs d’énumération **WebErrorStatus** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Pour les erreurs de validation de paramètre, une application peut également utiliser la valeur **HRESULT** à partir de l’exception pour obtenir des informations plus détaillées sur l’erreur à l’origine de l’exception. Les valeurs **HRESULT** possibles sont répertoriées dans le fichier d’en-tête *Winerror.h*. Pour la plupart des erreurs de validation de paramètre, la valeur **HRESULT** renvoyée est **E\_INVALIDARG**.

## API Winsock

Vous pouvez également utiliser [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673) dans votre application UWP. L’API Winsock prise en charge est basée sur celle de Microsoft Silverlight pour Windows Phone 8.1 et continue de prendre en charge la plupart des types, propriétés et méthodes (certaines API jugées obsolètes ont été supprimées). Pour plus d’informations sur la programmation de Winsock, voir [ici](https://msdn.microsoft.com/library/windows/desktop/ms740673).




<!--HONumber=Mar16_HO1-->


