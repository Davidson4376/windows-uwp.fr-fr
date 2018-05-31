---
author: stevewhims
description: Les sockets constituent une technologie de transfert de données de faible niveau en priorité du nombre de protocoles de réseau implémentés. UWP offre les classes TCP et socket UDP pour les applications de serveur client ou pair à paire, que les connexions soient de longue durée ou qu'une connexion établie ne soit pas requise.
title: Sockets
ms.assetid: 23B10A3C-E33F-4CD6-92CB-0FFB491472D6
ms.author: stwhi
ms.date: 11/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 70d51009fc2231900ad0eba8dfc0e706433e2338
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1690985"
---
# <a name="sockets"></a>Sockets
Les sockets constituent une technologie de transfert de données de faible niveau en priorité du nombre de protocoles de réseau implémentés. UWP offre les classes TCP et socket UDP pour les applications de serveur client ou pair à paire, que les connexions soient de longue durée ou qu'une connexion établie ne soit pas requise.

Cette rubrique se concentre sur la façon d’utiliser les classes de socket de plateforme Windows universelle (UWP) qui se trouvent dans l'espace de noms [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets?branch=live). Vous pouvez également utiliser [Windows Sockets 2 (Winsock)](https://msdn.microsoft.com/library/windows/desktop/ms740673) dans une application UWP.

> [!NOTE]
> En conséquence l'[isolement réseau](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx), Windows refuse l’établissement d'une connexion de socket (Sockets ou WinSock) entre deux applications UWP qui s’exécutent sur le même ordinateur via soit l’adresse de bouclage locale (127.0.0.0) ou en spécifiant explicitement l’adresse IP locale. Pour plus d’informations sur les mécanismes par lesquels les applications UWP peuvent communiquer entre elles, consultez [Communication entre les applications](/windows/uwp/app-to-app/index?branch=live).

## <a name="build-a-basic-tcp-socket-client-and-server"></a>Générer un client et un serveur de socket TCP de base
Une socket TCP fournit des transferts de données réseau de bas niveau dans chaque direction pour des connexions à longue durée de vie. Les sockets TCP sont la fonctionnalité sous-jacente utilisée par la plupart des protocoles réseau utilisés sur Internet. Pour illustrer les opérations TCP de base, l'exemple de code ci-dessus affiche une [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live) et une [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener?branch=live) envoyant et recevant des données via TCP pour former un écho client et serveur.

Pour commencer avec le moins d'éléments mobiles que possible&mdash;et éviter les problèmes d’isolement réseau pour le présent&mdash;créez un nouveau projet et placez le client et le code de serveur ci-dessous dans le même projet.

Vous devrez [déclarer une fonctionnalité d’application](../packaging/app-capability-declarations.md) dans votre projet. Ouvrez votre fichier source de manifeste de package d'application (le fichier `Package.appxmanifest`) et, sous l’onglet Fonctionnalités, cochez **Réseaux privés (Client et serveur)**. Voici comment s’affiche le balisage `Package.appxmanifest`.

```xml
<Capability Name="privateNetworkClientServer" />
```

Au lieu de `privateNetworkClientServer`, vous pouvez déclarer `internetClientServer` si vous vous connectez via Internet. **StreamSocket** et **StreamSocketListener** ont toutes deux besoin de l'une de ces fonctionnalités d'application à déclarer.

### <a name="an-echo-client-and-server-using-tcp-sockets"></a>Un écho client et serveur à l'aide de sockets TCP
Construire une [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener?branch=live) et commencer à écouter les connexions TCP entrantes. L'événement [**StreamSocketListener.ConnectionReceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived) est déclenché chaque fois qu’un client établit une connexion avec la **StreamSocketListener **.

Construisez également un [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live), établissez une connexion au serveur, envoyez une demande et recevez une réponse.

Créez une **Page** nommée `StreamSocketAndListenerPage`. Placez le balisage XAML dans `StreamSocketAndListenerPage.xaml`et placez le code impératif à l’intérieur de la classe `StreamSocketAndListenerPage`.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel>
        <TextBlock Margin="9.6,0" Style="{StaticResource TitleTextBlockStyle}" Text="TCP socket example"/>
        <TextBlock Margin="7.2,0,0,0" Style="{StaticResource HeaderTextBlockStyle}" Text="StreamSocket &amp; StreamSocketListener"/>
    </StackPanel>

    <Grid Grid.Row="1">
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <TextBlock Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="client"/>
        <ListBox x:Name="clientListBox" Grid.Row="1" Margin="9.6"/>
        <TextBlock Grid.Column="1" Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="server"/>
        <ListBox x:Name="serverListBox" Grid.Column="1" Grid.Row="1" Margin="9.6"/>
    </Grid>
</Grid>
```

```csharp
// Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
// For this example, we'll choose an arbitrary port number.
static string PortNumber = "1337";

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.StartServer();
    this.StartClient();
}

private async void StartServer()
{
    try
    {
        var streamSocketListener = new Windows.Networking.Sockets.StreamSocketListener();

        // The ConnectionReceived event is raised when connections are received.
        streamSocketListener.ConnectionReceived += this.StreamSocketListener_ConnectionReceived;

        // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
        await streamSocketListener.BindServiceNameAsync(StreamSocketAndListenerPage.PortNumber);

        this.serverListBox.Items.Add("server is listening...");
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.serverListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void StreamSocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    string request;
    using (var streamReader = new StreamReader(args.Socket.InputStream.AsStreamForRead()))
    {
        request = await streamReader.ReadLineAsync();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server received the request: \"{0}\"", request)));

    // Echo the request back as the response.
    using (Stream outputStream = args.Socket.OutputStream.AsStreamForWrite())
    {
        using (var streamWriter = new StreamWriter(outputStream))
        {
            await streamWriter.WriteLineAsync(request);
            await streamWriter.FlushAsync();
        }
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server sent back the response: \"{0}\"", request)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add("server closed its socket"));
}

