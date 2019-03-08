---
title: Référence des objets JSON (JavaScript Object Notation)
assetID: 8efcc6f3-d88a-1b15-bcfc-d79a24581b0a
permalink: en-us/docs/xboxlive/rest/atoc-xboxlivews-reference-json.html
description: " Référence des objets JSON (JavaScript Object Notation)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c46557e3fb837bebccbb1039fb416f3e9787af2a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626554"
---
# <a name="javascript-object-notation-json-object-reference"></a>Référence des objets JSON (JavaScript Object Notation)
 
JavaScript Objet Notation (JSON) est une notation légère, basée sur des normes et orientée objet pour encapsuler les données sur le web.
 
Xbox Live Services définit les objets JSON qui sont utilisés dans les demandes et réponses du service. Cette section fournit des informations de référence sur chaque objet JSON utilisé avec les Services Xbox Live.
 
<a id="ID4EHB"></a>

 
## <a name="in-this-section"></a>Dans cette section

[Prime (JSON)](json-achievementv2.md)

&nbsp;&nbsp;Un objet de la prime (version 2).

[ActivityRecord (JSON)](json-activityrecord.md)

&nbsp;&nbsp;Une chaîne formatée et localisée sur la présence riche d’un ou plusieurs utilisateurs.

[ActivityRequest (JSON)](json-activityrequest.md)

&nbsp;&nbsp;Une demande d’informations sur la présence riche d’un ou plusieurs utilisateurs.

[AggregateSessionsResponse (JSON)](json-aggregatesessionsresponse.md)

&nbsp;&nbsp;Contient les données agrégées pour les sessions de Fitness (pertinence) d’un utilisateur.

[BatchRequest (JSON)](json-batchrequest.md)

&nbsp;&nbsp;Un tableau de propriétés permettant de filtrer les informations de présence, tels que les utilisateurs, appareils et des titres.

[DeviceEndpoint (JSON)](json-deviceendpoint.md)

[DeviceRecord (JSON)](json-devicerecord.md)

&nbsp;&nbsp;Informations sur un périphérique, y compris son type et les titres actives dessus.

[Commentaires (JSON)](json-feedback.md)

&nbsp;&nbsp;Contient des informations de commentaires sur un lecteur.

[GameClip (JSON)](json-gameclip.md)

[GameClipsServiceErrorResponse (JSON)](json-gameclipsserviceerrorresponse.md)

&nbsp;&nbsp;Une partie facultative de la réponse à la /clips/ de /scids/ {scid} /users/ {ownerId} {gameClipId} / URI/format / {gameClipUriType} API.

[GameClipThumbnail (JSON)](json-gameclipthumbnail.md)

&nbsp;&nbsp;Contient les informations relatives à une miniature individuelle. Il peut y avoir plusieurs tailles de chaque élément, et il incombe au client de sélectionner celui approprié pour l’affichage.

[GameClipUri (JSON)](json-gameclipuri.md)

[GameMessage (JSON)](json-gamemessage.md)

&nbsp;&nbsp;Un objet JSON de définition des données d’un message dans la file d’attente des messages d’une session de jeu.

[GameResult (JSON)](json-gameresult.md)

&nbsp;&nbsp;Un objet JSON qui représente les données qui décrit les résultats d’une session de jeu.

[GameSession (JSON)](json-gamesession.md)

&nbsp;&nbsp;Un objet JSON qui représente les données du jeu d’une session multijoueur.

[GameSessionSummary (JSON)](json-gamesessionsummary.md)

&nbsp;&nbsp;Un objet JSON représentant les données de synthèse pour une session de jeu.

[GetClipResponse (JSON)](json-getclipresponse.md)

&nbsp;&nbsp;Encapsule le clip de jeu.

[HopperStatsResults (JSON)](json-hopperstatsresults.md)

&nbsp;&nbsp;Un objet JSON qui représente les statistiques pour une hopper.

[InitialUploadRequest (JSON)](json-initialuploadrequest.md)

&nbsp;&nbsp;Le corps d’un clip de jeu POST transférer les demandes.

