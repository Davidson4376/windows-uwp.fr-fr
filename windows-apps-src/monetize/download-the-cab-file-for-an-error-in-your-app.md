---
ms.assetid: ''
description: Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour télécharger le fichier CAB en cas d'erreur dans votre app.
title: Télécharger le fichier CAB concernant une erreur dans votre application
ms.date: 06/16/2017
ms.topic: article
keywords: windows 10, uwp, API d'analyse du Microsoft Store, télécharger le fichier CAB
ms.localizationpriority: medium
ms.openlocfilehash: a4643f94236e62c46c12fd656ab5ddba5e1e0632
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594534"
---
# <a name="download-the-cab-file-for-an-error-in-your-app"></a>Télécharger le fichier CAB concernant une erreur dans votre application

Utilisez cette méthode dans l’API d’analytique Microsoft Store pour télécharger le fichier CAB qui est associé à une erreur particulière dans votre application qui a été signalée pour les partenaires. Cette méthode ne peut télécharger que le fichier CAB concernant une erreur d’app survenue dans les 30 derniers jours. Téléchargements de fichiers CAB sont également disponibles dans le **échecs** section de la [rapport d’intégrité](../publish/health-report.md) dans Partner Center.

Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md) afin de récupérer l’ID du fichier CAB à télécharger.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez l’ID du fichier CAB à télécharger. Pour obtenir cet ID, utilisez la méthode [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md) pour récupérer les détails d’une erreur spécifique dans votre application, en spécifiant la valeur **cabId** dans le corps de la réponse.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  |
|---------------|--------|---------------|------|
| applicationId | chaîne | ID Store de l’application pour laquelle vous voulez télécharger un fichier CAB. L’ID de Store est disponible sur le [page identité des applications](../publish/view-app-identity-details.md) de partenaires. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8. |  Oui  |
| cabId | chaîne | L’ID unique du fichier CAB à télécharger. Pour obtenir cet ID, utilisez la méthode [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md) pour récupérer les détails d’une erreur spécifique dans votre application, en spécifiant la valeur **cabId** dans le corps de la réponse. |  Oui  |

 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment télécharger un fichier CAB avec cette méthode. Remplacez les paramètres *applicationId* et *cabId* par les valeurs qui correspondent à votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

Cette méthode renvoie un code de réponse 302 (redirection) et l'en-tête **Services de localisation** dans la réponse est attribué à l'URI de la signature d’accès partagé (SAS) du fichier CAB. L’appelant est redirigé vers cette URI pour télécharger automatiquement le fichier CAB.

## <a name="related-topics"></a>Rubriques connexes

* [Rapport d’intégrité](../publish/health-report.md)
* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données de signalement d’erreurs](get-error-reporting-data.md)
* [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md)
* [Obtenir la trace de pile pour une erreur dans votre application](get-the-stack-trace-for-an-error-in-your-app.md)
