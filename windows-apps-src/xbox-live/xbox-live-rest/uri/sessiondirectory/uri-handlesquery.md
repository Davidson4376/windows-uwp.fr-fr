---
title: /handles/query
assetID: e00d31ad-b9ba-8e52-1333-83192eab0446
permalink: en-us/docs/xboxlive/rest/uri-handlesquery.html
description: " /handles/query"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eaa148972ce1e65056470a6c4082cb4e50de3f09
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598564"
---
# <a name="handlesquery"></a>/handles/query
Prend en charge les opérations POST pour créer des requêtes pour les descripteurs de session. 

> [!NOTE] 
> Cet URI est utilisé en mode multijoueur 2015 et s’applique uniquement à cette version multijoueur et versions ultérieures. Il est prévu pour une utilisation avec le contrat de modèle 104/105 ou version ultérieure.  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>domaine.
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
Cet URI prend en charge les requêtes pour les descripteurs. Contrairement aux requêtes de session, qui sont des requêtes de chaîne et de traitement par lots, les requêtes de handle utilisent un style de processeur de requêtes. Jusqu'à 100 handles sont pris en charge.  
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
Aucune   
<a id="ID4EEB"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[POST (requête/handles /)](uri-handlesquerypost.md)

&nbsp;&nbsp;Crée des requêtes pour les descripteurs de session.

[POST (/handles/query?include=relatedInfo)](uri-handlesqueryincludepost.md)

&nbsp;&nbsp;Crée des requêtes pour les descripteurs de session qui incluent des informations de session associée.
 
<a id="ID4EQB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ESB"></a>

 
##### <a name="parent"></a>Parent 

[URI du répertoire de session](atoc-reference-sessiondirectory.md)

   