---
description: Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour télécharger le fichier CAB en cas d'erreur dans votre application de bureau.
title: Télécharger le fichier CAB concernant une erreur dans votre application de bureau
ms.date: 03/06/2018
ms.topic: article
keywords: windows 10, uwp, API d'analyse du Microsoft Store, télécharger le fichier CAB, application de bureau
ms.localizationpriority: medium
ms.openlocfilehash: 7a0c6203b3a55ecf8ca5e9473a41a7e6fb233000
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371293"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>Télécharger le fichier CAB concernant une erreur dans votre application de bureau

Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour télécharger le fichier CAB associé à une erreur spécifique pour une application de bureau que vous avez ajoutée au [Du programme d’application de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Cette méthode ne peut télécharger que le fichier CAB concernant une erreur d’app survenue dans les 30 derniers jours. Téléchargements de fichiers CAB sont également disponibles dans le [rapport d’intégrité](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) pour les applications de bureau dans l’espace partenaires.

Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les informations sur une erreur de votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) afin de récupérer le hachage d'ID du fichier CAB que vous souhaitez télécharger.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenir le hachage de l'ID du fichier CAB que vous souhaitez télécharger. Pour obtenir cet valeur, utilisez la méthode [Obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer les détails d’une erreur spécifique dans votre app, en spécifiant la valeur **cabIdHash** dans le corps de la réponse.

## <a name="request"></a>Demande


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>En-tête de requête

| Header        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | type   |  Description      |  Obligatoire  |
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID de produit de l’application de bureau pour laquelle vous souhaitez télécharger un fichier CAB. Pour obtenir l’ID de produit d’une application de bureau, ouvrez les [analytique de partenaires de rapports pour votre application de bureau](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (telles que la **rapport d’intégrité**) et récupérer l’ID de produit dans l’URL. |  Oui  |
| cabIdHash | chaîne | Le hachage de l'ID unique du fichier CAB que vous souhaitez télécharger. Pour obtenir cette valeur, utilisez la méthode [Obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer les détails d’une erreur spécifique dans votre app, en spécifiant la valeur **cabIdHash** dans le corps de la réponse. |  Oui  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment télécharger un fichier CAB avec cette méthode. Remplacez les paramètres *applicationId* et *cabIdHash* par les valeurs qui correspondent à votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

Cette méthode renvoie un code de réponse 302 (redirection) et l'en-tête **Services de localisation** dans la réponse est attribué à l'URI de la signature d’accès partagé (SAS) du fichier CAB. L’appelant est redirigé vers cette URI pour télécharger automatiquement le fichier CAB.

## <a name="related-topics"></a>Rubriques connexes

* [Rapport d’intégrité](../publish/health-report.md)
* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données pour votre application de bureau de signalement d’erreurs](get-desktop-application-error-reporting-data.md)
* [Obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md)
* [Obtenir la trace de pile pour une erreur dans votre application de bureau](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
