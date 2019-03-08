---
title: /serviceconfigs/{scid}/sessions
assetID: d1932817-dcce-5a5c-d7e4-9fd97e1d287c
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessions.html
description: " /serviceconfigs/{scid}/sessions"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 700575ee9e9afa7bc7a1d34388dc071873d3b9ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612574"
---
# <a name="serviceconfigsscidsessions"></a>/serviceconfigs/{scid}/sessions
Prend en charge une opération GET pour récupérer un ensemble de documents de la session. 
<a id="ID4EO"></a>

 
## <a name="domain"></a>domaine.
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| scid| GUID| Identificateur de configuration de service (SCID). ID de la partie 1 de la session.| 
| keyword| chaîne| Un mot clé utilisé pour filtrer les résultats aux sessions simples identifiées par cette chaîne.| 
| xuid| GUID| ID d’utilisateur de Xbox pour les utilisateurs pour lequel vous souhaitez récupérer les sessions. Les utilisateurs doivent être actives dans les sessions.| 
| réservations| chaîne| Valeur qui indique si la liste des sessions inclut celles que les utilisateurs n’ont pas accepté. Ce paramètre peut uniquement être défini sur true. Ce paramètre exige de l’appelant à accéder au niveau du serveur à la session ou en XUID l’appelant demander faire correspondre le filtre d’ID utilisateur Xbox. | 
| inactif| chaîne| Valeur qui indique si la liste des sessions inclut celles qui les utilisateurs ont accepté mais ne sont pas en cours d’exécution. Ce paramètre peut uniquement être défini sur true.| 
| Privé| chaîne| Valeur qui indique si la liste des sessions inclut des sessions privées. Ce paramètre peut uniquement être défini sur true. Il est valide uniquement lors de l’interrogation de vos propres sessions, ou lors de l’interrogation de serveur à serveur. Ce paramètre sur true requiert que l’appelant à accéder au niveau du serveur à la session ou en XUID l’appelant demander faire correspondre le filtre d’ID utilisateur Xbox. | 
| visibility| chaîne| Valeur d’énumération indiquant l’état de visibilité utilisé lors du filtrage des résultats. Actuellement ce paramètre peut uniquement être défini à ouvrir pour inclure les sessions ouvertes. Consultez <b>MultiplayerSessionVisibility</b>.| 
| version| chaîne| Entier positif indiquant la version de session principale ou inférieur des sessions à inclure. La valeur doit être inférieure ou égale à la version de contrat de la demande modulo 100.| 
| Take| chaîne| Entier positif indiquant le nombre maximal de sessions à récupérer.| 
  
<a id="ID4E1D"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[OBTENIR (/serviceconfigs/ {scid} / sessions)](uri-serviceconfigsscidsessionsget.md)

&nbsp;&nbsp;Récupère des informations de session spécifiées.
 
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Parent 

[URI du répertoire de session](atoc-reference-sessiondirectory.md)

   