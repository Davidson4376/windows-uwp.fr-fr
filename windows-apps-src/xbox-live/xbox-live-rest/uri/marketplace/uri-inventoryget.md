---
title: GET (/users/me/inventory)
assetID: 7b74dd08-2854-319d-3ed0-ddee75d922b9
permalink: en-us/docs/xboxlive/rest/uri-inventoryget.html
description: " GET (/users/me/inventory)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 31c787108fad84f06b02ded3958f9d2f99727cbe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632204"
---
# <a name="get-usersmeinventory"></a>GET (/users/me/inventory)
Fournit l’ensemble de l’inventaire actuellement associé à l’utilisateur fourni à l’appelant.
Le domaine pour ces URI est `inventory.xboxlive.com`.

  * [Remarques](#ID4EV)
  * [Paramètres de chaîne de requête](#ID4EHB)
  * [Exemple de demande](#ID4EDE)
  * [Corps de la réponse](#ID4ERE)

<a id="ID4EV"></a>


## <a name="remarks"></a>Notes

Aucune stratégie ne vérifie, l’application, ou de filtrage aura lieu dans le cadre de cet appel. Les appelants ont la possibilité de transmettre des paramètres de requête afin de limiter les résultats retournés.

Les appelants peuvent parcourir les résultats avec un jeton de liaison comme ceux fournis via une réponse précédente : **/users/me/inventory ? continuationToken = continuationTokenString**.

L’appelant peut effectuer un appel aux détails de l’API avec une URL de l’élément spécifique afin d’afficher des informations sur un élément spécifique.

<a id="ID4EHB"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

| Paramètre| Type| Description|
| --- | --- | --- |
| disponibilité| chaîne| La disponibilité actuelle d’éléments à retourner. Valeur par défaut est « Disponible », plage de dates qui retourne les éléments pour lesquels la date actuelle se situe entre la date de début et la fin. D’autres valeurs incluent « All », qui retourne tous les éléments et « Non disponible » qui retourne des éléments pour lesquels la date actuelle se situe en dehors de la plage de dates start date et de fin et il est par conséquent pas disponible actuellement. |
| Conteneur| chaîne| Facultatif. Si vous définissez la valeur de l’ID de produit d’un jeu, les résultats à partir de l’inventaire incluent uniquement les éléments associés à ce type de jeu. Cela est particulièrement utile lors de l’appel de l’inventaire à partir de votre serveur pour filtrer les résultats sur les produits d’un jeu spécifique.|
| expandSatisfyingEntitlements| chaîne| Un indicateur qui indique si la réponse inclut tous les droits satisfaisantes l’utilisateur dans les résultats retournés. La valeur par défaut est « false ». Lorsque ce paramètre est utilisé avec la valeur « true », tous les produits qui sont accordées à l’utilisateur via satisfaisant aux éléments regroupés comme des droits, les achats de Xbox 360 migrés pour Xbox One, avantages de l’abonnement, etc. sont ajoutés aux résultats. Lorsque cette valeur est « false » uniquement les éléments parents tels que ProductID du faisceau sont retournés dans les résultats et pas les éléments inclus. **Remarque :** À l’aide de ce paramètre avec la valeur « true » est uniquement pris en charge si le paramètre itemType n’est pas inclus dans l’URI, sinon vous recevrez une erreur HTTP 400. |  
  | productIds | chaîne |  Une collection d’identificateurs ProductID que vous souhaitez récupérer spécifiquement de l’inventaire de l’utilisateur, séparés par ','.  Si l’utilisateur ne dispose pas d’un ID de produit fournie dans les résultats de l’inventaire, cet élément n’apparaissent pas dans les résultats à partir de l’appel d’API. Si vous passez dans l’ID de produit d’une offre groupée, ainsi que le jeu de paramètres expandSatisfyingEntitlements sur true, tous les éléments inclus dans le regroupement sont retournés dans les résultats de l’appel (si vous avez spécifié leurs identificateurs ProductID dans votre chaîne de requête ou non).   |
  | État | chaîne | L’état des éléments à retourner. La valeur par défaut est « tout », qui retourne tous les éléments. Autres valeurs sont « activées », ce qui indique que seul itemsthat sont activés doit être retourné, « Suspendu », ce qui indique que seuls les éléments qui sont suspendus doivent être retournés, le « Expiré », ce qui indique que seuls les éléments qui ont expiré doivent être renvoyés, » Annulée », qui indique que seuls les éléments qui sont annulées doivent être retournées, et que « Renouvellement », ce qui indique que seuls les éléments qui ont été renouvelés doivent être renvoyées.  |

Outre ces options, la ressource prend en charge les mécanismes d’échange standard.

<a id="ID4EDE"></a>


## <a name="sample-request"></a>Exemple de demande

Le nom de domaine complet pour cette méthode de l’URI est `https://inventory.xboxlive.com/users/me/inventory.
         `

> [!NOTE] 
> Les utilisateurs qui sont considérés comme varie selon le jeton fourni, qui peut inclure plusieurs utilisateurs. Si vous souhaitez que l’inventaire d’un seul utilisateur, vous devez également fournir le hachage de l’utilisateur de l’utilisateur spécifique que vous souhaitez prendre en compte exclusivement.

.

<a id="ID4ERE"></a>


## <a name="response-body"></a>Corps de la réponse

Si l’appel réussit, le service retourne un tableau d’éléments d’inventaire. Consultez [inventoryItem (JSON)](../../json/json-inventoryitem.md).

<a id="ID4E4E"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
  "pagingInfo": {
    "continuationToken": string,
    "totalItems": int
  },
  "items":
  {
    "url": string,
    "itemType": "Music",
    "titleId": string,
    "containers": string,
    "obtained": DateTime,
    "startDate": DateTime,
    "endDate": DateTime,
    "state": "Enabled"  
}

```


<a id="ID4EHF"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EJF"></a>


##### <a name="parent"></a>Parent

[/users/me/inventory](uri-inventory.md)


<a id="ID4ETF"></a>


##### <a name="further-information"></a>Informations supplémentaires

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [URI de la place de marché](atoc-reference-marketplace.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)
