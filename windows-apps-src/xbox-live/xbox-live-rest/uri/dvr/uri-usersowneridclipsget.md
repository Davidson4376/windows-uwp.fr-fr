---
title: GET (/users/{ownerId}/clips)
assetID: da972b4e-bc38-66f5-2222-5e79d7c8a183
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclipsget.html
description: " GET (/users/{ownerId}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7c52daf4a07914c34f1aadc84a7238771669d65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623204"
---
# <a name="get-usersowneridclips"></a>GET (/users/{ownerId}/clips)
Récupérer la liste des éléments de l’utilisateur.
Les domaines pour ces URI sont `gameclipsmetadata.xboxlive.com` et `gameclipstransfer.xboxlive.com`, selon la fonction de l’URI en question.

  * [Remarques](#ID4EX)
  * [Paramètres d’URI](#ID4EEB)
  * [Paramètres de chaîne de requête](#ID4EPB)
  * [Corps de la demande](#ID4EPE)
  * [En-têtes de réponse requis](#ID4E1E)
  * [En-têtes de réponse facultative](#ID4ENH)
  * [Corps de la réponse](#ID4EOAAC)
  * [URI connexes](#ID4EABAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Notes

Cette API permet de différentes façons de liste d’un utilisateur propre découpe des clips, ainsi que d’autres utilisateurs qui sont stockés dans le service. Plusieurs points d’entrée renvoient des données à partir de différents niveaux et autoriser pour le filtrage via les paramètres de requête. Si le XUID dans la revendication correspond au propriétaire spécifié dans l’URI, les éléments de l’utilisateur sont retournés après vérification de l’isolation du contenu. Si le propriétaire dans l’URI ne correspond pas à la revendication XUID, puis les éléments de l’utilisateur spécifié sont retournées en fonction des vérifications de la confidentialité et isolation du contenu par rapport à la demande XUID.

Les requêtes sont optimisées par utilisateur par id de configuration de service (scid). Spécification de filtres ou autres que les valeurs par défaut, les ordres de tri supplémentaire comme indiqué ci-dessous peut dans certains cas prendre plus de temps à retourner. Il s’agit plus évidente pour les jeux plus volumineux de vidéos par utilisateur.

Il n’existe aucune API de lot pour obtenir la liste de plusieurs utilisateurs au sein de l’appel d’API. Le modèle recommandé (actuellement) à partir des architectes SLS consiste à interroger pour chaque utilisateur à son tour.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| ownerId| chaîne| Identité de l’utilisateur de l’utilisateur dont la ressource est accédée. Formats pris en charge : « me » ou « xuid(123456789) ». Longueur maximum : 16.|

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

| Paramètre| Type| Description|
| --- | --- | --- | --- | --- | --- |
| skipItems| entier signé 32 bits| Facultatif. Retourner les éléments en commençant à N + 1 dans la collection (par exemple, skip N éléments).|
| continuationToken| chaîne| Facultatif. Retourner les éléments commençant le jeton de continuation donné. Le paramètre continuationToken est prioritaire sur skipItems si les deux sont fournies. En d’autres termes, le paramètre skipItems est ignoré si le paramètre continuationToken est présent. Taille maximale : 36.|
| maxItems| entier signé 32 bits| Facultatif. Nombre maximal d’éléments à retourner à partir de la collection (peuvent être combinées avec skipItems et continuationToken pour retourner une plage d’éléments). Le service peut fournir une valeur par défaut si maxItems n’est pas présent et peut retourner moins de maxItems (même si la dernière page de résultats n’a pas encore été retournée).|
| Commande| Caractère Unicode| Facultatif. Spécifie si la liste est retournée dans (D) escending (valeur la plus élevée tout d’abord) ou (A) croissant (valeur la plus basse tout d’abord) ordre. Default : D.|
| type| GameClipTypes| Facultatif. Jeu délimité par des virgules du type des éléments à retourner. Default : Tous les.|
| eventId| chaîne| Facultatif. Jeu délimité par des virgules d’eventIDs pour filtrer les résultats par. Default : valeur null.|
| qualifer| chaîne| Facultatif. Spécifie le qualificateur de commande à utiliser pour l’obtention des éléments. <ul><li>créé : Spécifie les éléments sont retournés dans l’ordre de date dans le système</li><li>évaluation - [cotés] - Spécifie que les éléments sont retournés par leur valeur d’évaluation</li><li>vues - [les plus visitées] - Spécifie que les éléments sont retournés par le nombre de vues</li></ul><br/> Taille maximale : 12. Par défaut : « créé ».| 

<a id="ID4EPE"></a>


## <a name="request-body"></a>Corps de la requête

Il n’existe aucun membre requis pour cette demande.

<a id="ID4E1E"></a>


## <a name="required-response-headers"></a>En-têtes de réponse requis

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.|
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.|
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache.|
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.|
| Retry-After| chaîne| Indique le client d’essayer ultérieurement dans le cas d’un serveur non disponible.|
| Varier| chaîne| Indique comment mettre en cache des réponses à des proxys en aval.|

<a id="ID4ENH"></a>


## <a name="optional-response-headers"></a>En-têtes de réponse facultative

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple : « 686897696a7c876b7e ».|

<a id="ID4EOAAC"></a>


## <a name="response-body"></a>Corps de la réponse

<a id="ID4EUAAC"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
       "gameClips":
       [
           {
               "xuid": "2716903703773872",
               "clipName": "[en-US] Localized Greatest Moment Name",
               "titleName": "[en-US] Localized Title Name",
               "gameClipLocale": "en-US",
               "gameClipId": "6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
               "state": "Published",
               "dateRecorded": "2013-06-14T01:02:55.4918465Z",
               "lastModified": "2013-06-14T01:05:41.3652693Z",
               "userCaption": "Set by user!",
               "type": "DeveloperInitiated",
               "source": "Console",
               "visibility": "Public",
               "durationInSeconds": 30,
               "scid": "00000000-0000-0012-0023-000000000070",
               "titleId": 354975,
               "rating": 3.75,
               "ratingCount": 245,
               "views": 7453,
               "titleData": "",
               "systemProperties": "",
               "savedByUser": false,
               "achievementId": "AchievementId",
               "greatestMomentId": "GreatestMomentId",
               "thumbnails": [
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/large",
                       "fileSize": 637293,
                       "thumbnailType": "Large"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/small",
                       "fileSize": 163998,
                       "thumbnailType": "Small"
                   }
               ],
               "gameClipUris": [
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest",
                       "uriType": "SmoothStreaming",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest(format=m3u8-aapl)",
                       "uriType": "Ahls",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
                       "fileSize": 88820,
                       "uriType": "Download",
                       "expiration": "2999-12-31T11:59:40Z"
                   }
               ]
           }
       ],
   "pagingInfo":
       {
           "continuationToken": null,
           "totalItems": 1
       }
   }

```


<a id="ID4EABAC"></a>


## <a name="related-uris"></a>URI connexes

L’URI suivant est identique à celui dans ce document, mais avec un paramètre de chemin d’accès supplémentaires pour spécifier un SCID principal. Uniquement les éléments de cet utilisateur pour ce SCID seront affichera. L’utilisateur demandeur doit avoir accès à la SCID demandé, sinon une erreur HTTP 403 sera retournée.

   * **/users/{ownerId}/scids/{scid}/clips**

<a id="ID4ENBAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EPBAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/clips](uri-usersowneridclips.md)
