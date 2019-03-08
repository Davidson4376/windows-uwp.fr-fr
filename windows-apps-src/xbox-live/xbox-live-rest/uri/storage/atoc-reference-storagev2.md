---
title: URI de Stockage de titres
assetID: 32bba1e4-0980-785e-c098-a96cd88a8e5f
permalink: en-us/docs/xboxlive/rest/atoc-reference-storagev2.html
description: " URI de Stockage de titres"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e0296eff0937ea5075630db0e049c86e2ea2c8ce
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622674"
---
# <a name="title-storage-uris"></a>URI de Stockage de titres
 
Cette section fournit des détails sur les adresses de l’identificateur URI (Universal Resource) et de méthodes de protocole HTTP (Hypertext Transport) associées à partir de Xbox Live Services pour *titre stockage*.
 
Jeux qui s’exécutent sur toutes les plateformes peuvent utiliser ce service.
 
Le domaine pour ces URI est titlestorage.xboxlive.com.
 
<a id="ID4EFB"></a>

 
## <a name="in-this-section"></a>Dans cette section

[/ global/scids / {scid}](uri-globalscidsscid.md)

&nbsp;&nbsp;Récupère les informations de quota pour ce type de stockage.

[/global/scids/{scid}/data/{path}](uri-globalscidssciddatapath.md)

&nbsp;&nbsp;Répertorie des informations de fichier dans un chemin d’accès spécifié. 

[/global/scids/{scid}/data/{pathAndFileName},{type}](uri-globalscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Télécharge un fichier.

[/json/users/batch/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Télécharge plusieurs fichiers à partir de plusieurs utilisateurs avec le même nom de fichier.

[/json/users/xuid({xuid})/scids/{scid}](uri-jsonusersxuidscidsscid.md)

&nbsp;&nbsp;Récupère les informations de quota pour ce type de stockage.

[/json/users/xuid({xuid})/scids/{scid}/data/{path}](uri-jsonusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Répertorie des informations de fichier dans un chemin d’accès spécifié. 

[/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Téléchargements, télécharge ou supprime un fichier.

[/sessions/{sessionId}/scids/{scid}](uri-sessionssessionidscidsscid.md)

&nbsp;&nbsp;Récupère les informations de quota pour ce type de stockage.

[/sessions/{sessionId}/scids/{scid}/data/{path}](uri-sessionssessionidscidssciddatapath.md)

&nbsp;&nbsp;Répertorie des informations de fichier dans un chemin d’accès spécifié. 

[/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}](uri-sessionssessionidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Télécharge un fichier.

[/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Télécharge plusieurs fichiers à partir de plusieurs utilisateurs avec le même nom de fichier.

[/trustedplatform/users/xuid({xuid})/scids/{scid}](uri-trustedplatformusersxuidscidsscid.md)

&nbsp;&nbsp;Récupère les informations de quota pour ce type de stockage.

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}](uri-trustedplatformusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Répertorie des informations de fichier dans un chemin d’accès spécifié. 

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Téléchargements, télécharge ou supprime un fichier.

[/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Télécharge plusieurs fichiers à partir de plusieurs utilisateurs avec le même nom de fichier.

[/untrustedplatform/users/xuid({xuid})/scids/{scid}](uri-untrustedplatformusersxuidscidsscid.md)

&nbsp;&nbsp;Récupère les informations de quota pour ce type de stockage.

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}](uri-untrustedplatformusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Répertorie des informations de fichier dans un chemin d’accès spécifié. 

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Téléchargements, télécharge ou supprime un fichier.
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Parent 

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)

   