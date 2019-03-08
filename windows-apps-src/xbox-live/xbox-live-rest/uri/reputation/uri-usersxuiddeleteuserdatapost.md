---
title: POST (/users/xuid({xuid})/deleteuserdata)
assetID: 8be13ff9-5d42-43a1-f2fa-d550d845a552
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddeleteuserdatapost.html
description: " POST (/users/xuid({xuid})/deleteuserdata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab43079dbba3729ff39f3a2116c377c3b73142a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604064"
---
# <a name="post-usersxuidxuiddeleteuserdata"></a>POST (/users/xuid({xuid})/deleteuserdata)
Réinitialise entièrement les données de réputation d’un utilisateur de test. Fins de test uniquement.

  * [Remarques](#ID4EQ)
  * [Paramètres d’URI](#ID4E5)
  * [Autorisation](#ID4EJB)
  * [En-têtes de demande nécessaires](#ID4E3B)
  * [Codes d’état HTTP](#ID4EHC)
  * [Corps de la réponse](#ID4EJF)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Notes

Appel de cette API supprimera tous les commentaires et les données de réputation d’un utilisateur. Les partenaires peuvent appeler cette API par rapport à n’importe quel bac à sable, à l’exception de vente au détail. L’équipe de l’application peut appeler cette API avec n’importe quel ID de bac à sable.

Le domaine pour ces URI est `reputation.xboxlive.com`. Cet URI est toujours appelé sur le port 10443.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| xuid| entier non signé 64 bits| Xbox utilisateur ID (XUID) de l’utilisateur dont les données sont en cours de suppression.|

<a id="ID4EJB"></a>


## <a name="authorization"></a>Authorization

Pour le bac à sable de la vente au détail, **PartnerClaim** à partir de l’équipe de mise en œuvre.

Pour tous les autres bacs à sable, **PartnerClaim** et **SandboxIdClaim**.

<a id="ID4E3B"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

**Content-Type : application/json** et **X-Xbl-contrat-Version** (version actuelle est 101).

<a id="ID4EHC"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Expression du motif| Description|
| --- | --- | --- | --- | --- | --- |
| 200| OK| La session a été récupérée.|
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.|
| 401| Non autorisé| La demande requiert une authentification utilisateur.|
| 404| Introuvable| Impossible de trouver la ressource spécifiée.|
| 500| erreur de serveur interne| Le serveur a rencontré une situation inattendue qui l’a empêché de répondre à la demande.|
| 503| Service non disponible| La demande a été limitée, la demande, puis réessayez la valeur de nouvelle tentative du client en secondes (par exemple, 5 secondes plus tard).|

<a id="ID4EJF"></a>


## <a name="response-body"></a>Corps de la réponse

Aucun cas de réussite ; Sinon, un [constatez (JSON)](../../json/json-serviceerror.md) document.

<a id="ID4EWF"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EYF"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)
