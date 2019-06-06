---
description: Utilisez HttpClient et le reste de l’API d’espace de noms Windows.Web.Http pour envoyer et recevoir des informations à l’aide des protocoles HTTP 2.0 et HTTP 1.1.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 6/5/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb098aae346c7a81771262793f5f6a042d62d5a3
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721611"
---
# <a name="httpclient"></a>HttpClient

**API importantes**

-   [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)
-   [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)
-   [**Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage)

Utilisez [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) et le reste de l’API d’espace de noms [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) pour envoyer et recevoir des informations à l’aide des protocoles HTTP 2.0 et HTTP 1.1.

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>Vue d’ensemble de HttpClient et de l’espace de noms Windows.Web.Http

Les classes dans l’espace de noms [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) et les espaces de noms [**Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) et [**Windows.Web.Http.Filters**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Filters) associés fournissent une interface de programmation pour les applications de plateforme Windows universelle (UWP) qui agissent en tant que clients HTTP pour effectuer des requêtes GET de base, ou implémenter des fonctionnalités HTTP plus avancées répertoriées ci-dessous :

-   méthodes pour les verbes courants (**DELETE**, **GET**, **PUT** et **POST**). Ces requêtes sont envoyées en tant qu’opération asynchrone.

-   prise en charge des modèles et paramètres d’authentification courants ;

-   accès aux détails SSL (Secure Sockets Layer) sur le transport ;

-   capacité à inclure des filtres personnalisés dans des applications avancées ;

-   capacité à obtenir, définir et supprimer des cookies ;

-   informations de progression de requête HTTP disponibles sur des méthodes asynchrones.

La classe [**Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) représente un message de requête HTTP envoyé par [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient). La classe [**Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) représente un message de réponse HTTP reçu d’une requête HTTP. Les messages HTTP sont définis par la norme [RFC 2616](https://go.microsoft.com/fwlink/p/?linkid=241642) de l’IETF (Internet Engineering Task Force).

L’espace de noms [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) représente le contenu HTTP comme corps d’entité HTTP et en-têtes, y compris les cookies. Le contenu HTTP peut être associé à une requête HTTP ou à une réponse HTTP. L’espace de noms **Windows.Web.Http** fournit plusieurs classes pour représenter le contenu HTTP.

-   [**HttpBufferContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpBufferContent). Contenu, en tant que mémoire tampon.
-   [**HttpFormUrlEncodedContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpFormUrlEncodedContent). Contenu en tant que tuples de nom et valeur encodés avec le type MIME **application/x-www-form-urlencoded**.
-   [**HttpMultipartContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartContent). Contenu sous la forme de la **à parties multiples /\***  type MIME.
-   [**HttpMultipartFormDataContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartFormDataContent). Contenu encodé comme le type MIME **multipart/form-data**.
-   [**HttpStreamContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStreamContent). Contenu en tant que flux (le type interne est utilisé par la méthode GET HTTP pour recevoir des données et par la méthode POST HTTP pour charger des données).
-   [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent). Contenu en tant que chaîne.
-   [**IHttpContent** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.IHttpContent) -une interface de base pour les développeurs à créer leurs propres objets de contenu

L’extrait de code dans la section « Envoyer une requête GET simple sur HTTP » utilise la classe [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent) pour représenter la réponse HTTP d’une requête GET HTTP sous forme de chaîne.

L’espace de noms [**Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) prend en charge la création de cookies et d’en-têtes HTTP, qui sont ensuite associés comme propriétés aux objets [**HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) et [**HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage).

## <a name="send-a-simple-get-request-over-http"></a>Envoyer une requête GET simple sur HTTP

Comme mentionné plus haut dans cet article, l’espace de noms [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) permet aux applications UWP d’envoyer des requêtes GET. L’extrait de code suivant montre comment envoyer une demande GET à http://www.contoso.com à l’aide de la [ **Windows.Web.Http.HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) classe et le [  **Windows.Web.Http.HttpResponseMessage** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) classe pour lire la réponse de la demande GET.

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

    Windows::Web::Http::Headers::HttpContentDispositionHeaderValue disposition{ L"form-data" };
    binaryContent.Headers().ContentDisposition(disposition);
    // The 'name' directive contains the name of the form field representing the data.
    disposition.Name(L"fileForUpload");
    // Here, the 'filename' directive is used to indicate to the server a file name
    // to use to save the uploaded data.
    disposition.FileName(L"file.dat");

    Windows::Web::Http::HttpMultipartFormDataContent postContent;
    postContent.Add(binaryContent); // Add the binary data content as a part of the form data content.

    // Send the POST request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the POST request.
        Uri requestUri{ L"https://www.contoso.com/post" };
        Windows::Web::Http::HttpClient httpClient;
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

## <a name="post-json-data-over-http"></a>Données POST JSON via HTTP

L’exemple suivant valide un JSON à un point de terminaison, puis écrit le corps de réponse.

```cs
using System;
using System.Diagnostics;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Web.Http;

private async Task TryPostJsonAsync()
{
    try
    {
        // Construct the HttpClient and Uri. This endpoint is for test purposes only.
        HttpClient httpClient = new HttpClient();
        Uri uri = new Uri("https://www.contoso.com/post");

        // Construct the JSON to post.
        HttpStringContent content = new HttpStringContent(
            "{ \"firstName\": \"Eliot\" }",
            UnicodeEncoding.Utf8,
            "application/json");

        // Post the JSON and wait for a response.
        HttpResponseMessage httpResponseMessage = await httpClient.PostAsync(
            uri,
            content);

        // Make sure the post succeeded, and write out the response.
        httpResponseMessage.EnsureSuccessStatusCode();
        var httpResponseBody = await httpResponseMessage.Content.ReadAsStringAsync();
        Debug.WriteLine(httpResponseBody);
    }
    catch (Exception ex)
    {
        // Write out any exceptions.
        Debug.WriteLine(ex);
    }
}
```

## <a name="exceptions-in-windowswebhttp"></a>Exceptions dans Windows.Web.Http

Une exception est levée quand une chaîne d’URI non valide est transmise au constructeur pour l’objet [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri).

**.NET :**   le [ **Windows.Foundation.Uri** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) type apparaît sous la forme [ **System.Uri** ](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN) dans C# et VB.

En C# et Visual Basic, cette erreur peut être évitée en utilisant la classe [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN) dans .NET 4.5 et l’une des méthodes [**System.Uri.TryCreate**](https://docs.microsoft.com/dotnet/api/system.uri.trycreate?redirectedfrom=MSDN#overloads) pour tester la chaîne envoyée par un utilisateur avant la construction de l’URI.

En C++, aucune méthode ne permet d’essayer et d’analyser une chaîne passée à un URI. Si une application obtient une entrée de l’utilisateur pour la classe [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri), le constructeur doit se trouver dans un bloc try/catch. Si une exception est levée, l’application peut notifier l’utilisateur et demander un nouveau nom d’hôte.

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) est dépourvu d’une fonction pratique. De ce fait, une application utilisant [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) et les autres classes de cet espace de noms doit utiliser la valeur **HRESULT**.