private async void StartClient()
{
    try
    {
        // Create the StreamSocket and establish a connection to the echo server.
        using (var streamSocket = new Windows.Networking.Sockets.StreamSocket())
        {
            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            var hostName = new Windows.Networking.HostName("localhost");

            this.clientListBox.Items.Add("client is trying to connect...");

            await streamSocket.ConnectAsync(hostName, StreamSocketAndListenerPage.PortNumber);

            this.clientListBox.Items.Add("client connected");

            // Send a request to the echo server.
            string request = "Hello, World!";
            using (Stream outputStream = streamSocket.OutputStream.AsStreamForWrite())
            {
                using (var streamWriter = new StreamWriter(outputStream))
                {
                    await streamWriter.WriteLineAsync(request);
                    await streamWriter.FlushAsync();
                }
            }

            this.clientListBox.Items.Add(string.Format("client sent the request: \"{0}\"", request));

            // Read data from the echo server.
            string response;
            using (Stream inputStream = streamSocket.InputStream.AsStreamForRead())
            {
                using (StreamReader streamReader = new StreamReader(inputStream))
                {
                    response = await streamReader.ReadLineAsync();
                }
            }

            this.clientListBox.Items.Add(string.Format("client received the response: \"{0}\" ", response));
        }

        this.clientListBox.Items.Add("client closed its socket");
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.clientListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}
```

```cpp
#include <ppltasks.h>
#include <sstream>

    ...
    
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;

    ...

private:
    Windows::Networking::Sockets::StreamSocketListener^ streamSocketListener;
    Windows::Networking::Sockets::StreamSocket^ streamSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->StartServer();
        this->StartClient();
    }

private:
    void StartServer()
    {
        try
        {
            this->streamSocketListener = ref new Windows::Networking::Sockets::StreamSocketListener();

            // The ConnectionReceived event is raised when connections are received.
            streamSocketListener->ConnectionReceived += ref new TypedEventHandler<Windows::Networking::Sockets::StreamSocketListener^, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^>(this, &StreamSocketAndListenerPage::StreamSocketListener_ConnectionReceived);

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            // Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
            // For this example, we'll choose an arbitrary port number.
            Concurrency::create_task(streamSocketListener->BindServiceNameAsync(L"1337")).then(
                [=]
            {
                this->serverListBox->Items->Append(L"server is listening...");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
    {
        try
        {
            auto dataReader = ref new DataReader(args->Socket->InputStream);

            Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
                [=](unsigned int bytesLoaded)
            {
                unsigned int stringLength = dataReader->ReadUInt32();
                Concurrency::create_task(dataReader->LoadAsync(stringLength)).then(
                    [=](unsigned int bytesLoaded)
                {
                    Platform::String^ request = dataReader->ReadString(bytesLoaded);
                    this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                        [=]
                    {
                        std::wstringstream wstringstream;
                        wstringstream << L"server received the request: \"" << request->Data() << L"\"";
                        this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                    }));

                    // Echo the request back as the response.
                    auto dataWriter = ref new DataWriter(args->Socket->OutputStream);
                    dataWriter->WriteUInt32(request->Length());
                    dataWriter->WriteString(request);
                    Concurrency::create_task(dataWriter->StoreAsync()).then(
                        [=](unsigned int)
                    {
                        dataWriter->DetachStream();

                        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                            [=]()
                        {
                            std::wstringstream wstringstream;
                            wstringstream << L"server sent back the response: \"" << request->Data() << L"\"";
                            this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                        }));

                        delete this->streamSocketListener;
                        this->streamSocketListener = nullptr;

                        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(L"server closed its socket"); }));
                    });
                });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message); }));
        }
    }

    void StartClient()
    {
        try
        {
            // Create the StreamSocket and establish a connection to the echo server.
            this->streamSocket = ref new Windows::Networking::Sockets::StreamSocket();

            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            auto hostName = ref new Windows::Networking::HostName(L"localhost");

            this->clientListBox->Items->Append(L"client is trying to connect...");

            Concurrency::create_task(this->streamSocket->ConnectAsync(hostName, L"1337")).then(
                [=](Concurrency::task< void >)
            {
                this->clientListBox->Items->Append(L"client connected");

                // Send a request to the echo server.
                auto dataWriter = ref new DataWriter(this->streamSocket->OutputStream);
                auto request = ref new Platform::String(L"Hello, World!");
                dataWriter->WriteUInt32(request->Length());
                dataWriter->WriteString(request);

                Concurrency::create_task(dataWriter->StoreAsync()).then(
                    [=](Concurrency::task< unsigned int >)
                {
                    std::wstringstream wstringstream;
                    wstringstream << L"client sent the request: \"" << request->Data() << L"\"";
                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                    Concurrency::create_task(dataWriter->FlushAsync()).then(
                        [=](Concurrency::task< bool >)
                    {
                        dataWriter->DetachStream();

                        // Read data from the echo server.
                        auto dataReader = ref new DataReader(this->streamSocket->InputStream);
                        Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
                            [=](unsigned int bytesLoaded)
                        {
                            unsigned int stringLength = dataReader->ReadUInt32();
                            Concurrency::create_task(dataReader->LoadAsync(stringLength)).then(
                                [=](unsigned int bytesLoaded)
                            {
                                Platform::String^ response = dataReader->ReadString(bytesLoaded);
                                this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                                    [=]
                                {
                                    std::wstringstream wstringstream;
                                    wstringstream << L"client received the response: \"" << response->Data() << L"\"";
                                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                                    delete this->streamSocket;
                                    this->streamSocket = nullptr;

                                    this->clientListBox->Items->Append(L"client closed its socket");
                                }));
                            });
                        });
                    });
                });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }
