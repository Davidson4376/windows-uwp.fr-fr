---
title: /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: edcb19bd-87a5-732b-0c45-6f7355fc2dd1
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindex.html
description: " /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b56b563c72c206340aa2c1ce9f73aa8dfe50809d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641224"
---
# <a name="usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
Déplace un élément dans une liste. Le domaine pour ces URI est `eplists.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI 
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| XUID| chaîne| XUID de l’utilisateur.| 
| nom de la liste| chaîne| Nom de la liste à manipuler.| 
| index| chaîne| Spécifie l’index actuel de l’élément à déplacer. Si la valeur d’index est égal à zéro ou un entier positif, cela fait référence à l’index de l’élément actuel et le corps de la demande de l’appel doit être vide. Toutefois, si la valeur d’index est « -1 », l’élément à déplacer doit être spécifié par ItemId ou fournisseur/ProviderID dans le corps de la demande de l’appel. | 
  
<a id="ID4EHC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[POST](uri-usersxuidlistspinslistnameindexpost.md)

&nbsp;&nbsp;Déplace un élément dans une liste vers un autre emplacement dans la liste.
 
<a id="ID4ERC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ETC"></a>

 
##### <a name="parent"></a>Parent 

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)

   