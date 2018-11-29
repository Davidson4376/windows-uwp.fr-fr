---
title: Mise en réseau pour les jeux
description: Apprenez à développer et à incorporer des fonctionnalités réseau dans votre jeu DirectX.
ms.assetid: 212eee15-045c-8ba1-e274-4532b2120c55
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, jeux, mise en réseau, directx
ms.localizationpriority: medium
ms.openlocfilehash: 2697564e703cfe290e33f204125346f3e8bad8ac
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8209434"
---
# <a name="networking-for-games"></a>Mise en réseau pour les jeux



Apprenez à développer et à incorporer des fonctionnalités réseau dans votre jeu DirectX.

## <a name="concepts-at-a-glance"></a>Aperçu rapide des concepts


Vous pouvez utiliser un large éventail de fonctionnalités réseau dans votre jeu DirectX, qu’il s’agisse d’un simple jeu autonome ou de jeux multijoueurs de large portée. L’utilisation du réseau la plus simple consiste à stocker des noms d’utilisateur et des scores de jeu sur un serveur réseau central.

Les API de réseau sont nécessaires dans les jeux multijoueurs qui ont recours au modèle d’infrastructure (client-serveur ou pair à pair Internet) tout comme dans les jeux ad hoc (pair à pair local). Pour les jeux multijoueurs sur serveur, un serveur de jeux centralisé gère en règle générale toutes les opérations de jeu et l’application de jeu cliente est utilisée pour la saisie, l’affichage des éléments graphiques, la lecture audio et d’autres fonctionnalités. La vitesse et la latence des transferts réseau sont des aspects indispensables à la garantie d’une expérience de jeu satisfaisante.

Dans le cas de jeux pair à pair, chaque application de joueur gère les entrées et les graphiques. Dans la plupart des cas, les joueurs se trouvent proches l’un de l’autre. La latence réseau s’en trouve réduite mais peut néanmoins poser des problèmes. La découverte d‘homologues et l’établissement d’une connexion peuvent alors devenir une source de préoccupation.

Dans les jeux pour joueur unique, un service ou serveur Web central est souvent utilisé pour stocker les noms d’utilisateurs, les scores de jeu et d’autres informations diverses. Dans ces jeux, la vitesse et la latence des transferts réseau sont moins préoccupantes car elles ne gênent pas directement le jeu.

