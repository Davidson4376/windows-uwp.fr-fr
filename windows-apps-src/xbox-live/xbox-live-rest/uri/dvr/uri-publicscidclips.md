---
title: /public/scids/{scid}/clips
assetID: 55a1f0ae-08bb-6d1e-a1da-cbeb481c42ee
permalink: en-us/docs/xboxlive/rest/uri-publicscidclips.html
description: " /public/scids/{scid}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: db279d546780ed40158894f73ecb84687ef35ba6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627974"
---
# <a name="publicscidsscidclips"></a>/public/scids/{scid}/clips
Clips publiques d’accès. Cet URI peut réellement être spécifié sous deux formes, `/public/scids/{scid}/clips` et `/public/titles/{titleId}/clips`. Pour plus d’informations, voir ci-dessous. Le domaine pour cet URI est `gameclipsmetadata.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4E1)
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| scid| chaîne| Identificateur de la configuration de service principal des éléments publics.| 
| titleid| chaîne| N ° titre des éléments publics. Ne peut pas être spécifié dans le même URI que le SCID. Si spécifié, sera utilisé pour rechercher le SCID principal.| 
  
<a id="ID4E6B"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[OBTENIR (/ public/scids / {scid} / découpe)](uri-publicscidclipsget.md)

&nbsp;&nbsp;Liste des éléments publics.
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[URI de la place de marché](../marketplace/atoc-reference-marketplace.md)

   