```

## <a name="references-to-streamsockets-in-c-ppl-continuations"></a>Références à StreamSockets dans les continuations PPL C++
Une [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live) reste active tant que lecture/écriture active se trouve sur son flux d'entrée/de sortie (prenons l'exemple de [**StreamSocketListenerConnectionReceivedEventArgs.Socket**](/uwp/api/windows.networking.sockets.streamsocketlistenerconnectionreceivedeventargs.Socket) auquel vous avez accès dans votre gestionnaire d'événement [**StreamSocketListener.ConnectionReceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived)). Lorsque vous appelez [**DataReader.LoadAsync**](/uwp/api/windows.storage.streams.datareader.loadasync?branch=live) (ou `ReadAsync/WriteAsync/StoreAsync`), alors il contient une référence à la socket (via le flux d’entrée de la socket) jusqu'à ce que le gestionnaire d’achèvement de l'élément **LoadAsync** ait terminé son exécution.

Néanmoins, la bibliothèque de motif parallèle (PPL) l'inclue aucune continuation de tâche programmée et incorporée par défaut. En d’autres termes, l'ajout d’une tâche de continuation (avec `task::then()`) ne garantit pas que la tâche de continuation s’exécute de manière incorporée en tant que gestionnaire d’achèvement.

```cpp
void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    auto dataReader = ref new DataReader(args->Socket->InputStream);
    Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
        [=](unsigned int bytesLoaded)
    {
        // Work in here isn't guaranteed to execute inline as the completion handler of the LoadAsync.
    });
}
```

Du point de vue de la **StreamSocket**, le gestionnaire d’achèvement termine son exécution (et la socket est éligible pour être cédée) avant l'exécution du corps de la continuation. Par conséquent, afin que votre socket ne soit pas cédée si vous souhaitez l'utiliser à l'intérieur de cette continuation, vous devez soit faire référence à la socket de manière directe (via une capture lambda) et l'utiliser, soit de manière indirecte (en poursuivant l'accès `args->Socket` à l'intérieure des continuations), ou forcer les tâches de continuation à être incorporées. Vous pouvez découvrir la première technique (capture lambda) en action dans l'[Exemple de StreamSocket](http://go.microsoft.com/fwlink/p/?LinkId=620609). Le code C++ de la section [Générer un client et un serveur de socket TCP de base](#build-a-basic-tcp-socket-client-and-server) ci-dessus utilise la deuxième technique&mdash;il fait écho à la demande en réponse et il accède à `args->Socket` à partir de l'une des continuations les plus profondes.

La troisième technique est appropriée lorsque vous ne faites pas écho à une réponse en retour. Vous utilisez l'option `task_continuation_context::use_synchronous_execution()` pour forcer PPL à exécuter le corps de continuation incorporé. Voici un exemple de code illustrant comment procéder.

```cpp
void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    auto dataReader = ref new DataReader(args->Socket->InputStream);

    Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
        [=](unsigned int bytesLoaded)
    {
        unsigned int messageLength = dataReader->ReadUInt32();
        Concurrency::create_task(dataReader->LoadAsync(messageLength)).then(
            [=](unsigned int bytesLoaded)
        {
            Platform::String^ request = dataReader->ReadString(bytesLoaded);
            this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                [=]
            {
                std::wstringstream wstringstream;
                wstringstream << L"server received the request: \"" << request->Data() << L"\"";
                this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
            }));
        });
    }, Concurrency::task_continuation_context::use_synchronous_execution());
}
```

Ce comportement s'applique à toutes les sockets et à toutes les classes WebSockets dans l'espace de noms [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets?branch=live). Toutefois, les scénarios côté client stockent généralement les sockets dans les variables de membre. Ainsi, le problèmes s'applique particulièrement au scénario [**StreamSocketListener.ConnectionReceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived), tel qu'illustré ci-dessus.

## <a name="build-a-basic-udp-socket-client-and-server"></a>Générer un client et un serveur de socket UDP de base
La socket UDP (User Datagram Protocol) est similaire à la socket TCP. En effet, elle fournit également des transferts de données de réseau de niveau inférieur dans les deux directions. Néanmoins, alors qu'une socket TCP est destinée aux connexions de longue durée, une socket UDP est destinée aux applications pour lesquelles une connexion établie n'est pas requise. Dans la mesure où les sockets UDP ne conservent pas de connexion sur les deux points de terminaison, ils constituent une solution simple et rapide de mise en réseau entre des ordinateurs distants. Néanmoins, les sockets UDP ne garantissent pas l’intégrité des paquets réseau et ne s’assurent pas que les paquets atteignent effectivement la destination distante. De ce fait, votre application doit être conçue pour le tolérer. Par exemple, les sockets UDP sont utilisés par les clients de conversation locale et les clients de découverte de réseau local.

Pour illustrer les opérations UDP de base, l'exemple de code ci-dessus affiche la classe [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live) envoyant et recevant à la fois des données via UDP pour former un écho client et serveur. Créez un projet et placez le code client et le code serveur ci-dessous dans le même projet. Tout comme pour les sockets TCP, vous devez déclarer la fonctionnalité d'application **Réseaux privés (client et serveur)**.

### <a name="an-echo-client-and-server-using-udp-sockets"></a>Un écho client et serveur à l'aide de sockets UDP
Construisez une [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live) qui jouera le rôle du serveur d'écho. Elle l'insèrera dans un nombre de port spécifique, écoutera le message UDP entrant et enverra une réponse en écho. L'événement [**DatagramSocket.MessageReceived**](/uwp/api/Windows.Networking.Sockets.DatagramSocket.MessageReceived) est déclenché lorsqu’un message est reçu sur la socket.

Construisez une autre **DatagramSocket** qui jouera le rôle du client d'écho. Elle l'insèrera dans un nombre de port spécifique, enverra le message UDP et recevra une réponse.

Placez le balisage XAML dans `MainPage.xaml`et placez le code impératif à l’intérieur de votre classe `MainPage`.

Créez une **Page** nommée `DatagramSocketPage`. Placez le balisage XAML dans `DatagramSocketPage.xaml`et placez le code impératif à l’intérieur de la classe `DatagramSocketPage`.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel>
        <TextBlock Margin="9.6,0" Style="{StaticResource TitleTextBlockStyle}" Text="UDP socket example"/>
        <TextBlock Margin="7.2,0,0,0" Style="{StaticResource HeaderTextBlockStyle}" Text="DatagramSocket"/>
    </StackPanel>

    <Grid Grid.Row="1">
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <TextBlock Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="client"/>
        <ListBox x:Name="clientListBox" Grid.Row="1" Margin="9.6"/>
        <TextBlock Grid.Column="1" Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="server"/>
        <ListBox x:Name="serverListBox" Grid.Column="1" Grid.Row="1" Margin="9.6"/>
    </Grid>
</Grid>
```