[InitialUploadResponse (JSON)](json-initialuploadresponse.md)

[inventoryItem (JSON)](json-inventoryitem.md)

&nbsp;&nbsp;L’article en stock core représente l’élément standard sur lequel un droit peut être accordé.

[LastSeenRecord (JSON)](json-lastseenrecord.md)

&nbsp;&nbsp;Informations sur lorsque le système vu dernière un utilisateur, disponible lorsque l’utilisateur ne dispose d’aucune DeviceRecord valide.

[MatchTicket (JSON)](json-matchticket.md)

&nbsp;&nbsp;Un objet JSON représentant un ticket de correspondance, permet de localiser d’autres joueurs via l’annuaire de sessions multijoueur (MPSD) par les lecteurs.

[MediaAsset (JSON)](json-mediaasset.md)

&nbsp;&nbsp;Les éléments multimédias associés à la réalisation ou ses récompenses.

[MediaRecord (JSON)](json-mediarecord.md)

[MediaRequest (JSON)](json-mediarequest.md)

[MultiplayerActivityDetails (JSON)](json-multiplayeractivitydetails.md)

&nbsp;&nbsp;Un objet JSON qui représente le **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**.

[MultiplayerSessionReference (JSON)](json-multiplayersessionreference.md)

&nbsp;&nbsp;Un objet JSON qui représente le **MultiplayerSessionReference**. 

[MultiplayerSessionRequest (JSON)](json-multiplayersessionrequest.md)

&nbsp;&nbsp;L’objet JSON de demande transmise pour une opération sur un **MultiplayerSession** objet.

[MultiplayerSession (JSON)](json-multiplayersession.md)

&nbsp;&nbsp;Un objet JSON qui représente le **MultiplayerSession**. 

[PagingInfo (JSON)](json-paginginfo.md)

&nbsp;&nbsp;Contient des informations de pagination pour les résultats sont retournés dans les pages de données.

[PeopleList (JSON)](json-peoplelist.md)

&nbsp;&nbsp;Collection de [personne](json-person.md) objets.

[PermissionCheckBatchRequest (JSON)](json-permissioncheckbatchrequest.md)

&nbsp;&nbsp;Collection d’objets de PermissionCheckBatchRequest.

[PermissionCheckBatchResponse (JSON)](json-permissioncheckbatchresponse.md)

&nbsp;&nbsp;Les résultats d’une autorisation de traitement par lots recherchent une liste de valeurs d’autorisation pour plusieurs utilisateurs.

[PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md)

&nbsp;&nbsp;Les raisons d’une autorisation de lot vérifier pour la liste des valeurs d’autorisation pour un utilisateur cible unique.

[PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)

&nbsp;&nbsp;Les résultats d’une vérification à partir d’un seul utilisateur pour un paramètre d’autorisation unique par rapport à un utilisateur de cible unique.

[PermissionCheckResult (JSON)](json-permissioncheckresult.md)

&nbsp;&nbsp;Les résultats d’une vérification à partir d’un seul utilisateur pour un paramètre d’autorisation unique par rapport à un utilisateur de cible unique.

[Personne (JSON)](json-person.md)

&nbsp;&nbsp;Métadonnées relatives à une seule personne dans le système de personnes.

[PersonSummary (JSON)](json-personsummary.md)

&nbsp;&nbsp;Collection de [personne (JSON)](json-person.md) objets.

[Lecteur JSON)](json-player.md)

&nbsp;&nbsp;Contient les données d’un lecteur dans une session de jeu.

[PresenceRecord (JSON)](json-presencerecord.md)

&nbsp;&nbsp;Données relatives à la présence en ligne d’un utilisateur unique.

[Profil (JSON)](json-profile.md)

&nbsp;&nbsp;Les paramètres de profil personnel pour un utilisateur.

[Progression (JSON)](json-progression.md)

&nbsp;&nbsp;Progression de l’utilisateur vers le déverrouillage de la prime.

[Propriété (JSON)](json-property.md)

