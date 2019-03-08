---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
assetID: f7235c68-3214-db10-52ad-c3665b31b8cd
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameremoveitemspost.html
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5e62d978e94286c815f2274c56684e4fd6bbf2d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637294"
---
# <a name="post-usersxuidxuidlistspinslistnameremoveitems"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
Supprime les éléments d’une liste par itemId. Le domaine pour ces URI est `eplists.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EFB)
  * [Paramètres de chaîne de requête](#ID4EOC)
  * [Corps de la demande](#ID4EZC)
  * [Codes d’état HTTP](#ID4EED)
  * [En-têtes de demande nécessaires](#ID4E1AAC)
  * [Corps de la réponse](#ID4EQCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes 
 
Supprimer des éléments dans la liste en spécifiant l’ID d’élément au lieu de l’index. Uniquement 100 éléments sont autorisés à supprimer dans un seul appel **si aucun élément n’est passés, l’intégralité de la liste est supprimé (liste restera mais vide, numéro de version continuera à être incrémenté).** Une fois que les éléments sont supprimés, la liste est « compactée » tels qu’aucune faille n’est laissés dans l’ordre d’index. 
 
Un « si-correspondance : versionNumber « en-tête est facultatif pour cet appel. Si elle est incluse, elle est validée. VersionNumber est le numéro de version actuelle du fichier. S’il est inclus et ne correspond pas à l’actuelle version liste nombre, puis un HTTP 412 – code d’état d’échec de la condition préalable est renvoyée et le corps de la réponse contient les métadonnées les plus récentes de la liste qui inclut le numéro de version actuel. Pour cela, pour vous protéger contre les mises à jour à partir de différents clients trampling sur eux. 
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI 
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| XUID| chaîne| XUID de l’utilisateur.| 
| nom de la liste| chaîne| Nom de la liste à manipuler.| 
  
<a id="ID4EOC"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête 
 
Paramètres de requête ne sont pas pris en charge.
  
<a id="ID4EZC"></a>

 
## <a name="request-body"></a>Corps de la requête 
 

```cpp
{
   "Items":
   [{"ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
      "ProviderId":  null,
      "Provider":  null
   }]
}

    
```

  
<a id="ID4EED"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP 
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | 
| 200| OK | La demande a abouti. Le corps de réponse doit contenir la ressource demandée (pour une opération GET). POST et PUT demandes recevra des métadonnées de la liste à jour (version de la liste, nombre, etc.).| 
| 201| Créé le | Une nouvelle liste a été créée. Il est renvoyé dans le menu Insertion initial à une liste. La réponse inclut des métadonnées à jour dans la liste et l’en-tête d’emplacement contient l’URI de la liste.| 
| 304| Non modifié| Obtient renvoyées sur. Indique que le client dispose de la dernière version de la liste. Le service compare la valeur de la <b>If-Match</b> en-tête vers la version de la liste. Si elles sont égales, un 304 obtient retourné sans aucune donnée. | 
| 400| demande incorrecte | La demande est incorrecte. Peut être une valeur non valide ou un type pour un paramètre de chaîne d’URI ou d’une requête. Peut également être le résultat d’un paramètre obligatoire manquant ou valeur de données, ou la demande indiqué une version manquante ou non valide de l’API. Consultez le <b>X-XBL--Version de contrat</b> en-tête. | 
| 401| Non autorisé | La demande requiert une authentification utilisateur.| 
| 403| Interdit | La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 404| Introuvable | L’appelant n’a pas accès à la ressource. Cela indique qu’aucune liste de ce type n’a été créé.| 
| 412| Échoué de la précondition | Indique que la version de la liste a changé et qu’une demande de modification a échoué. Une demande de modification est une insertion, mise à jour ou supprimer. Le service vérifie les <b>If-Match</b> en-tête pour la version de la liste. Si elle ne correspond pas à la version actuelle de la liste, puis l’opération échoue et il est renvoyé, ainsi que les métadonnées de liste actuel (qui incluent la version actuelle). | 
| 500| erreur de serveur interne | Le service refuse la demande en raison d’une erreur côté serveur.| 
| 501| Non implémenté | L’appelant a demandé un URI qui n’a pas été implémenté sur le serveur. (Actuellement uniquement utilisé lorsqu’une demande est effectuée pour un nom de la liste qui n’est pas dans la liste verte.)| 
| 503| Service non disponible | Le serveur refuse la demande, généralement en raison d’une charge excessive. Après un certain délai (voir la <b>Retry-after</b> en-tête), la demande peut être retentée. | 
  
<a id="ID4E1AAC"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| Contient le jeton STS utilisé pour authentifier et autoriser la demande. Doit être un jeton à partir du service XSTS et inclure le XUID dans l’un des revendications. | 
| Version X-XBL-contrat| Spécifie la version de l’API est demandées (entiers positifs). Version de prend en charge des codes confidentiels 2. Si cet en-tête est manquant ou la valeur n’est pas pris en charge le service renvoie un code 400 – demande incorrecte qu’avec « en-tête de version de contrat non pris en charge ou manquant » dans la description d’état. | 
| Content-Type| Spécifie si le contenu du corps de demande/réponse est dans json ou xml. Valeurs prises en charge sont « application/json » et « application/xml »| 
| If-Match| Cet en-tête doit contenir le numéro de version en cours d’une liste lors de l’établissement de modifier des demandes. Utilisation de demandes PUT, POST, de modifier ou supprimer des verbes. Si la version dans l’en-tête « If-Match » ne correspond pas à la version actuelle de la liste, la demande est rejetée avec un HTTP 412 – Échec de la précondition code de retour. (facultatif) Peut également être utilisé pour obtient, si présent et la version passée correspond à la version actuelle de la liste puis aucune donnée de liste et un HTTP 304 – pas modifié le code sera retourné. | 
  
<a id="ID4EQCAC"></a>

 
## <a name="response-body"></a>Corps de la réponse 
 
Si l’appel réussit, le service retourne les métadonnées les plus récentes pour obtenir la liste. 
 
<a id="ID4E1CAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse 
 

```cpp
{
        "ListVersion":  1,
        "ListCount":  1,
        "MaxListSize": 200,
        "AllowDuplicates": "true",
        "AccessSetting":  "OwnerOnly"
        }

      
```

   
<a id="ID4EGDAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EIDAC"></a>

 
##### <a name="parent"></a>Parent 

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)

   