```csharp
// Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
// For this example, we'll choose different arbitrary port numbers for client and server, since both will be running on the same machine.
static string ClientPortNumber = "1336";
static string ServerPortNumber = "1337";

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.StartServer();
    this.StartClient();
}

private async void StartServer()
{
    try
    {
        var serverDatagramSocket = new Windows.Networking.Sockets.DatagramSocket();

        // The ConnectionReceived event is raised when connections are received.
        serverDatagramSocket.MessageReceived += ServerDatagramSocket_MessageReceived;

        this.serverListBox.Items.Add("server is about to bind...");

        // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
        await serverDatagramSocket.BindServiceNameAsync(DatagramSocketPage.ServerPortNumber);

        this.serverListBox.Items.Add(string.Format("server is bound to port number {0}", DatagramSocketPage.ServerPortNumber));
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.serverListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void ServerDatagramSocket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    string request;
    using (DataReader dataReader = args.GetDataReader())
    {
        request = dataReader.ReadString(dataReader.UnconsumedBufferLength).Trim();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server received the request: \"{0}\"", request)));

    // Echo the request back as the response.
    using (Stream outputStream = (await sender.GetOutputStreamAsync(args.RemoteAddress, DatagramSocketPage.ClientPortNumber)).AsStreamForWrite())
    {
        using (var streamWriter = new StreamWriter(outputStream))
        {
            await streamWriter.WriteLineAsync(request);
            await streamWriter.FlushAsync();
        }
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server sent back the response: \"{0}\"", request)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add("server closed its socket"));
}

private async void StartClient()
{
    try
    {
        // Create the DatagramSocket and establish a connection to the echo server.
        var clientDatagramSocket = new Windows.Networking.Sockets.DatagramSocket();

        clientDatagramSocket.MessageReceived += ClientDatagramSocket_MessageReceived;

        // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
        var hostName = new Windows.Networking.HostName("localhost");

        this.clientListBox.Items.Add("client is about to bind...");

        await clientDatagramSocket.BindServiceNameAsync(DatagramSocketPage.ClientPortNumber);

        this.clientListBox.Items.Add(string.Format("client is bound to port number {0}", DatagramSocketPage.ClientPortNumber));

        // Send a request to the echo server.
        string request = "Hello, World!";
        using (var serverDatagramSocket = new Windows.Networking.Sockets.DatagramSocket())
        {
            using (Stream outputStream = (await serverDatagramSocket.GetOutputStreamAsync(hostName, DatagramSocketPage.ServerPortNumber)).AsStreamForWrite())
            {
                using (var streamWriter = new StreamWriter(outputStream))
                {
                    await streamWriter.WriteLineAsync(request);
                    await streamWriter.FlushAsync();
                }
            }
        }

        this.clientListBox.Items.Add(string.Format("client sent the request: \"{0}\"", request));
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.clientListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void ClientDatagramSocket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    string response;
    using (DataReader dataReader = args.GetDataReader())
    {
        response = dataReader.ReadString(dataReader.UnconsumedBufferLength).Trim();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.clientListBox.Items.Add(string.Format("client received the response: \"{0}\"", response)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.clientListBox.Items.Add("client closed its socket"));
}
```

