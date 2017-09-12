---
author: mcleanbyron
ms.assetid: 
description: "Utilisez cette méthode dans l’API d’analyse du Windows Store pour télécharger le fichier CAB concernant une erreur dans votre application."
title: "Téléchargez le fichier CAB concernant une erreur dans votre application"
ms.author: mcleans
ms.date: 06/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, API d’analyse du Windows Store, téléchargement CAB"
ms.openlocfilehash: 6c9e3f024a0b222be89ede5cf81a14bc923b7af4
ms.sourcegitcommit: 7aabd2e59d45bbc5512dd4ddd9110ae62b79d552
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/19/2017
---
# <a name="download-the-cab-file-for-an-error-in-your-app"></a>Téléchargez le fichier CAB pour une erreur dans votre application

Utilisez cette méthode dans l’API d’analyse du Windows Store pour télécharger le fichier CAB associé à une erreur spécifique dans votre application qui a été signalée au Centre de développement. Cette méthode ne permet de télécharger que le fichier CAB concernant une erreur d’application survenue dans les 30derniers jours. Les téléchargements de fichier CAB sont également disponibles dans la section **Échecs** du [rapport d’intégrité](../publish/health-report.md) dans le tableau de bord du Centre de développement Windows.

Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md) afin de récupérer l’ID du fichier CAB à télécharger.

## <a name="prerequisites"></a>Éléments prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez l’ID du fichier CAB à télécharger. Pour obtenir cet ID, utilisez la méthode [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md) pour récupérer les détails d’une erreur spécifique dans votre application, en spécifiant la valeur **cabId** dans le corps de la réponse.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload``` |

<span/> 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/> 

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  |
|---------------|--------|---------------|------|
| applicationId | chaîne | ID Store de l’application pour laquelle vous voulez télécharger un fichier CAB. L’ID Store est disponible dans la [page Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID WindowsStore: 9WZDNCRFJ3Q8. |  Oui  |
| cabId | chaîne | L’ID unique du fichier CAB à télécharger. Pour obtenir cet ID, utilisez la méthode [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md) pour récupérer les détails d’une erreur spécifique dans votre application, en spécifiant la valeur **cabId** dans le corps de la réponse. |  Oui  |

<span/>
 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment télécharger un fichier CAB à l'aide de cette méthode. Remplacez les paramètres *applicationId* et *cabId* par les valeurs qui correspondent à votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

Cette méthode renvoie un code de réponse 302 (redirection) et l'en-tête **Services de localisation** dans la réponse est attribué à l'URI de la signature d’accès partagé (SAS) du fichier CAB. L’appelant est redirigé vers cette URI pour télécharger automatiquement le fichier CAB.

## <a name="related-topics"></a>Rubriques associées

* [Rapport d’intégrité](../publish/health-report.md)
* [Accéder aux données d’analyse à l’aide des services du Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
* [Obtenir les informations sur une erreur de votre application](get-details-for-an-error-in-your-app.md)
* [Obtenir la trace de pile concernant une erreur dans votre application](get-the-stack-trace-for-an-error-in-your-app.md)