Les conditions du réseau peuvent changer à tout moment. Un jeu qui utilise les API de réseau doit pouvoir gérer les exceptions réseau qui sont susceptibles de se produire. Pour plus d’informations sur la gestion des exceptions réseau, voir [Notions de base en matière de réseau](https://msdn.microsoft.com/library/windows/apps/mt280233).

Les pare-feu et les proxys web sont courants et peuvent limiter l’utilisation des fonctionnalités réseau. Un jeu qui utilise le réseau doit être préparé pour gérer convenablement les pare-feux et les proxys.

Dans le cas des appareils mobiles, il est important de surveiller les ressources réseau disponibles et utiliser au mieux les connexions réseau limitées. En effet, les frais d’itinérance ou de données peuvent être non négligeables.

L’isolement réseau fait partie du modèle de sécurité des applications utilisé par Windows. Windows détecte activement les limites du réseau et applique les restrictions d’accès pour l’isolement réseau. Les applications doivent déclarer les fonctionnalités d’isolement réseau afin de définir l’étendue de l’accès au réseau. Sans déclarer ces fonctionnalités, votre application ne peut pas avoir accès aux ressources réseau. Pour plus d’informations sur la façon dont Windows applique l’isolement réseau pour les applications, voir [Comment configurer les fonctionnalités d’isolement réseau](https://msdn.microsoft.com/library/windows/apps/hh770532).

## <a name="design-considerations"></a>Considérations relatives à la conception


Vous pouvez utiliser un éventail d’API de réseau dans les jeux DirectX. Il est donc important de choisir la bonne API. Windows prend en charge une grande variété d’API de réseau que votre application peut utiliser pour communiquer avec d’autres ordinateurs et appareils sur Internet ou sur des réseaux privés. Vous devez donc commencer par déterminer les fonctionnalités réseau dont votre application a besoin.

Les API de réseau les plus connues pour les jeux sont les suivantes :

-   TCP et sockets : fournit une connexion fiable. Utilisez TCP pour les jeux qui ne nécessitent aucune sécurité. TCP permet au serveur d’effectuer facilement une montée en puissance. Il est ainsi couramment utilisé dans les jeux qui ont recours au modèle d’infrastructure (client-serveur ou pair à pair Internet). TCP peut également servir pour les jeux ad hoc (pair à pair local) via Wi-Fi Direct et Bluetooth. TCP est couramment utilisé pour les déplacements d’objets, l’interaction entre les personnages, la conversation textuelle et bien d’autres opérations. La classe [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) fournit un socket TCP qui peut être utilisé dans les jeux Microsoft Store. La classe **StreamSocket** est utilisée avec les classes associées de l’espace de noms [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960).
-   TCP et sockets avec SSL: offre une connexion fiable qui empêche l’écoute clandestine. Utilisez les connexions TCP avec SSL pour les jeux qui nécessitent de la sécurité. Le chiffrement et le traitement de SSL ajoute un coût en termes de latence et de performance. Il est donc uniquement utilisé quand la sécurité est nécessaire. TCP avec SSL est couramment utilisé pour les connexions, les ressources d’achat et de commerce, la création de personnages de jeu et la gestion. La classe [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) fournit un socket TCP prenant en charge SSL.
-   UDP et sockets : offre des transferts réseau non fiables avec un traitement faible. UDP est utilisé pour les jeux qui tolèrent un faible niveau de latence et quelques pertes de paquets. Il sert souvent dans les jeux de combat, pour les tirs et les trajectoires, l’audio sur le réseau et la conversation vocale. La classe [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) fournit un socket UDP qui peut être utilisé dans les jeux Microsoft Store. La classe **DatagramSocket** est utilisée avec les classes associées de l’espace de noms [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960).
-   Client HTTP: offre une connexion fiable aux serveurs HTTP. Le scénario de réseau le plus courant consiste à accéder à un site Web pour récupérer ou stocker des informations. Il peut s’agir tout simplement d’un site Web permettant de stocker des informations utilisateur et des scores de jeu. Combiné à SSL à des fins de sécurité, un client HTTP peut être utilisé pour la connexion, l’achat, les ressources de commerce, la création de personnages de jeu et la gestion. La classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) fournit un client HTTP moderne API pour une utilisation dans les jeux Microsoft Store. La classe **HttpClient** est utilisée avec les classes associées de l’espace de noms [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/dn279692).

## <a name="handling-network-exceptions-in-your-directx-game"></a>Gestion des exceptions réseau dans votre jeu DirectX


Quand une exception réseau survient dans votre jeu DirectX, cela peut être le signe d’un problème existant ou d’une défaillance sérieuse. Des exceptions peuvent survenir pour de multiples raisons lorsque vous utilisez les API de réseau. Bien souvent, l’exception peut être due à des changements de connectivité réseau ou d’autres problèmes réseau liés à l’hôte ou au serveur distant.

Certaines exceptions provenant de l’utilisation d’API de réseau peuvent avoir les causes suivantes :

-   entrée utilisateur d’un nom d’hôte ou d’un URI contenant des erreurs ou non valide ;
-   échecs de résolution de noms lors de la recherche d’un nom d’hôte ou d’un URI ;
-   perte ou modification de la connectivité réseau ;
-   échecs de connexion réseau lors de l’utilisation de sockets ou d’API clientes HTTP ;
-   erreurs de serveur réseau ou de point de terminaison distant ;
-   erreurs réseau diverses.

Les exceptions liées à des erreurs réseau (perte ou modification de connectivité, échecs de connexion et défaillances de serveur, par exemple) peuvent se produire à tout moment. Ces erreurs donnent lieu à la levée d’exceptions. Si votre application ne gère pas une exception, le runtime peut arrêter entièrement votre application.

Vous devez écrire du code capable de gérer les exceptions au moment où vous appelez la plupart des méthodes réseau asynchrones. Parfois, quand une exception se produit, vous pouvez essayer de résoudre le problème en appliquant de nouveau une méthode réseau. Dans d’autres cas, votre application doit pouvoir poursuivre sans connectivité réseau en utilisant les données préalablement mises en cache.

Les applications de plateforme Windows universelle (UWP) lèvent généralement une seule exception. Votre gestionnaire d’exceptions peut récupérer des informations plus détaillées sur la cause de l’exception afin d’analyser la défaillance et de prendre des décisions appropriées.

Lorsqu’une exception survient dans un jeu DirectX qui correspond à une application UWP, il est possible de récupérer la valeur **HRESULT** permettant d’identifier la cause de l’erreur. Le fichier include *Winerror.h* contient une liste importante de valeurs **HRESULT** possibles qui inclut les erreurs réseau.

Les API de réseau prennent en charge différentes méthodes permettant de récupérer ces informations détaillées sur la cause d’une exception:

-   Méthode permettant d’extraire la valeur **HRESULT** de l’erreur à l’origine de l’exception. La liste des valeurs **HRESULT** potentielles est importante et non spécifiée. La valeur **HRESULT** peut être extraite lors de l’utilisation d’une API de réseau quelconque.
-   Méthode d’assistance qui convertit la valeur **HRESULT** en valeur d’énumération. La liste des valeurs d’énumération potentielles est précisée et relativement petite. Une méthode d’assistance est disponible pour les classes de socket dans l’espace de noms [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960).

### <a name="exceptions-in-windowsnetworkingsockets"></a>Exceptions dans Windows.Networking.Sockets

Le constructeur pour la classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) utilisée avec des sockets peut lever une exception si la chaîne transmise n’est pas un nom d’hôte valide (c’est-à-dire si elle contient des caractères non autorisés dans un nom d’hôte). Si une application obtient une entrée utilisateur pour le **HostName** d’une connexion homologue de jeu, le constructeur doit se trouver dans un bloc try/catch. Si une exception est levée, l’application peut notifier l’utilisateur et demander un nouveau nom d’hôte.