```cpp
#include <ppltasks.h>
#include <sstream>

    ...

using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;

    ...

private:
    Windows::Networking::Sockets::DatagramSocket^ clientDatagramSocket;
    Windows::Networking::Sockets::DatagramSocket^ serverDatagramSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->StartServer();
        this->StartClient();
    }

private:
    void StartServer()
    {
        try
        {
            this->serverDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();

            // The ConnectionReceived event is raised when connections are received.
            this->serverDatagramSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::DatagramSocket^, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^>(this, &DatagramSocketPage::ServerDatagramSocket_MessageReceived);

            this->serverListBox->Items->Append(L"server is about to bind...");

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            Concurrency::create_task(this->serverDatagramSocket->BindServiceNameAsync("1337")).then(
                [=]
            {
                this->serverListBox->Items->Append(L"server is bound to port number 1337");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void ServerDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket^ sender, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^ args)
    {
        DataReader^ dataReader = args->GetDataReader();
        Platform::String^ request = dataReader->ReadString(dataReader->UnconsumedBufferLength);
        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
            [=]
        {
            std::wstringstream wstringstream;
            wstringstream << L"server received the request: \"" << request->Data() << L"\"";
            this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
        }));

        // Echo the request back as the response.
        Concurrency::create_task(sender->GetOutputStreamAsync(args->RemoteAddress, "1336")).then(
            [=](IOutputStream^ outputStream)
        {
            auto dataWriter = ref new DataWriter(outputStream);
            dataWriter->WriteString(request);
            Concurrency::create_task(dataWriter->StoreAsync()).then(
                [=](unsigned int)
            {
                dataWriter->DetachStream();

                this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                    [=]()
                {
                    std::wstringstream wstringstream;
                    wstringstream << L"server sent back the response: \"" << request->Data() << L"\"";
                    this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                    delete this->serverDatagramSocket;
                    this->serverDatagramSocket = nullptr;

                    this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(L"server closed its socket"); }));
                }));
            });
        });
    }

    void StartClient()
    {
        try
        {
            // Create the DatagramSocket and establish a connection to the echo server.
            this->clientDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();

            this->clientDatagramSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::DatagramSocket^, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^>(this, &DatagramSocketPage::ClientDatagramSocket_MessageReceived);

            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            auto hostName = ref new Windows::Networking::HostName(L"localhost");

            this->clientListBox->Items->Append(L"client is about to bind...");

            Concurrency::create_task(this->clientDatagramSocket->BindServiceNameAsync("1336")).then(
                [=]
            {
                this->clientListBox->Items->Append(L"client is bound to port number 1336");
            });

            // Send a request to the echo server.
            auto serverDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();
            Concurrency::create_task(serverDatagramSocket->GetOutputStreamAsync(hostName, "1337")).then(
                [=](IOutputStream^ outputStream)
            {
                auto request = ref new Platform::String(L"Hello, World!");
                auto dataWriter = ref new DataWriter(outputStream);
                dataWriter->WriteString(request);
                Concurrency::create_task(dataWriter->StoreAsync()).then(
                    [=](unsigned int)
                {
                    dataWriter->DetachStream();
                    std::wstringstream wstringstream;
                    wstringstream << L"client sent the request: \"" << request->Data() << L"\"";
                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                });

            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void ClientDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket^ sender, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^ args)
    {
        DataReader^ dataReader = args->GetDataReader();
        Platform::String^ response = dataReader->ReadString(dataReader->UnconsumedBufferLength);
        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
            [=]
        {
            std::wstringstream wstringstream;
            wstringstream << L"client received the response: \"" << response->Data() << L"\"";
            this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
        }));

        delete this->clientDatagramSocket;
        this->clientDatagramSocket = nullptr;

        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->clientListBox->Items->Append(L"client closed its socket"); }));
    }
```

## <a name="background-operations-and-the-socket-broker"></a>Opérations en arrière-plan et broker de socket
Vous pouvez utiliser le broker de socket et contrôle les déclencheurs de canal. Ainsi, vous garantissez que votre application reçoit correctement les connections ou données sur les sockets alors qu'elles ne sont pas sur le premier plan. Pour plus d'informations, consultez [Communications réseau en arrière-plan](network-communications-in-the-background.md).

## <a name="batched-sends"></a>Envois par lot
Lorsque vous écrivez sur le flux associé à la socket, une transition se produit à partir du mode utilisateur (votre code) jusqu'au mode noyau (là où se trouve la pile réseau). Si vous écrivez un grand nombre de tampons à la fois, ces transitions répétées composent dans une surcharge substantielle. Le traitement par lot de vos envois constitue un moyen d'envoyer plusieurs tampons de données ensemble et évite ainsi la surcharge. Cela est particulièrement utile si votre application effectue des tâches VoIP, VPN ou autres impliquant le déplacement d’un grand nombre de données aussi efficacement que possible.

Cette section illustre un couple de techniques d'envoi par lot que vous pouvez utiliser avec une [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live) ou une [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live) connectée.

Pour obtenir un planning de référence, découvrons comment envoyer un grand nombre de tampons de manière efficace. Voici une démonstration minimale utilisant un **StreamSocket**.

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    var streamSocketListener = new Windows.Networking.Sockets.StreamSocketListener();
    streamSocketListener.ConnectionReceived += this.StreamSocketListener_ConnectionReceived;
    await streamSocketListener.BindServiceNameAsync("1337");

    var streamSocket = new Windows.Networking.Sockets.StreamSocket();
    await streamSocket.ConnectAsync(new Windows.Networking.HostName("localhost"), "1337");
    this.SendMultipleBuffersInefficiently(streamSocket, "Hello, World!");
    //this.BatchedSendsCSharpOnly(streamSocket, "Hello, World!");
    //this.BatchedSendsAnyUWPLanguage(streamSocket, "Hello, World!");
}