&nbsp;&nbsp;Contient des données de propriété fournies par le client pour les critères de requête matchmaking.

[QueryClipsResponse (JSON)](json-queryclipsresponse.md)

&nbsp;&nbsp;Encapsule la liste des éléments de jeu retournés, ainsi que des informations de pagination de la liste.

[quotaInfo (JSON)](json-quota.md)

&nbsp;&nbsp;Contient des informations de quota sur un groupe de titre.

[Exigence (JSON)](json-requirement.md)

&nbsp;&nbsp;Critères pour l’obtention et la distance à laquelle l’utilisateur est l’élaboration les déverrouiller.

[ResetReputation (JSON)](json-resetreputation.md)

&nbsp;&nbsp;Contient les scores de réputation de le base à laquelle les scores existant d’un utilisateur doivent être modifiées.

[Récompense (JSON)](json-reward.md)

&nbsp;&nbsp;La récompense associée à la réalisation.

[RichPresenceRequest (JSON)](json-richpresencerequest.md)

&nbsp;&nbsp;Demande d’informations sur les informations de présence riche doivent être utilisées.

[Constatez (JSON)](json-serviceerror.md)

&nbsp;&nbsp;Contient des informations sur une erreur retournée lors de l’échec d’un appel au service.

[ServiceErrorResponse (JSON)](json-serviceerrorresponse.md)

&nbsp;&nbsp;Lorsqu’une erreur de service est rencontrée, un code d’erreur HTTP approprié s’affichera. Si vous le souhaitez, le service peut également inclure un objet ServiceErrorResponse tel que défini ci-dessous. Dans les environnements de production, moins de données peut être inclus.

[SessionEntry (JSON)](json-sessionentry.md)

&nbsp;&nbsp;Contient les données d’une session de Fitness (pertinence).

[TitleAssociation (JSON)](json-titleassociation.md)

&nbsp;&nbsp;Un titre qui est associé à la réalisation.

[TitleBlob (JSON)](json-titleblob.md)

&nbsp;&nbsp;Contient des informations sur une vignette à partir de stockage.

[TitleRecord (JSON)](json-titlerecord.md)

&nbsp;&nbsp;Informations sur un titre, y compris son nom et un horodatage de dernière modification.

[TitleRequest (JSON)](json-titlerequest.md)

&nbsp;&nbsp;Demande d’informations sur un titre.

[UpdateMetadataRequest (JSON)](json-updatemetadatarequest.md)

&nbsp;&nbsp;Les métadonnées doivent être mises à jour pour un élément.

[Utilisateur (JSON)](json-user.md)

&nbsp;&nbsp;Contient les données de classement utilisateur.

[UserClaims (JSON)](json-userclaims.md)

&nbsp;&nbsp;Retourne des informations sur l’utilisateur authentifié actuel.

[UserList (JSON)](json-userlist.md)

&nbsp;&nbsp;Une collection de [utilisateur](json-user.md) objets.

[UserSettings (JSON)](json-usersettings.md)

&nbsp;&nbsp;Retourne les paramètres pour un utilisateur authentifié actuel.

[UserTitle (JSON)](json-usertitlev2.md)

&nbsp;&nbsp;Contient les données de titre d’utilisateur.

[VerifyStringResult (JSON)](json-verifystringresult.md)

&nbsp;&nbsp;Résultat des codes correspondant à chaque chaîne envoyée à [/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md).

[XuidList (JSON)](json-xuidlist.md)

&nbsp;&nbsp;Liste des XUIDs sur laquelle effectuer une opération.
 
<a id="ID4ENH"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EPH"></a>

 
##### <a name="parent"></a>Parent 

[Référence RESTful de Services de Xbox Live](../atoc-xboxlivews-reference.md)

  
<a id="ID4EZH"></a>

 
##### <a name="external-links-ecma-international-standard-262-ecmascript-language-specificationhttpswwwecma-internationalorgpublicationsfilesecma-stecma-262pdf"></a>Liens externes [ECMA International 262 Standard : Spécification du langage de ECMAScript](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-262.pdf)

   