Ajouter du code pour valider une chaîne de nom d’hôte de l’utilisateur

```cpp

    // Define some variables at the class level.
    Windows::Networking::HostName^ remoteHost;

    bool isHostnameFromUser = false;
    bool isHostnameValid = false;

    ///...

    // If the value of 'remoteHostname' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^hostString = remoteHostname;

    try 
    {
        remoteHost = ref new Windows::Networking:Host(hostString);
        isHostnameValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad hostname, please re-enter a valid hostname.";
        return;
    }

    isHostnameFromUser = true;


    // ... Continue with code to execute with a valid hostname.
```

L’espace de noms [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) propose des méthodes et des énumérations d’assistance pratiques qui permettent de gérer les erreurs quand des sockets sont utilisés. Ceci peut s’avérer utile pour gérer différemment certaines exceptions réseau dans votre application.

Une erreur rencontrée durant une opération [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) ou [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) entraîne la levée d’une exception. La cause de l’exception est une valeur d’erreur représentée en tant que valeur **HRESULT**. La méthode [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462) sert à convertir une erreur réseau résultant d’une opération de socket en valeur d’énumération [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457). La plupart des valeurs d’énumération **SocketErrorStatus** correspondent à une erreur renvoyée par l’opération de socket Windows native. Une application peut filtrer des valeurs d’énumération **SocketErrorStatus** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Pour les erreurs de validation de paramètre, une application peut également utiliser la valeur **HRESULT** de l’exception pour obtenir des informations plus détaillées sur l’erreur à l’origine de l’exception. Les valeurs **HRESULT** possibles sont répertoriées dans le fichier d’en-tête *Winerror.h*. Pour la plupart des erreurs de validation de paramètre, la valeur **HRESULT** renvoyée est **E\_INVALIDARG**.

