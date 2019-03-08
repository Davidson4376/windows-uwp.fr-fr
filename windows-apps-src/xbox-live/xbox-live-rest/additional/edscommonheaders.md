---
title: En-têtes communs EDS
assetID: 91297c9e-709d-7886-1da0-8a896c4953f4
permalink: en-us/docs/xboxlive/rest/edscommonheaders.html
description: " En-têtes communs EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51a85383ee75fa51cb8376451ac955dcc4fa2edf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630704"
---
# <a name="eds-common-headers"></a>En-têtes communs EDS

<a id="ID4EO"></a>



## <a name="entertainment-discovery-services-eds-common-request-headers"></a>Les en-têtes de demande courants (EDS) des Services de découverte de divertissement

| Nom d’en-tête| Description| Obligatoire ?| Remarques|
| --- | --- | --- | --- |
| <b>x-xbl-contract-version</b>| Version du Service d’EDS| oui| 3.2|
| <b>x-xbl-client-type</b>| En-tête de Type de client| oui| Communiquez avec l’équipe pour obtenir votre propre Type de Client.|
| <b>x-xbl-client-version</b>| Version du client| oui| Toute chaîne non vide.|
| <b>x-xbl-parent-ig</b>| Guid de l’impression| oui| Utilisé pour effectuer le suivi de la demande dans les journaux et entre les autres appels de service.|
| <b>x-xbl-device-type</b>| Type de périphérique| oui| Appareil qui représente le client.|
| <b>Accepter</b>| Acceptez le type| oui| XML ou JSON.|
| <b>Autorisation</b>| En-tête d’authentification| oui|  |
| <b>x-xbl-autoSuggest-seed-text</b>| Texte de la valeur initiale de suggestion automatique| non| Utilisé pour la BI et la pertinence|
| <b>x-xbl-search-term</b>| Terme de recherche| non|  |
| <b>x-xbl-input-method</b>| Méthode d’entrée utilisé par l’utilisateur| non| Contrôleur, de reconnaissance vocale, Kinect.|
| <b>x-xbl-kinect-enabled</b>| Kinect activé| non| Oui/non.|
| <b>x-xbl-speech-session-id</b>| ID de session vocale| non| Indique si le début de session à l’aide de la reconnaissance vocale.|
| <b>x-xbl-client-id</b>| Id de Client anonyme| non| Utilisé pour la création de rapports BI et la pertinence.|
| <b>x-xbl-device-id</b>| ID Périphérique| non| Utilisé pour la création de rapports BI et la pertinence.|
| <b>x-xbl-user-agent</b>| Agent utilisateur du client| non| Utilisé pour le décisionnel. «&lt;nom > /&lt;version > (&lt;version du système d’exploitation > ; &lt;plateforme > ; &lt;fonctionnalité > ; &lt;fabriquer > ; &lt;modèle >) ».|
| <b>x-xbl-parent-ig</b>| Précédent Guid d’Impression pour les appels de « Chained »| non (mais fortement recommandé)| Il est important de pertinence de BI. Par exemple, IG d’un appel Parcourir est le parent de l’IG pour un suit un appel de détail.|
| <b>delid</b>| Déléguer l’identité| non| Utilisé par les services internes pour travailler pour le compte d’un utilisateur.|

## <a name="common-response-headers"></a>En-têtes de réponse courants

| Nom d’en-tête| Description| Obligatoire ?| Remarques|
| --- | --- | --- | --- | --- | --- | --- | --- |
| <b>Cache</b>| En-têtes de cache| oui| Consultez <a href="https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9"> http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9 </a>.|
| <b>x-xbl-errors</b>| Erreurs| non| Liste d’erreurs à partir de différents fournisseurs de données.|
| <b>x-xbl-traceid</b>| Id de trace à partir des journaux| oui| Permet de retrouver les journaux des demandes spécifiques.|
| <b>x-server-name</b>| Nom masqué du serveur qui gère la demande.| oui| Utilisé pour le débogage interne.|

<a id="ID4EECAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EGCAC"></a>


##### <a name="parent"></a>Parent  

[Référence supplémentaire](atoc-xboxlivews-reference-additional.md)


<a id="ID4ESCAC"></a>


##### <a name="further-information"></a>Informations supplémentaires

[URI de la place de marché](../uri/marketplace/atoc-reference-marketplace.md)
