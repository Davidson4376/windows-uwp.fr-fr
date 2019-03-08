---
title: GET (/inventory/{itemID})
assetID: d3ca14a5-0214-ef42-091e-3f05f2a3482d
permalink: en-us/docs/xboxlive/rest/uri-inventoryitemurlget.html
description: " GET (/inventory/{itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 446197eb20820304088ddac4a6379fa3b2510873
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606694"
---
# <a name="get-inventoryitemid"></a>GET (/inventory/{itemID})
Fournit l’ensemble des détails pour un élément d’inventaire spécifique. Le domaine pour ces URI est `inventory.xboxlive.com`.
 
  * [Remarques](#ID4EX)
  * [Paramètres d’URI](#ID4EAB)
  * [Corps de la réponse](#ID4ELB)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Notes
 
Aucune stratégie ne vérifie, l’application, ou de filtrage aura lieu dans le cadre de cet appel.
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| itemID| chaîne| l’ID unique pour chaque utilisateur pour un article en stock au singulier| 
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
<a id="ID4ERB"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 
La réponse à une demande GET, en supposant qu’elle transmet l’authentification et que vous est attribué le contexte d’autorisation appropriée, est un élément unique d’inventaire avec l’ensemble complet des propriétés de l’élément.
 

```cpp
{inventoryItem}
         
```

   
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Parent 

[GET (/inventory/{itemID})](uri-inventoryget.md)

  
<a id="ID4EJC"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [URI de la place de marché](atoc-reference-marketplace.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   