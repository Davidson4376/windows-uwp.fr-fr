---
title: /users/{requestorId}/permission/validate
assetID: 400a9721-bf43-76df-4cd1-9f2ae6ca5035
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidate.html
description: " /users/{requestorId}/permission/validate"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a062fd417bae37fd66c944e0e534ef7a50de5fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599364"
---
# <a name="usersrequestoridpermissionvalidate"></a>/users/{requestorId}/permission/validate
 
  * [Paramètres d’URI](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| requestorId| chaîne| Obligatoire. Identificateur de l’utilisateur qui effectue l’action. Les valeurs possibles sont <code>xuid({xuid})</code> et <code>me</code>. Il doit s’agir d’un utilisateur connecté Exemple de valeur : <code>xuid(0987654321)</code>.| 
  
<a id="ID4ETB"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[OBTENIR (/users/ {requestorId} / autorisation/valider)](uri-privacyusersrequestoridpermissionvalidateget.md)

&nbsp;&nbsp;Obtient une réponse Oui ou non sur indique si l’utilisateur est autorisé à effectuer l’action spécifiée avec un utilisateur cible.

[POST (/users/ {requestorId} / autorisation/valider)](uri-privacyusersrequestoridpermissionvalidatepost.md)

&nbsp;&nbsp;Obtient un ensemble de réponses Oui ou non sur indique si l’utilisateur est autorisé à effectuer les actions spécifiées avec un ensemble d’utilisateurs cibles.
 
<a id="ID4EAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ECC"></a>

   [URI de la confidentialité](atoc-reference-privacyv2.md)

 [Énumération de PermissionId](../../enums/privacy-enum-permissionid.md)

   