private async void StreamSocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    using (var dataReader = new DataReader(args.Socket.InputStream))
    {
        dataReader.InputStreamOptions = InputStreamOptions.Partial;
        while (true)
        {
            await dataReader.LoadAsync(256);
            if (dataReader.UnconsumedBufferLength == 0) break;
            IBuffer requestBuffer = dataReader.ReadBuffer(dataReader.UnconsumedBufferLength);
            string request = Windows.Security.Cryptography.CryptographicBuffer.ConvertBinaryToString(Windows.Security.Cryptography.BinaryStringEncoding.Utf8, requestBuffer);
            Debug.WriteLine(string.Format("server received the request: \"{0}\"", request));
        }
    }
}

// This implementation incurs kernel transition overhead for each packet written.
private async void SendMultipleBuffersInefficiently(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    foreach (IBuffer packet in packetsToSend)
    {
        await streamSocket.OutputStream.WriteAsync(packet);
    }
}
```

```cpp
#include <ppltasks.h>
#include <sstream>

    ...

using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;

    ...

private:
    Windows::Networking::Sockets::StreamSocketListener^ streamSocketListener;
    Windows::Networking::Sockets::StreamSocket^ streamSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->streamSocketListener = ref new Windows::Networking::Sockets::StreamSocketListener();
        streamSocketListener->ConnectionReceived += ref new TypedEventHandler<Windows::Networking::Sockets::StreamSocketListener^, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^>(this, &BatchedSendsPage::StreamSocketListener_ConnectionReceived);
        Concurrency::create_task(this->streamSocketListener->BindServiceNameAsync(L"1337")).then(
            [=]
        {
            this->streamSocket = ref new Windows::Networking::Sockets::StreamSocket();
            Concurrency::create_task(this->streamSocket->ConnectAsync(ref new Windows::Networking::HostName(L"localhost"), L"1337")).then(
                [=](Concurrency::task< void >)
            {
                this->SendMultipleBuffersInefficiently(L"Hello, World!");
                // this->BatchedSendsAnyUWPLanguage(L"Hello, World!");
            }, Concurrency::task_continuation_context::use_synchronous_execution());
        });
    }

private:
    void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
    {
        auto dataReader = ref new DataReader(args->Socket->InputStream);
        dataReader->InputStreamOptions = Windows::Storage::Streams::InputStreamOptions::Partial;
        this->ReceiveStringRecurse(dataReader, args->Socket);
    }

    void ReceiveStringRecurse(DataReader^ dataReader, Windows::Networking::Sockets::StreamSocket^ streamSocket)
    {
        Concurrency::create_task(dataReader->LoadAsync(256)).then(
            [this, dataReader, streamSocket](unsigned int bytesLoaded)
        {
            if (bytesLoaded == 0) return;
            Platform::String^ message = dataReader->ReadString(bytesLoaded);
            ::OutputDebugString(message->Data());
            this->ReceiveStringRecurse(dataReader, streamSocket);
        });
    }

    // This implementation incurs kernel transition overhead for each packet written.
    void SendMultipleBuffersInefficiently(Platform::String^ message)
    {
        std::vector< IBuffer^ > packetsToSend{};
        for (unsigned int count = 0; count < 5; ++count)
        {
            packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
        }

        for (auto element : packetsToSend)
        {
            Concurrency::create_task(this->streamSocket->OutputStream->WriteAsync(element)).wait();
        }
    }
```

Ce premier exemple de technique plus efficace est uniquement adéquat si vous utilisez C#. Modifiez `OnNavigatedTo` pour appeler `BatchedSendsCSharpOnly` au lieu de `SendMultipleBuffersInefficiently`.

```csharp
// A C#-only technique for batched sends.
private async void BatchedSendsCSharpOnly(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    var pendingTasks = new System.Threading.Tasks.Task[packetsToSend.Count];

    for (int index = 0; index < packetsToSend.Count; ++index)
    {
        // track all pending writes as tasks, but don't wait on one before beginning the next.
        pendingTasks[index] = streamSocket.OutputStream.WriteAsync(packetsToSend[index]).AsTask();
        // Don't modify any buffer's contents until the pending writes are complete.
    }

    // Wait for all of the pending writes to complete.
    System.Threading.Tasks.Task.WaitAll(pendingTasks);
}
```

L’exemple suivant est adéquat pour n’importe quelle langue UWP et pas seulement pour C# (mais illustré ici en C#). Elle repose sur le comportement dans [**StreamSocket.OutputStream**](/uwp/api/windows.networking.sockets.streamsocket.OutputStream) et [**DatagramSocket.OutputStream**](/uwp/api/windows.networking.sockets.datagramsocket.OutputStream) qui envoient les lots ensemble. La technique appelle [**FlushAsync**](/uwp/api/windows.storage.streams.ioutputstream.FlushAsync) sur ce flux de sortie qui, dans le cadre de Windows10, est assuré de revenir uniquement après l'achèvement de toutes les opérations sur le flux de sortie.

```csharp
// An implementation of batched sends suitable for any UWP language.
private async void BatchedSendsAnyUWPLanguage(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    var pendingWrites = new IAsyncOperationWithProgress<uint, uint>[packetsToSend.Count];

    for (int index = 0; index < packetsToSend.Count; ++index)
    {
        // track all pending writes as tasks, but don't wait on one before beginning the next.
        pendingWrites[index] = streamSocket.OutputStream.WriteAsync(packetsToSend[index]);
        // Don't modify any buffer's contents until the pending writes are complete.
    }

    // Wait for all of the pending writes to complete. This step enables batched sends on the output stream.
    await streamSocket.OutputStream.FlushAsync();
}
```

```cpp
private:
    // An implementation of batched sends suitable for any UWP language.
    void BatchedSendsAnyUWPLanguage(Platform::String^ message)
    {
        std::vector< IBuffer^ > packetsToSend{};
        std::vector< IAsyncOperationWithProgress< unsigned int, unsigned int >^ >pendingWrites{};
        for (unsigned int count = 0; count < 5; ++count)
        {
            packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
        }

        for (auto element : packetsToSend)
        {
            // track all pending writes as tasks, but don't wait on one before beginning the next.
            pendingWrites.push_back(this->streamSocket->OutputStream->WriteAsync(element));
            // Don't modify any buffer's contents until the pending writes are complete.
        }

        // Wait for all of the pending writes to complete. This step enables batched sends on the output stream.
        Concurrency::create_task(this->streamSocket->OutputStream->FlushAsync());
    }
