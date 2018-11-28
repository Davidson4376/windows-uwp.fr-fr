---
description: Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour télécharger le fichier CAB en cas d'erreur dans votre application de bureau.
title: Télécharger le fichier CAB pour une erreur dans votre application de bureau
ms.date: 03/06/2018
ms.topic: article
keywords: windows 10, uwp, API d'analyse du MicrosoftStore, télécharger le fichier CAB, application de bureau
ms.localizationpriority: medium
ms.openlocfilehash: 1e3535f18b8127ea18bca234cdcc9b695e89ebfd
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7826046"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>Télécharger le fichier CAB pour une erreur dans votre application de bureau

Utilisez cette méthode dans l'API d'analyse du MicrosoftStore pour télécharger le fichier CAB associé à une erreur spécifique pour une application de bureau que vous avez ajoutée au [Du programme d’application de bureau Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504). Cette méthode ne peut télécharger que le fichier CAB concernant une erreur d’app survenue dans les 30derniers jours. Téléchargements de fichier CAB sont également disponibles dans le [rapport d’intégrité](https://msdn.microsoft.com/library/windows/desktop/mt826504) pour les applications de bureau dans l’espace partenaires.

Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les informations sur une erreur de votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) afin de récupérer le hachage d'ID du fichier CAB que vous souhaitez télécharger.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenir le hachage de l'ID du fichier CAB que vous souhaitez télécharger. Pour obtenir cet valeur, utilisez la méthode [Obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer les détails d’une erreur spécifique dans votre app, en spécifiant la valeur **cabIdHash** dans le corps de la réponse.

## <a name="request"></a>Demande


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  |
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID de produit de l’application de bureau pour laquelle vous souhaitez télécharger un fichier CAB. Pour obtenir l’ID de produit d’une application de bureau, ouvrez tout [l’espace partenaires analytique créer des rapports pour votre application de bureau](https://msdn.microsoft.com/library/windows/desktop/mt826504) (par exemple, le **rapport d’intégrité**) et récupérez l’ID produit à partir de l’URL. |  Oui  |
| cabIdHash | chaîne | Le hachage de l'ID unique du fichier CAB que vous souhaitez télécharger. Pour obtenir cet valeur, utilisez la méthode [Obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer les détails d’une erreur spécifique dans votre app, en spécifiant la valeur **cabIdHash** dans le corps de la réponse. |  Oui  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment télécharger un fichier CAB avec cette méthode. Remplacez les paramètres *applicationId* et *cabIdHash* par les valeurs qui correspondent à votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

Cette méthode renvoie un code de réponse 302 (redirection) et l'en-tête **Services de localisation** dans la réponse est attribué à l'URI de la signature d’accès partagé (SAS) du fichier CAB. L’appelant est redirigé vers cette URI pour télécharger automatiquement le fichier CAB.

## <a name="related-topics"></a>Rubriques associées

* [Rapport d’intégrité](../publish/health-report.md)
* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données de signalement d’erreurs pour votre application de bureau](get-desktop-application-error-reporting-data.md)
* [Obtenir les informations sur une erreur de votre application de bureau](get-details-for-an-error-in-your-desktop-application.md)
* [Obtenir la trace de pile concernant une erreur dans votre application de bureau](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
