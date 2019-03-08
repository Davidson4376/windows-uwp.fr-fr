---
title: Demande HTTP standard et en-têtes de réponse
assetID: a5f8fd96-9393-5234-04ad-837e5c117c92
permalink: en-us/docs/xboxlive/rest/httpstandardheaders.html
description: " Demande HTTP standard et en-têtes de réponse"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca97e82365eab40266b3ffdd84924f71289eede6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636514"
---
# <a name="standard-http-request-and-response-headers"></a>Demande HTTP standard et en-têtes de réponse
 
<a id="ID4ES"></a>

 
## <a name="request-headers"></a>En-têtes de demande
 
Le tableau suivant répertorie les en-têtes HTTP standard utilisés pour effectuer des demandes de Services de Xbox Live.
 
| En-tête| Valeur| Description| 
| --- | --- | --- | 
| x-xbl-contract-version| 1| Version de contrat d’API. Requis sur toutes les demandes de Services de Xbox Live.| 
| Authorization| STSTokenString| Jeton d’authentification STS. La valeur de cet en-tête est récupérée à partir de la <b>GetTokenAndSignatureResult.Token</b> propriété. | 
| Content-Type| application/xml, application/json, multipart/form-data ou application/x--www-form-urlencoded| Spécifie le type de contenu est soumise avec une demande.| 
| Content-Length| Valeur entière| Spécifie la longueur des données en cours de soumission dans une requête POST.| 
| Accept-Language | Chaîne| Spécifie comment localiser les chaînes retournées. Consultez <a href="https://msdn.microsoft.com/en-us/library/bb975829.aspx">avancé de Xbox 360 programmation</a> pour obtenir la liste des combinaisons de langue/région valide.| 
  
<a id="ID4E6C"></a>

 
## <a name="response-headers"></a>En-têtes de réponse
 
Le tableau suivant répertorie l’en-tête HTTP standard utilisé dans les réponses des Services de Xbox Live.
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| application/xml, application/json| Spécifie le type de contenu qui est retourné.| 
| Content-Length| Valeur entière| Spécifie la longueur des données renvoyées.| 
  
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Parent  

[Référence supplémentaire](atoc-xboxlivews-reference-additional.md)

   