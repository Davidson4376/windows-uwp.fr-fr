---
title: DELETE (/users/xuid(xuid)/lists/PINS/{listname})
assetID: b43e3faa-7791-8bcb-3aec-7bdad8ffbebf
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamedelete.html
description: " DELETE (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eed1d73917be450038fdf098b802d0d7c1c44a7b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603344"
---
# <a name="delete-usersxuidxuidlistspinslistname"></a>DELETE (/users/xuid(xuid)/lists/PINS/{listname})
Supprime les éléments d’une liste. Le domaine pour ces URI est `eplists.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EIB)
  * [Paramètres de chaîne de requête](#ID4ETB)
  * [Autorisation](#ID4ETC)
  * [En-têtes de demande nécessaires](#ID4EAD)
  * [Corps de la demande](#ID4EWE)
  * [Codes d’état HTTP](#ID4EBF)
  * [Corps de la réponse](#ID4E6BAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
Éléments à supprimer sont indiquées par leur index dans la liste et sont identifiés dans le paramètre de chaîne de requête **index**. La liste d’index doit être délimitée par des virgules et les index uniquement 100 peuvent être passés dans un seul appel. Toutefois, si aucun index n’est transmis (paramètre de chaîne de requête vide) puis le contenu de la liste entière est supprimé (une liste vide restera et continue à incrémenter le numéro de version). Une fois que les éléments sont supprimés, la liste est « compactée » tels qu’aucune faille n’est laissés dans l’ordre d’index. Par conséquent, cet appel n’est pas idempotente.
 
Cet appel nécessite un **si-correspondance : versionNumber** en-tête à inclure dans la demande, où **versionNumber** est le numéro de version actuelle du fichier. S’il n’est pas inclus, ou ne correspond pas au numéro de version actuelle de liste, puis un HTTP 412 (Échec de la précondition), code d’état est renvoyé et le corps de la réponse contient les métadonnées les plus récentes de la liste qui inclut le numéro de version actuel. Pour cela, pour vous protéger contre les mises à jour à partir de différents clients trampling sur l’autre.
  
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
| indexes| chaîne| Facultatif. Spécifie l’emplacement supprimer des éléments. Valeurs prises en charge : 0, entiers positifs et « end ». Valeur par défaut : 0.| 
 
Exemple : **index = 1, 8** supprime les éléments aux index 1 et 8. Les index doivent être uniques. Si aucun index n’est fourni, l’intégralité de la liste est effacée.
  
<a id="ID4ETC"></a>

 
## <a name="authorization"></a>Authorization
 
Cet appel attend un jeton XSTS SAML dans le **autorisation** en-tête. Une revendication Xuid doit exister au sein de ce jeton SAML pour identifier l’appelant. Cette valeur est utilisée pour déterminer si l’appelant dispose des droits d’accès aux données de liste en question. La liste elle-même est identifiée par le Xuid ainsi et sera incluse dans l’URI pour obtenir la liste. À l’aide de cela, nous peut prochainement accès prise en charge partagé à des listes, mais qui n’est pas une fonctionnalité pour l’instant. Actuellement, toutes les listes qui accède à un utilisateur sera leurs propres et il n’existe aucun accès partagé. Par conséquent, le Xuid dans l’URI doit correspondre à la Xuid dans le jeton de revendications SAML. 

> [!NOTE] 
> XBL Auth 2.0 et 3.0 jetons sont pris en charge à l’heure actuelle. 


  
<a id="ID4EAD"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| Contient le jeton STS utilisé pour authentifier et autoriser la demande. Doit être un jeton à partir du service XSTS et inclure le XUID dans l’un des revendications. | 
| Version X-XBL-contrat| Spécifie la version de l’API est demandées (entiers positifs). Version de prend en charge des codes confidentiels 2. Si cet en-tête est manquant ou la valeur n’est pas pris en charge le service renvoie un code 400 – demande incorrecte qu’avec « en-tête de version de contrat non pris en charge ou manquant » dans la description d’état.| 
| Content-Type| Spécifie si le contenu du corps de demande/réponse est dans json ou xml. Valeurs prises en charge sont « application/json » et « application/xml »| 
| If-Match| Cet en-tête doit contenir le numéro de version en cours d’une liste lors de l’établissement de modifier des demandes. Utilisation de demandes PUT, POST, de modifier ou supprimer des verbes. Si la version dans l’en-tête « If-Match » ne correspond pas à la version actuelle de la liste, la demande est rejetée avec un HTTP 412 – Échec de la précondition code de retour. (facultatif) Peut également être utilisé pour obtient, si présent et la version passée correspond à la version actuelle de la liste puis aucune donnée de liste et un HTTP 304 – pas modifié le code sera retourné. | 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EBF"></a>

 
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
  
<a id="ID4E6BAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
<a id="ID4EFCAC"></a>

 
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

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   