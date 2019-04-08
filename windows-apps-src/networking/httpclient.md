---
description: Utilisez HttpClient et le reste de l’API d’espace de noms Windows.Web.Http pour envoyer et recevoir des informations à l’aide des protocoles HTTP 2.0 et HTTP 1.1.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dd4b8c137d65339701b40027bb3230162e2c2456
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620474"
---
# <a name="httpclient"></a>HttpClient

**API importantes**

-   [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
-   [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
-   [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)

Utilisez [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) et le reste de l’API d’espace de noms [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) pour envoyer et recevoir des informations à l’aide des protocoles HTTP 2.0 et HTTP 1.1.

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>Vue d’ensemble de HttpClient et de l’espace de noms Windows.Web.Http

Les classes dans l’espace de noms [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) et les espaces de noms [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) et [**Windows.Web.Http.Filters**](https://msdn.microsoft.com/library/windows/apps/dn298623) associés fournissent une interface de programmation pour les applications de plateforme Windows universelle (UWP) qui agissent en tant que clients HTTP pour effectuer des requêtes GET de base, ou implémenter des fonctionnalités HTTP plus avancées répertoriées ci-dessous :

-   méthodes pour les verbes courants (**DELETE**, **GET**, **PUT** et **POST**). Ces requêtes sont envoyées en tant qu’opération asynchrone.

-   prise en charge des modèles et paramètres d’authentification courants ;

-   accès aux détails SSL (Secure Sockets Layer) sur le transport ;

-   capacité à inclure des filtres personnalisés dans des applications avancées ;

-   capacité à obtenir, définir et supprimer des cookies ;

-   informations de progression de requête HTTP disponibles sur des méthodes asynchrones.

La classe [**Windows.Web.Http.HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) représente un message de requête HTTP envoyé par [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639). La classe [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) représente un message de réponse HTTP reçu d’une requête HTTP. Les messages HTTP sont définis par la norme [RFC 2616](https://go.microsoft.com/fwlink/p/?linkid=241642) de l’IETF (Internet Engineering Task Force).

L’espace de noms [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) représente le contenu HTTP comme corps d’entité HTTP et en-têtes, y compris les cookies. Le contenu HTTP peut être associé à une requête HTTP ou à une réponse HTTP. L’espace de noms **Windows.Web.Http** fournit plusieurs classes pour représenter le contenu HTTP.

-   [**HttpBufferContent**](https://msdn.microsoft.com/library/windows/apps/dn298625). Contenu, en tant que mémoire tampon.
-   [**HttpFormUrlEncodedContent**](https://msdn.microsoft.com/library/windows/apps/dn298685). Contenu en tant que tuples de nom et valeur encodés avec le type MIME **application/x-www-form-urlencoded**.
-   [**HttpMultipartContent**](https://msdn.microsoft.com/library/windows/apps/dn298708). Contenu sous la forme de la **à parties multiples /\***  type MIME.
-   [**HttpMultipartFormDataContent**](https://msdn.microsoft.com/library/windows/apps/dn279596). Contenu encodé comme le type MIME **multipart/form-data**.
-   [**HttpStreamContent**](https://msdn.microsoft.com/library/windows/apps/dn279649). Contenu en tant que flux (le type interne est utilisé par la méthode GET HTTP pour recevoir des données et par la méthode POST HTTP pour charger des données).
-   [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661). Contenu en tant que chaîne.
-   [**IHttpContent** ](https://msdn.microsoft.com/library/windows/apps/dn279684) -une interface de base pour les développeurs à créer leurs propres objets de contenu

L’extrait de code dans la section « Envoyer une requête GET simple sur HTTP » utilise la classe [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661) pour représenter la réponse HTTP d’une requête GET HTTP sous forme de chaîne.

L’espace de noms [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) prend en charge la création de cookies et d’en-têtes HTTP, qui sont ensuite associés comme propriétés aux objets [**HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) et [**HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631).

## <a name="send-a-simple-get-request-over-http"></a>Envoyer une requête GET simple sur HTTP

Comme mentionné plus haut dans cet article, l’espace de noms [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) permet aux applications UWP d’envoyer des requêtes GET. L’extrait de code suivant montre comment envoyer une demande GET à http://www.contoso.com à l’aide de la [ **Windows.Web.Http.HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe et le [  **Windows.Web.Http.HttpResponseMessage** ](https://msdn.microsoft.com/library/windows/apps/dn279631) classe pour lire la réponse de la demande GET.

```csharp
//Create an HTTP client object
Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();

//Add a user-agent header to the GET request. 
var headers = httpClient.DefaultRequestHeaders;

//The safe way to add a header value is to use the TryParseAdd method and verify the return value is true,
//especially if the header value is coming from user input.
string header = "ie";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

header = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

Uri requestUri = new Uri("http://www.contoso.com");

//Send the GET request asynchronously and retrieve the response as a string.
Windows.Web.Http.HttpResponseMessage httpResponse = new Windows.Web.Http.HttpResponseMessage();
string httpResponseBody = "";

try
{
    //Send the GET request
    httpResponse = await httpClient.GetAsync(requestUri);
    httpResponse.EnsureSuccessStatusCode();
    httpResponseBody = await httpResponse.Content.ReadAsStringAsync();
}
catch (Exception ex)
{
    httpResponseBody = "Error: " + ex.HResult.ToString("X") + " Message: " + ex.Message;
}
```

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    init_apartment();

    // Create an HttpClient object.
    Windows::Web::Http::HttpClient httpClient;

    // Add a user-agent header to the GET request.
    auto headers{ httpClient.DefaultRequestHeaders() };

    // The safe way to add a header value is to use the TryParseAdd method, and verify the return value is true.
    // This is especially important if the header value is coming from user input.
    std::wstring header{ L"ie" };
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    header = L"Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    Uri requestUri{ L"http://www.contoso.com" };

    // Send the GET request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the GET request.
        httpResponseMessage = httpClient.GetAsync(requestUri).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

## <a name="post-binary-data-over-http"></a>Données POST binaires sur HTTP

Le [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis) exemple de code ci-dessous illustre l’utilisation des données de formulaire et une requête POST pour envoyer une petite quantité de données binaires comme un téléchargement de fichier à un serveur web. Le code utilise le [ **HttpBufferContent** ](/uwp/api/windows.web.http.httpbuffercontent) classe pour représenter les données binaires et le [ **HttpMultipartFormDataContent** ](/uwp/api/windows.web.http.httpmultipartformdatacontent) classe représentent les données de formulaire en plusieurs parties.

> [!NOTE]
> Appel **obtenir** (tels que présentés dans l’exemple de code ci-dessous) ne convient pas à un thread d’interface utilisateur. Pour la technique correcte à utiliser dans ce cas, consultez [concurrence et des opérations asynchrones avec C / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
#include <sstream>
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;

int main()
{
    init_apartment();

    Windows::Web::Http::HttpClient httpClient;

    Uri requestUri{ L"https://www.contoso.com/post" };

    Windows::Web::Http::HttpMultipartFormDataContent postContent;
    Windows::Web::Http::Headers::HttpContentDispositionHeaderValue disposition{ L"form-data" };
    postContent.Headers().ContentDisposition(disposition);
    // The 'name' directive contains the name of the form field representing the data.
    disposition.Name(L"fileForUpload");
    // Here, the 'filename' directive is used to indicate to the server a file name
    // to use to save the uploaded data.
    disposition.FileName(L"file.dat");

    auto buffer{
        Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
            L"A sentence of text to encode into binary to serve as sample data.",
            Windows::Security::Cryptography::BinaryStringEncoding::Utf8
        )
    };
    Windows::Web::Http::HttpBufferContent binaryContent{ buffer };
    // You can use the 'image/jpeg' content type to represent any binary data;
    // it's not necessarily an image file.
    binaryContent.Headers().Append(L"Content-Type", L"image/jpeg");

    postContent.Add(binaryContent); // Add the binary data content as a part of the form data content.

    // Send the POST request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the POST request.
        httpResponseMessage = httpClient.PostAsync(requestUri, postContent).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

Pour valider le contenu d’un fichier binaire (plutôt que les données binaires explicites ci-dessus), vous pouvez la retrouver plus facile à utiliser un [HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent) objet. Construit un et, en tant qu’argument à son constructeur, passez la valeur retournée à partir d’un appel à [StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync). Cette méthode retourne un flux pour les données à l’intérieur de votre fichier binaire.

En outre, si vous téléchargez un fichier volumineux (supérieur à environ 10 Mo), puis nous vous recommandons d’utiliser le Runtime Windows [transfert en arrière-plan](/uwp/api/windows.networking.backgroundtransfer) API.

## <a name="exceptions-in-windowswebhttp"></a>Exceptions dans Windows.Web.Http

Une exception est levée quand une chaîne d’URI non valide est transmise au constructeur pour l’objet [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998).

**.NET :**  le [ **Windows.Foundation.Uri** ](https://msdn.microsoft.com/library/windows/apps/br225998) type apparaît sous la forme [ **System.Uri** ](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) dans C# et VB.

En C# et Visual Basic, cette erreur peut être évitée en utilisant la classe [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) dans .NET 4.5 et l’une des méthodes [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) pour tester la chaîne envoyée par un utilisateur avant la construction de l’URI.

En C++, aucune méthode ne permet d’essayer et d’analyser une chaîne passée à un URI. Si une application obtient une entrée de l’utilisateur pour la classe [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), le constructeur doit se trouver dans un bloc try/catch. Si une exception est levée, l’application peut notifier l’utilisateur et demander un nouveau nom d’hôte.

[  **Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) est dépourvu d’une fonction pratique. De ce fait, une application utilisant [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) et les autres classes de cet espace de noms doit utiliser la valeur **HRESULT**.

Dans les applications à l’aide de .NET Framework 4.5 dans C#, VB.NET, le [System.Exception](https://msdn.microsoft.com/library/system.exception.aspx) représente une erreur pendant l’exécution d’application lorsqu’une exception se produit. La propriété [System.Exception.HResult](https://msdn.microsoft.com/library/system.exception.hresult.aspx) renvoie la valeur **HRESULT** affectée à l’exception spécifique. La propriété [System.Exception.Message](https://msdn.microsoft.com/library/system.exception.message.aspx) renvoie le message de description de l’exception. Les valeurs **HRESULT** possibles sont répertoriées dans le fichier d’en-tête *Winerror.h*. Une application peut filtrer sur des valeurs **HRESULT** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Quand une exception se produit dans une application utilisant le C++ managé et que cette application est en cours d’exécution, [Platform::Exception](https://msdn.microsoft.com/library/windows/apps/hh755825.aspx) représente une erreur. La propriété [Platform::Exception::HResult](https://msdn.microsoft.com/library/windows/apps/hh763371.aspx) renvoie la valeur **HRESULT** affectée à l’exception spécifique. La propriété [Platform::Exception::Message](https://msdn.microsoft.com/library/windows/apps/hh763375.aspx) renvoie la chaîne fournie par le système associée à la valeur **HRESULT**. Les valeurs **HRESULT** possibles sont répertoriées dans le fichier d’en-tête *Winerror.h*. Une application peut filtrer sur des valeurs **HRESULT** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Pour la plupart des erreurs de validation de paramètre, le **HRESULT** retourné est **E\_INVALIDARG**. Pour certains appels de méthode non conforme, le **HRESULT** retourné est **E\_non conforme\_méthode\_appeler**.

