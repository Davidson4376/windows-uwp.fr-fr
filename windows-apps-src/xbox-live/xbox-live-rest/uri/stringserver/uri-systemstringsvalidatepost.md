---
title: POST (/system/strings/validate)
assetID: 6a59bc0b-8edd-87bf-efaf-f16efa3bedf7
permalink: en-us/docs/xboxlive/rest/uri-systemstringsvalidatepost.html
description: " POST (/system/strings/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 70e86567f449674c7a046e072437d9ee715dc6d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595504"
---
# <a name="post-systemstringsvalidate"></a>POST (/system/strings/validate)
Accepte un tableau de chaînes pour la validation et retourne un tableau de résultats de taille égale. Le domaine pour ces URI est `client-strings.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [En-têtes de demande nécessaires](#ID4EIB)
  * [Corps de la demande](#ID4ELC)
  * [Codes d’état HTTP](#ID4E4C)
  * [Corps de la réponse](#ID4ETF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
Chaque résultat indique si la chaîne correspondante est acceptable sur Xbox LIVE et qu’il contient la chaîne problématique, le cas échéant.
 
Chaînes identiques donnera toujours des résultats identiques. Si vous recevez un résultat non réussies, analyser le résultat et modifier la chaîne en conséquence.
 
 

> [!NOTE] 
> Résultant <b>VerifyStringResult</b> signalera uniquement le premier mot incriminé dans la chaîne. Il peut y avoir supplémentaires incriminé mots dans la chaîne. Si vous envisagez de remplacer les mots incriminés pour que la chaîne soit utilisable, vous devez remplacer le mot ou la sous-chaîne qui posent problème et vérifier à nouveau la chaîne à rechercher des sous-chaînes incriminés supplémentaires.  

 
  
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Description| 
| --- | --- | --- | 
| Authorization| Jeton d’authentification. Exemple : XBL3.0 x = [hash] ; [jeton].| 
| x-xbl-contract-version| Version de contrat entier API. Doit être 1 ou 2 pour cette API.| 
  
<a id="ID4ELC"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Le corps de la demande est un tableau de chaînes, dont aucune limite concernant la taille du tableau et 512 caractères par chaîne.
 
<a id="ID4ETC"></a>

 
### <a name="sample-request"></a>Exemple de demande
 

```cpp
{
    "stringstoVerify":
    [
        "Poppycock",
        "The quick brown fox jumped over the lazy dog.",
        "Hey, want to hang out?",
        "oh noes"
    ]
}
      
```

   
<a id="ID4E4C"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | 
| 200| OK| Toutes les chaînes ont été traités avec succès. Cela ne signifie pas nécessairement toutes les chaînes avaient HRESULT positif.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 406| Non Acceptable| Manquant <b>Content-type : application/json</b> en-tête.| 
| 408| Expiration du délai de la demande| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
  
<a id="ID4ETF"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Retourne un tableau de [VerifyStringResult (JSON)](../../json/json-verifystringresult.md), de la même taille que le tableau de la demande.
  
<a id="ID4EAG"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ECG"></a>

 
##### <a name="parent"></a>Parent 

[/system/strings/validate](uri-systemstringsvalidate.md)

   