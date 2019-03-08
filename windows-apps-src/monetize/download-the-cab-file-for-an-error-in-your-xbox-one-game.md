---
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour télécharger le fichier CAB pour une erreur dans votre jeu Xbox One.
title: Télécharger le fichier CAB concernant une erreur dans votre jeu Xbox One
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, API d'analyse du Microsoft Store, télécharger le fichier CAB
ms.localizationpriority: medium
ms.openlocfilehash: 736219533a254e6380c10600e97f707f15e37de6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604324"
---
# <a name="download-the-cab-file-for-an-error-in-your-xbox-one-game"></a>Télécharger le fichier CAB concernant une erreur dans votre jeu Xbox One

Utilisez cette méthode dans l’API d’analytique Microsoft Store pour télécharger le fichier CAB qui est associé à une erreur particulière dans votre jeu Xbox One qui a été reçue via le portail de développement Xbox (XDP) et disponible dans le tableau de bord XDP Analytique Partner Center. Cette méthode peut télécharger uniquement le fichier CAB pour une erreur s’est produite au cours des 30 derniers jours.

Avant de pouvoir utiliser cette méthode, vous devez d’abord utiliser le [obtenir les détails d’une erreur dans votre Xbox One jeu](get-details-for-an-error-in-your-xbox-one-game.md) méthode pour récupérer l’ID du fichier CAB à télécharger.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez l’ID du fichier CAB à télécharger. Pour obtenir cet ID, utilisez le [obtenir les détails d’une erreur dans votre Xbox One jeu](get-details-for-an-error-in-your-xbox-one-game.md) méthode pour récupérer les détails pour une erreur spécifique dans votre application, utilisez le **cabId** valeur dans le corps de réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  |
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID de produit du jeu Xbox One pour lequel vous téléchargez le fichier CAB. Pour obtenir l’ID produit de votre jeu, accédez à votre jeu dans le portail de développement Xbox (XDP) et récupérez l’ID produit à partir de l’URL. Si vous téléchargez vos données d’intégrité à partir du rapport d’analytique de partenaires de Windows, l’ID de produit est également incluse dans le fichier .tsv. |  Oui  |
| cabId | chaîne | L’ID unique du fichier CAB à télécharger. Pour obtenir cet ID, utilisez le [obtenir les détails d’une erreur dans votre Xbox One jeu](get-details-for-an-error-in-your-xbox-one-game.md) méthode pour récupérer les détails pour une erreur spécifique dans votre application, utilisez le **cabId** valeur dans le corps de réponse de cette méthode. |  Oui  |

 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment télécharger un fichier CAB avec cette méthode. Remplacez les paramètres *applicationId* et *cabId* par les valeurs qui correspondent à votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

Cette méthode renvoie un code de réponse 302 (redirection) et l'en-tête **Services de localisation** dans la réponse est attribué à l'URI de la signature d’accès partagé (SAS) du fichier CAB. L’appelant est redirigé vers cette URI pour télécharger automatiquement le fichier CAB.

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des rapports d’erreurs données pour votre Xbox One jeu](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obtenir les détails d’une erreur dans votre Xbox One jeu](get-details-for-an-error-in-your-xbox-one-game.md)
* [Obtenir la trace de pile pour une erreur dans votre Xbox One jeu](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