Ajouter du code pour gérer les exceptions quand vous essayez d’établir une connexion de socket de flux

```cpp
using namespace Windows::Networking;
using namespace Windows::Networking::Sockets;

    
    // Define some more variables at the class level.

    bool isSocketConnected = false
    bool retrySocketConnect = false;

    // The number of times we have tried to connect the socket.
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid remoteHost and serviceName parameter.
    // The hostname can contain a name or an IP address.
    // The servicename can contain a string or a TCP port number.

    StreamSocket ^ socket = ref new StreamSocket();
    SocketErrorStatus errorStatus; 
    HResult hr;

    // Save the socket, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("clientSocket", socket);

    // Connect to the remote server. 
    create_task(socket->ConnectAsync(
            remoteHost,
            serviceName,
            SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isSocketConnected = true;
            // Mark the socket as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
            errorStatus = SocketStatus::GetStatus(hr); 
            if (errorStatus != Unknown)
            {
                                                                switch (errorStatus) 
                   {
                    case HostNotFound:
                        // If the hostname is from the user, this may indicate a bad input.
                        // Set a flag to ask the user to re-enter the hostname.
                        isHostnameValid = false;
                        return;
                        break;
                    case ConnectionRefused:
                        // The server might be temporarily busy.
                        retrySocketConnect = true;
                        return;
                        break; 
                    case NetworkIsUnreachable: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case UnreachableHost: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case NetworkIsDown: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    // Handle other errors. 
                    default: 
                        // The connection failed and no options are available.
                        // Try to use cached data if it is available. 
                        // You may want to tell the user that the connect failed.
                        break;
                }
                }
                else 
                {
                    // Received an Hresult that is not mapped to an enum.
                    // This could be a connectivity issue.
                    retrySocketConnect = true;
                }
            }
        });
    }

```

### <a name="exceptions-in-windowswebhttp"></a>Exceptions dans Windows.Web.Http

Le constructeur de la classe [**Windows::Foundation::Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) utilisée avec [**Windows::Web::Http::HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) peut lever une exception si la chaîne passée n’est pas un URI valide (si elle contient des caractères non autorisés dans un URI). En C++, aucune méthode ne permet d’essayer et d’analyser une chaîne transmise à un URI. Si une application obtient une entrée utilisateur pour **Windows::Foundation::Uri**, le constructeur doit se trouver dans un bloc try/catch. Si une exception est levée, l’application peut informer l’utilisateur et demander un nouvel URI.

Votre application doit également vérifier si le schéma de l’URI est HTTP ou HTTPS, dans la mesure où il s’agit des seuls schémas pris en charge par [**Windows::Web::Http::HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639).

Ajouter du code pour valider une chaîne d’URI de l’utilisateur

```cpp

    // Define some variables at the class level.
    Windows::Foundation::Uri^ resourceUri;

    bool isUriFromUser = false;
    bool isUriValid = false;

    ///...

    // If the value of 'inputUri' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^uriString = inputUri;

    try 
    {
        isUriValid = false;
        resourceUri = ref new Windows::Foundation:Uri(uriString);

        if (resourceUri->SchemeName != "http" && resourceUri->SchemeName != "https")
        {
            statusText->Text = "Only 'http' and 'https' schemes supported. Please re-enter URI";
            return;
        }
        isUriValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad URI, please re-enter Uri to continue.";
        return;
    }

    isUriFromUser = true;


    // ... Continue with code to execute with a valid URI.
```

L’espace de noms [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/windows.web.http.aspx) est dépourvu d’une fonction pratique. De ce fait, une application utilisant [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) et les autres classes de cet espace de noms doit utiliser la valeur **HRESULT**.

