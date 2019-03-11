---
title: GET (/users/xuid(xuid)/lists/PINS/{listname})
assetID: a63f595a-61dd-5885-c405-9833230abb94
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameget.html
description: " GET (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a31e6a6b541276d3191ba5d40767a1929c70168
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641624"
---
# <a name="get-usersxuidxuidlistspinslistname"></a>GET (/users/xuid(xuid)/lists/PINS/{listname})
Retourne le contenu d’une liste. Le domaine pour ces URI est `eplists.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EIB)
  * [Paramètres de chaîne de requête](#ID4ETB)
  * [Autorisation](#ID4ESD)
  * [En-têtes de demande nécessaires](#ID4E6D)
  * [Corps de la demande](#ID4EVF)
  * [Codes d’état HTTP](#ID4EAG)
  * [Corps de la réponse](#ID4E5CAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
Le **listCount** champ dans les données renvoyées indique combien d’éléments se trouvent dans la liste de total conservée par le service, par conséquent, il peut être utilisé pour déterminer où la fin de la liste, et il s’agit potentiellement d’un nombre différent de nombre les éléments spécifiques ont été renvoyés par la requête.
 
Si la liste n’existe pas encore, les résultats ne contiennent aucun élément de liste, le **listCount** sera égal à zéro et le **listVersion** sera égal à zéro.
  
<a id="ID4EIB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| chaîne| ID d’utilisateur de la Xbox (XUID).| 
| listtype| chaîne| Type de la liste (comment il est utilisé et comment il agit). Toujours « PIN » pour ces méthodes associées.| 
| nom de la liste| chaîne| Nom de la liste (liste d’un type donné pour agir sur). Toujours « XBLPins » pour les éléments dans les codes confidentiels.| 
  
<a id="ID4ETB"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| entier signé 32 bits| Facultatif. Nombre d’éléments à ignorer dans l’énumération avant de retourner des résultats. Valeur par défaut : 0.| 
| maxItems| entier signé 32 bits| Facultatif. Nombre maximal d’éléments à retourner. La valeur par défaut est 25 articles si aucun maximum n’est spécifié dans la demande. Le service ne place pas de maximum sur cette valeur ; Si la valeur est supérieure au nombre d’éléments dans la liste, puis tous les éléments seront affichera avec aucune erreur.| 
| filterItemId| chaîne| Facultatif. Spécifie l’élément à rechercher dans la liste. Retourne toutes les instances de l’élément dans la liste. Permet au client déterminer rapidement si et où un élément est dans une liste. Portée de main pour les grandes listes déterminer toutes les instances d’un élément sans l’itération dans la liste entière. Valeur par défaut : null.| 
| filterContentType| chaîne| Facultatif. Spécifie une liste séparée par des virgules des types de contenu à retourner (ne retourne pas les types pas dans la liste). Utilisé pour obtenir uniquement certains types de contenu à partir d’une liste. Une liste séparée par des virgules des types de contenu est utilisée pour ce filtre. (Plusieurs types de contenu peuvent être interrogées dans un seul appel). Types de contenu pris en charge incluent tous les types de média définis par Entertainment découverte de Services (EDS). Valeur par défaut : null (tous les types de contenu).| 
| filterDeviceType| chaîne| Facultatif. Spécifie une liste séparée par des virgules des types de périphérique à retourner (ne retourne pas les types pas dans la liste). Filtre l’ensemble de retour pour retourner uniquement les éléments qui ont été insérées à partir d’un ensemble spécifique de types d’appareils. Une liste séparée par des virgules des types d’appareils est utilisée pour ce filtre (plusieurs types d’appareils peuvent être interrogées dans un seul appel). Valeurs possibles : XboxOne, MCapensis, WindowsPhone, WindowsPhone7, Web, PC, MoLive. Valeur par défaut : null (tous les types de contenu).| 
  
<a id="ID4ESD"></a>

 
## <a name="authorization"></a>Authorization
 
Cet appel attend un jeton XSTS SAML dans le **autorisation** en-tête. Une revendication Xuid doit exister au sein de ce jeton SAML pour identifier l’appelant. Cette valeur est utilisée pour déterminer si l’appelant dispose des droits d’accès aux données de liste en question. La liste elle-même est identifiée par le Xuid ainsi et sera incluse dans l’URI pour obtenir la liste. À l’aide de cela, nous peut prochainement accès prise en charge partagé à des listes, mais qui n’est pas une fonctionnalité pour l’instant. Actuellement, toutes les listes qui accède à un utilisateur sera leurs propres et il n’existe aucun accès partagé. Par conséquent, le Xuid dans l’URI doit correspondre à la Xuid dans le jeton de revendications SAML. 

> [!NOTE] 
> XBL Auth 2.0 et 3.0 jetons sont pris en charge à l’heure actuelle. 


  
<a id="ID4E6D"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| Contient le jeton STS utilisé pour authentifier et autoriser la demande. Doit être un jeton à partir du service XSTS et inclure le XUID dans l’un des revendications. | 
| Version X-XBL-contrat| Spécifie la version de l’API est demandées (entiers positifs). Version de prend en charge des codes confidentiels 2. Si cet en-tête est manquant ou la valeur n’est pas pris en charge le service renvoie un code 400 – demande incorrecte qu’avec « en-tête de version de contrat non pris en charge ou manquant » dans la description d’état.| 
| Content-Type| Spécifie si le contenu du corps de demande/réponse est dans json ou xml. Valeurs prises en charge sont « application/json » et « application/xml »| 
| If-Match| Cet en-tête doit contenir le numéro de version en cours d’une liste lors de l’établissement de modifier des demandes. Utilisation de demandes PUT, POST, de modifier ou supprimer des verbes. Si la version dans l’en-tête « If-Match » ne correspond pas à la version actuelle de la liste, la demande est rejetée avec un HTTP 412 – Échec de la précondition code de retour. (facultatif) Peut également être utilisé pour obtient, si présent et la version passée correspond à la version actuelle de la liste puis aucune donnée de liste et un HTTP 304 – pas modifié le code sera retourné. | 
  
<a id="ID4EVF"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EAG"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| La demande a abouti. Le corps de réponse doit contenir la ressource demandée (pour une opération GET). POST et PUT demandes recevra des métadonnées de la liste à jour (version de la liste, nombre, etc.).| 
| 201| Créé le| Une nouvelle liste a été créée. Il est renvoyé dans le menu Insertion initial à une liste. La réponse inclut des métadonnées à jour dans la liste et l’en-tête d’emplacement contient l’URI de la liste.| 
| 304| Non modifié| Obtient renvoyées sur. Indique que le client dispose de la dernière version de la liste. Le service compare la valeur de la <b>If-Match</b> en-tête vers la version de la liste. Si elles sont égales, un 304 obtient retourné sans aucune donnée.| 
| 400| demande incorrecte| La demande est incorrecte. Peut être une valeur non valide ou un type pour un paramètre de chaîne d’URI ou d’une requête. Peut également être le résultat d’un paramètre obligatoire manquant ou valeur de données, ou la demande indiqué une version manquante ou non valide de l’API. Consultez le <b>X-XBL--Version de contrat</b> en-tête.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 404| Introuvable| L’appelant n’a pas accès à la ressource. Cela indique qu’aucune liste de ce type n’a été créé.| 
| 412| Échoué de la précondition| Indique que la version de la liste a changé et qu’une demande de modification a échoué. Une demande de modification est une insertion, mise à jour ou supprimer. Le service vérifie les <b>If-Match</b> en-tête pour la version de la liste. Si elle ne correspond pas à la version actuelle de la liste, puis l’opération échoue et il est renvoyé, ainsi que les métadonnées de liste actuel (qui incluent la version actuelle).| 
| 500| erreur de serveur interne| Le service refuse la demande en raison d’une erreur côté serveur.| 
| 501| Non implémenté| L’appelant a demandé un URI qui n’a pas été implémenté sur le serveur. (Actuellement uniquement utilisé lorsqu’une demande est effectuée pour un nom de la liste qui n’est pas dans la liste verte.)| 
| 503| Service non disponible| Le serveur refuse la demande, généralement en raison d’une charge excessive. Après un certain délai (voir la <b>Retry-after</b> en-tête), la demande peut être retentée.| 
  
<a id="ID4E5CAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
<a id="ID4EEDAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{ 
"ListMetadata":
  {"ListVersion": 12,
   "ListCount": 6,
   "MaxListSize": 200,
   "AccessSetting": "OwnerOnly",
   "AllowDuplicates": true
  },
"ListItems":
  [{ 
   "Index": 0,
   "DateAdded": "\/Date(1198908717056)/",
   "DateModified": "\/Date(1198908717056)/",
   "HydrationResult": "Indeterminate",
   "HydratedItem": null

   "Item":
   {
     "ContentType": "Movie",
     "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
     "ProviderId": null,
     "Provider": null,
     "ImageUrl": "https://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC",
     "Title": "The Dark Knight",
     "SubTitle": null,
     "Locale": "en-us",
     "AltImageUrl": null,
     "DeviceType": "XboxOne"
    }
  }]
}
         
```

   
<a id="ID4EODAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EQDAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

  
<a id="ID4E1DAC"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[URI de la place de marché](../marketplace/atoc-reference-marketplace.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   