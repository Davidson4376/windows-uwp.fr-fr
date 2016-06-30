---
author: DelfCo
description: "Utilisez HttpClient et le reste de l’API d’espace de noms Windows.Web.Http pour envoyer et recevoir des informations à l’aide des protocoles HTTP 2.0 et HTTP 1.1."
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: b1908e83ffcab562c12c82cfcf7b5fe281d7ada1

---

# HttpClient

\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**API importantes**

-   [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
-   [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
-   [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)

Utilisez [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) et le reste de l’API d’espace de noms [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) pour envoyer et recevoir des informations à l’aide des protocoles HTTP 2.0 et HTTP 1.1.

## Vue d’ensemble de HttpClient et de l’espace de noms Windows.Web.Http

Les classes dans l’espace de noms [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) et les espaces de noms [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) et [**Windows.Web.Http.Filters**](https://msdn.microsoft.com/library/windows/apps/dn298623) associés fournissent une interface de programmation pour les applications de plateforme Windows universelle (UWP) qui agissent en tant que clients HTTP pour effectuer des requêtes GET de base, ou implémenter des fonctionnalités HTTP plus avancées répertoriées ci-dessous :

-   méthodes pour les verbes courants (**DELETE**, **GET**, **PUT** et **POST**). Ces requêtes sont envoyées en tant qu’opération asynchrone.

-   prise en charge des modèles et paramètres d’authentification courants ;

-   accès aux détails SSL (Secure Sockets Layer) sur le transport ;

-   capacité à inclure des filtres personnalisés dans des applications avancées ;

-   capacité à obtenir, définir et supprimer des cookies ;

-   informations de progression de requête HTTP disponibles sur des méthodes asynchrones.

La classe [**Windows.Web.Http.HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) représente un message de requête HTTP envoyé par [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639). La classe [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) représente un message de réponse HTTP reçu d’une requête HTTP. Les messages HTTP sont définis par la norme [RFC 2616](http://go.microsoft.com/fwlink/p/?linkid=241642) de l’IETF (Internet Engineering Task Force).

L’espace de noms [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) représente le contenu HTTP comme corps d’entité HTTP et en-têtes, y compris les cookies. Le contenu HTTP peut être associé à une requête HTTP ou à une réponse HTTP. L’espace de noms **Windows.Web.Http** fournit plusieurs classes pour représenter le contenu HTTP.

-   [
            **HttpBufferContent**](https://msdn.microsoft.com/library/windows/apps/dn298625). Contenu, en tant que mémoire tampon.
-   [
            **HttpFormUrlEncodedContent**](https://msdn.microsoft.com/library/windows/apps/dn298685). Contenu en tant que tuples de nom et valeur encodés avec le type MIME **application/x-www-form-urlencoded**.
-   [
            **HttpMultipartContent**](https://msdn.microsoft.com/library/windows/apps/dn298708). Contenu sous la forme du type MIME **multipart/\***.
-   [
            **HttpMultipartFormDataContent**](https://msdn.microsoft.com/library/windows/apps/dn279596). Contenu encodé comme le type MIME **multipart/form-data**.
-   [
            **HttpStreamContent**](https://msdn.microsoft.com/library/windows/apps/dn279649). Contenu en tant que flux (le type interne est utilisé par la méthode GET HTTP pour recevoir des données et par la méthode POST HTTP pour charger des données).
-   [
            **HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661). Contenu en tant que chaîne.
-   [
            **IHttpContent**](https://msdn.microsoft.com/library/windows/apps/dn279684). Interface de base permettant aux développeurs de créer leurs propres objets de contenu

L’extrait de code dans la section « Envoyer une requête GET simple sur HTTP » utilise la classe [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661) pour représenter la réponse HTTP d’une requête GET HTTP sous forme de chaîne.

L’espace de noms [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) prend en charge la création de cookies et d’en-têtes HTTP, qui sont ensuite associés comme propriétés aux objets [**HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) et [**HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631).

## Envoyer une requête GET simple sur HTTP

Comme mentionné plus haut dans cet article, l’espace de noms [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) permet aux applications UWP d’envoyer des requêtes GET. L’extrait de code suivant montre comment envoyer une requête GET à http://www.contoso.com à l’aide de la classe [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) et de la classe [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) pour lire la réponse de la requête GET.

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

## Exceptions dans Windows.Web.Http

Une exception est levée quand une chaîne d’URI non valide est transmise au constructeur pour l’objet [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998).

**.NET :** le type [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) apparaît en tant que [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) en C# et VB.

En C# et Visual Basic, cette erreur peut être évitée en utilisant la classe [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) dans .NET 4.5 et l’une des méthodes [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) pour tester la chaîne envoyée par un utilisateur avant la construction de l’URI.

En C++, aucune méthode ne permet d’essayer et d’analyser une chaîne passée à un URI. Si une application obtient une entrée de l’utilisateur pour la classe [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), le constructeur doit se trouver dans un bloc try/catch. Si une exception est levée, l’application peut notifier l’utilisateur et demander un nouveau nom d’hôte.

[
            **Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) est dépourvu d’une fonction pratique. De ce fait, une application utilisant [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) et les autres classes de cet espace de noms doit utiliser la valeur **HRESULT**.

Quand une exception se produit dans une application utilisant .NET Framework 4.5 en C#, VB.NET et que cette application est en cours d’exécution, [System.Exception](http://msdn.microsoft.com/library/system.exception.aspx) représente une erreur. La propriété [System.Exception.HResult](http://msdn.microsoft.com/library/system.exception.hresult.aspx) renvoie la valeur **HRESULT** affectée à l’exception spécifique. La propriété [System.Exception.Message](http://msdn.microsoft.com/library/system.exception.message.aspx) renvoie le message de description de l’exception. Les valeurs **HRESULT** possibles sont répertoriées dans le fichier d’en-tête *Winerror.h*. Une application peut filtrer sur des valeurs **HRESULT** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Quand une exception se produit dans une application utilisant le C++ managé et que cette application est en cours d’exécution, [Platform::Exception](http://msdn.microsoft.com/library/windows/apps/hh755825.aspx) représente une erreur. La propriété [Platform::Exception::HResult](http://msdn.microsoft.com/library/windows/apps/hh763371.aspx) renvoie la valeur **HRESULT** affectée à l’exception spécifique. La propriété [Platform::Exception::Message](http://msdn.microsoft.com/library/windows/apps/hh763375.aspx) renvoie la chaîne fournie par le système associée à la valeur **HRESULT**. Les valeurs **HRESULT** possibles sont répertoriées dans le fichier d’en-tête *Winerror.h*. Une application peut effectuer un filtrage sur des valeurs **HRESULT** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Pour la plupart des erreurs de validation de paramètre, la valeur **HRESULT** renvoyée est **E\_INVALIDARG**. Pour certains appels de méthodes non conformes, la valeur **HRESULT** renvoyée est **E\_ILLEGAL\_METHOD\_CALL**.




<!--HONumber=Jun16_HO4-->


