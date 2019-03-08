---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: df61be42-c229-7408-5e4c-dbf4ae95b52b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindexpost.html
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7711beee6551c40afe1afcb031278484a3dc5820
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627464"
---
# <a name="post-usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
Déplace un élément dans une liste vers un autre emplacement dans la liste. Le domaine pour ces URI est `eplists.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EEB)
  * [Paramètres de chaîne de requête](#ID4EWC)
  * [Corps de la demande](#ID4EVD)
  * [Codes d’état HTTP](#ID4EEE)
  * [En-têtes de demande nécessaires](#ID4E1BAC)
  * [Corps de la réponse](#ID4EQDAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes 
 
Cet appel est fourni pour permettre au client de facilement déplacer un élément vers un autre index au sein de la liste en une seule opération. Un seul élément peut être déplacé à la fois. Si l’index de l’élément à déplacer n’existe pas, un HTTP 400 sera retourné. L’index du point d’insertion suit les mêmes règles que l’insertion d’une standard. Par défaut est 0 : début de la liste et n’importe quel nombre supérieur au nombre d’éléments dans la liste de réinsérer l’élément à la fin de la liste. De même la fin de la liste peut être indiquée en passant « end » pour insertIndex. 
 
Cet appel permet également d’identifier l’élément à être déplacé par l’itemId (ou de la liste déroulante de fournisseur/providerId). Dans ce cas, l’ID d’élément doit être passé dans le corps de la demande et il doit correspondre à un élément existant dans la liste. Si elle n’est pas le cas, une erreur HTTP 400 s’affichera avec IndexOutOfRange dans la description ; pour cette version spéciale de l’appel, utilisez « -1 » en tant que l’index de l’élément à déplacer. 
 
Cet appel nécessite un « si-correspondance : versionNumber « en-tête à inclure dans la demande si vous spécifiez l’élément par index. Si vous utilisez itemId pour indiquer quel élément à déplacer, l’en-tête « If-Match » est facultative. S’il est présent, l’en-tête « If-Match » est toujours validée. Dans l’en-tête « if-Match », versionNumber est le numéro de version actuelle de la liste. Si elle n'est pas inclus (et nécessaire), ou ne correspond pas à l’actuelle version liste nombre, puis un HTTP 412 – code d’état d’échec de la condition préalable est renvoyée et le corps de la réponse contient les métadonnées les plus récentes de la liste qui inclut le numéro de version actuel . Pour cela, pour vous protéger contre les mises à jour à partir de différents clients trampling sur eux. 
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI 
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| XUID| chaîne| XUID de l’utilisateur.| 
| nom de la liste| chaîne| Nom de la liste à manipuler.| 
| index| chaîne| Spécifie l’index actuel de l’élément à déplacer. Si la valeur d’index est égal à zéro ou un entier positif, cela fait référence à l’index de l’élément actuel et le corps de la demande de l’appel doit être vide. Toutefois, si la valeur d’index est « -1 », l’élément à déplacer doit être spécifié par ItemId ou fournisseur/ProviderID dans le corps de la demande de l’appel.| 
  
<a id="ID4EWC"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête 
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| chaîne| Spécifie l’emplacement de la liste pour insérer l’élément. Valeurs autorisées sont zéro, des entiers positifs et « end ». « end » place l’élément à la fin de la liste actuelle. Si la valeur spécifiée est au-delà de la fin de la liste, l’élément est inséré à la fin de la liste. | 
  
<a id="ID4EVD"></a>

 
## <a name="request-body"></a>Corps de la requête 
 
Un corps de demande est envoyé uniquement lorsque vous spécifiez l’élément à déplacer par itemId ou par le fournisseur/ProviderId.
 
<a id="ID4E6D"></a>

  
Exemple de demande 

```cpp
{
  "Item":
  {
    "ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId":  null,
    "Provider": null
  }
}
    
```

  
<a id="ID4EEE"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP 
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4E1BAC"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| Contient le jeton STS utilisé pour authentifier et autoriser la demande. Doit être un jeton à partir du service XSTS et inclure le XUID dans l’un des revendications. | 
| Version X-XBL-contrat| Spécifie la version de l’API est demandées (entiers positifs). Version de prend en charge des codes confidentiels 2. Si cet en-tête est manquant ou la valeur n’est pas pris en charge le service renvoie un code 400 – demande incorrecte qu’avec « en-tête de version de contrat non pris en charge ou manquant » dans la description d’état.| 
| Content-Type| Spécifie si le contenu du corps de demande/réponse est dans json ou xml. Valeurs prises en charge sont « application/json » et « application/xml »| 
| If-Match| Cet en-tête doit contenir le numéro de version en cours d’une liste lors de l’établissement de modifier des demandes. Utilisation de demandes PUT, POST, de modifier ou supprimer des verbes. Si la version dans l’en-tête « If-Match » ne correspond pas à la version actuelle de la liste, la demande est rejetée avec un HTTP 412 – Échec de la précondition code de retour. (facultatif) Peut également être utilisé pour obtient, si présent et la version passée correspond à la version actuelle de la liste puis aucune donnée de liste et un HTTP 304 – pas modifié le code sera retourné. | 
  
<a id="ID4EQDAC"></a>

 
## <a name="response-body"></a>Corps de la réponse 
 
Si l’appel réussit, le service retourne les métadonnées les plus récentes pour obtenir la liste. 
 
<a id="ID4E1DAC"></a>

 
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

   
<a id="ID4EIEAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EKEAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}](uri-usersxuidlistspinslistnameindex.md)

   