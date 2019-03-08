---
title: URI de l'annuaire de sessions
assetID: e3ba951d-b21f-0014-c358-2603d549d118
permalink: en-us/docs/xboxlive/rest/atoc-reference-sessiondirectory.html
description: " URI de l'annuaire de sessions"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9492ff3272af830404a546c9b01d62178adbac96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651194"
---
# <a name="session-directory-uris"></a>URI de l'annuaire de sessions

Cette section fournit des informations sur les adresses de l’identificateur URI (Universal Resource) et les méthodes de protocole HTTP (Hypertext Transport) associées à partir de Xbox Live Services pour le répertoire de Session multijoueur (MPSD).


> [!NOTE] 
> Uniquement les titres de jeux qui s’exécutent sur une Xbox 360, sur un appareil Windows Phone ou sur Xbox.com peuvent utiliser l’URI de l’annuaire de sessions.  


  * [Domaine](#ID4EUB)
  * [Version du service](#ID4EZB)
  * [Propriétés et des objets système](#ID4EAC)
  * [Handles](#ID4EBE)

<a id="ID4EUB"></a>


## <a name="domain"></a>domaine.
sessiondirectory.xboxlive.com  
<a id="ID4EZB"></a>


## <a name="service-version"></a>Version du service

Les appelants de ces URI REST doivent passer la valeur 104/105 ou version ultérieure pour X-Xbl--Version de contrat, l’en-tête HTTP qui spécifie la version de service des Services de découverte divertissement (EDS).

<a id="ID4EAC"></a>


## <a name="system-objects-and-properties"></a>Propriétés et des objets système

Pour la configuration de ses sessions et de modèles, le MPSD utilise un nombre d’objets JSON de session qui sont conformes avec des schémas fixes qui le répertoire applique et interprète. Lors des appels aux méthodes prises en charge par l’annuaire de sessions différents URI, ces objets sont validés et fusionnées, basés sur les schémas pris en charge. Les principaux objets JSON de configuration multijoueur sont :

   *  [MultiplayerActivityDetails (JSON)](../../json/json-multiplayeractivitydetails.md)
   *  [MultiplayerSession (JSON)](../../json/json-multiplayersession.md)
   *  [MultiplayerSessionReference (JSON)](../../json/json-multiplayersessionreference.md)
   *  [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md)


Les objets JSON associés qui concernent spécifiquement jeux sont :

   *  [GameMessage (JSON)](../../json/json-gamemessage.md)
   *  [GameResult (JSON)](../../json/json-gameresult.md)
   *  [GameSession (JSON)](../../json/json-gamesession.md)
   *  [GameSessionSummary (JSON)](../../json/json-gamesessionsummary.md)


<a id="ID4EBE"></a>


## <a name="handles"></a>Handles

Pour le mode multijoueur 2015 uniquement, les sessions sont accessibles via les handles de session. Plusieurs URI ont été ajoutés pour fournir des fonctionnalités pour prendre en charge des handles.  
<a id="ID4EFE"></a>


## <a name="in-this-section"></a>Dans cette section

[/handles](uri-handles.md)

&nbsp;&nbsp;Prend en charge une opération POST pour définir la session pour l’activité actuelle de l’utilisateur à afficher dans l’expérience d’utilisateur du tableau de bord Xbox One et inviter des membres de la session si nécessaire.

[/handles/{handleId}](uri-handleshandleid.md)

&nbsp;&nbsp;Prend en charge les opérations DELETE et GET pour les descripteurs de session spécifiés par l’identificateur.

[/handles/{handleId}/session](uri-handleshandleidsession.md)

&nbsp;&nbsp;Prend en charge les opérations GET et PUT pour une session, à l’aide de la poignée de déréférencement.

[/handles/query](uri-handlesquery.md)

&nbsp;&nbsp;Prend en charge les opérations POST pour créer des requêtes pour les descripteurs de session.

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)

&nbsp;&nbsp;Prend en charge une opération POST pour une requête par lot au niveau d’identificateur de la configuration de service.

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)

&nbsp;&nbsp;Prend en charge une opération GET pour récupérer un ensemble de documents de la session.

[/serviceconfigs/{scid}/sessiontemplates](uri-serviceconfigsscidsessiontemplates.md)

&nbsp;&nbsp;Prend en charge une opération GET pour récupérer un ensemble de modèles de session MPSD.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}](uri-serviceconfigsscidsessiontemplatessessiontemplatename.md)

&nbsp;&nbsp;Prend en charge une opération GET pour récupérer un jeu de noms de modèle de session.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.md)

&nbsp;&nbsp;Prend en charge une opération POST pour créer une requête par lot au niveau du modèle de session.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.md)

&nbsp;&nbsp;Prend en charge une opération GET pour récupérer un ensemble de modèles de session avec les noms de modèle spécifié.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)

&nbsp;&nbsp;Prend en charge les opérations GET et PUT pour créer et récupérer des sessions.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.md)

&nbsp;&nbsp;Prend en charge une opération de suppression pour supprimer le membre de la session spécifiée.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.md)

&nbsp;&nbsp;Prend en charge une opération de suppression pour supprimer des membres de la session.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)

&nbsp;&nbsp;Prend en charge une opération de suppression pour supprimer le serveur spécifié d’une session.

<a id="ID4ESF"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EUF"></a>

   [URI de Matchmaking](../matchtickets/atoc-reference-matchtickets.md)


<a id="ID4E1F"></a>


##### <a name="parent"></a>Parent

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)