```

Certaines limitations importantes découlent de l’utilisation d’envoi par lot dans votre code.

-   Vous ne pouvez pas modifier le contenu des instances **IBuffer** en cours d’écriture tant que l’écriture asynchrone n’est pas terminée.
-   Le modèle **FlushAsync** fonctionne uniquement sur **StreamSocket.OutputStream** et **DatagramSocket.OutputStream**.
-   Le modèle **FlushAsync** fonctionne uniquement à partir de Windows 10.
-   Dans les autres cas, utilisez [**Task.WaitAll**](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.waitall?view=netcore-2.0#System_Threading_Tasks_Task_WaitAll_System_Threading_Tasks_Task___) au lieu du modèle **FlushAsync**.

## <a name="port-sharing-for-datagramsocket"></a>Partage de port pour DatagramSocket
Vous pouvez configurer une [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live) pour qu'elle coexiste avec d’autres sockets Win32 ou UWP en multidiffusion, liée à la même adresse et au même port. Pour cela, définissez la [**DatagramSocketControl.MulticastOnly**](/uwp/api/Windows.Networking.Sockets.DatagramSocketControl.MulticastOnly) sur `true` avant de lier ou de connecter la socket. Vous accédez à une instance de **DatagramSocketControl** à partir de l'objet **DatagramSocket** lui-même via sa propriété [**DatagramSocket.Control**](/uwp/api/windows.networking.sockets.datagramsocket.Control).

## <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>Fourniture d’un certificat client avec la classe StreamSocket
[**Windows.Networking.StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live) prend en charge l’utilisation des protocoles SSL/TLS pour authentifier le serveur avec lequel l’application client communique. Dans certains cas, l’application client doit également s’authentifier auprès du serveur à l’aide d’un certificat client SSL/TLS. Vous pouvez fournir au certificat client la propriété [**StreamSocketControl.ClientCertificate**](/uwp/api/windows.networking.sockets.streamsocketcontrol.ClientCertificate) avant de lier ou de connecter la socket (elle doit être définie avant l'établissement d'une liaison SSL/TLS). Vous accédez à une instance de **StreamSocketControl** à partir de l'objet **StreamSocket** lui-même via sa propriété [**StreamSocket.Control**](/uwp/api/windows.networking.sockets.streamsocket.Control). Si le serveur demande le certificat client, Windows répond avec le certificat client que vous avez fourni.

Utilisez un remplacement de [**StreamSocket.ConnectAsync**](/uwp/api/windows.networking.sockets.streamsocket.connectasync?branch=live) qui emploi un [**SocketProtectionLevel**](/uwp/api/windows.networking.sockets.socketprotectionlevel?branch=live), comme illustré dans cet exemple de code minimal.

```csharp
// For this code to work, you need at least one certificate to be present in the user MY certificate store.
// Plugging a smartcard into a smartcard reader connected to your PC will achieve that.
// Also, your project needs to declare the sharedUserCertificates app capability.
var certificateQuery = new Windows.Security.Cryptography.Certificates.CertificateQuery();
certificateQuery.StoreName = "MY";
IReadOnlyList<Windows.Security.Cryptography.Certificates.Certificate> certificates = await Windows.Security.Cryptography.Certificates.CertificateStores.FindAllAsync(certificateQuery);
if (certificates.Count > 0)
{
    streamSocket.Control.ClientCertificate = certificates[0];
    await streamSocket.ConnectAsync(hostName, "1337", Windows.Networking.Sockets.SocketProtectionLevel.Tls12);
}
```

```cpp
// For this code to work, you need at least one certificate to be present in the user MY certificate store.
// Plugging a smartcard into a smartcard reader connected to your PC will achieve that.
// Also, your project needs to declare the sharedUserCertificates app capability.
auto certificateQuery = ref new Windows::Security::Cryptography::Certificates::CertificateQuery();
certificateQuery->StoreName = L"MY";
Concurrency::create_task(Windows::Security::Cryptography::Certificates::CertificateStores::FindAllAsync(certificateQuery)).then(
    [=](IVectorView< Windows::Security::Cryptography::Certificates::Certificate^ >^ certificates)
{
    if (certificates->Size > 0)
    {
        this->streamSocket->Control->ClientCertificate = certificates->GetAt(0);
        Concurrency::create_task(this->streamSocket->ConnectAsync(ref new Windows::Networking::HostName(L"localhost"), L"1337", Windows::Networking::Sockets::SocketProtectionLevel::Tls12)).then(
            [=]
        {
            ...
        });
    }
});
```

## <a name="handling-exceptions"></a>Traitement des exceptions
Une erreur rencontrée dans une opération [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live), [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live) ou [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener?branch=live) est renvoyée sous forme de valeur **HRESULT**. Vous pouvez transmettre cette valeur **HRESULT** à la méthode [**SocketError.GetStatus**](/uwp/api/windows.networking.sockets.socketerror.getstatus?branch=live) pour la convertir en une valeur d'énumération [**SocketErrorStatus**](/uwp/api/Windows.Networking.Sockets.SocketErrorStatus?branch=live).

La plupart des valeurs d’énumération **SocketErrorStatus** correspondent à une erreur renvoyée par l’opération de socket Windows native. Votre application peut activer les valeurs d’énumération **WebErrorStatus** pour modifier son comportement en fonction de la cause de l’exception.

Pour les erreurs de validation de paramètre, vous pouvez utiliser la valeur **HRESULT** de l’exception pour obtenir des informations plus détaillées sur l’erreur. Les valeurs **HRESULT** possibles sont répertoriées dans `Winerror.h`, disponible dans votre installation de kit SDK (par exemple, dans le dossier `C:\Program Files (x86)\Windows Kits\10\Include\<VERSION>\shared`). Pour la plupart des erreurs de validation de paramètre, la valeur **HRESULT** renvoyée est **E_INVALIDARG**.

Le constructeur [**HostName**](/uwp/api/Windows.Networking.HostName?branch=live) peut produire une exception si la chaîne transmise n’est pas un nom d’hôte valide. Par exemple, elle contient des caractères non autorisés, ce qui est probable si le nom d'hôte est saisi par l'utilisateur sur votre application. Construisez un **nom d’hôte** à l’intérieur d’un bloc try/catch. Ainsi, si une exception est levée, l’application peut notifier l’utilisateur et demander un nouveau nom d’hôte.

## <a name="important-apis"></a>API importantes
* [CertificateQuery](/uwp/api/windows.security.cryptography.certificates.certificatequery?branch=live)
* [CertificateStores.FindAllAsync](/uwp/api/windows.security.cryptography.certificates.certificatestores.findallasync?branch=live)
* [DatagramSocket](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live)
* [DatagramSocket.BindServiceNameAsync](/uwp/api/windows.networking.sockets.datagramsocket.bindservicenameasync?branch=live)
* [DatagramSocket.Control](/uwp/api/windows.networking.sockets.datagramsocket.Control)
* [DatagramSocket.GetOutputStreamAsync](/uwp/api/windows.networking.sockets.datagramsocket.getoutputstreamasync?branch=live)
* [DatagramSocket.MessageReceived](/uwp/api/Windows.Networking.Sockets.DatagramSocket.MessageReceived)
* [DatagramSocketControl.MulticastOnly](/uwp/api/Windows.Networking.Sockets.DatagramSocketControl.MulticastOnly)
* [DatagramSocketMessageReceivedEventArgs](/uwp/api/windows.networking.sockets.datagramsocketmessagereceivedeventargs?branch=live)
* [DatagramSocketMessageReceivedEventArgs.GetDataReader](/uwp/api/windows.networking.sockets.datagramsocketmessagereceivedeventargs.GetDataReader)
* [DataReader.LoadAsync](/uwp/api/windows.storage.streams.datareader.loadasync?branch=live)
* [IOutputStream.FlushAsync](/uwp/api/windows.storage.streams.ioutputstream.FlushAsync)
* [SocketError.GetStatus](/uwp/api/windows.networking.sockets.socketerror.getstatus?branch=live)
* [SocketErrorStatus](/uwp/api/Windows.Networking.Sockets.SocketErrorStatus?branch=live)
* [SocketProtectionLevel](/uwp/api/windows.networking.sockets.socketprotectionlevel?branch=live)
* [StreamSocket](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live)
* [StreamSocketControl.ClientCertificate](/uwp/api/windows.networking.sockets.streamsocketcontrol.ClientCertificate)
* [StreamSocket.ConnectAsync](/uwp/api/windows.networking.sockets.streamsocket.connectasync?branch=live)
* [StreamSocket.InputStream](/uwp/api/windows.networking.sockets.streamsocket.InputStream)
* [StreamSocket.OutputStream](/uwp/api/windows.networking.sockets.streamsocket.OutputStream)
* [StreamSocketListener](/uwp/api/Windows.Networking.Sockets.StreamSocketListener?branch=live)
* [StreamSocketListener.BindServiceNameAsync](/uwp/api/windows.networking.sockets.streamsocketlistener.bindservicenameasync?branch=live)
* [StreamSocketListener.ConnectionReceived](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived)
* [StreamSocketListenerConnectionReceivedEventArgs](/uwp/api/windows.networking.sockets.streamsocketlistenerconnectionreceivedeventargs?branch=live)
* [Windows.Networking.Sockets](/uwp/api/Windows.Networking.Sockets?branch=live)

## <a name="related-topics"></a>Rubriquesassociées
* [Windows Sockets 2 (Winsock)](https://msdn.microsoft.com/library/windows/desktop/ms740673)
* [Comment définir les fonctionnalités de réseau](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx)
* [Communication entre les applications](/windows/uwp/app-to-app/index?branch=live)

## <a name="samples"></a>Exemples
* [Exemple StreamSocket](http://go.microsoft.com/fwlink/p/?LinkId=620609)
