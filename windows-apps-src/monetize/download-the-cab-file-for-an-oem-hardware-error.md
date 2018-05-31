---
author: mcleanbyron
ms.assetid: E64030CA-EC00-4113-9939-26D5688C61BC
description: Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour télécharger le fichier CAB en cas d'erreur matérielle. Cette méthode est uniquement destinée aux fabricants OEM.
title: Téléchargez le fichier CAB pour une erreur matérielle d'OEM
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API d'analyse du MicrosoftStore, télécharger le fichier CAB
ms.localizationpriority: medium
ms.openlocfilehash: 0be709136ed5875d69431f0ab60efd76f5bbc80b
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/17/2018
ms.locfileid: "1662819"
---
# <a name="download-the-cab-file-for-an-oem-hardware-error"></a>Téléchargez le fichier CAB pour une erreur matérielle d'OEM

Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour télécharger le fichier CAB associé à un erreur matérielle OEM spécifique. Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [obtenir des informations détaillées sur une erreur matérielle d'OEM](get-details-for-an-oem-hardware-error.md) afin de récupérer l’ID du fichier CAB à télécharger.

Vous pouvez obtenir davantage d'informations sur les erreurs matérielles OEM en utilisant les méthodes [Obtenir des données de rapport d'erreur matérielle OEM](get-oem-hardware-error-reporting-data.md) et [obtenir des détails sur une erreur matérielle OEM](get-details-for-an-oem-hardware-error.md) dans l'API d'analyse du MicrosoftStore.

> [!NOTE]
> Cette méthode peut uniquement être utilisée par les comptes de développeur qui participent au [programme du centre de développement matériel Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez l’ID du fichier CAB à télécharger. Pour obtenir cet ID, utilisez la méthode [obtenir les détails d’une erreur matérielle d'OEM](get-details-for-an-oem-hardware-error.md) pour récupérer les détails d’une erreur matérielle, en spécifiant la valeur **cabIdHash** dans le corps de la réponse de cette méthode.

## <a name="request"></a>Demande


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/cabdownload``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  |
|---------------|--------|---------------|------|
| cabIdHash | chaîne | L’ID unique du fichier CAB à télécharger. Pour obtenir cet ID, utilisez la méthode [obtenir les détails d’une erreur matérielle d'OEM](get-details-for-an-oem-hardware-error.md) pour récupérer les détails d’une erreur spécifique dans votre application, en spécifiant la valeur **cabIdHash** dans le corps de la réponse de cette méthode. |  Oui  |

 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment télécharger un fichier CAB à l'aide de cette méthode.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/cabdownload?cabIdHash=c1a51104-d682-4230-be14-7278b18e3555 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

Cette méthode renvoie un code de réponse 302 (redirection) et l'en-tête **Services de localisation** dans la réponse est attribué à l'URI de la signature d’accès partagé (SAS) du fichier CAB. L’appelant est redirigé vers cette URI pour télécharger automatiquement le fichier CAB.

## <a name="related-topics"></a>Rubriques associées

* [Obtenir les données de rapport d’erreurs matérielles de fabricant d'ordinateurs (OEM)](get-oem-hardware-error-reporting-data.md)
* [Obtenir des informations détaillées sur une erreur matérielle d'OEM](get-details-for-an-oem-hardware-error.md)
