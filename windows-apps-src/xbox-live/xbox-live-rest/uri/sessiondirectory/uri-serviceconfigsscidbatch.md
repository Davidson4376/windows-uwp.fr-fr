---
title: /serviceconfigs/{scid}/batch
assetID: eb1b510f-d92e-ae9b-a3e6-0edf58b4f075
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatch.html
description: " /serviceconfigs/{scid}/batch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 208ee92106563372dd4d92a8c800cc08f513e8c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589714"
---
# <a name="serviceconfigsscidbatch"></a>/serviceconfigs/{scid}/batch
Prend en charge une opération POST pour une requête par lot au niveau d’identificateur de la configuration de service.

> [!IMPORTANT]
> Cette méthode est utilisée par le mode multijoueur 2015 et s’applique uniquement à cette version multijoueur et versions ultérieures. Il est prévu pour une utilisation avec le contrat de modèle 104/105 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 104/105 ou version ultérieure sur chaque demande.

<a id="ID4ER"></a>


## <a name="domain"></a>domaine.
sessiondirectory.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID). Partie 1 de l’identificateur de session.|

<a id="ID4ESB"></a>


## <a name="valid-methods"></a>Méthodes valides

[POST (/serviceconfigs/{scid}/batch)](uri-serviceconfigsscidbatchpost.md)

&nbsp;&nbsp;Crée une requête par lot sur plusieurs Xbox ID utilisateur pour la configuration du service.

<a id="ID4E3B"></a>


## <a name="see-also"></a>Voir également

<a id="ID4E5B"></a>


##### <a name="parent"></a>Parent

[URI du répertoire de session](atoc-reference-sessiondirectory.md)
