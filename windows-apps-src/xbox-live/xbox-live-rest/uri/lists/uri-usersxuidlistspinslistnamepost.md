---
title: POST (/users/xuid(xuid)/lists/PINS/{listname})
assetID: 813c0bd2-aca9-a1f6-9e81-a84a4c897b1e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamepost.html
description: " POST (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d30e5407be128032947f3f701f59ef25a16daaea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589974"
---
# <a name="post-usersxuidxuidlistspinslistname"></a>POST (/users/xuid(xuid)/lists/PINS/{listname})
Insère des éléments dans la liste à l’index en fonction du paramètre de chaîne de requête **insertIndex**. Le domaine pour ces URI est `eplists.xboxlive.com`.
 
  * [Remarques](#ID4EY)
  * [Paramètres d’URI](#ID4ETB)
  * [Paramètres de chaîne de requête](#ID4E5B)
  * [Autorisation](#ID4EZC)
  * [Codes d’état HTTP](#ID4EGD)
  * [En-têtes de demande nécessaires](#ID4EEAAC)
  * [Exemple de demande](#ID4E1BAC)
  * [Corps de la réponse](#ID4EPCAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>Notes
 
Cet appel insère des éléments dans la liste à l’index en fonction du paramètre de chaîne de requête **insertIndex** (par défaut 0 ou au début de la liste). Tous les éléments dans le corps de la requête vont être insérées à ce stade dans la liste. Si le **insertIndex** est supérieur au nombre d’éléments dans la liste existante, les nouveaux éléments vont être insérées à la fin.
 
Éléments à insérer doivent avoir les champs requis indiqués dans les spécifications fonctionnelles ; Sinon, un HTTP 400 s’affichera. De même, si le résultat de l’instruction insert dépasse la taille maximale de la liste (définie par le type de liste) un HTTP 400 est retourné et rien ne sera inséré.
 
Si l’élément est *pas* à insérer au début ou à la fin de la liste, puis le **si-correspondance : versionNumber** en-tête est requis pour être inclus dans la demande. L’en-tête est facultatif si l’insertion est destiné au début ou à la fin. S’il est présent, l’en-tête sera validé, quel que soit l’emplacement d’insertion. Dans l’en-tête **VersionNumber** est le numéro de version actuelle de la liste. S’il n’est pas inclus et requis, ou ne correspond pas au numéro de version actuelle de liste, puis un HTTP 412 (Échec de la précondition) code d’état est renvoyé et le corps de la réponse contient les métadonnées les plus récentes de la liste qui inclut le numéro de version actuel. Il s’agit pour vous prémunir contre les mises à jour à partir de différents clients trampling sur l’autre.
 
Notez que cet appel n’est pas idempotent ; les appels répétés avec les mêmes données peut entraîner plusieurs insertions. Toutefois, dans la mesure où aucune liste prend actuellement en charge les doublons, des appels répétés seront probablement pour résultat HTTP 400 codes renvoyés.
  
<a id="ID4ETB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| chaîne| ID d’utilisateur de la Xbox (XUID).| 
| listtype| chaîne| Type de la liste (comment il est utilisé et comment il agit). Toujours « PIN » pour ces méthodes associées.| 
| nom de la liste| chaîne| Nom de la liste (liste d’un type donné pour agir sur). Toujours « XBLPins » pour les éléments dans les codes confidentiels.| 
  
<a id="ID4E5B"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| chaîne| Facultatif. Définit l’emplacement insérer des éléments. Valeurs prises en charge : 0, entiers positifs et « end ». Toute valeur d’index supérieur au nombre d’éléments de liste ajoutera le nouvel élément en bas de la liste et n’insère pas d’espace « vide » dans la liste. Valeur par défaut : 0.| 
  
<a id="ID4EZC"></a>

 
## <a name="authorization"></a>Authorization
 
Cet appel attend un jeton XSTS SAML dans le **autorisation** en-tête. Une revendication Xuid doit exister au sein de ce jeton SAML pour identifier l’appelant. Cette valeur est utilisée pour déterminer si l’appelant dispose des droits d’accès aux données de liste en question. La liste elle-même est identifiée par le Xuid ainsi et sera incluse dans l’URI pour obtenir la liste. À l’aide de cela, nous peut prochainement accès prise en charge partagé à des listes, mais qui n’est pas une fonctionnalité pour l’instant. Actuellement, toutes les listes qui accède à un utilisateur sera leurs propres et il n’existe aucun accès partagé. Par conséquent, le Xuid dans l’URI doit correspondre à la Xuid dans le jeton de revendications SAML. 

> [!NOTE] 
> XBL Auth 2.0 et 3.0 jetons sont pris en charge à l’heure actuelle. 


  
<a id="ID4EGD"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EEAAC"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| Contient le jeton STS utilisé pour authentifier et autoriser la demande. Doit être un jeton à partir du service XSTS et inclure le XUID dans l’un des revendications. | 
| Version X-XBL-contrat| Spécifie la version de l’API est demandées (entiers positifs). Version de prend en charge des codes confidentiels 2. Si cet en-tête est manquant ou la valeur n’est pas pris en charge le service renvoie un code 400 – demande incorrecte qu’avec « en-tête de version de contrat non pris en charge ou manquant » dans la description d’état.| 
| Content-Type| Spécifie si le contenu du corps de demande/réponse est dans json ou xml. Valeurs prises en charge sont « application/json » et « application/xml »| 
| If-Match| Cet en-tête doit contenir le numéro de version en cours d’une liste lors de l’établissement de modifier des demandes. Utilisation de demandes PUT, POST, de modifier ou supprimer des verbes. Si la version dans l’en-tête « If-Match » ne correspond pas à la version actuelle de la liste, la demande est rejetée avec un HTTP 412 – Échec de la précondition code de retour. (facultatif) Peut également être utilisé pour obtient, si présent et la version passée correspond à la version actuelle de la liste puis aucune donnée de liste et un HTTP 304 – pas modifié le code sera retourné. | 
  
<a id="ID4E1BAC"></a>

 
## <a name="sample-request"></a>Exemple de demande
 
**ContentType**, **ItemId** ou **ProviderId**, **fournisseur** et **paramètres régionaux** sont obligatoires.
 

```cpp
{"Items":
  [{
    "ContentType": "Movie",
    "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
    "ProviderId": "",
    "Provider": "",
    "ImageUrl": "https://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC", 
    "AltImageUrl": null, 
    "Title": "The Dark Knight", 
    "SubTitle": null, 
    "Locale": "en-us",
    "DeviceType": "XboxOne"
  }]}
      
```

  
<a id="ID4EPCAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
<a id="ID4EVCAC"></a>

 
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

   
<a id="ID4E6CAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EBDAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   