Dans les applications à l’aide de .NET Framework 4.5 dans C#, VB.NET, le [System.Exception](https://docs.microsoft.com/dotnet/api/system.exception?redirectedfrom=MSDN) représente une erreur pendant l’exécution d’application lorsqu’une exception se produit. La propriété [System.Exception.HResult](https://docs.microsoft.com/dotnet/api/system.exception.hresult?redirectedfrom=MSDN#System_Exception_HResult) renvoie la valeur **HRESULT** affectée à l’exception spécifique. La propriété [System.Exception.Message](https://docs.microsoft.com/dotnet/api/system.exception.message?redirectedfrom=MSDN#System_Exception_Message) renvoie le message de description de l’exception. Les valeurs **HRESULT** possibles sont répertoriées dans le fichier d’en-tête *Winerror.h*. Une application peut filtrer sur des valeurs **HRESULT** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Quand une exception se produit dans une application utilisant le C++ managé et que cette application est en cours d’exécution, [Platform::Exception](https://docs.microsoft.com/cpp/cppcx/platform-exception-class) représente une erreur. La propriété [Platform::Exception::HResult](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#hresult) renvoie la valeur **HRESULT** affectée à l’exception spécifique. La propriété [Platform::Exception::Message](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#message) renvoie la chaîne fournie par le système associée à la valeur **HRESULT**. Les valeurs **HRESULT** possibles sont répertoriées dans le fichier d’en-tête *Winerror.h*. Une application peut filtrer sur des valeurs **HRESULT** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Pour la plupart des erreurs de validation de paramètre, le **HRESULT** retourné est **E\_INVALIDARG**. Pour certains appels de méthode non conforme, le **HRESULT** retourné est **E\_non conforme\_méthode\_appeler**.

