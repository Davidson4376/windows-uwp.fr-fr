---
title: POST (/titles/{titleId}/variants)
assetID: 84303448-5a11-d96f-907d-77f57f859741
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants-post.html
description: " POST (/titles/{titleId}/variants)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 17974ddf7dec26abac18ccee9fda5249bc9d656f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618494"
---
# <a name="post-titlestitleidvariants"></a>POST (/titles/{titleId}/variants)
URI appelée par un client qui Récupère une liste des variantes de jeux pour le titre spécifié ID. Les domaines pour ces URI sont `gameserverds.xboxlive.com` et `gameserverms.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EZ)
  * [En-têtes de demande nécessaires](#ID4EIB)
  * [En-têtes de demande facultatif](#ID4EED)
  * [Autorisation](#ID4E3D)
  * [Corps de la demande](#ID4EEE)
  * [En-têtes de réponse requis](#ID4ELF)
  * [En-têtes de réponse facultative](#ID4EMG)
  * [Corps de la réponse](#ID4EEH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Description| 
| --- | --- | 
| titleid| ID du titre qui doit traiter la demande.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Nom d'hôte

gameserverds.xboxlive.com
 
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
Lorsque vous élaborez une demande, les en-têtes affichés dans le tableau suivant sont requis.
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | 
| Content-Type| application/json| Type de données en cours de soumission.| 
| Host| gameserverds.xboxlive.com|  | 
| Content-Length|  | Longueur de l’objet de demande.| 
| x-xbl-contract-version| 1| Version de contrat d’API.| 
| Authorization| XBL3.0 x = [hash] ; [jeton]| Jeton d’authentification.| 
  
<a id="ID4EED"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
Lorsque vous élaborez une demande, les en-têtes affichés dans le tableau suivant sont facultatifs.
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | Le type mime du corps de la demande.| 
  
<a id="ID4E3D"></a>

 
## <a name="authorization"></a>Authorization

La requête doit inclure un en-tête d’autorisation de Xbox Live valide. Si l’appelant n’est pas autorisé à accéder à cette ressource, le service retourne 403 interdit en réponse. Si l’en-tête est manquant ou non valide, le service retourne 401 non autorisé dans la réponse.
 
<a id="ID4EEE"></a>

 
## <a name="request-body"></a>Corps de la demande
 
La demande doit contenir un objet JSON avec les membres suivants.
 
| Membre| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Paramètres régionaux| Local de variants à retourner.| 
| maxVariants| Le nombre maximal de variants à retourner.| 
| publisherOnly|  | 
| restriction|  | 
 
<a id="ID4EDF"></a>

 
### <a name="sample-request"></a>Exemple de demande
 

```cpp
{
  "locale": "en-us",
  "maxVariants": "100",
  "publisherOnly": "false",
  "restriction": null
}

```

   
<a id="ID4ELF"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
Une réponse inclut toujours les en-têtes affichés dans le tableau suivant.
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| application/json| Type de données dans le corps de réponse.| 
| Content-Length|  | Longueur du corps de réponse.| 
  
<a id="ID4EMG"></a>

 
## <a name="optional-response-headers"></a>En-têtes de réponse facultative
 
Une réponse peut inclure les en-têtes affichés dans le code suivant.
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | Le type mime du corps de réponse.| 
  
<a id="ID4EEH"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Si l’appel réussit, le service retourne un objet JSON avec les membres suivants.
 
| Membre| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Variantes| Un tableau de Variant.| 
| variantId| L’Id d’un variant.| 
| name| Le nom d’un variant.| 
| isPublisher|  | 
| rang|  | 
| gameVariantSchemaId|  | 
| variantSchemas| Un tableau de schémas variants.| 
| variantSchemaId| L’Id du schéma.| 
| schemaContent| Le contenu de schéma| 
| name| Nom du schéma| 
| gsiSets| Un tableau d’ensembles GSI.| 
| minRequiredPlayers| Le nombre minimal de lecteurs pour la variante.| 
| maxAllowedPlayers| Le nombre maximal de lecteurs pour la variante.| 
| gsiSetId| L’Id de l’ensemble GSI.| 
| gsiSetName| Le nom de l’ensemble GSI.| 
| selectionOrder|  | 
| variantSchemaId| ID du schéma varaint utilisé dans le GSI défini.| 
 
<a id="ID4EYBAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
 "variants": [
     { 
       "variantId": "8B6EF8A0-7807-42C4-9CB0-1D9B8B8CE742", 
       "name": "tankWarsV2.0",
       "isPublisher": "true",
       "rank": "1",
       "gameVariantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }],
  "variantSchemas": [
     {
        "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB",
        "schemaContent": "&lt;?xml version=\"1.0\" encoding=\"UTF-8\" ?>&lt;xs:schema xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">&lt;xs:element name=\"root\">&lt;/xs:element>&lt;/xs:schema>"
        "name": "tanksSchema"
     }],
     "gsiSets":
     [{ 
          "minRequiredPlayers": "5", 
          "maxAllowedPlayers": "10", 
          "gsiSetId": "B28047F5-B52F-477E-97C2-4C1C39E31D42",
          "gsiSetName": "TanksGSISet",
          "selectionOrder": "1",
          "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }]
 }

  

```

   
<a id="ID4ERCAC"></a>

 
## <a name="see-also"></a>Voir également
 [/titles/{titleId}/variants](uri-titlestitleidvariants.md)

  