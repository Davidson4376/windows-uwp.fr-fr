---
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: Utilisez cette méthode dans l’API des promotions du Microsoft Store pour gérer les contenus créatifs dans le cadre de campagnes publicitaires.
title: Gérer des contenus créatifs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de promotions du Microsoft Store, campagnes de publicité
ms.localizationpriority: medium
ms.openlocfilehash: 3411ee4c947d809009c2185389f5513a49afce98
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215032"
---
# <a name="manage-creatives"></a>Gérer des contenus créatifs

Utilisez ces méthodes dans l’API des promotions du Microsoft Store pour charger vos propres contenus créatifs personnalisés à utiliser dans la cadre des campagnes publicitaires ou bien récupérez un contenu créatif existant. Un contenu peut être associé à plusieurs chaînes de distribution, dans plusieurs campagnes, sous réserve qu’il représente toujours la même application.

Pour plus d’informations sur la relation entre les contenus créatifs et les campagnes de publicité, les lignes de livraison et les profils ciblage, voir [Exécuter des campagnes publicitaires à l’aide des services du Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

> [!NOTE]
> Lorsque vous utilisez cette API pour charger votre propre contenu créatif, la taille maximale autorisée pour votre contenu créatif est de 40 Ko. Si vous soumettez un fichier de contenu créatif supérieur, cette API ne renvoie pas une erreur, mais la campagne ne sera pas créée.

## <a name="prerequisites"></a>Prérequis

Pour utiliser ces méthodes, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](run-ad-campaigns-using-windows-store-services.md#prerequisites) relatives à l’API de promotions du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de ces méthodes. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.


## <a name="request"></a>Demande

Ces méthodes présentent les URI suivants.

| Type de méthode | URI de requête     |  Description  |
|--------|-----------------------------|---------------|
| PUBLIER   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  Crée un nouveau contenu.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  Obtient le contenu défini par *creativeId*.  |

> [!NOTE]
> Cette API ne prend actuellement pas en charge de méthode PUT.


### <a name="header"></a>Header

| Header        | Type   | Description         |
|---------------|--------|---------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |
| ID de suivi   | GUID   | Facultatif. ID de suivi du flux d’appels.                                  |


### <a name="request-body"></a>Corps de la requête

La méthode POST nécessite un corps de requête JSON avec les champs requis d’un objet de [contenu](#creative).


### <a name="request-examples"></a>Exemples de demande

L’exemple suivant vous explique comment appeler la méthode POST pour créer un élément de contenu. Dans cet exemple, la valeur *content* a été raccourcie, par souci de concision.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative HTTP/1.1
Authorization: Bearer <your access token>

{
  "name": "Contoso App Campaign - Creative 1",
  "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
  "height": 80,
  "width": 480,
  "imageAttributes":
  {
    "imageExtension": "PNG"
  }
}
```

L’exemple suivant vous explique comment appeler la méthode GET pour récupérer un élément de contenu.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Réponse

Ces méthodes renvoient un corps de réponse JSON avec un objet de [contenu](#creative) qui comporte des informations sur le contenu créé ou récupéré. L’exemple suivant représente un corps de réponse associé à ces méthodes. Dans cet exemple, la valeur *content* a été raccourcie, par souci de concision.

```json
{
    "Data": {
        "id": 106126,
        "name": "Contoso App Campaign - Creative 2",
        "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
        "height": 50,
        "width": 300,
        "format": "Banner",
        "imageAttributes":
        {
          "imageExtension": "PNG"
        },
        "storeProductId": "9nblggh42cfd"
    }
}
```


<span id="creative"/>

## <a name="creative-object"></a>Objet de contenu

Les corps de requête et de réponse associés à ces méthodes comportent les champs suivants. Ce tableau signale les champs en lecture seule (qui ne peuvent être modifiés dans la méthode PUT) et les champs requis dans le corps de requête associé à la méthode POST.

| Champ        | Type   |  Description      |  Lecture seule  | Par défaut  |  Requis pour POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  entier   |  ID de l’élément de contenu.     |   Oui    |      |    Non   |       
|  name   |  chaîne   |   Nom de l’élément de contenu.    |    Non   |      |  Oui     |       
|  content   |  chaîne   |  Le contenu de l’image du contenu créatif, au format codé Base64.<br/><br/>**Remarque**&nbsp;&nbsp;La taille maximale autorisée pour votre contenu créatif est de 40 Ko. Si vous soumettez un fichier de contenu créatif supérieur, cette API ne renvoie pas une erreur, mais la campagne ne sera pas créée.     |  Non     |      |   Oui    |       
|  hauteur   |  entier   |   La hauteur du contenu créatif.    |    Non    |      |   Oui    |       
|  largeur   |  entier   |  La largeur du contenu créatif.     |  Non    |     |    Oui   |       
|  landingUrl   |  chaîne   |  Si vous utilisez une campagne de service comme AppsFlyer, Kochava, régler ou Vungle de suivi pour mesurer l’analytique de l’installation de votre application, affectez votre URL de suivi dans ce champ lorsque vous appelez la méthode POST (si spécifié, cette valeur doit être un URI valide). Si vous n’utilisez pas de service de suivi de campagne, omettez cette valeur lorsque vous appelez la méthode POST (dans ce cas, cette URL est créée automatiquement).   |  Non    |     |   Oui    |
|  Format   |  chaîne   |   Le format d’annonce. Actuellement, la seule valeur prise en charge est  **Banner**.    |   Non    |  Bannière   |  Non     |       
|  imageAttributes   | [ImageAttributes](#image-attributes)    |   Fournit des attributs pour l’élément de contenu.     |   Non    |      |   Oui    |       
|  storeProductId   |  chaîne   |   L' [ID Store](in-app-purchases-and-trials.md#store-ids) de l’application à laquelle est associée cette campagne publicitaire. Un produit peut par exemple présenter l’ID 9nblggh42cfd du Windows Store.    |   Non    |    |  Non     |   |  


<span id="image-attributes"/>

## <a name="imageattributes-object"></a>Objet ImageAttributes

| Champ        | Type   |  Description      |  en lecture seule  | Valeur par défaut  | Requis pour POST |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   chaîne  |   Une des valeurs suivantes : **PNG** ou **JPG**.    |    Non   |      |   Oui    |       |


## <a name="related-topics"></a>Rubriques connexes

* [Exécuter des campagnes marketing à l’aide des Services de Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Gérer des campagnes de publicité](manage-ad-campaigns.md)
* [Gérer les lignes de remise pour les campagnes de publicité](manage-delivery-lines-for-ad-campaigns.md)
* [Gérer les profils de ciblage pour les campagnes de publicité](manage-targeting-profiles-for-ad-campaigns.md)
* [Obtenir des données de performances de campagne ad](get-ad-campaign-performance-data.md)