Quand une exception se produit dans une application en C++ et que cette application est en cours d’exécution, [**Platform::Exception**](https://msdn.microsoft.com/library/windows/apps/hh755825.aspx) représente une erreur. La propriété [**Platform::Exception::HResult**](https://msdn.microsoft.com/library/windows/apps/hh763371.aspx) renvoie la valeur **HRESULT** affectée à l’exception spécifique. La propriété [**Platform::Exception::Message**](https://msdn.microsoft.com/library/windows/apps/hh763375.aspx) renvoie la chaîne fournie par le système associée à la valeur **HRESULT**. Les valeurs **HRESULT** possibles sont répertoriées dans le fichier d’en-tête *Winerror.h*. Une application peut effectuer un filtrage sur des valeurs **HRESULT** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Pour la plupart des erreurs de validation de paramètre, la valeur **HRESULT** renvoyée est **E\_INVALIDARG**. Pour certains appels de méthodes non conformes, la valeur **HRESULT** renvoyée est **E\_ILLEGAL\_METHOD\_CALL**.

Ajouter du code pour gérer les exceptions durant l’utilisation de [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) pour la connexion à un serveur HTTP

```cpp
using namespace Windows::Foundation;
using namespace Windows::Web::Http;
    
    // Define some more variables at the class level.

    bool isHttpClientConnected = false
    bool retryHttpClient = false;

    // The number of times we have tried to connect the socket
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid resourceUri parameter.
    // The URI must contain a scheme and a name or an IP address.

    HttpClient ^ httpClient = ref new HttpClient();
    HResult hr;

    // Save the httpClient, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("httpClient", httpClient);

    // Send a GET request to the HTTP server. 
    create_task(httpClient->GetAsync(resourceUri)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isHttpClientConnected = true;
            // Mark the HttClient as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
                                                switch (errorStatus) 
               {
                case WININET_E_NAME_NOT_RESOLVED:
                    // If the Uri is from the user, this may indicate a bad input.
                    // Set a flag to ask user to re-enter the Uri.
                    isUriValid = false;
                    return;
                    break;
                case WININET_E_CANNOT_CONNECT:
                    // The server might be temporarily busy.
                    retryHttpClientConnect = true;
                    return;
                    break; 
                case WININET_E_CONNECTION_ABORTED: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case WININET_E_CONNECTION_RESET: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case INET_E_RESOURCE_NOT_FOUND: 
                    // The server cannot locate the resource specified in the uri.
                    // If the Uri is from user, this may indicate a bad input.
                    // Set a flag to ask the user to re-enter the Uri
                    isUriValid = false;
                    return;
                    break;
                // Handle other errors. 
                default: 
                    // The connection failed and no options are available.
                    // Try to use cached data if it is available. 
                    // You may want to tell the user that the connect failed.
                    break;
            }
            else 
            {
                // Received an Hresult that is not mapped to an enum.
                // This could be a connectivity issue.
                retrySocketConnect = true;
            }
        }
    });
    

```

## <a name="related-topics"></a>Rubriques connexes


**Autres ressources**

* [Connexion à l’aide d’un socket datagramme](https://msdn.microsoft.com/library/windows/apps/xaml/jj635238)
* [Connexion à une ressource réseau avec un socket de flux](https://msdn.microsoft.com/library/windows/apps/xaml/jj150599)
* [Connexion aux services réseau](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
* [Connexion aux services Web](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
* [Notions de base en matière de réseau](https://msdn.microsoft.com/library/windows/apps/mt280233)
* [Comment configurer les fonctionnalités d’isolement réseau](https://msdn.microsoft.com/library/windows/apps/hh770532)
* [Comment activer le bouclage et déboguer l’isolement réseau](https://msdn.microsoft.com/library/windows/apps/hh780593)

**Référence**

* [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)
* [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
* [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)
* [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
* [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

**Exemples**

* [Exemple DatagramSocket](http://go.microsoft.com/fwlink/p/?LinkID=243037)
* [Exemple HttpClient]( http://go.microsoft.com/fwlink/p/?linkid=242550)
* [Exemple de proximité](http://go.microsoft.com/fwlink/p/?linkid=245082)
* [Exemple StreamSocket](http://go.microsoft.com/fwlink/p/?linkid=243037)
